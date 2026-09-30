
<h1 align="left"> Using SonarQube for Monitoring Metrics</h1>

---

## Author Information

| **Author** | **Created on** | **Version** | **Last edited on** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ---------- | -------------- | ----------- | ------------------ | --------------- | --------------- | --------------- |
| Sahil      | 30-09-26       | v1.0        | 30-09-26           | Divya M. / Vishal | Aayush Verma  | Mahesh / Varun  |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What is SonarQube Monitoring?](#2-what-is-sonarqube-monitoring)
3. [Why Monitor SonarQube?](#3-why-monitor-sonarqube)
4. [Monitoring Workflow](#4-monitoring-workflow)
5. [Types of SonarQube Metrics](#5-types-of-sonarqube-metrics)
6. [Monitoring Methods](#6-monitoring-methods)
7. [Thresholds and Alerts](#7-thresholds-and-alerts)
8. [Advantages](#8-advantages)
9. [Best Practices](#9-best-practices)
10. [Conclusion](#10-conclusion)
11. [Contact Information](#11-contact-information)
12. [References](#12-references)

---

# 1. Introduction

SonarQube is a quality and security analysis platform used to continuously inspect projects for reliability, security, maintainability, test coverage, duplication, and other quality characteristics.

Monitoring SonarQube serves two different purposes:

1. **Application/Project Monitoring:** monitoring the quality of analyzed projects.
2. **SonarQube Platform Monitoring:** monitoring the health and operational state of the SonarQube instance itself.

This document defines which SonarQube metrics should be monitored, how to monitor them, and the thresholds and alerts recommended for them. It also summarizes the results of the SonarQube monitoring POC.

---

# 2. What is SonarQube Monitoring?

* SonarQube monitoring is the process of continuously observing SonarQube's project-level quality metrics and platform-level health metrics.
* It helps identify quality issues such as bugs, vulnerabilities, code smells and security hotspots.
* It tracks test and maintainability measures such as test coverage, duplication and complexity.
* It detects Quality Gate failures, which can be used to control the CI pipeline.
* It observes platform health, including the Web service, Compute Engine, Elasticsearch and Elasticsearch disk utilization.
* It monitors processing, including background analysis tasks and API performance.
* SonarQube provides Web APIs for retrieving project measures and monitoring information.
* Project metrics are retrieved through the measures API, while the monitoring metrics endpoint exposes platform monitoring metrics.

---

# 3. Why Monitor SonarQube?

| Objective | Description |
|-----------|-------------|
| Early Quality Detection | Detect quality issues early in the development cycle. |
| Security Assurance | Detect security vulnerabilities before release. |
| Coverage Tracking | Track test coverage over time. |
| Technical Debt Visibility | Identify increasing technical debt and maintainability issues. |
| Quality Gate Control | Detect Quality Gate failures and act on them in the CI pipeline. |
| Service Reliability | Detect SonarQube service failures and Compute Engine processing problems. |
| Storage Health | Detect Elasticsearch disk or index problems. |
| Processing Visibility | Monitor analysis processing and background tasks. |
| Proactive Alerting | Generate alerts when important operational conditions occur. |

---

# 4. Monitoring Workflow

<img width="1260" height="1665" alt="image" src="https://github.com/user-attachments/assets/5d5ca586-bd47-4877-a765-595ee68b1ded" />

### Workflow Summary

1. A developer pushes changes to the Git repository.
2. The CI pipeline (Jenkins) triggers SonarScanner.
3. SonarScanner analyzes the project and uploads the report to SonarQube.
4. SonarQube computes project metrics (bugs, vulnerabilities, code smells, coverage, duplication, complexity) and evaluates the Quality Gate.
5. SonarQube separately exposes platform metrics (web health, Compute Engine, Elasticsearch, pending tasks, disk usage, API performance).
6. If the Quality Gate passes, the pipeline continues. If it fails, or a platform condition becomes unhealthy, an alert or notification is generated.

---

# 5. Types of SonarQube Metrics

## 5.1 Code Quality Metrics

These metrics describe the quality of the projects analyzed by SonarQube.

| Metric | Purpose |
|--------|---------|
| Bugs | Detect reliability-related issues |
| Vulnerabilities | Identify security vulnerabilities |
| Code Smells | Identify maintainability issues |
| Security Hotspots | Identify areas requiring security review |
| Coverage | Measure how much is covered by tests |
| Line Coverage | Percentage of executable lines covered by tests |
| Branch Coverage | Percentage of branches covered by tests |
| Duplicated Lines Density | Percentage of duplicated lines |
| Cognitive Complexity | Measures how difficult the project is to understand |
| Cyclomatic Complexity | Measures decision complexity |
| Technical Debt | Estimates remediation effort |
| NCLOC | Number of non-comment lines analyzed |

---

## 5.2 Quality Gate Metrics

A Quality Gate is a collection of conditions used to determine whether an analyzed project meets the configured quality requirements. It can be used as a CI/CD control point, and its status can be reported back to the CI pipeline.

| Condition | Description |
|---|---|
| New Bugs | Bugs introduced in new work |
| New Vulnerabilities | Vulnerabilities introduced in new work |
| New Code Smells | Maintainability issues introduced in new work |
| Security Hotspot Review | Share of new hotspots that have been reviewed |
| New-Code Coverage | Test coverage on new work |
| New-Code Duplication | Duplication on new work |
| Ratings | Reliability, security and maintainability ratings |

---

## 5.3 Platform Health Metrics

These metrics monitor the SonarQube server itself.

| Metric | Purpose |
|---|---|
| Web Status | Determines whether the SonarQube Web service is healthy |
| Compute Engine Status | Determines whether background processing is healthy |
| Elasticsearch Status | Determines whether Elasticsearch is healthy |
| Pending Tasks | Shows analysis tasks waiting for processing |
| Elasticsearch Disk Usage | Monitors Elasticsearch storage utilization |
| Read-only Indices | Detects Elasticsearch indices becoming read-only |
| API Request Duration | Monitors API response/processing time |
| Web Uptime | Tracks SonarQube Web service uptime |
| Connected SonarLint Clients | Shows connected clients where applicable |

---

# 6. Monitoring Methods

## 6.1 SonarQube Web UI

The SonarQube dashboard provides project-level visibility into bugs, vulnerabilities, code smells, coverage, duplication, complexity and Quality Gate status. It is useful for developers and reviewers who need to inspect individual projects.

<!-- Screenshot placeholder: SonarQube project dashboard -->

---

## 6.2 SonarQube Web API

The Web API retrieves SonarQube information programmatically and is the method used for automated monitoring.

| Purpose | API | What it returns |
|---|---|---|
| Project metrics | Measures API (component measures) | Bugs, vulnerabilities, code smells, coverage, line coverage, branch coverage, duplicated lines density, cognitive complexity, NCLOC |
| Quality Gate status | Quality Gates API (project status) | Quality Gate status and its conditions for a project |
| System health | System API (health) | Overall health (GREEN / YELLOW / RED) and any causes |
| Monitoring metrics | Monitoring API (metrics) | Platform metrics in Prometheus-compatible format |

The monitoring endpoint exposes metrics in Prometheus-compatible exposition format. Current SonarQube documentation shows authentication for this endpoint using an appropriate SonarQube passcode.

<img width="2850" height="1670" alt="image" src="https://github.com/user-attachments/assets/9a2b1680-bc1f-4ea7-af69-99b7a0004182" />

---

## 6.3 POC Validation

A sample Java project named **sonarqube-demo** was analyzed with SonarScanner on SonarQube Community Build 26.9.0.129388 (Ubuntu, 2 vCPU, ~7.6 GiB RAM). The values below were retrieved directly from the SonarQube APIs after analysis.

### Project Metrics

| Metric | POC Value | Observation |
|---|---:|---|
| Bugs | 0 | No bugs detected |
| Vulnerabilities | 0 | No vulnerabilities detected |
| Code Smells | 5 | Five maintainability issues detected |
| Coverage | 0% | No test/coverage report was provided |
| Duplicated Lines Density | 0% | No duplication detected |
| Cognitive Complexity | 1 | Low complexity for the sample project |
| NCLOC | 11 | Eleven non-comment lines analyzed |

### Quality Gate and System Health

| Check | Result |
|---|---|
| Quality Gate status | OK (no individual conditions returned for this POC project) |
| System health | GREEN, no causes reported |

### Platform Monitoring Metrics

| Monitoring Metric | POC Value | Interpretation |
|---|---:|---|
| Web Status | 1 | Web service healthy |
| Compute Engine Status | 1 | Compute Engine healthy |
| Elasticsearch Status | 1 | Elasticsearch healthy |
| Pending Tasks | 0 | No pending analysis tasks |
| Elasticsearch Disk Usage | ~12.1% | Low disk utilization |
| Elasticsearch Read-only Indices | 0 | No read-only indices |
| Web Uptime | ~11 minutes at measurement time | Service running |
| ES Free / Total Disk Space | ~44.7 GB / ~50.9 GB | Ample storage available |


---

# 7. Thresholds and Alerts

Thresholds are divided into two categories: quality thresholds (analyzed projects) and operational thresholds (SonarQube platform).

## 7.1 Quality Thresholds

Quality thresholds should be implemented through SonarQube Quality Gates. The exact values should come from the organization's approved Quality Gate configuration rather than arbitrary per-metric thresholds.

| Metric | Example Policy |
|---|---|
| New Bugs | No new bugs |
| New Vulnerabilities | No new vulnerabilities |
| Security Hotspots | New hotspots reviewed |
| New-Code Coverage | Organization-defined minimum |
| New-Code Duplication | Organization-defined maximum |
| New Issues | Organization-defined maximum |

SonarQube's built-in Sonar way Quality Gate focuses on the quality of new work and includes conditions covering new issues, security hotspots, coverage and duplication.

---

## 7.2 Operational Monitoring Thresholds

The following are **recommended starting thresholds for operational monitoring**, not SonarQube default thresholds.

| Metric | Recommended Starting Alert | Severity |
|---|---|---|
| System Health | RED | Critical |
| Web Status | 0 | Critical |
| Compute Engine Status | 0 | Critical |
| Elasticsearch Status | 0 | Critical |
| Pending Tasks | Sustained high queue | Warning/Critical |
| Elasticsearch Disk Usage | >80% Warning, >90% Critical | Warning/Critical |
| Read-only Indices | Greater than 0 | Critical |
| API Request Duration | Sustained increase above established baseline | Warning |
| Web Uptime | Unexpected reset | Warning |

Thresholds should be tuned according to actual SonarQube workload, project count, analysis frequency and infrastructure capacity.

---

## 7.3 Example Alerts

An alert should be generated when an important condition persists.

| Alert | Condition | Action |
|---|---|---|
| SonarQube Down | System health is RED | Generate Critical alert |
| Elasticsearch Failure | Elasticsearch status is 0 | Generate Critical alert |
| High Disk Usage | Elasticsearch disk usage above 80% | Generate Warning alert |
| Read-only Elasticsearch Index | Read-only indices greater than 0 | Generate Critical alert |
| Quality Gate Failure | Project Quality Gate is ERROR | Fail/block the CI quality-control stage according to pipeline policy |

> **Note:** The POC validated the APIs directly. Integrating the monitoring endpoint with Prometheus and Alertmanager (email/Slack notifications) is a proposed production approach and was **not implemented in this POC**.

---

# 8. Advantages

* Early detection of application quality issues.
* Continuous visibility into security-related issues.
* Improved maintainability.
* Visibility into test coverage.
* Detection of duplication.
* Early detection of SonarQube service failures.
* Monitoring of background analysis processing.
* Detection of Elasticsearch storage problems.
* Supports CI/CD quality controls.
* Enables automated alerting and operational monitoring.

---

# 9. Best Practices

1. **Monitor both project quality and SonarQube infrastructure health.**
2. Use Quality Gates for application-quality policies.
3. Use operational monitoring thresholds for SonarQube infrastructure.
4. Prefer monitoring **new work** for continuous quality improvement.
5. Tune operational thresholds based on historical workload and baseline.
6. Avoid creating alerts for every minor quality change.
7. Use severity levels such as Warning and Critical.
8. Integrate Quality Gate status with the CI pipeline where required.
9. Review alerts regularly and remove noisy or unnecessary alerts.
10. Keep monitoring credentials and tokens secure, use dedicated monitoring credentials with minimum permissions, and store them in a secrets manager or CI/CD credential store.
11. Do not use the default administrator password, and rotate any exposed token immediately.
12. Do not expose SonarQube (port 9000) or its monitoring endpoint publicly without network controls, and use HTTPS in production.

---

# 10. Conclusion

The POC showed that SonarQube exposes both project quality metrics and platform health metrics that can be monitored through its Web API and monitoring endpoint. Project quality should be enforced through Quality Gates, while platform health should be tracked with tuned Warning and Critical thresholds. Together, these help catch quality issues and SonarQube failures early and keep the CI pipeline reliable. As a next step, the monitoring endpoint can be integrated with Prometheus and an alerting system for production use.

---

# 11. Contact Information

| Auntor | email |
|---|---|
| Sahil | [sahil.butola.snaatak@mygurukulam.co](mailto:sahil.butola.snaatak@mygurukulam.co) |


---

# 12. References

| Topic | Description |
|---|---|
| [SonarQube Server – Web API](https://docs.sonarsource.com/sonarqube-server/extension-guide/web-api) | Web API reference and authentication |
| [SonarQube Server – Understanding Quality Gates](https://docs.sonarsource.com/sonarqube-server/2026.1/quality-standards-administration/managing-quality-gates/introduction-to-quality-gates) | Quality Gate concepts and conditions |
| [SonarQube Community Build – Understanding Quality Gates](https://docs.sonarsource.com/sonarqube-community-build/quality-standards-administration/managing-quality-gates/introduction-to-quality-gates) | Quality Gates for the Community Build |
| [SonarQube Server – Monitoring Metrics through Web API](https://docs.sonarsource.com/sonarqube-server/10.6/user-guide/code-metrics/monitoring-metrics-through-web-api) | Monitoring metrics endpoint guide |
