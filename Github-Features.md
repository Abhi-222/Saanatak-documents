# VCS Design + POC | Features of VCS | GitHub features

## Author Table

| **Author** | **Created on** | **Version** | **Last edited on**  | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ---------- | -------------- | ----------- | ------------------- | ---------------- | ----------------|---------------- |
| Sahil      | 10-09-26       | v1.0        | 10-09-26            | `Vishal/Divya M`|   `Aayush Verma`| `Mahesh Kumar / Varun|

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [What is GitHub?](#2-what-is-github)
3. [Why GitHub is Required?](#3-why-github-is-required)
4. [Key Features](#4-key-features)
5. [GitHub Features](#5-github-features)
   - 5.1 [Repository Management](#51-repository-management)
   - 5.2 [Branching & Merging](#52-branching--merging)
   - 5.3 [Pull Requests & Code Review](#53-pull-requests--code-review)
   - 5.4 [GitHub Actions (CI/CD)](#54-github-actions-cicd)
   - 5.5 [Issues & Projects](#55-issues--projects)
   - 5.6 [Access Control & Permissions](#56-access-control--permissions)
   - 5.7 [Security Features](#57-security-features)
6. [Advantages and Disadvantages](#6-advantages-and-disadvantages)
7. [Use Cases](#7-use-cases)
8. [Best Practices](#8-best-practices)
9. [Conclusion](#9-conclusion)
10. [Contact Information](#10-contact-information)
11. [References](#11-references)

---

## 1. Introduction

This document provides an overview of GitHub, covering its core features, common use cases, and recommended best practices. It is intended as a reference for teams looking to understand how GitHub supports version control, collaboration, and secure software development workflows.

---

## 2. What is GitHub?

GitHub is a cloud-based platform where developers store, manage, share, and collaborate on software code. Owned by Microsoft, it functions like a social network and a highly advanced backup system specially tailored for programmers.

---

## 3. Why GitHub is Required?

GitHub is required because traditional ways of handling files — like emailing zip folders or using Google Drive — fail completely when applied to software development. Without GitHub, large-scale software engineering would be incredibly chaotic, slow, and prone to catastrophic data loss.

---

## 4. Key Features

- **Version Control** — Tracks every change made to a file, allowing you to easily roll back to older versions if a bug is introduced.
- **Conflict Prevention** — Automatically highlights code conflicts, preventing developers from accidentally overwriting or erasing each other's work.
- **Cloud Backup** — Safely stores the entire history of a project in the cloud, protecting data if a developer's computer crashes or is stolen.
- **Pull Requests & Review** — Creates a formal review system where teammates can comment on, discuss, and approve code before it goes live.
- **Project Management** — Includes built-in task boards and bug trackers so teams can organize workflows right next to their code.

---

## 5. GitHub Features

### 5.1 Repository Management

| **Feature** | **Description** |
| ----------- | ---------------- |
| Repositories | Store code, history, and metadata for a project |
| Templates | Standardize new repo structure across teams |
| Wikis | Built-in documentation space per repository |
| Forks | Personal copies of a repository for independent changes |
| Releases | Tagged snapshots of the repo for versioned distribution |

---

### 5.2 Branching & Merging

| **Feature** | **Description** |
| ----------- | ---------------- |
| Branches | Isolated lines of development for features/fixes |
| Merge strategies | Merge commit, squash, or rebase options |
| Branch protection rules | Prevent direct pushes to protected branches (e.g., `main`) |
| Required status checks | Block merges until CI checks pass |

---

### 5.3 Pull Requests & Code Review

| **Feature** | **Description** |
| ----------- | ---------------- |
| Pull Requests (PRs) | Propose, discuss, and review changes before merging |
| Required reviewers | Enforce review approval before merge |
| Inline comments | Reviewers can comment on specific lines of code |
| Draft PRs | Mark work-in-progress before requesting review |
| Suggested changes | Reviewers can propose exact code edits inline |

---

### 5.4 GitHub Actions (CI/CD)

| **Feature** | **Description** |
| ----------- | ---------------- |
| Workflows | YAML-defined automation triggered on events (push, PR, schedule) |
| Marketplace Actions | Reusable pre-built automation steps |
| Self-hosted runners | Run pipelines on custom infrastructure |
| Matrix builds | Run jobs across multiple OS/language versions in parallel |
| Secrets management | Store credentials securely for use in workflows |

---

### 5.5 Issues & Projects

| **Feature** | **Description** |
| ----------- | ---------------- |
| Issues | Track bugs, tasks, and enhancement requests |
| Projects (Boards) | Kanban-style tracking linked to issues/PRs |
| Milestones | Group issues/PRs toward a release goal |
| Labels | Categorize and filter issues/PRs |

---

### 5.6 Access Control & Permissions

| **Feature** | **Description** |
| ----------- | ---------------- |
| Teams | Group-based access management |
| Role-based permissions | Read, Triage, Write, Maintain, Admin levels |
| Branch protection | Restrict who can push/merge to key branches |
| Organization-level policies | Control repo visibility, member access org-wide |

---

### 5.7 Security Features

| **Feature** | **Description** |
| ----------- | ---------------- |
| Dependabot | Automated dependency vulnerability alerts and updates |
| Secret scanning | Detects committed credentials/API keys |
| Code scanning (CodeQL) | Static analysis to catch security issues |
| Security advisories | Privately disclose and patch vulnerabilities |

---

## 6. Advantages and Disadvantages

GitHub offers unmatched collaboration and version control for software development, but it also presents challenges regarding pricing limits, security risks, and a steep learning curve.

### Advantages

- **Centralized Collaboration** — Streamlines teamwork through pull requests, code reviews, and inline comments.
- **Robust Version Control** — Safeguards your project history, making it easy to track changes and revert bugs.
- **Built-In Security** — Scans repositories automatically for exposed passwords, secrets, and vulnerable dependencies.

### Disadvantages

- **Steep Learning Curve** — Requires a solid understanding of Git command-line tools, which can be confusing for beginners.
- **Costly for Large Teams** — Becomes expensive quickly as teams grow, since advanced features and team seats require paid monthly subscriptions.
- **Repository Size Limits** — Imposes strict limits on large files (over 100MB requires special setup), making it poorly suited for game development or massive datasets.

---

## 7. Use Cases

- **Open-Source Collaboration** — Hosts thousands of public projects (like Linux or VS Code) where global communities contribute code, report bugs, and suggest features.
- **Team Code Management** — Enables engineering teams to collaborate on proprietary software using private repositories without overriding each other's work.
- **Continuous Integration/Continuous Deployment (CI/CD)** — Uses GitHub Actions to automatically build, test, and deploy software straight to servers or app stores whenever changes are pushed.
- **Developer Portfolios** — Serves as a digital resume for developers to showcase their coding skills, contributions, and project history to potential employers.

---

## 8. Best Practices

- **Protect `main`** — require PRs with at least one approval and passing status checks before merge; no direct pushes.
- **Keep PRs small and linked to issues** — easier review, and traceable back to the task/ticket.
- **Automate checks via Actions** — run lint/tests/security scans (Dependabot, secret scanning, CodeQL) on every PR, not just before release.
- **Apply least-privilege access** — use Teams with role-based permissions, and audit access periodically.
- **Store secrets properly** — never hardcode credentials in code or workflows; use GitHub's encrypted secrets.

---

## 9. Conclusion

GitHub combines repository management, code review, CI/CD automation, and security tooling into a single platform, making it a widely adopted choice for teams managing source code collaboratively. By following the best practices outlined in this document — protecting main branches, automating checks, and enforcing least-privilege access — teams can use GitHub not just as a code host, but as the back

---

## 10. Contact Information

| **Name** | **Email** |
| -------- | --------- |
| Sahil    | [sahil.butola.snaatak@mygurukulam.co](mailto:sahil.butola.snaatak@mygurukulam.co)|

---

## 11. References

| Topic | Description |
| ----- | ------------ |
| [GitHub Docs](https://docs.github.com/) | Official GitHub documentation |
| [GitHub Actions](https://docs.github.com/en/actions) | Official documentation for GitHub Actions CI/CD |
| [About Branch Protection Rules](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches) | GitHub docs on branch protection |
| [About Pull Requests](https://docs.github.com/en/pull-requests) | GitHub docs on the PR workflow |
