<img width="1920" height="600" alt="image" src="https://github.com/user-attachments/assets/6254e48a-adf9-43bf-9e7f-9b3d4b0a36e6" />

# Application CI Design | Generic CI Operation | Commit Sign-off

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

## 1. Objective

Verify that a Git commit can be created with a `Signed-off-by:` trailer and that the sign-off can be validated from the commit history.

---

## 2. Prerequisites

* Git installed.
* Access to a Git repository.
* Git user name and email configured.

```bash
git --version
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

---

## 3. Create Signed-off Commit

```bash
git clone <repository-url>
cd <repository>
git checkout -b commit-signoff-poc

echo "Commit sign-off POC" > signoff.txt
git add signoff.txt
git commit -s -m "Add commit sign-off POC"
```

The `-s` option adds the `Signed-off-by:` trailer automatically.

---

## 4. Verify Sign-off

Display the latest commit:

```bash
git show -s --format=%B HEAD
```

Expected output:

```text
Add commit sign-off POC

Signed-off-by: Your Name <your-email@example.com>
```

The sign-off can also be checked using:

```bash
git log -1 --format=%B
```

---

## 5. Validation

| Check                            | Expected Result          |
| -------------------------------- | ------------------------ |
| Git installed                    | Version displayed        |
| User identity configured         | Name and email available |
| Commit created with `-s`         | Successful               |
| `Signed-off-by:` present         | Yes                      |
| Commit history contains sign-off | Verified                 |

---

## 6. Result

The POC successfully demonstrates that `git commit -s` adds a `Signed-off-by:` trailer to the commit and that the trailer can be verified from Git history.

___

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
