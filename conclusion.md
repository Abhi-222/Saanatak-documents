<p align="center">
<img width="202" height="204" alt="image" src="https://github.com/user-attachments/assets/d208b716-2e8d-470e-8895-f431d6976759" />
</p>

---

# Comparison between different CI Orchestration Tools 

---

## Document Information

| Author       | Created On | Version | Last Edited On | L0 Reviewer       | L1 Reviewer  | L2 Reviewer        |
| ------------ | ---------- | ------- | -------------- | ----------------- | ------------ | ------------------ |
| Sahil Butola | 30-09-2026 | 1.0     | 30-09-2026     | Divya M. / Vishal | Aayush verma | Mahesh kumar/Varun |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What are CI Orchestration Tools](#2-what-are-ci-orchestration-tools)
3. [Why CI Orchestration Tools are Required](#3-why-ci-orchestration-tools-are-required)
4. [Requirements and Assumptions](#4-requirements-and-assumptions)
5. [CI Orchestration Workflow](#5-ci-orchestration-workflow)
6. [CI Orchestration Tools Under Evaluation](#6-ci-orchestration-tools-under-evaluation)
7. [Comparison Table](#7-comparison-table)
8. [Tool Selection Criteria and Weighted Scoring](#8-tool-selection-criteria-and-weighted-scoring)
9. [Cost and Operational Effort](#9-cost-and-operational-effort)
10. [Final Tool Recommendation](#10-final-tool-recommendation)
11. [Risks and Trade-offs of Jenkins](#11-risks-and-trade-offs-of-jenkins)
12. [Advantages](#12-advantages)
13. [Best Practices](#13-best-practices)
14. [Conclusion](#14-conclusion)
15. [Contact Information](#15-contact-information)
16. [References](#16-references)

---

# 1. Introduction

This document compares **CI orchestration tools** for **Application Continuous Integration (CI)** and recommends the tool best suited to the defined requirements.

Application CI automates **building, testing, validating, and packaging application code** after changes are committed. CI orchestration tools coordinate source-code management, build tools, testing frameworks, code-quality tools, security scanners, and artifact repositories through automated workflows.

The following tools are evaluated:

1. **Jenkins**
2. **GitLab CI/CD**
3. **BuildPiper**

It covers the **CI workflow, requirements, tool capabilities, comparison, weighted scoring, cost and effort, final recommendation, risks, and best practices**.

---

# 2. What are CI Orchestration Tools

CI orchestration tools are platforms used to **define, automate, and coordinate multiple CI activities within a structured workflow**.

They coordinate activities such as source-code checkout, dependency installation, application build, automated testing, code-quality analysis, security validation, artifact generation and publishing, and notifications.

The tool controls the **execution order, conditions, integrations, credentials, execution environments, and results** of these activities.

---

# 3. Why CI Orchestration Tools are Required

* Automation: Automates repetitive build, test, validation, and packaging activities.
* Consistency: Ensures applications follow a standardized CI workflow.
* Repeatability: Executes the same pipeline repeatedly from defined configuration.
* Early Validation: Identifies build, test, quality, and security issues early.
* Tool Integration: Connects source control, build, testing, quality, security, and artifact tools.
* Parallel Execution: Runs independent CI activities simultaneously.
* Centralized Visibility: Provides visibility into pipeline execution, logs, stages, and failures.
* Credential Management: Supplies credentials to CI jobs securely.
* Scalability: Executes CI workloads across multiple agents or runners.
* Standardization: Enables teams to follow common CI processes and practices.

---

# 4. Requirements and Assumptions

The evaluation is based on the following requirements and assumptions.

| Area                    | Requirement / Assumption                                                                                         |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------- |
| **Scope**               | Application CI only: checkout, build, test, quality, security, package, and publish.                             |
| **SCM Environment**     | The CI layer should work independently of the source-control platform, so it is not tied to a single SCM vendor. |
| **Hosting**             | The tool must be self-hostable.                                                                                  |
| **Technology Coverage** | The tool must support multiple application technologies and build tools.                                         |
| **Integrations**        | The tool must integrate with code-quality tools, security scanners, and an artifact repository.                  |
| **Execution Model**     | CI workloads must run on separate workers (agents or runners), with support for containerized execution.         |
| **Security**            | Credentials must not be hardcoded. Role-based access control with least privilege is required.                   |
| **Maintainability**     | Pipelines must be defined as code, version-controlled, and reusable across applications.                         |


---

# 5. CI Orchestration Workflow

<img width="300" height="700" alt="image" src="https://github.com/user-attachments/assets/e23f9738-2ae8-41aa-95ff-7f517ee2ae90" />

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

# 6. CI Orchestration Tools Under Evaluation

### Jenkins

Jenkins is an open-source automation server used to automate build, test, delivery, and deployment workflows. It supports **Pipeline as Code** through a `Jenkinsfile`, distributed execution through agents, credential management, triggers, Shared Libraries, and a large plugin ecosystem. It is self-hosted.

### GitLab CI/CD

GitLab CI/CD is the CI/CD capability integrated into the GitLab platform. Pipelines are defined in `.gitlab-ci.yml` and executed by GitLab Runners. It provides native SCM integration, CI/CD variables and secrets, and includes/templates for reuse. It is available self-managed or as SaaS.

### BuildPiper

BuildPiper is an application delivery and orchestration platform. It provides **Application Pipelines** for application-level workflows and **Global Pipelines** for workflows across applications and teams, using the structure **Pipeline → Stage → Job → Step**, with reusable job templates.

BuildPiper is a commercial, paid platform. Its website describes CI/CD pipelines with built-in security analysis, delivery observability, and integrations with version control, work management, and team communication tools.

> BuildPiper details are based on its website and public documentation. Entries marked "Not verified" could not be confirmed from public sources.

### Key Capabilities

| Capability                     | Jenkins                    | GitLab CI/CD                           | BuildPiper                    |
| ------------------------------ | -------------------------- | -------------------------------------- | ----------------------------- |
| **Pipeline as Code**           | `Jenkinsfile`              | `.gitlab-ci.yml`                       | Pipeline configuration        |
| **Distributed Execution**      | Agents                     | GitLab Runners                         | Not verified                  |
| **Source Control Integration** | Broad, SCM-agnostic        | Native GitLab                          | Not verified                  |
| **Credentials Management**     | Jenkins Credentials        | CI/CD Variables / Secrets              | Not verified                  |
| **Tool Integration**           | Extensive plugin ecosystem | Built-in capabilities and integrations | Platform modules              |
| **Pipeline Reusability**       | Shared Libraries           | Includes / Templates                   | Job Templates                 |
| **Pipeline Structure**         | Stages / Steps             | Stages / Jobs                          | Pipeline → Stage → Job → Step |
| **Self-Hosted**                | Yes                        | Yes                                    | Available                     |

---

# 7. Comparison Table

| Criteria                        | Jenkins                                     | GitLab CI/CD                                         | BuildPiper                             |
| ------------------------------- | ------------------------------------------- | ---------------------------------------------------- | -------------------------------------- |
| **Primary Focus**               | Automation and CI/CD orchestration          | Integrated DevOps platform                           | Application delivery and orchestration |
| **Pipeline as Code**            | `Jenkinsfile` (Groovy)                      | `.gitlab-ci.yml` (YAML)                              | Pipeline configuration                 |
| **Distributed Execution**       | Agents (static, container, Kubernetes)      | Runners (shell, Docker, Kubernetes)                  | Not verified                           |
| **Parallel Execution**          | Yes                                         | Yes                                                  | Not verified                           |
| **SCM Integration**             | Broad; SCM-agnostic                         | Native to GitLab; external SCM supported with limits | Not verified                           |
| **Credentials / Secrets**       | Jenkins Credentials; Vault plugin           | CI/CD variables; external secrets integrations       | Not verified                           |
| **Extensibility**               | Large plugin ecosystem (maintenance burden) | Built-in features; fewer third-party extensions      | Platform modules                       |
| **Reusable Components**         | Shared Libraries                            | Includes / Templates                                 | Job Templates                          |
| **Access Control / Audit**      | RBAC via plugins; audit via plugin          | Built-in RBAC; audit events (tier-dependent)         | Not verified                           |
| **Multi-Service Orchestration** | Pipeline-based                              | Pipeline-based (multi-project, parent-child)         | Application Pipelines (core)           |
| **Cross-Application Workflows** | Pipeline-based                              | Pipeline-based                                       | Global Pipelines (core)                |
| **Licensing**                   | Open source (free)                          | Free tier; paid tiers for advanced features          | Commercial (paid, per-user pricing)    |
| **Operational Overhead**        | High (controller, plugins, upgrades)        | Low to medium                                        | Not verified                           |
| **Community and Ecosystem**     | Very large, mature                          | Large                                                | Smaller                                |
| **SCM / Tool Independence**     | High                                        | Closely tied to GitLab                               | Platform-oriented                      |

---

# 8. Tool Selection Criteria and Weighted Scoring

### Criteria and Weights

| No.    | Selection Criterion              | Weight | Requirement                                                 |
| ------ | -------------------------------- | ------ | ----------------------------------------------------------- |
| **1**  | Pipeline as Code                 | 10%    | CI workflows should be maintainable as code.                |
| **2**  | Build and Test Automation        | 10%    | Tool should support automated builds and tests.             |
| **3**  | Distributed Execution            | 10%    | CI workloads should be executable on separate workers.      |
| **4**  | Tool Integration                 | 15%    | Tool should integrate with required DevOps tools.           |
| **5**  | Credential Management            | 10%    | Sensitive credentials should not be hardcoded.              |
| **6**  | Pipeline Reusability             | 10%    | Common pipeline logic should be reusable.                   |
| **7**  | Scalability                      | 10%    | CI execution should support increasing workloads.           |
| **8**  | Flexibility and SCM Independence | 10%    | Tool should work across technologies and SCM environments.  |
| **9**  | Centralized Visibility           | 5%     | Pipeline execution and logs should be centrally accessible. |
| **10** | Maintainability                  | 10%    | Configuration should be manageable and version-controlled.  |

### Scores (1 = poor, 5 = excellent)

| Criterion                            | Weight | Jenkins  | GitLab CI/CD | BuildPiper |
| ------------------------------------ | ------ | -------- | ------------ | ---------- |
| **Pipeline as Code**                 | 10%    | 5        | 5            | 3          |
| **Build and Test Automation**        | 10%    | 5        | 5            | 4          |
| **Distributed Execution**            | 10%    | 5        | 5            | 3          |
| **Tool Integration**                 | 15%    | 5        | 4            | 3          |
| **Credential Management**            | 10%    | 4        | 4            | 3          |
| **Pipeline Reusability**             | 10%    | 4        | 4            | 3          |
| **Scalability**                      | 10%    | 4        | 4            | 3          |
| **Flexibility and SCM Independence** | 10%    | 5        | 3            | 3          |
| **Centralized Visibility**           | 5%     | 3        | 5            | 4          |
| **Maintainability**                  | 10%    | 3        | 4            | 3          |
| **Weighted Total (out of 5)**        | 100%   | **4.40** | **4.25**     | **3.15**   |

Jenkins and GitLab CI/CD are close (0.15 apart). The result is most sensitive to the weight given to SCM independence. BuildPiper's score is lower mainly because several CI capabilities could not be verified from public sources.

---

# 9. Cost and Operational Effort

| Factor                  | Jenkins                                                        | GitLab CI/CD                                    | BuildPiper                          |
| ----------------------- | -------------------------------------------------------------- | ----------------------------------------------- | ----------------------------------- |
| **Licensing**           | Free                                                           | Free tier; paid tiers for advanced features     | Commercial (paid, per-user pricing) |
| **Infrastructure**      | Controller + agents (self-provided)                            | Runners (self-provided); server if self-managed | Not verified                        |
| **Setup Effort**        | Medium                                                         | Low (if GitLab already used)                    | Not verified                        |
| **Ongoing Maintenance** | High: plugin updates, security patches, controller HA, backups | Low to medium                                   | Not verified                        |
| **Skills Required**     | Groovy / Jenkins administration                                | YAML / GitLab administration                    | Not verified                        |

Jenkins has no licensing cost, but it requires more operational effort than the managed or integrated options. This trade-off is covered in Section 11.

---

# 10. Final Tool Recommendation

## Selected Tool: Jenkins

Jenkins scores highest against the weighted criteria, mainly on tool integration and SCM independence.

| Capability                 | Why It Supports the Selection                                                                                                 |
| -------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| **Pipeline as Code**       | `Jenkinsfile` is stored with application source and managed through version control.                                          |
| **Distributed Execution**  | Agents run build and test workloads off the controller. Containerized or Kubernetes-based ephemeral agents are recommended.   |
| **Broad Tool Integration** | Plugins connect SCM, build tools, test frameworks, quality and security tools, artifact repositories, and cloud platforms.    |
| **Credential Management**  | Jenkins Credentials store passwords, tokens, SSH keys, secret text/files, and certificates. Vault integration is recommended. |
| **Pipeline Flexibility**   | Supports stages, conditional execution, parallel activities, external commands, and post-build actions.                       |
| **Pipeline Reusability**   | Shared Libraries centralize common CI logic across application pipelines.                                                     |

### Why GitLab CI/CD Was Not Selected

GitLab CI/CD is a complete option and scored close to Jenkins. It was not selected because the requirement calls for a CI layer that operates independently of the SCM platform. It remains the stronger choice where GitLab is the primary SCM and DevOps platform.

### Why BuildPiper Was Not Selected

BuildPiper targets application delivery and cross-team orchestration, which is broader than the CI-focused requirement. Several CI details (supported SCMs, runner model, secrets handling, RBAC and audit) could not be verified from public sources. It can be re-evaluated if the scope expands to end-to-end delivery orchestration.

### Selection Summary

<img width="700" height="700" alt="image" src="https://github.com/user-attachments/assets/1b032771-4df1-4148-aa51-03d84b70ff76" />

---

# 11. Risks and Trade-offs of Jenkins

| Risk                                      | Impact                                      | Mitigation                                                            |
| ----------------------------------------- | ------------------------------------------- | --------------------------------------------------------------------- |
| **Plugin sprawl and compatibility**       | Upgrade failures, instability               | Approved plugin list; test upgrades in staging; remove unused plugins |
| **Security patching load**                | Exposure to core and plugin vulnerabilities | Regular patch cycle; follow Jenkins security advisories               |
| **Controller as single point of failure** | CI outage                                   | Backups, Configuration as Code (JCasC), HA or fast-recovery plan      |
| **Groovy learning curve**                 | Slower onboarding, inconsistent pipelines   | Shared Library templates; training; standard pipeline patterns        |
| **Higher operational effort**             | Ongoing administration cost                 | Assign ownership; automate provisioning and upgrades                  |
| **Snowflake configuration**               | Hard to reproduce environments              | Manage configuration with JCasC and store it in Git                   |

---

# 12. Advantages

* Automation: Automates build, test, validation, and packaging activities.
* Pipeline as Code: CI workflows are maintained in a version-controlled Jenkinsfile.
* Flexibility: Supports different application technologies and DevOps tools.
* Distributed Execution: Agents execute workloads independently from the controller.
* Extensibility: Plugins and integrations extend Jenkins functionality.
* Security: Provides credential management and authorization capabilities.
* Reusability: Shared Libraries reduce duplicated pipeline logic.
* Visibility: Stages, results, and logs provide centralized visibility.
* Scalability: Workloads can be distributed across multiple agents.
* Standardization: Teams can implement common CI stages and practices.

---

# 13. Best Practices

| Best Practice                   | Description                                                                                |
| ------------------------------- | ------------------------------------------------------------------------------------------ |
| **Pipeline as Code**            | Store the `Jenkinsfile` in the application source repository and version it with the code. |
| **Secure Credentials**          | Never hardcode passwords, tokens, SSH keys, or cloud credentials.                          |
| **Use RBAC / Least Privilege**  | Give users and jobs only the permissions they require.                                     |
| **Use Agents**                  | Run workloads on agents, preferably ephemeral container agents, not on the controller.     |
| **Standardize Pipeline Stages** | Use a common flow: Checkout → Build → Test → Quality → Security → Package → Publish.       |
| **Use Shared Libraries**        | Centralize common CI logic.                                                                |
| **Add Quality Gates**           | Stop the pipeline when mandatory quality checks fail.                                      |
| **Add Security Checks**         | Integrate SAST, dependency, and container scanning.                                        |
| **Manage Plugins**              | Maintain an approved plugin list and patch regularly.                                      |
| **Configuration as Code**       | Manage Jenkins configuration with JCasC.                                                   |
| **Backup and Recovery**         | Back up controller state and test restores.                                                |
| **Monitor Failures**            | Track failed builds, execution time, agent availability, and tool connectivity.            |
| **Protect Logs**                | Ensure credentials and sensitive variables are not exposed in pipeline logs.               |

---

# 14. Conclusion

Three CI orchestration tools were evaluated against weighted Application CI criteria. Jenkins scored highest (4.40), narrowly ahead of GitLab CI/CD (4.25), with BuildPiper lower (3.15).

Jenkins is selected for its **Pipeline as Code, distributed execution through agents, broad integrations, credential management, flexibility, and reusable components**, and because it operates independently of the source-control platform.

Its main trade-off is higher operational effort, which should be managed through the practices in Section 13 and the mitigations in Section 11.

---

# 15. Contact Information

| Name         | Email Address                                                                       |
| ------------ | ----------------------------------------------------------------------------------- |
| Sahil Butola | [sahil.butola.snaatak@mygurukulam.co](mailto:sahil.butola.snaatak@mygurukulam.co)   |

---

# 16. References

| Resource                          | Link                                                                                          |
| --------------------------------- | --------------------------------------------------------------------------------------------- |
| Jenkins Documentation             | [Jenkins Documentation](https://www.jenkins.io/doc/)                                          |
| Jenkins Pipeline Documentation    | [Jenkins Pipeline Documentation](https://www.jenkins.io/doc/book/pipeline/)                   |
| Jenkinsfile Documentation         | [Jenkinsfile Documentation](https://www.jenkins.io/doc/book/pipeline/jenkinsfile/)            |
| Jenkins Credentials Documentation | [Jenkins Credentials Documentation](https://www.jenkins.io/doc/book/using/using-credentials/) |
| Jenkins Configuration as Code     | [Jenkins Configuration as Code](https://www.jenkins.io/projects/jcasc/)                       |
| Jenkins Security Advisories       | [Jenkins Security Advisories](https://www.jenkins.io/security/advisories/)                    |
| GitLab CI/CD Documentation        | [GitLab CI/CD Documentation](https://docs.gitlab.com/ci/)                                     |
| GitLab CI/CD YAML Reference       | [GitLab CI/CD YAML Reference](https://docs.gitlab.com/ci/yaml/)                               |
| GitLab Runner Documentation       | [GitLab Runner Documentation](https://docs.gitlab.com/runner/)                                |
| BuildPiper Website                | [BuildPiper](https://buildpiper.io/)                                                          |
| BuildPiper Documentation          | [BuildPiper Documentation](https://docs.buildpiper.io/docs/)                                  |
| BuildPiper Application Pipelines  | [BuildPiper Application Pipelines](https://docs.buildpiper.io/docs/application-pipelines/)    |
| BuildPiper Global Pipelines       | [BuildPiper Global Pipelines](https://docs.buildpiper.io/docs/global-pipelines/)              |
