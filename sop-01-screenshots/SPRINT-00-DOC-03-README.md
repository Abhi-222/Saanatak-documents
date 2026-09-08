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
5. [Prerequisites and System Requirements](#5-prerequisites-and-system-requirements)
6. [Installation and Verification on Required OS](#6-installation-and-verification-on-required-os)
7. [Troubleshooting](#7-troubleshooting)
8. [Use Cases](#8-use-cases)
9. [Best Practices](#9-best-practices)
10. [Conclusion](#10-conclusion)
11. [Contact Information](#11-contact-information)
12. [References](#12-references)

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

## 5. Prerequisites and System Requirements

| Requirement | Details |
|---|---|
| Operating System | Windows 10+, macOS 11+, or a common Linux distribution |
| Access Level | Administrator (Windows/macOS) or `sudo` access (Linux) |
| Disk Space | ~500 MB free |
| Architecture | `amd64`/`x86_64` or `arm64` — check with `uname -m` on macOS/Linux before downloading (Apple Silicon Macs, Windows on ARM, and many newer Linux servers use `arm64`) |

---

## 6. Installation and Verification on Required OS

<details>
<summary><strong>macOS</strong></summary>

### Step 1: Download and Extract

```bash
curl -LO https://go.dev/dl/go1.26.2.darwin-arm64.tar.gz
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf go1.26.2.darwin-arm64.tar.gz
```

<details>
<summary>Screenshot: Go archive downloaded and extracted</summary>
<img width="1437" height="223" alt="Screenshot 2026-09-08 at 3 12 17 AM" src="https://github.com/user-attachments/assets/792556f7-3c00-452f-92b3-fbccf942ba48" />
</details>

### Step 2: Set the PATH

```bash
export PATH=$PATH:/usr/local/go/bin
source ~/.zshrc
```

<details>
<summary>Screenshot: PATH exported and shell reloaded</summary>
<img width="1437" height="63" alt="Screenshot 2026-09-08 at 3 12 20 AM" src="https://github.com/user-attachments/assets/23e64882-5762-44e9-9da7-c2fb743158d3" />
</details>

### Step 3: Verify the Installation

```bash
go version
```

<details>
<summary>Screenshot: go version output</summary>
<img width="1437" height="44" alt="Screenshot 2026-09-08 at 3 12 44 AM" src="https://github.com/user-attachments/assets/ab32f7f2-4a44-48d2-8d21-135cc98dde84" />
</details>
</details>


<details>
<summary><strong>Linux</strong></summary>

### Step 1: Download and Extract

```bash
wget https://go.dev/dl/go1.23.0.darwin-amd64.tar.gz
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf go1.23.0.darwin-amd64.tar.gz
```
<details>
<summary>Screenshot: Go archive downloaded and extracted</summary>
<img width="1431" height="353" alt="Screenshot 2026-09-08 at 1 36 22 AM" src="https://github.com/user-attachments/assets/4c934e28-c02e-4156-a08e-4c91708efa4f" />
</details>

### Step 2: Set the PATH

```bash
export PATH=$PATH:/usr/local/go/bin
source ~/.zshrc
```
<details>
<summary>Screenshot: PATH exported and shell reloaded</summary>
<img width="545" height="69" alt="Screenshot 2026-09-08 at 1 37 14 AM" src="https://github.com/user-attachments/assets/a7f15525-2355-4834-8518-4943b8636e7e" />
</details>

### Step 3: Verify the Installation

```bash
go version
```
<details>
<summary>Screenshot: go version output</summary>
<img width="545" height="69" alt="Screenshot 2026-09-08 at 1 37 40 AM" src="https://github.com/user-attachments/assets/d5dded88-c516-440e-b810-e77bd6a87589" />
</details>
</details>



<details>
<summary><strong>Windows</strong></summary>

### Step 1: Official Download Page
<details>
<summary>Screenshot: Go archive downloaded and extracted</summary>
<img width="1600" height="676" alt="WhatsApp Image 2026-09-08 at 11 38 52" src="https://github.com/user-attachments/assets/7d752d9b-85f8-4384-8370-be85d0f4afdd" />
</details>

### Step 2: Go to Msi Installer
<details>
<summary>Screenshot: PATH exported and shell reloaded</summary>
<img width="1600" height="730" alt="WhatsApp Image 2026-09-08 at 11 38 52 (1)" src="https://github.com/user-attachments/assets/9c57e380-34ef-4d7c-a2b9-45ba7eb99b5b" />
</details>

### Step 3: Installation
<details>
<summary>Screenshot: PATH exported and shell reloaded</summary>
<img width="885" height="649" alt="WhatsApp Image 2026-09-08 at 11 38 52 (2)" src="https://github.com/user-attachments/assets/e1c30ac6-bbcb-4e8f-9f50-cb4f9f6639f4" />
</details>


### Step 4: Verify the Installation

```bash
go version
```

<details>
<summary>Screenshot: go version output</summary>
<img width="844" height="274" alt="WhatsApp Image 2026-09-08 at 11 38 52 (3)" src="https://github.com/user-attachments/assets/835c39b7-ebe3-43f1-b321-b3a618d9ee33" />
</details>
</details>



---

## 7. Troubleshooting

| Issue | Resolution |
|-------|------------|
| `go: command not found` | Add the Go `bin` directory to `PATH` in your shell profile (`.bashrc`/`.zshrc`) and reload the shell |
| Old version after upgrade | Remove the old Go directory before extracting the new one |
| Permission denied on install | Run with `sudo` or as administrator |
| `go.mod` errors | Re-run `go mod init` in the project directory |
| Wrong architecture binary ("exec format error") | Re-download using the correct `amd64`/`arm64` build for your machine |
| Unsure which Go is active | Run `which go` (macOS/Linux) or `where go` (Windows) and `go env GOROOT` to confirm the install location being used |

---

## 8. Use Cases

- **Backend Web Services** – Building fast, scalable APIs and microservices (used by companies like Uber and Twitch).
- **Cloud & DevOps Tooling** – Powers major infrastructure tools like Docker, Kubernetes, and Terraform.
- **Command-Line Tools** – Its static binaries make it easy to distribute standalone CLI utilities with no dependencies.
- **Networked Applications** – Well-suited for servers, proxies, and systems that handle many simultaneous connections (thanks to goroutines).
- **Distributed Systems** – Used for building systems that need to coordinate across multiple machines, like message queues and distributed databases.

---

## 9. Best Practices

- **Download from the official source** – Always get Go from the official website to avoid outdated or unsafe versions.
- **Match your system type** – Check whether your computer uses amd64 or arm64 before downloading, so you install the right version.
- **Remove old versions first** – Uninstall any previous Go version before installing a new one to avoid conflicts.
- **Use Go modules** – Manage your project dependencies with go.mod instead of the older GOPATH method.
- **Verify after installing** – Run go version to confirm Go is installed correctly and ready to use.

---

## 10. Conclusion

Go can be installed easily on Windows, macOS, or Linux using official installers, package managers, or manual setup — whichever suits your system best. Once installed, running a couple of quick checks confirms everything is working correctly, leaving you with a solid foundation ready to start building Go applications.

---

## 11. Contact Information

| Name         | Email |
|--------------|-------|
| Sahil Butola | [sahil.butola.snaatak@mygurukulam.co](mailto:sahil.butola.snaatak@mygurukulam.co) |

---

## 12. References

| Reference | Link |
|-----------|------|
| Go Downloads | https://go.dev/dl/ |
| Go Installation Docs | https://go.dev/doc/install |
| Go Modules Reference | https://go.dev/ref/mod |
| Effective Go | https://go.dev/doc/effective_go |
