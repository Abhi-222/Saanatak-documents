<img width="731" height="273" alt="image" src="https://github.com/user-attachments/assets/e2b57a22-3f39-493f-81d2-04183bfc597e" />

# GoLang Code Compilation POC

---

## Author Table

| **Author** | **Created on** | **Version** | **Last edited on** | **L0 Reviewer**   | **L1 Reviewer** | **L2 Reviewer** |
| ---------- | -------------- | ----------- | ------------------ | ----------------- | --------------- | --------------- |
| Sahil      | 30-09-26       | v1.0        | 01-09-26           | Divya M. / Vishal | Aayush Verma    | Mahesh / Varun  |

---

## 1. Objective

The objective of this POC is to verify that the `employee-api` Go project can be successfully compiled using the Official Go Toolchain.

This POC validates:

* Go environment availability.
* Repository setup.
* Dependency preparation.
* Successful Go package compilation.
* Generated executable binary verification.

---

## 2. Prerequisites

The following prerequisites are required:

* Ubuntu 26.04 LTS system.
* Git installed.
* Go already installed and configured.
* Go version 1.26.0.
* Access to the `employee-api` repository.
* Internet access for downloading Go dependencies.
* Sufficient disk space for dependencies and the generated binary.

---

## 3. Environment

| Component        | Details                                |
| ---------------- | -------------------------------------- |
| Operating System | Ubuntu 26.04 LTS                       |
| Codename         | Resolute Raccoon                       |
| Architecture     | Linux amd64                            |
| Go Version       | Go 1.26.0                              |
| Project          | `employee-api`                         |

### Environment Verification

```bash
go version
cat /etc/os-release
```

<details>
<summary> Screenshot: Go version and OS verification</summary>

<br>

<img width="806" height="272" alt="Go version and OS verification" src="https://github.com/user-attachments/assets/5595e305-38d1-439b-a6ee-2452b46235f9" />

</details>

---

## 4. Repository Setup

Clone the `employee-api` repository and navigate to the project directory:

```bash
git clone https://github.com/OT-MICROSERVICES/employee-api.git
cd employee-api
```

Verify the project files:

```bash
ls
```

<details>
<summary> Screenshot: Repository clone and project files</summary>

<br>

<img width="989" height="73" alt="Repository clone and project files" src="https://github.com/user-attachments/assets/e7f2d9fd-c5ac-4b6e-9899-8cb274614f57" />

</details>

### Project Structure

<details>
<summary> Screenshot: Project structure</summary>

<br>

<img width="989" height="712" alt="Project structure" src="https://github.com/user-attachments/assets/c3bd44c6-4c82-4baa-b088-575894532d3b" />

</details>

---

## 5. Download Go Dependencies

```bash
go mod download
```

<details>
<summary> Screenshot: Successful go mod download execution</summary>

<br>

<img width="989" height="45" alt="Successful go mod download execution" src="https://github.com/user-attachments/assets/27dc533d-2ff1-45fa-baa5-bbc2dbc38838" />

</details>

---

## 6. Go Code Compilation

Run the compilation check:

```bash
go build ./...
```

<details>
<summary> Screenshot: Successful go build ./... execution</summary>

<br>

<img width="989" height="45" alt="Successful go build ./... execution" src="https://github.com/user-attachments/assets/1e6561bc-abd3-4bae-92cf-9ea536a333f1" />

</details>

---

## 7. Binary Generation and Verification

### Generate the Executable Binary

```bash
go build -o employee-api
```

<details>
<summary> Screenshot: Successful go build -o employee-api execution</summary>

<br>

<img width="989" height="108" alt="Successful go build -o employee-api execution" src="https://github.com/user-attachments/assets/1cace86b-c0b9-4088-8d4b-1af44081f53d" />

</details>

### Verify the Executable Binary

```bash
ls -lh employee-api
```

<details>
<summary> Screenshot: employee-api binary verification</summary>

<br>

<img width="989" height="66" alt="employee-api binary verification" src="https://github.com/user-attachments/assets/ada28660-613a-45d1-b8a5-36b384810d5e" />

</details>

---

## 8. POC Workflow

<img width="565" height="1121" alt="POC Workflow" src="https://github.com/user-attachments/assets/d2821b6e-8c8e-4471-9e18-70d42259c400" />

---

## 9. POC Summary

| Step                     | Command                              | Result   |
| ------------------------ | ------------------------------------ | -------- |
| Environment Verification | `go version` / `cat /etc/os-release` | Verified |
| Repository Setup         | `git clone`                          | Success  |
| Dependency Download      | `go mod download`                    | Success  |
| Compilation Check        | `go build ./...`                     | Success  |
| Binary Creation          | `go build -o employee-api`           | Success  |
| Binary Verification      | `ls -lh employee-api`                | 35 MB    |

---

## 10. Final Result

The `employee-api` Go project was successfully compiled using the Official Go Toolchain.

The POC verified:

* Go environment availability.
* Successful repository setup.
* Successful dependency preparation.
* Successful Go package compilation.
* Successful executable binary generation and verification.

---

## 11. Conclusion

The POC demonstrates that the `employee-api` project can be successfully compiled using the Go Toolchain. The `go build ./...` compilation check completed without errors, `go build -o employee-api` generated the executable, and the resulting `employee-api` binary was successfully verified.

---

## 12. Contact Information

| Author | Email                                                                             |
| ------ | --------------------------------------------------------------------------------- |
| Sahil  | [sahil.butola.snaatak@mygurukulam.co](mailto:sahil.butola.snaatak@mygurukulam.co) |

---

## 13. References

| Reference                                                         | Description              |
| ----------------------------------------------------------------- | ------------------------ |
| [Go Documentation](https://go.dev/doc/)                           | Go Documentation         |
| [Go Command Documentation](https://pkg.go.dev/cmd/go)             | Go Command Documentation |
| [Go Modules](https://go.dev/ref/mod)                              | Go Modules               |
||[employee-repository](https://github.com/OT-MICROSERVICES/employee-api)| employee-repository |
