<img width="800" height="184" alt="image" src="https://github.com/user-attachments/assets/a960a0ea-eb5e-42a1-9686-13347f29c653" />

<h1 align="left"> Using SonarQube for Monitoring Metrics</h1>

---
## Author Information

| **Author** | **Created on** | **Version** | **Last edited on** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ---------- | -------------- | ----------- | ------------------ | --------------- | --------------- | --------------- |
| Sahil      | 30-09-26       | v1.0        | 01-09-26           | Divya M. / Vishal | Aayush Verma  | Mahesh / Varun  |

## Table of Contents

1. [Introduction](#1-introduction)
2. [What is SonarQube?](#2-what-is-sonarqube)
3. [Why SonarQube?](#3-why-sonarqube)
4. [SonarQube Workflow](#4-sonarqube-workflow)
5. [Types of Metrics](#5-types-of-metrics)
6. [Advantages](#6-advantages)
7. [Best Practices](#7-best-practices)
8. [Conclusion](#8-conclusion)
9. [Contact Information](#9-contact-information)
10. [References](#10-references)

---

## 1. Introduction

SonarQube is a platform used to analyze source code for quality, reliability, maintainability, and security-related issues. It provides measurable metrics that help developers understand the overall quality of a codebase.

> **Note:** SonarQube monitoring metrics refer to source-code quality metrics, not CPU, memory, network, or application runtime monitoring.

---

## 2. What is SonarQube?

SonarQube analyzes source code using predefined rules and generates a detailed analysis report.

### Main Components

* **SonarQube Server** – Processes, stores, and displays analysis results.
* **SonarScanner** – Analyzes source code and sends the analysis results to SonarQube.
* **Quality Profile** – Defines the coding rules used during analysis.
* **Quality Gate** – Defines conditions used to determine whether the code meets the required quality standard.
* **Project** – Represents the application or codebase being analyzed.

---

## 3. Why SonarQube?

* Identifies bugs and potential security vulnerabilities.
* Detects code smells and maintainability issues.
* Measures test coverage and code duplication.
* Provides consistent coding standards.
* Helps developers understand code-quality problems.
* Provides measurable quality metrics.
* Allows quality trends to be monitored over time.
* Provides Quality Gates for automated quality evaluation.

---

## 4. SonarQube Workflow

<img width="918" height="1339" alt="image" src="https://github.com/user-attachments/assets/f5eae8e2-9b40-42df-9505-5087110a0843" />


### Workflow Explanation

1. **Source Code** – The application source code is provided for analysis.
2. **SonarScanner** – Scans the source code using the configured analysis settings.
3. **SonarQube Server** – Receives and processes the analysis results.
4. **Source Code Analysis** – SonarQube evaluates the code against configured rules.
5. **Metrics Generation** – SonarQube generates metrics such as bugs, vulnerabilities, code smells, coverage, duplication, and complexity.
6. **Quality Gate** – The collected results are evaluated against configured quality conditions.
7. **PASS/FAIL** – The Quality Gate indicates whether the code meets the defined quality requirements.

---

## 5. Types of Metrics

SonarQube groups its metrics by the aspect of code quality they measure. Each category below lists what the metric means and what to watch for when monitoring it.

### Reliability

* **Bugs** – Issues that can cause incorrect or unexpected application behavior.
* **Reliability Rating** – A rating from A to E based on the severity of the bugs found.
* **What to monitor** – Number of bugs, especially new bugs introduced by recent changes.

### Security

* **Vulnerabilities** – Security weaknesses identified in the source code.
* **Security Hotspots** – Security-sensitive code that must be manually reviewed to decide whether it is safe.
* **Security Rating** – A rating from A to E based on the severity of the vulnerabilities found.
* **What to monitor** – Number of open vulnerabilities and the percentage of security hotspots reviewed.

### Maintainability

* **Code Smells** – Code patterns that may make the code difficult to maintain or understand.
* **Technical Debt** – Estimated time needed to fix all the maintainability issues in the project.
* **Maintainability Rating** – A rating from A to E based on the ratio of technical debt to the size of the code.
* **What to monitor** – Number of code smells and the growth of technical debt over time.

### Coverage

* **Coverage** – Indicates how much of the code is executed by automated tests.
* **What to monitor** – Overall coverage and coverage on new code, so new changes are always tested.

### Duplication

* **Duplicated Lines** – Measures repeated code within the project.
* **Duplicated Lines (%)** – The share of duplicated lines compared to the total lines of code.
* **What to monitor** – Duplication on new code, to stop repeated code from spreading.

### Size

* **Lines of Code (LOC)** – Measures the amount of source code in the project.
* **What to monitor** – Growth of the codebase, which helps put other metrics in context.

### Complexity

* **Cyclomatic Complexity** – Measures the number of independent paths through the code.
* **Cognitive Complexity** – Measures how difficult the code is for a person to read and understand.
* **What to monitor** – Files and functions with unusually high complexity, which are harder to test and maintain.

---

## 6. Advantages

* **Early Issue Detection** – Identifies code-quality and security issues early.
* **Improved Code Quality** – Helps developers identify maintainability problems.
* **Security Analysis** – Detects potential security vulnerabilities.
* **Automated Analysis** – Provides repeatable source-code analysis.
* **Quality Measurement** – Converts code quality into measurable metrics.
* **Duplication Detection** – Identifies repeated code.
* **Quality Tracking** – Allows quality metrics to be monitored over time.
* **Consistent Standards** – Quality Profiles provide standardized coding rules.

---

## 7. Best Practices

* **Use Appropriate Quality Profiles** – Apply relevant coding rules for the project and programming language.
* **Use Quality Gates** – Define measurable conditions for acceptable code quality.
* **Focus on New Code** – Prevent new changes from introducing additional quality issues.
* **Set Realistic Coverage Thresholds** – Define practical test-coverage requirements.
* **Review Security Issues** – Regularly review and address identified vulnerabilities.
* **Protect Credentials** – Keep SonarQube authentication tokens and credentials secure.
* **Avoid Unnecessary Exclusions** – Do not exclude important application code from analysis.
* **Review Metrics Regularly** – Monitor bugs, vulnerabilities, code smells, coverage, and duplication.
* **Use SonarQube with Testing** – SonarQube analysis should complement automated testing rather than replace it.

---

## 8. Conclusion

SonarQube provides automated source-code analysis and measurable quality metrics. It helps developers identify bugs, vulnerabilities, code smells, duplication, coverage, and complexity issues. Quality Profiles and Quality Gates provide consistent and measurable standards for evaluating source-code quality.

---

## 9. Contact Information

| Auntor | email |
|--------|-------|
| Sahil | [sahil.butola.snaatak@mygurukulam.co](mailto:sahil.butola.snaatak@mygurukulam.co) |

---

## 10. References

| Topic | Description |
|-------|-------------|
| [SonarQube Documentation](https://docs.sonarsource.com/sonarqube/) | SonarQube Documentation |
| [SonarQube Metrics](https://docs.sonarsource.com/sonarqube/latest/user-guide/code-metrics/metrics-definition/) | SonarQube Metrics |
| [SonarQube Quality Gates](https://docs.sonarsource.com/sonarqube/latest/user-guide/quality-gates/) | SonarQube Quality Gates |
| [SonarQube Quality Profiles](https://docs.sonarsource.com/sonarqube/latest/instance-administration/quality-profiles/) | SonarQube Quality Profiles |
| [SonarScanner Documentation](https://docs.sonarsource.com/sonarqube/latest/analyzing-source-code/scanners/)| SonarScanner Documentation |

