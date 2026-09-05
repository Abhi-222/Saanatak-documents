# Common Stack | Applications | Golang | Installation Guide Documentation

## Author Table

| **Author**   | **Created On** | **Version** | **Last Edited On**    | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
|--------------|----------------|-------------|-----------------------|-----------------|-----------------|------------------|
| Sahil Butola | 30-08-2026     | 1.1         | 05-09-2026            | Divya M.        | Aayush Verma    | Mahesh Kumar |

## Table of Contents

1. [Introduction](#introduction)
2. [What Is Go?](#what-is-go)
3. [Why Is Go Used?](#why-is-go-used)
4. [Prerequisites](#prerequisites)
5. [Installing Go on Windows](#installing-go-on-windows)
6. [Installing Go on macOS](#installing-go-on-macos)
7. [Installing Go on Linux](#installing-go-on-linux)
8. [Verifying the Installation](#verifying-the-installation)
9. [Setting Up the Go Workspace](#setting-up-the-go-workspace)
10. [Writing and Running a First Go Program](#writing-and-running-a-first-go-program)
11. [Common Installation Issues](#common-installation-issues)
12. [Uninstalling Go](#uninstalling-go)
13. [Best Practices](#best-practices)
14. [Conclusion](#conclusion)
15. [Contact Information](#contact-information)
16. [References](#references)

## Introduction

Go, also known as Golang, is an open-source language developed by Google, used for backend services, CLI tools, and networked applications.
This guide covers installing Go on Windows, macOS, and Linux, verifying the setup, and running a first program.

## What Is Go?

Go is a statically typed, compiled language. A Go installation includes the compiler, standard library, and CLI tools, all accessed through the `go` command.

| Component | Purpose |
|---|---|
| `go` | CLI tool to build, run, and manage Go code |
| Go compiler | Compiles source code into a native binary |
| `GOROOT` | Directory where Go itself is installed |
| Go modules | Dependency management for Go projects |

## Why Is Go Used?

| Reason | Description |
|---|---|
| Fast compilation | Compiles directly to native machine code |
| Built-in concurrency | Goroutines make concurrent code simple |
| Static binaries | Single executable, no external runtime |
| Cross-platform | Builds binaries for other OS from one machine |

## Prerequisites

- Windows 10+, macOS 11+, or a common Linux distribution.
- Administrator or `sudo` access.
- Internet connection and ~500 MB free disk space.
- Know your machine's architecture (`amd64`/`x86_64` or `arm64`) before downloading — Apple Silicon Macs, Windows on ARM, and many newer Linux servers use `arm64`, not `amd64`. Check with `uname -m` on macOS/Linux.

## Installing Go on Windows

### Step 1: Download and Run the Installer

Download the `.msi` installer from the official downloads page and run it, accepting the default settings. Choose the `amd64` or `arm64` installer to match your machine's architecture.

<details>
<summary>Screenshot: Go downloads page</summary>

<img width="1522" height="1310" alt="image" src="https://github.com/user-attachments/assets/63464441-3824-482f-81e7-f5d6595210c2" />

</details>

<details>
<summary>Screenshot: Go setup wizard</summary>

<img width="622" height="497" alt="643209360-d65e5cff-faa7-4b46-b8ae-a9223554d466" src="https://github.com/user-attachments/assets/0bd8d39a-2437-4df4-a5b3-cf94636ffb58" />

</details>

### Step 2: Verify the PATH Variable

The installer adds Go to `PATH` automatically. This can be confirmed manually if needed.

<details>
<summary>Screenshot: Environment Variables dialog</summary>

<img width="529" height="63" alt="WhatsApp Image 2026-09-05 at 18 57 11" src="https://github.com/user-attachments/assets/dfffb69b-f517-4fa1-8010-3a9f0e13443f" />

</details>

## Installing Go on macOS

### Option 1: Official Installer

Download and run the `.pkg` installer, choosing the build that matches your chip (Intel = `amd64`, Apple Silicon = `arm64`). It installs Go to `/usr/local/go` and updates the shell path.

### Option 2: Homebrew

```bash
brew install go
```
<details>
<summary>Screenshot: Environment Variables dialog</summary>

<img width="1076" height="743" alt="Screenshot 2026-09-05 at 5 57 45 PM" src="https://github.com/user-attachments/assets/dbd8e8bc-41d5-4ea7-aee5-20951beead33" />

</details>

> **Note:** Homebrew installs Go under its own prefix (`/opt/homebrew` on Apple Silicon, `/usr/local` on Intel Macs), which differs from the official installer's `/usr/local/go`. If both are installed at different times, this can cause version conflicts — run `which go` and `go version` to confirm which one is active on `PATH`.

### Verify the PATH Variable

```bash
which go
go version
```
<details>
<summary>Screenshot: Environment Variables dialog</summary>

<img width="507" height="128" alt="Screenshot 2026-09-05 at 5 58 47 PM" src="https://github.com/user-attachments/assets/d447c93b-9c63-479c-b5cf-97826718dbd7" />

</details>

If `go` is not found, add it to your shell profile (`~/.zshrc` by default on modern macOS, or `~/.bash_profile`/`~/.bashrc` if using bash):

```bash
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.zshrc
source ~/.zshrc
```

## Installing Go on Linux

Download the latest stable release for your architecture (`amd64` or `arm64`):

```bash
GO_VERSION=$(curl -s https://go.dev/VERSION?m=text | head -n 1)
ARCH=$(dpkg --print-architecture 2>/dev/null || uname -m)
wget https://go.dev/dl/${GO_VERSION}.linux-${ARCH}.tar.gz
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf ${GO_VERSION}.linux-${ARCH}.tar.gz
```
<details>
<summary><strong>Screenshot - verified runtime value</strong></summary>

<img width="1438" height="443" alt="Screenshot 2026-09-05 at 5 17 28 PM" src="https://github.com/user-attachments/assets/e84dd056-1334-4cb0-bbe4-a6f7765f597e" />
</details>

Add Go to `PATH` permanently by appending it to your shell profile, then reload the shell:

```bash
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc
source ~/.bashrc
```
> **Note:** If your default shell is zsh (`echo $SHELL` to check), use `~/.zshrc` instead of `~/.bashrc`.

## Verifying the Installation

```bash
go version
```

<details>
<summary><strong>Screenshot - verified runtime value</strong></summary>

<img width="342" height="78" alt="Screenshot 2026-09-05 at 5 18 52 PM" src="https://github.com/user-attachments/assets/ac84f5a7-8ddd-47ae-8eb0-a719ee5afee4" />

</details>

It's also useful to confirm the environment Go is picking up, particularly when troubleshooting `PATH` or `GOROOT`/`GOPATH` issues:

```bash
go env
```
<details>
<summary><strong>Screenshot - verified runtime value</strong></summary>

<img width="1396" height="638" alt="Screenshot 2026-09-05 at 5 19 25 PM" src="https://github.com/user-attachments/assets/6f6757cb-6931-4118-b395-de9d8b3150f4" />


</details>

## Setting Up the Go Workspace

```bash
mkdir my-go-app && cd my-go-app
go mod init example.com/my-go-app
```
<details>
<summary><strong>Screenshot - verified runtime value</strong></summary>

<img width="623" height="148" alt="Screenshot 2026-09-05 at 5 20 02 PM" src="https://github.com/user-attachments/assets/2414c03e-1583-4e03-b7b4-1fe29fdad48a" />


</details>

This creates a `go.mod` file that tracks the module name and dependencies.

| File | Purpose |
|---|---|
| `go.mod` | Module name and dependency versions |
| `go.sum` | Dependency checksums |
| `main.go` | Application entry point |

## Writing and Running a First Go Program

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, Go!")
}
```

```bash
go run main.go
```
<details>
<summary><strong>Screenshot - verified runtime value</strong></summary>

<img width="623" height="148" alt="Screenshot 2026-09-05 at 5 22 34 PM" src="https://github.com/user-attachments/assets/871a164c-86d2-414a-86e9-948f66e7baf0" />

</details>

Build a standalone binary with:

```bash
go build -o my-go-app
```
<details>
<summary><strong>Screenshot - verified runtime value</strong></summary>

<img width="623" height="148" alt="Screenshot 2026-09-05 at 5 23 55 PM" src="https://github.com/user-attachments/assets/71f56b7a-54c6-435c-917b-d1891389e230" />


</details>

## Common Installation Issues

| Issue | Resolution |
|-------|------------|
| `go: command not found` | Add the Go `bin` directory to `PATH` in your shell profile (`.bashrc`/`.zshrc`) and reload the shell |
| Old version after upgrade | Remove the old Go directory before extracting the new one |
| Permission denied on install | Run with `sudo` or as administrator |
| `go.mod` errors | Re-run `go mod init` in the project directory |
| Wrong architecture binary ("exec format error") | Re-download using the correct `amd64`/`arm64` build for your machine |
| Unsure which Go is active | Run `which go` (macOS/Linux) or `where go` (Windows) and `go env GOROOT` to confirm the install location being used |

## Uninstalling Go

**Windows:** Use "Add or Remove Programs" and remove the Go entry, or re-run the `.msi` installer and choose Uninstall.

**macOS:**
```bash
sudo rm -rf /usr/local/go
# If installed via Homebrew instead:
brew uninstall go
```

**Linux:**
```bash
sudo rm -rf /usr/local/go
```
Then remove the corresponding `PATH` line from `~/.bashrc` or `~/.zshrc`.

## Best Practices

- Install the latest stable release from the official source.
- Confirm your OS and CPU architecture before downloading (`amd64` vs `arm64`).
- Remove older versions before installing a new one.
- Use Go modules (`go.mod`) instead of `GOPATH`.
- Keep the toolchain updated for security fixes.
- Run `go version` and `go env` after every install or upgrade.

## Conclusion

Go installs quickly on Windows, macOS, and Linux through official installers, package managers, or manual extraction. Running `go version` and `go mod init` afterward gives a working foundation to start building applications.

## Contact Information

| Name         | Email |
|--------------|-------|
| Sahil Butola | [sahil.butola.snaatak@mygurukulam.co](mailto:sahil.butola.snaatak@mygurukulam.co) |

## References

| Reference | Link |
|-----------|------|
| Go Downloads | https://go.dev/dl/ |
| Go Installation Docs | https://go.dev/doc/install |
| Go Modules Reference | https://go.dev/ref/mod |
| Effective Go | https://go.dev/doc/effective_go |
