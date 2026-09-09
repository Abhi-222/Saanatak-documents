# Common Stack | Applications | Python | SOP's for requirements.txt

---

# Author Table

| **Author** | **Created On** | **Version** | **Last Updated **  | **L0 Reviewer**  | **L1 Reviewer** | **L2 Reviewer**        |
| ---------- | -------------- | ----------- | -------------------| ----------------| -----------------| ---------------------- |
| Sahil      | 27-08-26       | 1.1         | 03-09-26           | `Vishal/Divya M`| `Aayush Verma`   | `Mahesh Kumar / Varun` |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What is requirements.txt](#2-what-is-requirementstxt)
3. [Why requirements.txt is Required](#3-why-requirementstxt-is-required)
4. [Key Features of requirements.txt](#4-key-features-of-requirementstxt)
5. [Prerequisites](#5-prerequisites)
   - [5.1 Access & Permissions](#51-access--permissions)
   - [5.2 System Requirements](#52-system-requirements)
6. [Install Dependencies from requirements.txt](#6-install-dependencies-from-requirementstxt)
7. [Generate requirements.txt](#7-generate-requirementstxt)
8. [Quick Commands](#8-quick-commands)
9. [Validation](#9-validation)
10. [Troubleshooting](#10-troubleshooting)
11. [Use Cases](#11-use-cases)
12. [Best Practices](#12-best-practices)
13. [Conclusion](#13-conclusion)
14. [Contact Information](#14-contact-information)
15. [References](#15-references)

---

# 1. Introduction

This SOP details how to install, generate, and troubleshoot Python dependencies using  requirements.txt on any Operating systems.

It applies to all Python developers ensuring environment consistency across Operating systems.It covers:
-Installing existing project dependencies
-Generating updated requirements.txt files
-Troublesshooting environment and version conflicts.

---

# 2. What is requirements.txt?

A requirement.txt file is a list of all external packages your Python project needs to run.
Instead of forcing you to install each package manually one by one, it allows you to automatically download and configure the entire environment all at once.

---

# 3. Why requirements.txt is Required?

Without a requirement.txt file, team members and servers have to guess which packages to install. This file is required to solve three main problems:

- **Consistency** — It ensures everyone on the team uses the exact same software versions, preventing the classic "it works on my machine" problem.
- **Automation** — It allows cloud servers, deployment pipelines, and setup scripts to configure the project automatically without human intervention.
- **Easy Onboarding** — New developers can set up their local environment and start working immediately instead of manually installing missing packages one by one.

---

# 4. Key Features of requirements.txt

- **Exact Matches (==)** Installs one specific version of a tool (e.g., Flask==3.0.0) so nothing unexpectedly updates or breaks. 
- **Minimum Versions (>=)** — Tells Python it needs at least a certain version or newer (e.g., requests>=2.31.0)
- **Clean Comments(#)** — Allows you to add notes in plain text to explain why a package is needed, which Python will completely ignore when installing.
                                   |
---

# 5. Prerequisites % System Requirements

| **Requirement**     | **Specification**                                                |
| --------------------| ---------------------------------------------------------------- |
| OS                  | Any major operating system (Windows, macOS, or Linux). |
| RAM                 | 512 MB or higher (enough to run standard Python scripts) |
| Disk Space          | Minimal (varies depending on the size of the packages you install)|
| Required Packages   | Python 3 (includes pip and venv for environment management) |

---

# 6. Install Dependencies from requirements.txt

## Step 6.1: Run the Installation Script
Navigate to your project folder containing the file and run the following command to deploy the dependencies:

```bash
pip install -r requirements.txt
```

<details>
<summary><strong>Screenshot - Virtual environment activated</strong></summary>

</details>

---

## Step 6.2: Verify the Installation

```bash
pip list
```

<details>
<summary><strong>Screenshot - Dependencies installed successfully</strong></summary>
</details>

---

# 7. How to Generate a requirements.txt File

## Step 7.1: Install the genrator tool

```bash
pip install pipreqs
pipreqs . --force
```

<details>
<summary><strong>Screenshot - pipreqs output</strong></summary>
</details>


## Step 7.2: Inspect the Content

```bash
cat requirements.txt    [linux/macos]
type requirements.txt   [Windows]
```

<details>
<summary><strong>Screenshot - pipreqs output</strong></summary>
</details>


---

# 8. Quick Commands

| Task                                              | Command                                     |
| ---------------------------------------------------- | ---------------------------------------------- |
| Install dependencies                              | `pip install -r requirements.txt`           |
| Generate from imports only                        | `pipreqs . --force`                         |
| List installed packages                           | `pip list`                                  |

---

# 10. Troubleshooting

| **Issue**                                        | **Cause**                                                           | **Solution**                                                 |
| --------------------------------------------------- | ----------------------------------------------------------------------- | ----------------------------------------------------------------- |
| `error: externally-managed-environment`            | Some Linux distributions block pip installs outside a venv (PEP 668)   | Activate a virtual environment before installing                  |

---

# 11. Use Cases

-**Setting Up a Cloned Project**: Run the installation script immediately after downloading a project to ensure it works on your computer without missing library errors.
-**Sharing Code with Others**: Run the generation script before sending your project to a teammate so they know exactly what packages to download.
-**Fixing Version Errors**: Refer to this file whenever you run into "ModuleNotFoundError" bugs to confirm which tool version your code expects.
-**Updating Project Dependencies**: Run the pipreqs generation workflow when introducing new architectural components or open-source libraries into the codebase
-**Security Auditing & Compliance**: Use the verified requirements list as an inventory tracker to scan for known security vulnerabilities or deprecated versions.

---

# 12. Best Practices

-**Pin Exact Versions**: Always use the == operator for production releases (e.g., requests==2.31.0) to avoid unexpected updates that might break your code.
-**Keep Comments Clean**: Use the # symbol to document why a non-standard or unusual package is required, ensuring team clarity.
-**Update the File Frequently**: Run the generation workflow immediately after importing a new package so your team never encounters missing module errors.
-**Avoid Global Clutter**: Never use pip freeze on global system environments to prevent bloating the configuration with local, unrelated tools.
-**Track via Version Control**: Always commit requirements.txt to Git so that every dependency shift can be tracked, reviewed, and rolled back if necessary.Separate

---

# 13. Conclusion

This SOP provides a clear, standardized workflow for installing, generating, and verifying Python dependencies using requirements.txt on any operating system.
Following these steps ensures that our development environments remain consistent, stable, and error-free. Whether you are working locally or deploying to production, keeping this file updated guarantees that the code runs perfectly on every machine.

---

# 14. Contact Information

| **Name** | **Email**                              |
| ---------- | ------------------------------------------ |
| Sahil      | [sahil.butola.snaatak@mygurukulam.co](mailto:sahil.butola.snaatak@mygurukulam.co)|

---

# 15. References

| **Topic**                                                                                     | **Description**                          |
| -------------------------------------------------------------------------------------------------- | ---------------------------------------------- |
| [pip Documentation](https://pip.pypa.io/en/stable/)                                                | Official pip documentation                     |
| [requirements.txt Format](https://pip.pypa.io/en/stable/reference/requirements-file-format/)       | requirements.txt file format reference         |
| [pipreqs](https://pypi.org/project/pipreqs/)                                                       | pipreqs documentation                          |
