<img width="1920" height="600" alt="image" src="https://github.com/user-attachments/assets/6254e48a-adf9-43bf-9e7f-9b3d4b0a36e6" />

# Commit Sign-off POC

## Author Table

| Author       | Created On | Version | Last Edited On | L0 Reviewer       | L1 Reviewer  | L2 Reviewer          |
| ------------ | ---------- | ------- | -------------- | ----------------- | ------------ | -------------------- |
| Sahil Butola | 02-10-2026 | 1.0     | 02-10-2026     | Divya M. / Vishal | Aayush Verma | Mahesh Kumar / Varun |

## Table of Contents

1. [Objective](#1-objective)
2. [Prerequisites](#2-prerequisites)
3. [POC Implementation](#3-poc-implementation)
4. [Verification](#4-verification)
5. [Validation](#5-validation)
6. [Result](#6-result)
7. [Conclusion](#7-conclusion)
8. [Contact Information](#8-contact-information)
9. [References](#9-refrences)



---

## 1. Objective
 
To verify that Git can create a commit with a `Signed-off-by:` trailer using the `git commit -s` option and that the sign-off can be verified from commit history.

---

## 2. Prerequisites
 
* Ubuntu/Linux system
* Git installed
* Git username and email configured
Verify:
 
```bash
git --version
git config --global user.name
git config --global user.email
```
 
**POC Environment:**
 
* Git version: `2.43.0`
* Username: `sahil`
* Email: `sahil@example.com`
  
<details>
<summary> Screenshot: Prerequisites verification</summary>
<img width="1060" height="278" alt="image" src="https://github.com/user-attachments/assets/99244d75-8a91-4cd1-a88f-7057e12622eb" />
</details>

___


## 3. POC Implementation
 
### Step 1: Create Repository
 
```bash
mkdir ~/commit-signoff-poc
cd ~/commit-signoff-poc
git init
```
 
Verify:
 
```bash
git status
```
 

<details>
<summary> Screenshot: Repository creation and status</summary> 
<img width="1156" height="278" alt="image" src="https://github.com/user-attachments/assets/1d2a36a8-a3a3-4530-bc8c-e2e9d7f1552d" />
</details>

### Step 2: Create Test File
 
```bash
echo "Commit sign-off POC" > signoff.txt
git add signoff.txt
git status
```
 
<details>
<summary> Screenshot: Test file created and staged</summary>
<img width="1632" height="448" alt="image" src="https://github.com/user-attachments/assets/975a5836-203a-4659-9a7c-13f0458d0709" /> 
</details>


### Step 3: Create Signed-off Commit
 
```bash
git commit -s -m "Add commit sign-off POC"
```
 
The `-s` option adds the `Signed-off-by:` trailer automatically.
 
<details>
<summary> Screenshot: Signed-off commit created</summary>
<img width="1632" height="448" alt="image" src="https://github.com/user-attachments/assets/ddfaba86-fdcf-4e87-9e2e-bf9502c56dff" />
</details>


## 4. Verification
 
Run:
 
```bash
git show -s --format=%B HEAD
```
 
This confirms that the commit contains the required sign-off information.
 
<details>
<summary> Screenshot: Sign-off verification</summary>
<img width="1632" height="448" alt="image" src="https://github.com/user-attachments/assets/f7abd428-fa59-4149-a33a-372b35daf6f9" />
</details>

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

## 7. Conclusion

Commit sign-off provides a standardized and traceable way to record contributor acknowledgement in Git commits. Integrating sign-off validation into CI can ensure that required commits satisfy the project's contribution policy before continuing the applicable CI workflow.

---

## 8. Contact Information

| Name         | Email                                                                             |
| ------------ | --------------------------------------------------------------------------------- |
| Sahil Butola | [sahil.butola.snaatak@mygurukulam.co](mailto:sahil.butola.snaatak@mygurukulam.co) |

---

## 9. References

| Resource | Links    |
|----------|----------|
|  Git Documentation | [Git Documentation](https://git-scm.com/docs/git-commit) | 
|  Git Commit Signing Options | [Git Commit Signing Options](https://git-scm.com/docs/git-commit#Documentation/git-commit.txt---signoff) | 
