# Common Stack | Applications | Golang | Installation Guide
---

## Author Table

| **Author**   | **Created On** | **Version** | **Last Edited On**    | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
|--------------|----------------|-------------|-----------------------|-----------------|-----------------|------------------|
| Sahil Butola | 30-08-2026     | 1.0         | 05-09-2026            | Divya M.        | Aayush Verma    | Mahesh Kumar |

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [What Is Go?](#2-what-is-go)
3. [Why Go is Required](#3-why-go-is-required)
4. [Key Features](#4-key-features)
5. [Core Components](#5-core-components)
6. [Prerequisites and System Requirements](#6-prerequisites-and-system-requirements)
7. [Installation and Verification on Required OS](#7-installation-and-verification-on-required-os)
8. [Troubleshooting](#8-troubleshooting)
9. [Use Cases](#9-use-cases)
10. [Best Practices](#10-best-practices)
11. [Conclusion](#11-conclusion)
12. [Contact Information](#12-contact-information)
13. [References](#13-references)

---

## 1. Introduction

Go, also known as Golang, is a free and open-source programming language created by Google. It's popular for building backend systems, command-line tools, and applications that communicate over networks. This guide shows you how to install Go on Windows, macOS, and Linux, and how to check that it's installed correctly.

---

## 2. What Is Go?

Go is a programming language where code is checked for errors before it runs (statically typed) and then converted into a program your computer can run directly (compiled). When you install Go, you get everything you need — the compiler, a set of ready-to-use tools, and a library of pre-built code — all accessible through a single command called `go`.

---

## 3. Why Go is Required

- **Fast compilation** – Code turns into a ready-to-run program in seconds.
- **Built-in concurrency** – Handling multiple tasks at once is easy with goroutines.
- **Static binaries** – Produces one self-contained file — no extra installs needed to run it.
- **Cross-platform** – You can build a version for another OS without leaving your machine.

---

## 4. Key Features

- **Single Self-Contained Package** – Installing Go gives you the compiler, standard library, and CLI tools all in one download — no separate installs needed.
- **Cross-Platform Support** – Official installers and binaries are available for Windows, macOS, and Linux, so the setup process feels consistent across systems.
- **Simple PATH Setup** – Most installers (MSI, PKG, Homebrew) automatically configure your system `PATH`, so `go` is ready to use right after installation.
- **Built-In Version Checking** – Running `go version` instantly confirms what's installed, making it easy to verify or troubleshoot after setup.
- **Clean Uninstall/Upgrade** – Since Go installs to a single directory (`/usr/local/go` or similar), removing or upgrading it is as simple as deleting that folder and reinstalling.

---

## 5. Core Components

| Component | Purpose |
|---|---|
| `go` | CLI tool to build, run, and manage Go code |
| Go compiler | Compiles source code into a native binary |
| `GOROOT` | Directory where Go itself is installed |
| Go modules | Dependency management for Go projects |

---

## 6. Prerequisites and System Requirements

| Requirement | Details |
|---|---|
| Operating System | Windows 10+, macOS 11+, or a common Linux distribution |
| Access Level | Administrator (Windows/macOS) or `sudo` access (Linux) |
| Disk Space | ~500 MB free |
| Architecture | `amd64`/`x86_64` or `arm64` — check with `uname -m` on macOS/Linux before downloading (Apple Silicon Macs, Windows on ARM, and many newer Linux servers use `arm64`) |

---

## 7. Installation and Verification on Required OS

<details>
<summary><strong>Linux</strong></summary>

Your content goes here — text, code blocks, images, anything.

</details>

<details>
<summary><strong>Macos</strong></summary>

Your content goes here — text, code blocks, images, anything.

</details>

<details>
<summary><strong>Windows</strong></summary>

Your content goes here — text, code blocks, images, anything.

</details>

---

## 8. Troubleshooting

| Issue | Resolution |
|-------|------------|
| `go: command not found` | Add the Go `bin` directory to `PATH` in your shell profile (`.bashrc`/`.zshrc`) and reload the shell |
| Old version after upgrade | Remove the old Go directory before extracting the new one |
| Permission denied on install | Run with `sudo` or as administrator |
| `go.mod` errors | Re-run `go mod init` in the project directory |
| Wrong architecture binary ("exec format error") | Re-download using the correct `amd64`/`arm64` build for your machine |
| Unsure which Go is active | Run `which go` (macOS/Linux) or `where go` (Windows) and `go env GOROOT` to confirm the install location being used |

---

## 9. Use Cases

- **Backend Web Services** – Building fast, scalable APIs and microservices (used by companies like Uber and Twitch).
- **Cloud & DevOps Tooling** – Powers major infrastructure tools like Docker, Kubernetes, and Terraform.
- **Command-Line Tools** – Its static binaries make it easy to distribute standalone CLI utilities with no dependencies.
- **Networked Applications** – Well-suited for servers, proxies, and systems that handle many simultaneous connections (thanks to goroutines).
- **Distributed Systems** – Used for building systems that need to coordinate across multiple machines, like message queues and distributed databases.

---

## 10. Best Practices

- **Download from the official source** – Always get Go from the official website to avoid outdated or unsafe versions.
- **Match your system type** – Check whether your computer uses amd64 or arm64 before downloading, so you install the right version.
- **Remove old versions first** – Uninstall any previous Go version before installing a new one to avoid conflicts.
- **Use Go modules** – Manage your project dependencies with go.mod instead of the older GOPATH method.
- **Verify after installing** – Run go version to confirm Go is installed correctly and ready to use.

---

## 11. Conclusion

Go can be installed easily on Windows, macOS, or Linux using official installers, package managers, or manual setup — whichever suits your system best. Once installed, running a couple of quick checks confirms everything is working correctly, leaving you with a solid foundation ready to start building Go applications.

---

## 12. Contact Information

| Name         | Email |
|--------------|-------|
| Sahil Butola | [sahil.butola.snaatak@mygurukulam.co](mailto:sahil.butola.snaatak@mygurukulam.co) |

---

## 13. References

| Reference | Link |
|-----------|------|
| Go Downloads | https://go.dev/dl/ |
| Go Installation Docs | https://go.dev/doc/install |
| Go Modules Reference | https://go.dev/ref/mod |
| Effective Go | https://go.dev/doc/effective_go |
