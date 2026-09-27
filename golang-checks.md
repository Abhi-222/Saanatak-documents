# GoLang CI Checks — Code Compilation

## Author Table

| Author | Created on | Version | Last edited on | L0 Reviewer    | L1 Reviewer  |L2 Reviewer |
|--------|------------|---------|----------------|----------------|--------------|-------------|
| Sahil  | 22/09/2026 | 1.0     | 22/09/2026     | Vishal Tyagi / Divya M.| Aayush Verma | Varun / Mahesh Kumar  |

## Table of Contents

1. [Introduction](#1-introduction)
2. [Why](#2-why)
3. [Go Code Compilation](#3-go-code-compilation)
4. [CI Workflow](#4-ci-workflow)
5. [Tools at a Glance](#5-tools-at-a-glance)
6. [CI Pipeline Stages](#6-ci-pipeline-stages)
7. [POC Results](#7-poc-results)
8. [Best Practices](#8-best-practices)
9. [Recommendation](#9-recommendation)
10. [Contact](#10-contact)
11. [References](#11-references)

---

## 1. Introduction

GoLang CI Checks are automated validations performed on Go source code whenever changes are integrated into a repository. In a CI environment, the goal is to catch problems early — before broken code moves on to testing, packaging, or deployment.

For this POC:
- **Jenkins** is used as the CI automation platform
- **GitHub** is used for source-code management
- The **native Go toolchain** handles dependency management and compilation

The primary compilation check is:

```
go build ./...
```

This verifies that all Go packages in the project compile successfully.

---

## 2. Why

| Benefit | Description |
|---------|-------------|
| Early Error Detection | Catches compilation problems before the code reaches later CI/CD stages |
| Automation | Compilation runs automatically via Jenkins, with no manual intervention |
| Consistency | The same build process runs on every CI execution |
| Fast Feedback | Developers get quick feedback when a change introduces a compilation error |
| Dependency Validation | Go Modules downloads and verifies dependencies before compilation |
| Deployment Protection | A failed compilation blocks broken code from progressing further |
| Repeatability | The compilation process is reproducible across CI runs |

---

## 3. Go Code Compilation

Go provides a native compiler and build toolchain. The primary command used in the CI check is:

```
go build ./...
```

**What does `go build` do?**
Compiles Go packages and reports errors if the source code fails to build.

**What does `./...` mean?**
Tells Go to process packages in the current module and all its subdirectories — a convenient way to validate compilation across a multi-package project.

### Compilation Flow

```
Go Source Code
      |
      v
  Go Compiler
      |
      v
Package Compilation
      |
      v
  Build Result
   /        \
SUCCESS   FAILURE
```

### Common Problems Detected

- Syntax errors
- Undefined variables
- Invalid imports
- Type mismatches
- Invalid function usage
- Dependency-related compilation issues

---

## 4. CI Workflow

```
Developer
    |
    v
GitHub Repository
    |
    v
   Jenkins
    |
    v
Checkout Source Code
    |
    v
Configure Go Environment
    |
    v
  go mod download
    |
    v
  go mod verify
    |
    v
  go build ./...
    |
    +----------------------+
    |                       |
    v                       v
Successful Compilation   Compilation Error
    |                       |
    v                       v
Jenkins SUCCESS         Jenkins FAILURE
```

**Process:**
1. Developer pushes code to GitHub
2. Jenkins retrieves the source code
3. Jenkins configures the Go environment
4. Project dependencies are downloaded
5. Dependencies are verified
6. Go packages are compiled
7. Jenkins reports SUCCESS or FAILURE

---

## 5. Tools at a Glance

| Tool | Purpose | Usage in CI |
|------|---------|-------------|
| GitHub | Source-code repository | Stores Go application source code |
| Jenkins | CI automation | Executes the CI pipeline |
| Go | Language and toolchain | Provides the compiler and build commands |
| Go Modules | Dependency management | Manages project dependencies |
| `go mod download` | Dependency download | Downloads required modules |
| `go mod verify` | Dependency verification | Verifies module integrity |
| `go build` | Code compilation | Validates that Go packages compile |

---

## 6. CI Pipeline Stages

### 6.1 Checkout

Jenkins retrieves the Go source code from the GitHub repository.

```
GitHub → Jenkins → Checkout
```

**Expected result:** `Checkout SUCCESS`

### 6.2 Configure Go Environment

Jenkins needs to be able to locate the Go executable. During the POC, Jenkins initially reported:

```
go: not found
```

**Fix:** Added the Go installation path to the Jenkins service environment:

```
/usr/local/go/bin
```

After this configuration, Jenkins executed Go commands successfully.

### 6.3 Download Dependencies

```
go mod download
```

Downloads the modules defined in the project's `go.mod` file.

**Expected result:** Dependency download `SUCCESS`

### 6.4 Verify Dependencies

```
go mod verify
```

Checks that downloaded module contents match their expected cryptographic checksums.

**POC result:** `all modules verified`

### 6.5 Compile Go Application

```
go build ./...
```

If compilation succeeds, Jenkins proceeds to the next CI stage. If it fails, Jenkins marks the pipeline as failed.

```
go build ./...
       |
       +----------------+
       |                |
       v                v
    SUCCESS          FAILURE
       |                |
       v                v
  Next Stage       Stop Pipeline
```

### 6.6 Jenkins Result

A successful pipeline returns:

```
Finished: SUCCESS
```

If `go build ./...` exits with a non-zero code, Jenkins marks the pipeline as `FAILURE`.

---

## 7. POC Results

| Test Case | Expected Result | Actual Result |
|-----------|-----------------|---------------|
| Checkout source code | Repository retrieved | PASS |
| Download dependencies | Modules downloaded | PASS |
| Verify modules | Modules verified | PASS |
| Compile Go packages | Build succeeds | PASS |
| Invalid Go source | Build fails | PASS |
| Jenkins failure detection | Pipeline marked FAILURE | PASS |

### Successful Compilation

```
GitHub → Checkout → go mod download → go mod verify → go build ./... → Jenkins SUCCESS
```

### Compilation Failure

A failure was tested by temporarily introducing invalid Go code:

```
Invalid Go Source Code
        |
        v
   go build ./...
        |
        v
   Compiler Error
        |
        v
 Non-Zero Exit Code
        |
        v
  Jenkins FAILURE
```

After the failure test, the temporary source-code change was reverted and the pipeline was run again to confirm a clean SUCCESS.

---

## 8. Best Practices

- Keep `go.mod` and `go.sum` under version control
- Pin a clearly defined Go version for the CI environment
- Run `go mod download` before compilation when an explicit dependency-preparation stage is needed
- Use `go mod verify` to validate dependency integrity
- Use `go build ./...` for multi-package projects
- Keep Go available in the Jenkins service `PATH`
- Store the Jenkinsfile in the application repository
- Keep CI stages separate to simplify troubleshooting
- Add `go test ./...` to validate application behavior alongside compilation
- Use `gofmt` to maintain standard Go formatting
- Add static analysis as the CI pipeline matures
- Never allow compilation failures to proceed to deployment
- Keep the Go version consistent between development and CI environments where practical

### Recommended Extended CI Flow

```
Checkout → Go Environment → go mod download → go mod verify
   → Formatting Check → go test ./... → go build ./...
   → Artifact → Deployment
```

---

## 9. Recommendation

For Go applications, use the native Go toolchain for the core compilation check:

- `go mod download` — dependency preparation
- `go mod verify` — dependency integrity verification
- `go build ./...` — compilation validation

Jenkins orchestrates these commands as part of the CI pipeline, while GitHub remains the source-code repository. The process can later be extended with `go test ./...` for testing and `gofmt` for formatting validation.

**Overall flow:**

```
GitHub → Jenkins → Dependencies → Verification → Compilation → Test → Next CI/CD Stage
```

---

## 10. Contact

| Name | Email |
|---|---|
| Yogesh | yogesh.rajput.snaatak@mygurukulam.co |

---

## 11. References

| Topic | Description |
|---|---|
| Go Documentation | Official documentation for the Go programming language |
| Go Command Documentation | Documentation for Go commands and module management |
| Go Modules | Official guidance for Go dependency and module management |
| `go build` Documentation | Documentation for the Go build command |
| Jenkins Documentation | Official Jenkins documentation |
| Jenkins Pipeline Documentation | Documentation for Jenkins CI/CD pipelines |
| GitHub Documentation | Official documentation for GitHub repositories and workflows |

### POC Environment

| Component | Version / Details |
|---|---|
| Operating System | Ubuntu 26.04.1 LTS |
| Architecture | x86_64 / amd64 |
| Go | 1.23.0 |
| Jenkins | 2.555.1 |
| Java | 21.0.12 |
| Source Control | GitHub |
| Build Tool | Go Compiler |
| Dependency Tool | Go Modules |

### Final CI Workflow

```
                    ┌───────────────────┐
                    │     Developer     │
                    └─────────┬─────────┘
                              │
                              v
                    ┌───────────────────┐
                    │      GitHub       │
                    │   Go Source Code  │
                    └─────────┬─────────┘
                              │
                              v
                    ┌───────────────────┐
                    │      Jenkins      │
                    └─────────┬─────────┘
                              │
                              v
                    ┌───────────────────┐
                    │      Checkout     │
                    └─────────┬─────────┘
                              │
                              v
                    ┌───────────────────┐
                    │  Go Environment   │
                    └─────────┬─────────┘
                              │
                              v
                    ┌───────────────────┐
                    │  go mod download  │
                    └─────────┬─────────┘
                              │
                              v
                    ┌───────────────────┐
                    │   go mod verify   │
                    └─────────┬─────────┘
                              │
                              v
                    ┌───────────────────┐
                    │   go build ./...  │
                    └─────────┬─────────┘
                              │
                       ┌──────┴──────┐
                       │             │
                       v             v
                ┌────────────┐ ┌────────────┐
                │   SUCCESS  │ │   FAILURE  │
                └──────┬─────┘ └──────┬─────┘
                       │              │
                       v              v
                ┌────────────┐ ┌────────────┐
                │ Next CI/CD │ │ Stop Build │
                │   Stage    │ │ / Notify   │
                └────────────┘ └────────────┘
```
