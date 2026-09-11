# VCS Design + POC | Features of VCS | BitBucket features

## Author Table

| **Author** | **Created on** | **Version** | **Last edited on**  | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ---------- | -------------- | ----------- | ------------------- | ---------------- | ----------------|---------------- |
| Sahil      | 11-09-26       | v1.0        | 11-09-26            | `Vishal/Divya M`|   `Aayush Verma` | `Mahesh Kumar / Varun|

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [What is BitBucket?](#2-what-is-bitbucket)
3. [Why BitBucket is Required?](#3-why-bitbucket-is-required)
4. [Key Features](#4-key-features)
5. [BitBucket Features](#5-bitbucket-features)
   - 5.1 [Repository Management](#51-repository-management)
   - 5.2 [Branching & Merging](#52-branching--merging)
   - 5.3 [Pull Requests & Code Review](#53-pull-requests--code-review)
   - 5.4 [Bitbucket Pipelines (CI/CD)](#54-bitbucket-pipelines-cicd)
   - 5.5 [Issues & Jira Integration](#55-issues--jira-integration)
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

This document provides an overview of BitBucket, covering its core features, common use cases, and recommended best practices. It is intended as a reference for teams looking to understand how BitBucket supports version control, collaboration, and secure software development workflows.

---

## 2. What is BitBucket?

BitBucket is a cloud-based (and self-hosted, via Data Center) Git repository management platform where developers store, manage, share, and collaborate on software code. Owned by Atlassian, it is built around tight, native integration with the rest of the Atlassian ecosystem — Jira, Confluence, and Trello — making it a natural fit for teams already standardized on those tools.

---

## 3. Why BitBucket is Required?

BitBucket is required because traditional ways of handling files — like emailing zip folders or using Google Drive — fail completely when applied to software development. Without BitBucket, large-scale software engineering would be incredibly chaotic, slow, and prone to catastrophic data loss. For teams already using Jira for issue tracking, BitBucket is particularly required because it links commits, branches, and pull requests directly to Jira tickets, giving traceability between "what was planned" and "what was actually coded" without any extra tooling.

---

## 4. Key Features

- **Version Control** — Tracks every change made to a file, allowing you to easily roll back to older versions if a bug is introduced.
- **Conflict Prevention** — Automatically highlights code conflicts, preventing developers from accidentally overwriting or erasing each other's work.
- **Cloud Backup** — Safely stores the entire history of a project in the cloud, protecting data if a developer's computer crashes or is stolen.
- **Pull Requests & Review** — Creates a formal review system where teammates can comment on, discuss, and approve code before it goes live.
- **Native Jira Integration** — Links branches, commits, and pull requests directly to Jira issues, so ticket status can update automatically as code moves through the pipeline.

---

## 5. BitBucket Features

### 5.1 Repository Management

| **Feature** | **Description** |
| ----------- | ---------------- |
| Repositories | Store code, history, and metadata for a project |
| Workspaces | Group of repositories and users, similar to a GitHub organization |
| Projects | Group related repositories within a workspace for easier navigation |
| Forks | Personal copies of a repository for independent changes |
| Snippets | Share small pieces of code or text without creating a full repository |

---

### 5.2 Branching & Merging

| **Feature** | **Description** |
| ----------- | ---------------- |
| Branches | Isolated lines of development for features/fixes |
| Merge strategies | Merge commit, squash, or fast-forward options |
| Branch permissions | Restrict who can write to or merge into specific branches (e.g., `main`) |
| Merge checks | Block merges until minimum approvals or successful builds are met (Premium) |

---

### 5.3 Pull Requests & Code Review

| **Feature** | **Description** |
| ----------- | ---------------- |
| Pull Requests (PRs) | Propose, discuss, and review changes before merging |
| Required reviewers | Enforce a minimum number of approvals before merge |
| Inline comments | Reviewers can comment on specific lines of code |
| Draft/Work-in-progress PRs | Mark work-in-progress before requesting review |
| Tasks on PRs | Create checklist-style tasks that must be resolved before merge |

---

### 5.4 Bitbucket Pipelines (CI/CD)

| **Feature** | **Description** |
| ----------- | ---------------- |
| Pipelines | YAML-defined (`bitbucket-pipelines.yml`) automation triggered on push, PR, or schedule |
| Pipes | Reusable, pre-built automation steps similar to GitHub Marketplace Actions |
| Self-hosted runners | Run pipelines on custom infrastructure instead of Atlassian-hosted runners |
| Deployment environments | Define and gate deployments to Test, Staging, and Production |
| Build minutes & caching | Cache dependencies between builds and manage usage against plan build-minute limits |

---

### 5.5 Issues & Jira Integration

| **Feature** | **Description** |
| ----------- | ---------------- |
| Built-in Issue Tracker | Track bugs, tasks, and enhancement requests directly in a repository |
| Jira Smart Commits | Update or transition Jira issues directly from a commit message |
| Branch-to-issue linking | Create a branch directly from a Jira ticket with naming auto-populated |
| Development panel in Jira | View linked branches, commits, PRs, and build status inside the Jira ticket itself |

---

### 5.6 Access Control & Permissions

| **Feature** | **Description** |
| ----------- | ---------------- |
| Workspace groups | Group-based access management across repositories |
| Role-based permissions | Read, Write, Admin levels at repository and workspace level |
| Branch permissions | Restrict who can push/merge to key branches |
| IP allowlisting | Restrict access to a workspace by IP range (Premium) |

---

### 5.7 Security Features

| **Feature** | **Description** |
| ----------- | ---------------- |
| Secret scanning | Detects committed credentials/API keys in repositories |
| IP allowlisting | Limit access to trusted network ranges |
| Two-step verification | Enforce 2FA for all workspace members |
| Audit logs | Track user and admin activity across a workspace (Premium) |
| Merge checks | Enforce passing builds and required approvals before code reaches protected branches |

---

## 6. Advantages and Disadvantages

BitBucket offers strong collaboration, version control, and tight Jira integration for software development, but it also presents challenges regarding UI performance, third-party ecosystem size, and feature gating behind paid plans.

### Advantages

- **Native Jira & Atlassian Integration** — Links commits, branches, and PRs to Jira issues out of the box, with no third-party app required.
- **Flexible Hosting** — Available as both Bitbucket Cloud and Bitbucket Data Center (self-hosted), suiting teams with strict compliance or on-premise needs.
- **Built-In CI/CD** — Bitbucket Pipelines ships as part of the platform, avoiding the need to wire up a separate CI tool for basic workflows.

### Disadvantages

- **Smaller Ecosystem** — Has a far smaller marketplace of third-party integrations and community plugins compared to GitHub.
- **Costly for Large Teams** — Becomes expensive quickly as teams grow, since Premium features (merge checks, IP allowlisting, audit logs) require paid tiers.
- **Slower UI at Scale** — Repository browsing and pipeline logs can feel noticeably slower than competitors on large monorepos or high pipeline volume.

---

## 7. Use Cases

- **Jira-Centric Engineering Teams** — Enables teams already using Jira for planning to keep code, tickets, and builds tied together without extra integration work.
- **Private/Proprietary Code Management** — Enables engineering teams to collaborate on proprietary software using private repositories without overriding each other's work.
- **Continuous Integration/Continuous Deployment (CI/CD)** — Uses Bitbucket Pipelines to automatically build, test, and deploy software to servers or environments whenever changes are pushed.
- **Regulated / On-Premise Environments** — Bitbucket Data Center allows organizations with compliance or data-residency requirements to self-host the platform.

---

## 8. Best Practices

- **Protect `main`** — require PRs with at least one approval and passing merge checks before merge; restrict direct pushes via branch permissions.
- **Keep PRs small and linked to Jira tickets** — easier review, and traceable back to the task via Smart Commits or branch linking.
- **Automate checks via Pipelines** — run lint/tests/security scans on every PR, not just before release.
- **Apply least-privilege access** — use workspace groups with role-based permissions, and audit access periodically.
- **Store secrets properly** — never hardcode credentials in code or pipeline YAML; use Bitbucket's repository/workspace variables marked as secured.

---

## 9. Conclusion

BitBucket combines repository management, code review, CI/CD automation via Pipelines, and native Jira integration into a single platform, making it a strong choice for teams already invested in the Atlassian ecosystem. By following the best practices outlined in this document — protecting main branches, automating checks, and enforcing least-privilege access — teams can use BitBucket not just as a code host, but as the backbone of a Jira-linked development workflow.

---

## 10. Contact Information

| **Name** | **Email** |
| -------- | --------- |
| Sahil    | [sahil.butola.snaatak@mygurukulam.co](mailto:sahil.butola.snaatak@mygurukulam.co)|

---

## 11. References

| Topic | Description |
| ----- | ------------ |
| [Bitbucket Features](https://bitbucket.org/product/en/features) | Official Bitbucket feature overview |
| [Bitbucket Pipelines](https://support.atlassian.com/bitbucket-cloud/docs/pipeline-start-conditions/) | Official documentation for Bitbucket Pipelines CI/CD |
| [Branch Permissions](https://support.atlassian.com/bitbucket-cloud/docs/use-branch-permissions/) | Bitbucket docs on branch permissions |
| [Configure Branch Restrictions](https://support.atlassian.com/bitbucket-cloud/docs/configure-a-projects-branch-restrictions/) | Bitbucket docs on merge checks and branch restrictions |
| [Repository Access Token Permissions](https://support.atlassian.com/bitbucket-cloud/docs/repository-access-token-permissions/) | Bitbucket docs on repository-level permissions |
