# Common Stack | Applications | Python | SOP's for requirements.txt

---

# Author Table

| **Author** | **Created On** | **Version** | **Last Updated** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Sahil | 27-08-26 | 1.1 | 03-09-26 | `Vishal/Divya M` | `Aayush Verma` | `Mahesh Kumar / Varun` |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What is requirements.txt](#2-what-is-requirementstxt)
3. [Why requirements.txt is Required](#3-why-requirementstxt-is-required)
4. [Key Features of requirements.txt](#4-key-features-of-requirementstxt)
5. [Prerequisites & System Requirements](#5-prerequisites--system-requirements)
6. [Install Dependencies from requirements.txt](#6-install-dependencies-from-requirementstxt)
7. [How to Generate a requirements.txt File](#7-how-to-generate-a-requirementstxt-file)
8. [Quick Commands](#8-quick-commands)
9. [Troubleshooting](#9-troubleshooting)
10. [Use Cases](#10-use-cases)
11. [Best Practices](#11-best-practices)
12. [Conclusion](#12-conclusion)
13. [Contact Information](#13-contact-information)
14. [References](#14-references)

---

# 1. Introduction

This SOP details how to **install, generate, and troubleshoot Python dependencies** using `requirements.txt` on any operating system.

It applies to all Python developers ensuring environment consistency across operating systems. It covers:
* **Installing** existing project dependencies.
* **Generating** updated `requirements.txt` files.
* **Troubleshooting** environment and version conflicts.

---

# 2. What is requirements.txt?

A `requirements.txt` file is a **list of all external packages** your Python project needs to run. 

Instead of forcing you to install each package manually one by one, it allows you to automatically download and configure the entire environment all at once.

---

# 3. Why requirements.txt is Required?

Without a `requirements.txt` file, team members and servers have to guess which packages to install. This file is required to solve three main problems:

* **Consistency** — It ensures everyone on the team uses the exact same software versions, preventing the classic "it works on my machine" problem.
* **Automation** — It allows cloud servers, deployment pipelines, and setup scripts to configure the project automatically without human intervention.
* **Easy Onboarding** — New developers can set up their local environment and start working immediately instead of manually installing missing packages one by one.

---

# 4. Key Features of requirements.txt

* **Exact Matches (`==`)** — Installs one specific version of a tool (e.g., `Flask==3.0.0`) so nothing unexpectedly updates or breaks.
* **Minimum Versions (`>=`)** — Tells Python it needs at least a certain version or newer (e.g., `requests>=2.31.0`).
* **Clean Comments (`#`)** — Allows you to add notes in plain text to explain why a package is needed, which Python will completely ignore when installing.

---

# 5. Prerequisites & System Requirements

| **Requirement** | **Specification** |
| :--- | :--- |
| **OS** | Any major operating system (Windows, macOS, or Linux). |
| **RAM** | 512 MB or higher (enough to run standard Python scripts). |
| **Disk Space** | Minimal (varies depending on the size of the packages you install). |
| **Required Packages** | Python 3 (includes `pip` for package management). |

---

# 6. Install Dependencies from requirements.txt

## Step 6.1: Run the Installation Script
Navigate to your project folder containing the file and run the following command to deploy the dependencies:

```bash
pip install -r requirements.txt
```

<details>
<summary><strong>Screenshot - Execution of installation script</strong></summary>
<img width="1440" height="525" alt="Screenshot 2026-09-09 at 11 01 26 PM" src="https://github.com/user-attachments/assets/90c6bedf-2603-4459-89ec-06ef08b7f0e8" />
</details>

---

## Step 6.2: Verify the Installation
Run the following list command to ensure all required packages are present in the active environment:

```bash
pip list
```

<details>
<summary><strong>Screenshot - Dependencies installed successfully</strong></summary>
<img width="1440" height="359" alt="Screenshot 2026-09-09 at 11 04 43 PM" src="https://github.com/user-attachments/assets/719261ff-67b9-4b89-9e38-f41cd55b3b9b" />
</details>

---

# 7. How to Generate a requirements.txt File

## Step 7.1: Install the Generator Tool
To avoid cluttering the configuration with unrelated global system packages, do not use `pip freeze`. Use `pipreqs` to scan your project code and list only the packages you actually import:

```bash
pip install pipreqs
pipreqs . --force
```

<details>
<summary><strong>Screenshot - pipreqs execution output</strong></summary>

<img width="1327" height="589" alt="Screenshot 2026-09-09 at 10 40 14 PM" src="https://github.com/user-attachments/assets/200166d2-586c-4429-9f38-fe0be6b013bd" />
</details>

## Step 7.2: Inspect the Content
Verify the file was created cleanly by checking its contents inside the terminal:

**For Linux / macOS:**
```bash
cat requirements.txt
```
<details>
<summary><strong>Screenshot - Inspection of generated content</strong></summary>
<img width="638" height="69" alt="Screenshot 2026-09-09 at 10 40 54 PM" src="https://github.com/user-attachments/assets/2081c6d0-c04c-4820-9717-30ab55a34be4" />
</details>

**For Windows:**
```bash
type requirements.txt
```

<details>
<summary><strong>Screenshot - Inspection of generated content</strong></summary>
<img width="638" height="69" alt="Screenshot 2026-09-09 at 10 40 54 PM" src="https://github.com/user-attachments/assets/2081c6d0-c04c-4820-9717-30ab55a34be4" />

</details>

---

# 8. Quick Commands

| Task | Command |
| :--- | :--- |
| **Install dependencies** | `pip install -r requirements.txt` |
| **Generate from imports only** | `pipreqs . --force` |
| **List installed packages** | `pip list` |

---

# 9. Troubleshooting

| **Issue** | **Cause** | **Solution** |
| :--- | :--- | :--- |
| `command not found: pip` | Python or `pip` is missing or its environment path variable is not registered. | Run `python3 -m pip install -r requirements.txt` or ensure Python is added to your system **PATH**. |
| `ResolutionImpossible` | Two or more packages require conflicting versions of the same sub-dependency. | Loosen version constraints from exact locks (`==`) to minimum tags (`>=`) for the conflicting elements. |

---

# 10. Use Cases
* **Setting Up a Cloned Project:** Run the installation script immediately after downloading a project to ensure it works on your computer without missing library errors.
* **Sharing Code with Others:** Run the generation script before sending your project to a teammate so they know exactly what packages to download.
* **Fixing Version Errors:** Refer to this file whenever you run into "ModuleNotFoundError" bugs to confirm which tool version your code expects.
* **Updating Project Dependencies:** Run the `pipreqs` generation workflow when introducing new architectural components or open-source libraries into the codebase.
* **Security Auditing & Compliance:** Use the verified requirements list as an inventory tracker to scan for known security vulnerabilities or deprecated versions.

---

# 11. Best Practices
* **Pin Exact Versions:** Always use the `==` operator for production releases (e.g., `requests==2.31.0`) to avoid unexpected updates that might break your code.
* **Keep Comments Clean:** Use the `#` symbol to document why a non-standard or unusual package is required, ensuring team clarity.
* **Update the File Frequently:** Run the generation workflow immediately after importing a new package so your team never encounters missing module errors.
* **Avoid Global Clutter:** Never use `pip freeze` on global system environments to prevent bloating the configuration with local, unrelated tools.
* **Track via Version Control:** Always commit `requirements.txt` to Git so that every dependency shift can be tracked, reviewed, and rolled back if necessary.

---

# 12. Conclusion

This SOP provides a clear, standardized workflow for **installing, generating, and verifying Python dependencies** using `requirements.txt` on any operating system.

Following these steps ensures that our development environments remain **consistent, stable, and error-free**. Whether you are working locally or deploying to production, keeping this file updated guarantees that the code runs perfectly on every machine.

---

# 13. Contact Information

| **Name** | **Email** |
| :--- | :--- |
| Sahil | [sahil.butola.snaatak@mygurukulam.co](mailto:sahil.butola.snaatak@mygurukulam.co) |

---

# 14. References

* Official Pip User Documentation: [https://pypa.io](https://pypa.io)
* Pipreqs Project Catalog: [https://pypi.org](https://pypi.org)
