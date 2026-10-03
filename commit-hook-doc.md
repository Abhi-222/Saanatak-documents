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

<img width="979" height="1441" alt="signoff-workflow" src="https://github.com/user-attachments/assets/940dfea3-564f-4f0f-8ee3-4a082c2aadf7" />

### Workflow Explanation

1. **Developer** – The contributor starts the change in their local clone of the repository.
2. **Create / Modify Code** – The developer creates or edits files and stages them with `git add`.
3. **git commit -s** – The developer commits using the `-s` (`--signoff`) option.
4. **Signed-off-by Added** – Git adds a `Signed-off-by: Name <email>` line to the commit message, using the configured Git user name and email.
5. **Push Commit** – The developer pushes the signed-off commit to the remote repository, which triggers the CI workflow.
6. **CI Validation** – CI checks the commit message for the required `Signed-off-by:` trailer. This is the decision point of the workflow.
7. **Valid → Continue CI** – If the trailer is present, the check passes and the remaining CI stages continue.
8. **Missing → Fail / Report** – If the trailer is missing, the check fails and the missing sign-off is reported. The developer then needs to fix the commit (for example `git commit --amend -s`) and push again.
   

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

