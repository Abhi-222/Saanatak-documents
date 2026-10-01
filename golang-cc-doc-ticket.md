<img width="731" height="273" alt="image" src="https://github.com/user-attachments/assets/e2b57a22-3f39-493f-81d2-04183bfc597e" />

# Code Compilation of Source Code Using Go

---
## Author Table

| **Author** | **Created on** | **Version** | **Last edited on** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ---------- | -------------- | ----------- | ------------------ | --------------- | --------------- | --------------- |
| Sahil      | 30-09-26       | v1.0        | 01-09-26           | Divya M. / Vishal | Aayush Verma  | Mahesh / Varun  |


## Table of Contents

1. [Introduction](#1-introduction)
2. [What is Go Code Compilation?](#2-what-is-go-code-compilation)
3. [Why is Compilation Required?](#3-why-is-compilation-required)
4. [Workflow](#4-workflow)
5. [Compilation Tools](#5-compilation-tools)
6. [Comparison](#6-comparison)
7. [Advantages](#7-advantages)
8. [POC](#8-poc)
9. [Best Practices](#9-best-practices)
10. [Recommendation and Conclusion](#10-recommendation-and-conclusion)
11. [Contact Information](#11-contact-information)
12. [References](#12-references)

---

## 1. Introduction

Go is a compiled programming language. Go source code is processed by a Go compiler/toolchain to produce executable machine code. A compilation check verifies that the Go source code can be successfully compiled without compilation errors.

---

## 2. What is Go Code Compilation?

Go code compilation is the process of converting Go source code into executable form using a Go compiler/toolchain.

The primary command for compilation validation is:

```bash
go build ./...
```

`go build ./...` checks that all packages in the Go module can be compiled successfully.

---

## 3. Why is Compilation Required?

Compilation checks are required to:

* Detect compilation errors early.
* Validate that the codebase is buildable.
* Identify package and dependency-related build problems.
* Provide faster feedback after code changes.
* Prevent compilation failures from progressing further.
* Support automated CI validation.

> **Note:** Compilation success does not guarantee that the application is functionally correct; testing is required for functional validation.

---

## Workflow

<img width="881" height="1253" alt="image" src="https://github.com/user-attachments/assets/9fdd6430-79c5-4e33-8624-475e370c3e2e" />

---

## 4. Compilation Tools

### 1. Official Go Toolchain

* Standard toolchain for Go application development.
* Suitable for backend applications and microservices.
* Provides commands such as `go build`, `go test`, and `go run`.

### 2. TinyGo

* Alternative Go compiler/toolchain.
* Designed mainly for resource-constrained environments.
* Supports targets such as microcontrollers and WebAssembly.
* Compatibility with normal Go projects depends on project requirements.

### 3. gccgo

* Alternative Go compiler implementation within the GCC ecosystem.
* Useful where GCC-based compilation is required.
* Provides an alternative to the standard Go compiler.

---

## 5. Comparison

| Feature                | Official Go        | TinyGo                   | gccgo                 |
| ---------------------- | ------------------ | ------------------------ | --------------------- |
| Type                   | Official toolchain | Alternative toolchain    | Alternative compiler  |
| General Go Apps        | Supported          | Depends on compatibility | Supported             |
| Microcontrollers       | Not primary target | Supported                | Not primary target    |
| WebAssembly            | Supported          | Supported                | Depends on setup      |
| Standard Compatibility | Highest            | Some limitations         | Differences may exist |
| Typical Use            | Go applications    | Constrained targets      | GCC ecosystem         |

---

## 6. Advantages

* **Early Error Detection:** Finds compilation errors before later stages.
* **Build Validation:** Confirms that packages can be compiled.
* **Faster Feedback:** Provides quick feedback after code changes.
* **Consistent Validation:** The same build command can be executed repeatedly.
* **Dependency Validation:** Helps identify package and dependency build problems.
* **Binary Creation:** Can generate an executable application binary.
* **Automation Friendly:** Suitable for automated CI checks.

---

## 7. POC

**Objective:** Verify that the `employee-api` Go project can be compiled successfully.

### Commands

```bash
cd ~/Opstree-Microservices/employee-api
go mod download
go build ./...
go build -o employee-api
ls -lh employee-api
```

### 8. Result

| Check               | Command                    | Result       |
| ------------------- | -------------------------- | ------------ |
| Dependency Download | `go mod download`          | Success      |
| Compilation         | `go build ./...`           | Success      |
| Binary Creation     | `go build -o employee-api` | Success      |
| Binary Verification | `ls -lh employee-api`      | 35 MB binary |

The POC successfully validated compilation and executable binary creation.

---

## 9. Best Practices

* Use the Official Go Toolchain for standard Go applications.
* Use `go build ./...` for compilation validation.
* Keep `go.mod` and `go.sum` under version control.
* Maintain a consistent Go version across environments.
* Use reproducible build commands.
* Validate compilation after code changes.
* Handle compilation errors before progressing.
* Use `go build -o` when a specific binary artifact is required.
* Combine compilation checks with appropriate testing and quality checks.

---

## 10. Recommendation and Conclusion

* Use the **Official Go Toolchain** for standard backend and microservice projects.
* Use `go build ./...` as the primary compilation validation check.
* Use `go build -o <binary>` when an executable artifact is required.
* Use TinyGo or gccgo when the project has requirements specifically suited to those toolchains.
* Compilation checks ensure that the codebase remains buildable, but they should not replace functional testing.

---

## 11. Contact Information

| Auntor | email |
|--------|-------|
| Sahil | [sahil.butola.snaatak@mygurukulam.co](mailto:sahil.butola.snaatak@mygurukulam.co) |

---

## 12. References

| [Go Documentation](https://go.dev/doc/) | Go Documentation |
| [Go Command Documentation](https://pkg.go.dev/cmd/go) | Go Command Documentation |
| [Go Modules](https://go.dev/ref/mod) | Go Modules |
| [TinyGo Documentation](https://tinygo.org/docs/) | TinyGo Documentation |
| [GCC gccgo Documentation](https://gcc.gnu.org/onlinedocs/gccgo/) | GCC gccgo Documentation |
| [POC]()| POC|
