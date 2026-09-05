# Common Stack | Applications | Python | SOP's for requirements.txt
---

# Author Table

| **Author** | **Created On** | **Version** | **Last Updated By** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer**        |
| ---------- | -------------- | ----------- | ------------------- | ------------------ | --------------- | --------------- | ---------------------- |
| Sahil      | 27-08-26       | 1.0         | Sahil               | 27-08-26           | `Divya M`      | `Aayush Verma`  | `Mahesh Kumar / Varun` |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [Purpose](#2-purpose)
3. [What is requirements.txt](#3-what-is-requirementstxt)
4. [Why requirements.txt is Required](#4-why-requirementstxt-is-required)
5. [Key Features of requirements.txt](#5-key-features-of-requirementstxt)
6. [Prerequisites](#6-prerequisites)
   - [6.1 Access & Permissions](#61-access--permissions)
   - [6.2 System Requirements](#62-system-requirements)
7. [Install Dependencies from requirements.txt](#7-install-dependencies-from-requirementstxt)
8. [Generate requirements.txt](#8-generate-requirementstxt)
9. [Validation](#9-validation)
10. [Use Cases](#10-use-cases)
11. [Troubleshooting](#11-troubleshooting)
12. [Best Practices](#12-best-practices)
13. [Conclusion](#13-conclusion)
14. [Contact Information](#14-contact-information)
15. [References](#15-references)

---

# 1. Introduction

This SOP provides a structured guide to **installing dependencies from `requirements.txt`**, **generating a `requirements.txt` file**, and **troubleshooting dependency issues** in Python projects on **Ubuntu 24.04**.

It covers the required setup, installation steps, dependency file generation, verification, validation, and troubleshooting needed to keep Python environments consistent and reproducible.

---

# 2. Purpose

The purpose of this SOP is to provide a standardized procedure for:

- Installing all project dependencies listed in `requirements.txt`
- Generating a `requirements.txt` file from an existing environment or from project imports only
- Verifying installed packages match the declared dependency list
- Resolving common dependency installation failures, including Ubuntu 24's externally-managed-environment restriction

These procedures help maintain **environment consistency, reproducible builds, and operational reliability** across development, CI, and production systems.

---

# 3. What is requirements.txt

`requirements.txt` is a plain-text file used in Python projects to declare the external packages a project depends on, along with their versions. Each line typically specifies a package name and an optional version constraint (e.g., `Flask==2.3.0`), and the file is read by `pip` to install those exact packages into an environment.

Rather than installing each dependency one by one, a single command — `pip install -r requirements.txt` — installs every package listed in the file, letting a developer, CI pipeline, or production server recreate the same set of dependencies used elsewhere.

---

# 4. Why requirements.txt is Required

- **Reproducibility** — every environment (developer machine, CI server, production) installs the exact same package versions, avoiding "works on my machine" issues.
- **Faster onboarding** — new contributors can set up a working environment with a single install command instead of hunting down dependencies manually.
- **Auditability** — since the file is plain text, it can be committed to version control, diffed, and reviewed alongside code changes.
- **Safer upgrades** — version changes are explicit and reviewable rather than silent, making it easier to roll back if an upgrade breaks something.
- **CI/CD reliability** — build and deployment pipelines can install a known, consistent dependency set on every run.

---

# 5. Key Features of requirements.txt

| **Feature**                     | **Description**                                                                 |
| ---------------------------------- | ------------------------------------------------------------------------------------ |
| Version pinning                    | Exact (`==`) or range-based version constraints per package                          |
| Reproducibility                    | Same file installs an identical set of packages on any machine                       |
| Two generation modes               | `pip freeze` (full snapshot) or `pipreqs` (imports-only)                             |
| Conflict detection                 | `pip check` flags incompatible dependency versions before they cause failures        |
| Environment-isolation friendly     | Designed to be installed inside a virtual environment, not the system Python         |
| Plain text & VCS-friendly          | Easy to diff, review, and track changes to in Git                                    |

---

# 6. Prerequisites

### 6.1 Access & Permissions

| **Prerequisite** | **Details**                                                                  |
| ----------------- | ------------------------------------------------------------------------------ |
| Terminal Access   | SSH/terminal access to the target machine                                      |
| Privileges        | `sudo` access to install system packages (`python3-venv`, `python3-pip`)       |
| Project Access    | Access to the project directory containing (or requiring) `requirements.txt`   |

### 6.2 System Requirements

| **Requirement**    | **Details**                                                   |
| -------------------- | ---------------------------------------------------------------- |
| OS & Access         | Ubuntu 24.04+ with SSH/terminal access                          |
| Required Packages   | `python3`, `python3-venv`, `python3-pip`                        |
| Permissions         | `sudo` access where required                                    |
| Configuration       | An active Python virtual environment (required on Ubuntu 24)    |

---

# 7. Install Dependencies from requirements.txt

## Step 7.1: Create and activate a virtual environment

```bash
python3 -m venv venv && source venv/bin/activate
```

<details>
<summary><strong>Screenshot - Virtual environment activated</strong></summary>

<img width="830" height="62" alt="Screenshot 2026-09-04 at 11 50 53 PM" src="https://github.com/user-attachments/assets/1827d3c2-cab1-4771-abd5-d8a6bf833ab1" />

</details>

---

## Step 7.2: Install all listed packages

```bash
pip install -r requirements.txt
```

<details>
<summary><strong>Screenshot - Dependencies installed successfully</strong></summary>

<img width="1401" height="690" alt="Screenshot 2026-09-04 at 11 51 25 PM" src="https://github.com/user-attachments/assets/67764302-465e-4dc4-9448-59563f3bd552" />

</details>

---

## Step 7.3: Install without cached packages

```bash
pip install -r requirements.txt --no-cache-dir
```

<details>
<summary><strong>Screenshot - Dependencies installed successfully</strong></summary>

<img width="1401" height="234" alt="Screenshot 2026-09-04 at 11 52 03 PM" src="https://github.com/user-attachments/assets/833fbf72-dfa3-4a2f-a2fa-d6f2b7aabccc" />

</details>

---

## Step 7.4: Upgrade existing packages to match requirements.txt

```bash
pip install -r requirements.txt --upgrade
```

<details>
<summary><strong>Screenshot - Packages upgraded</strong></summary>

<img width="1401" height="224" alt="Screenshot 2026-09-04 at 11 52 45 PM" src="https://github.com/user-attachments/assets/4cc61e46-d355-4729-adc0-d8737eab674f" />

</details>

---

# 8. Generate requirements.txt

## Step 8.1: Generate a full environment snapshot

```bash
pip freeze > requirements.txt
```

<details>
<summary><strong>Screenshot - pip freeze output</strong></summary>

<img width="772" height="275" alt="Screenshot 2026-09-04 at 11 54 49 PM" src="https://github.com/user-attachments/assets/efcc14bd-94d2-4f5b-ae1c-18e04655a7cd" />

</details>

### Verification

```bash
cat requirements.txt
```

<details>
<summary><strong>Screenshot - pip freeze output</strong></summary>

<img width="772" height="234" alt="Screenshot 2026-09-04 at 11 55 00 PM" src="https://github.com/user-attachments/assets/40a5e98c-d878-439a-8673-180528430b54" />

</details>

---

## Step 8.2: Generate based only on imported packages

```bash
pip install pipreqs
pipreqs . --force
```

<details>
<summary><strong>Screenshot - pipreqs output</strong></summary>

<img width="1416" height="525" alt="Screenshot 2026-09-04 at 11 57 38 PM" src="https://github.com/user-attachments/assets/54064709-2eac-4629-a8b3-dc7cdda9ee73" />

</details>

---

# 9. Validation

### Validate Installed Packages

```bash
pip list
```

<details>
<summary><strong>Screenshot - validation output</strong></summary>

<img width="480" height="333" alt="Screenshot 2026-09-04 at 11 58 15 PM" src="https://github.com/user-attachments/assets/198da34e-ac6c-435a-9ea2-7ee569dfb6fe" />

</details>

### Validate Against requirements.txt

```bash
pip freeze | diff requirements.txt -
```

<details>
<summary><strong>Screenshot - validation output</strong></summary>

<img width="794" height="333" alt="Screenshot 2026-09-04 at 11 58 39 PM" src="https://github.com/user-attachments/assets/debfa458-648c-4496-8f16-14b05fc905e4" />

</details>

### Validate No Dependency Conflicts

```bash
pip check
```

<details>
<summary><strong>Screenshot - validation output</strong></summary>

<img width="612" height="115" alt="Screenshot 2026-09-04 at 11 58 58 PM" src="https://github.com/user-attachments/assets/e40e689b-c465-499a-a38d-83572c663450" />

</details>

### Final Checklist

| **Step** | **Command**                                          | **Purpose**                                                |
| -------- | ----------------------------------------------------- | ------------------------------------------------------------ |
| 7.1      | `python3 -m venv venv && source venv/bin/activate`    | Create and activate an isolated environment                  |
| 7.2      | `pip install -r requirements.txt`                     | Install all listed dependencies                               |
| 7.3      | `pip install -r requirements.txt --no-cache-dir`      | Force a fresh download, bypassing pip's cache                 |
| 7.4      | `pip install -r requirements.txt --upgrade`           | Upgrade installed packages to match requirements.txt           |
| 8.1      | `pip freeze > requirements.txt`                       | Snapshot every installed package and version                   |
| 8.2      | `pipreqs . --force`                                   | Generate a file based only on packages actually imported       |
| 9.1      | `pip list`                                            | List all packages installed in the venv                        |
| 9.2      | `pip freeze \| diff requirements.txt -`               | No diff output                                                  |
| 9.3      | `pip check`                                           | No broken dependency messages                                  |

---

# 10. Use Cases

| **Scenario**                                    | **Commands / Actions**                  |
| ------------------------------------------------- | ------------------------------------------ |
| Setting up a new dev environment                 | `pip install -r requirements.txt`         |
| Capturing current environment state              | `pip freeze > requirements.txt`           |
| Generating file from actual imports only         | `pipreqs . --force`                       |
| Checking for dependency conflicts                | `pip check`                               |
| Confirming environment matches requirements.txt  | `pip freeze \| diff requirements.txt -`   |

---

# 11. Troubleshooting

| **Issue**                                        | **Cause**                                                           | **Solution**                                                 |
| --------------------------------------------------- | ----------------------------------------------------------------------- | ----------------------------------------------------------------- |
| `error: externally-managed-environment`            | Ubuntu 24 blocks pip installs outside a venv (PEP 668)                  | Activate a virtual environment before installing                  |
| `Could not find a version that satisfies...`       | Package/version does not exist or is unavailable                        | Verify package name/version on PyPI                                |
| Dependency version conflict                        | Two packages require incompatible versions of a shared dependency        | Run `pip check`; adjust version pins in `requirements.txt`         |
| Installed packages don't match requirements.txt    | Wrong virtual environment active, or file not regenerated               | Activate the correct venv; regenerate with `pip freeze`            |
| `pip: command not found`                           | pip not installed or not on PATH                                        | Install pip or use `python3 -m pip` instead                        |
| Slow or failed downloads                           | Network/proxy issues or corrupted cache                                 | Retry with `--no-cache-dir`; check network/proxy settings          |

> [!NOTE]
> Ubuntu 24.04 enforces [PEP 668](https://peps.python.org/pep-0668/), which marks the system Python as "externally managed" and blocks `pip install` from running outside a virtual environment (`error: externally-managed-environment`). `pip install --break-system-packages` bypasses this protection but is not recommended outside throwaway or test systems.

---

# 12. Best Practices

| **Best Practice**                          | **Description**                                                                                    |
| --------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| Always use a virtual environment             | Avoids Ubuntu 24's externally-managed-environment restriction and prevents dependency conflicts       |
| Pin exact versions for production            | Use `==` in `requirements.txt` to ensure reproducible builds                                          |
| Regenerate requirements.txt deliberately     | Use `pipreqs` for imports-only files rather than full `pip freeze` dumps where possible               |
| Run `pip check` after installs               | Catches broken or conflicting dependencies early                                                       |
| Keep requirements.txt in version control     | Provides an auditable, shared source of truth for the team                                             |

---

# 13. Conclusion

This SOP provides a standardized approach to installing, generating, and troubleshooting Python dependencies using `requirements.txt` on Ubuntu 24.04.

Following these procedures helps developers maintain **reliability, reproducibility, and operational stability**, while providing a consistent, evidence-backed approach to configuration, validation, and troubleshooting — including Ubuntu 24's PEP 668 environment restrictions.

---

# 14. Contact Information

| **Name** | **Email**                              |
| ---------- | ------------------------------------------ |
| Sahil      | `sahil.butola.snaatak@mygurukulam.co`      |

---

# 15. References

| **Topic**                                                                                     | **Description**                          |
| -------------------------------------------------------------------------------------------------- | ---------------------------------------------- |
| [pip Documentation](https://pip.pypa.io/en/stable/)                                                | Official pip documentation                     |
| [requirements.txt Format](https://pip.pypa.io/en/stable/reference/requirements-file-format/)       | requirements.txt file format reference         |
| [pipreqs](https://pypi.org/project/pipreqs/)                                                       | pipreqs documentation                          |
| [PEP 668](https://peps.python.org/pep-0668/)                                                       | Externally managed Python environments         |
