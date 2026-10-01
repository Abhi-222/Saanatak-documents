<p align="center"><img width="202" height="204" alt="CI Orchestration" src="https://github.com/user-attachments/assets/d208b716-2e8d-470e-8895-f431d6976759" /></p>

# Comparison between Different CI Orchestration Tools

## Author Table

| Author       | Created On | Version | Last Edited On | L0 Reviewer       | L1 Reviewer  | L2 Reviewer          |
| ------------ | ---------- | ------- | -------------- | ----------------- | ------------ | -------------------- |
| Sahil Butola | 30-09-2026 | 1.0     | 01-09-2026     | Divya M. / Vishal | Aayush Verma | Mahesh Kumar / Varun |

## Table of Contents

1. [Introduction](#1-introduction)
2. [What are CI Orchestration Tools](#2-what-are-ci-orchestration-tools)
3. [Why are CI Orchestration Tools Required](#3-why-are-ci-orchestration-tools-required)
4. [CI Orchestration Workflow](#4-ci-orchestration-workflow)
5. [Tools Under Evaluation](#5-tools-under-evaluation)
6. [Comparison Table](#6-comparison-table)
7. [Final Tool Recommendation](#7-final-tool-recommendation)
8. [Advantages](#8-advantages)
9. [Best Practices](#9-best-practices)
10. [Conclusion](#10-conclusion)
11. [Contact Information](#11-contact-information)
12. [References](#12-references)

---

## 1. Introduction

Application Continuous Integration (CI) automates building, testing, validation, and packaging after code changes. CI orchestration tools coordinate these activities and integrate SCM, build, testing, quality, security, and artifact tools. This document compares **Jenkins, GitLab CI/CD, and BuildPiper** and recommends a tool for Application CI.

---

## 2. What are CI Orchestration Tools

CI orchestration tools automate and coordinate CI activities through defined pipelines. They manage source checkout, builds, tests, quality checks, security validation, artifact creation, credentials, execution workers, logs, and pipeline results.

---

## 3. Why are CI Orchestration Tools Required

* Automate repetitive CI activities.
* Standardize and repeat application workflows.
* Detect build, test, quality, and security issues early.
* Integrate required development and DevOps tools.
* Support parallel and distributed execution.
* Provide centralized pipeline visibility.
* Secure credentials and access.
* Enable reusable and scalable CI workflows.

---

## 4. CI Orchestration Workflow

<img width="300" height="700" alt="image" src="https://github.com/user-attachments/assets/e23f9738-2ae8-41aa-95ff-7f517ee2ae90" />

> **Note:** The workflow covers Application CI only; deployment and CD are outside this ticket.

---
### Workflow Steps

| Step   | Activity     | Description                                                  |
| ------ | ------------ | ------------------------------------------------------------ |
| **1**  | Code Change  | Developer creates or modifies application code.              |
| **2**  | Commit       | Changes are committed to the source repository.              |
| **3**  | Trigger      | Webhook, schedule, or manual trigger starts the CI pipeline. |
| **4**  | Checkout     | CI tool retrieves the required source code.                  |
| **5**  | Build        | Application is compiled or packaged.                         |
| **6**  | Test         | Automated tests are executed.                                |
| **7**  | Quality      | Code-quality checks are performed.                           |
| **8**  | Security     | Security and dependency checks are performed.                |
| **9**  | Package      | Application packages or build artifacts are generated.       |
| **10** | Publish      | Artifacts are stored in the required repository.             |
| **11** | Notification | Pipeline status is communicated to the relevant team.        |

---

## 5. Tools Under Evaluation

**Jenkins:** Open-source automation server supporting `Jenkinsfile`, agents, credentials, Shared Libraries, plugins, and multiple SCM platforms.

**GitLab CI/CD:** GitLab's integrated CI/CD capability using `.gitlab-ci.yml` and GitLab Runners, with native GitLab integration and reusable templates.

**BuildPiper:** Application delivery and orchestration platform using Pipeline → Stage → Job → Step, with reusable job templates and application/global pipelines.

---

## 6. Comparison Table

| Criteria           | Jenkins               | GitLab CI/CD              | BuildPiper              |
| ------------------ | --------------------- | ------------------------- | ----------------------- |
| Pipeline as Code   | `Jenkinsfile`         | `.gitlab-ci.yml`          | Pipeline configuration  |
| Execution          | Agents                | Runners                   | Platform-based          |
| Parallel Jobs      | Yes                   | Yes                       | Supported               |
| SCM Flexibility    | High                  | GitLab-focused            | Platform integrations   |
| Credentials        | Jenkins Credentials   | CI/CD Variables / Secrets | Platform-based          |
| Integrations       | Extensive plugins     | Built-in integrations     | Platform integrations   |
| Reusability        | Shared Libraries      | Templates / Includes      | Job Templates           |
| Self-Hosted        | Yes                   | Yes                       | Available               |
| Scalability        | Distributed agents    | Distributed runners       | Platform-dependent      |
| Visibility         | Logs, stages, results | Pipelines, jobs, logs     | Pipeline/job visibility |
| Operational Effort | Medium–High           | Low–Medium                | Platform-dependent      |
| Licensing          | Open source           | Free and paid tiers       | Commercial              |

---

## 7. Final Tool Recommendation

### Selected Tool: Jenkins

Jenkins is recommended for the defined Application CI requirements because it provides:

* Pipeline as Code through `Jenkinsfile`.
* Distributed execution using agents.
* Broad integration with SCM, build, test, quality, security, and artifact tools.
* SCM independence and technology flexibility.
* Credential management and reusable Shared Libraries.
* Scalability through distributed agents.

GitLab CI/CD remains suitable where GitLab is the primary SCM and DevOps platform. BuildPiper is suitable for broader application delivery orchestration requirements.

---

## 8. Advantages

* Automated and standardized CI workflows.
* Flexible support for technologies and SCM platforms.
* Distributed and scalable execution.
* Broad tool integration.
* Secure credential management.
* Reusable pipeline logic.
* Centralized logs and pipeline results.

---

## 9. Best Practices

* **Pipeline as Code** – Version-control the `Jenkinsfile` with application code.
* **Credentials** – Never hardcode secrets; use Jenkins Credentials.
* **Least Privilege** – Grant only required user/job permissions.
* **Agents** – Run CI workloads on agents, not the controller.
* **Standardization** – Use Checkout → Build → Test → Quality → Security → Package → Publish.
* **Reusability** – Use Shared Libraries for common pipeline logic.
* **Quality/Security** – Enforce mandatory quality and security gates.
* **Plugins** – Maintain required plugins and apply security updates.
* **Configuration** – Use Configuration as Code where applicable.
  
---

## 10. Conclusion

Jenkins, GitLab CI/CD, and BuildPiper were compared against the defined Application CI requirements. Jenkins is selected for its Pipeline as Code, distributed execution, broad integrations, SCM flexibility, credential management, reusability, and scalability.

---

## 11. Contact Information

| Name         | Email                                                                             |
| ------------ | --------------------------------------------------------------------------------- |
| Sahil Butola | [sahil.butola.snaatak@mygurukulam.co](mailto:sahil.butola.snaatak@mygurukulam.co) |

---

## 12. References

| Resource                          | Link                                                                                          |
| --------------------------------- | --------------------------------------------------------------------------------------------- |
| Jenkins Documentation             | [Jenkins Documentation](https://www.jenkins.io/doc/)                                          |
| Jenkins Pipeline                  | [Jenkins Pipeline](https://www.jenkins.io/doc/book/pipeline/)                                 |
| Jenkins Credentials               | [Jenkins Credentials](https://www.jenkins.io/doc/book/using/using-credentials/)               |
| Jenkins Configuration as Code     | [Jenkins Configuration as Code](https://www.jenkins.io/projects/jcasc/)                       |
| GitLab CI/CD.                     | [GitLab CI/CD](https://docs.gitlab.com/ci/)                                                   |
| GitLab Runner                     | [GitLab Runner](https://docs.gitlab.com/runner/)                                              |
| BuildPiper                        | [BuildPiper](https://buildpiper.io/)                                                          |
| BuildPiper Documentation          | [BuildPiper Documentation](https://docs.buildpiper.io/docs/)                                  |
