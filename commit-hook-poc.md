# Commit Sign-off POC

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
