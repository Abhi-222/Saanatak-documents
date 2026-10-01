<img width="1920" height="600" alt="image" src="https://github.com/user-attachments/assets/6254e48a-adf9-43bf-9e7f-9b3d4b0a36e6" />

# Commit Sign-off Documentation

## Author Table

| Author       | Created On | Version | Last Edited On | L0 Reviewer       | L1 Reviewer  | L2 Reviewer          |
| ------------ | ---------- | ------- | -------------- | ----------------- | ------------ | -------------------- |
| Sahil Butola | 02-10-2026 | 1.0     | 02-10-2026     | Divya M. / Vishal | Aayush Verma | Mahesh Kumar / Varun |

## Table of Contents

1. [Introduction](#1-introduction)
2. [What is Commit Sign-off](#2-what-is-commit-sign-off)
3. [Why Commit Sign-off is Required](#3-why-commit-sign-off-is-required)
4. [Commit Sign-off Workflow](#4-commit-sign-off-workflow)
5. [Advantages](#5-advantages)
6. [POC](#6-poc)
7. [Best Practices](#7-best-practices)
8. [Conclusion](#8-conclusion)
9. [Contact Information](#9-contact-information)
10. [References](#10-references)

---

## 1. Introduction

Commit sign-off is a Git mechanism used to record that a contributor agrees to the project's contribution or sign-off requirements. The sign-off is added to a commit as a `Signed-off-by:` line and can be verified during the CI workflow.

---

## 2. What is Commit Sign-off

A commit sign-off is created using:

```bash
git commit -s -m "Add application changes"
```

Git adds a trailer similar to:

```text
Signed-off-by: Contributor Name <email@example.com>
```

The sign-off records the contributor identity and their acknowledgement of the applicable project requirements.

---

## 3. Why Commit Sign-off is Required

* Establishes contributor acknowledgement.
* Provides a traceable sign-off record in Git history.
* Supports projects that require signed-off commits.
* Allows CI to validate commit requirements automatically.
* Helps maintain consistent contribution practices.

---

## 4. Commit Sign-off Workflow

```text
Developer
    |
    v
Create / Modify Code
    |
    v
git commit -s
    |
    v
Signed-off-by Added
    |
    v
Push Commit
    |
    v
CI Validation
    |
    +---- Valid ----> Continue CI
    |
    +---- Missing --> Fail / Report
```

The CI validation checks whether the required `Signed-off-by:` trailer is present before continuing the applicable workflow.

---

## 5. Advantages

* **Traceability:** Sign-off is stored with the commit.
* **Automation:** CI can validate sign-off requirements.
* **Consistency:** Applies a standard contribution process.
* **Auditability:** Commit history contains the sign-off record.
* **Early Validation:** Missing sign-offs can be detected during CI.

---

## 6. POC

### Create a Signed-off Commit

```bash
git clone <repository-url>
cd <repository>
git checkout -b signoff-poc
echo "Commit sign-off POC" > signoff.txt
git add signoff.txt
git commit -s -m "Add commit sign-off POC"
git log -1
```

Verify that the latest commit contains:

```text
Signed-off-by: <name> <email>
```

### Validate the Sign-off

```bash
git show -s --format=%B HEAD
```

The output should contain the `Signed-off-by:` trailer.

---

## 7. Best Practices

* **Use `-s`** – Create sign-offs using Git's `--signoff` option.
* **Verify Identity** – Configure the correct Git user name and email.
* **CI Validation** – Validate required sign-offs automatically where applicable.
* **Do Not Fabricate** – Contributors should sign off their own commits according to project policy.
* **Document Policy** – Clearly define when sign-off is mandatory.
* **Preserve History** – Avoid removing sign-off information from required commits.

---

## 8. Conclusion

Commit sign-off provides a standardized and traceable way to record contributor acknowledgement in Git commits. Integrating sign-off validation into CI can ensure that required commits satisfy the project's contribution policy before continuing the applicable CI workflow.

---

## 9. Contact Information

| Name         | Email                                                                             |
| ------------ | --------------------------------------------------------------------------------- |
| Sahil Butola | [sahil.butola.snaatak@mygurukulam.co](mailto:sahil.butola.snaatak@mygurukulam.co) |

---

## 10. References

| Resource | Links    |
|----------|----------|
|  Git Documentation | [Git Documentation](https://git-scm.com/docs/git-commit) | 
|  Git Commit Signing Options | [Git Commit Signing Options](https://git-scm.com/docs/git-commit#Documentation/git-commit.txt---signoff) | 
|  Git Trailers | [Git Trailers](https://git-scm.com/docs/git-interpret-trailers) |


