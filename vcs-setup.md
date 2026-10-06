<img width="800" height="300" alt="image" src="https://github.com/user-attachments/assets/eda342a2-a571-4a4b-9ba8-ca6c6f43a171" />

# Setup Notification for Code Events in Slack

## Author Table

| Author       | Created On  | Version | Last Edited On | L0 Reviewer | L1 Reviewer | L2 Reviewer |
| ------------ | ----------- | ------- | -------------- | ----------- | ----------- | ----------- |
| Sahil Butola | 07-Oct-2026 | 1.0     | 07-Oct-2026    |  Divya M. / Vishal | Aayush Verma | Mahesh / Varun  |

---

## Table of Contents

1. [Objective](#1-objective)
2. [Acceptance Criteria](#2-acceptance-criteria)
3. [Notification Architecture](#3-notification-architecture)
4. [Slack Configuration](#4-slack-configuration)
5. [Branch Notification Test](#5-branch-notification-test)
6. [Pull Request Test](#6-pull-request-test)
7. [PR Comment / Update](#7-pr-comment--update)
8. [Commit Notifications](#8-commit-notifications)
9. [Branch Deletion Test](#9-branch-deletion-test)
10. [Test Matrix](#10-test-matrix)
11. [Best Practices](#11-best-practices)
12. [Contact Information](#12-contact-information)
13. [Conclusion](#13-conclusion)
    
---

## 1. Objective

Configure notifications for important GitHub VCS events and deliver them to Slack.

* **VCS:** GitHub
* **Organization:** `Final-fight`
* **Repository:** `Final-fight/github-notifications`
* **Notification Platform:** Slack

---

## 2. Acceptance Criteria

1. Notifications for PR creation, update, and comments.
2. Notifications for commits pushed to `main` and `develop`.
3. Notifications for branch creation and deletion.

---

## 3. Notification Architecture

<img width="1266" height="898" alt="image" src="https://github.com/user-attachments/assets/826af0be-d746-4303-9008-4963ffe054f7" />


<details>
<summary> Screenshot: GitHub repository showing Final-fight/github-notifications</summary>
<img width="2858" height="1148" alt="image" src="https://github.com/user-attachments/assets/72c8006c-6b0d-40c4-b8e7-5d9bcc2d1bea" />
</details>

---

## 4. Slack Configuration

Repository subscription:

```text
/github subscribe Final-fight/github-notifications
```

Additional event subscriptions:

```text
/github subscribe Final-fight/github-notifications branches
/github subscribe Final-fight/github-notifications comments
/github subscribe Final-fight/github-notifications commits:main
/github subscribe Final-fight/github-notifications commits:develop
```

Configured events:

| Event                | Configuration     |
| -------------------- | ----------------- |
| PR                   | `pulls`           |
| PR Comments          | `comments`        |
| Branch Create/Delete | `branches`        |
| Commit → main        | `commits:main`    |
| Commit → develop     | `commits:develop` |

<details>
<summary> Screenshot:Slack GitHub subscription confirmation</summary>
<img width="1274" height="327" alt="Screenshot 2026-10-07 at 3 16 21 AM" src="https://github.com/user-attachments/assets/ea103d73-079f-498e-84d1-0ecc50e09ba4" />
</details>

---

## 5. Branch Notification Test

A fresh test branch was created from `main`:

```bash
git checkout main
git pull origin main
git checkout -b ticket1-branch-test
git push -u origin ticket1-branch-test
```

<details>
<summary> Screenshot:GitHub showing `ticket1-branch-test</summary>
<img width="2858" height="650" alt="image" src="https://github.com/user-attachments/assets/40740ecd-3a0f-453a-bbef-b427bbaafd2c" />
</details>

<details>
<summary> Screenshot:Slack branch-created notification</summary>
<img width="1429" height="325" alt="Screenshot 2026-10-07 at 2 53 13 AM" src="https://github.com/user-attachments/assets/d6f02005-0974-4e06-89a0-f48960ce0782" />
</details>

---

## 6. Pull Request Test

A team member raised a Pull Request against `main`.

The `main` branch requires one approving review with write access.

GitHub displayed:

```text
Review required with write access
1 approving review required
```

<details>
<summary> Screenshot:Pull Request showing review required</summary>
<img width="1429" height="722" alt="Screenshot 2026-10-07 at 2 56 03 AM" src="https://github.com/user-attachments/assets/149958b2-7857-4d4c-ba33-02a4e7159b22" />
</details>


<details>
<summary> Screenshot:Slack PR notification</summary>
<img width="1429" height="712" alt="Screenshot 2026-10-07 at 2 59 12 AM" src="https://github.com/user-attachments/assets/168dd947-3474-48d3-9b9a-3ba79ed05c4d" />
</details>


---

## 7. PR Comment / Update

PR comment notifications are enabled through the Slack `comments` subscription.

For PR updates, a new commit pushed to an existing PR represents a PR `synchronize` event.

```yaml
on:
  pull_request:
    types: [synchronize]
```

<details>
<summary> Screenshot:PR comment and/or update notification in Slack</summary>
<img width="771" height="712" alt="Screenshot 2026-10-07 at 3 01 29 AM" src="https://github.com/user-attachments/assets/9008a483-e606-4d84-ab0b-10702a5608c3" />
</details>


---

## 8. Commit Notifications

Commit notifications are configured for the key branches:

```text
main
develop
```

A commit pushed to either branch should generate a Slack notification.

<details>
<summary> Screenshot:Commit pushed to main/develop</summary>
<img width="1119" height="712" alt="Screenshot 2026-10-07 at 3 04 28 AM" src="https://github.com/user-attachments/assets/08f1aeef-7e7f-4e43-b84c-66374cc37248" />
</details>


<details>
<summary> Screenshot:Slack commit notification </summary>
<img width="1416" height="712" alt="Screenshot 2026-10-07 at 3 05 08 AM" src="https://github.com/user-attachments/assets/35691df8-e5a4-491a-82dc-bcb6bc9cc9e0" />
</details>


---

## 9. Branch Deletion Test

After branch testing, the test branch can be deleted:

```bash
git push origin --delete ticket1-branch-test
```

This validates the branch deletion notification.

<details>
<summary> Screenshot:Branch deleted from GitHub </summary>
<img width="2832" height="752" alt="image" src="https://github.com/user-attachments/assets/694e3535-2086-474e-bf52-dd665cab8cc8" />
</details>


<details>
<summary> Screenshot:Slack branch-deletion notification </summary>
<img width="1085" height="322" alt="Screenshot 2026-10-07 at 3 08 17 AM" src="https://github.com/user-attachments/assets/74663fdc-420a-4dc3-ab35-4c1cbb0097c6" />
</details>


---

## 10. Test Matrix

| VCS Event          | Expected Notification | Status               |
| ------------------ | --------------------- | -------------------- |
| PR Created         | Slack                 | Configured / Testing |
| PR Updated         | Slack                 | Configured / Testing |
| PR Commented       | Slack                 | Configured           |
| Commit → `main`    | Slack                 | Configured           |
| Commit → `develop` | Slack                 | Configured           |
| Branch Created     | Slack                 | Tested               |
| Branch Deleted     | Slack                 | Configured / Testing |

---

## 11. Best Practices

* Use dedicated Slack channels for VCS notifications.
* Monitor only important branches such as `main` and `develop`.
* Avoid unnecessary notification noise.
* Protect important branches using PR reviews.
* Use GitHub Actions for events not fully covered by the standard Slack integration.
* Never expose webhook URLs or credentials in source code.

---

## 12. Contact Information

| Name | Email Address |
|------|---------------|
| Sahil Butola | [sahil.butola.snaatak@mygurukulam.co](mailto:sahil.butola.snaatak@mygurukulam.co) |

---

## 13. Conclusion

GitHub VCS notifications were configured for `Final-fight/github-notifications`.

The setup covers Pull Requests, comments, commits on `main`/`develop`, and branch creation/deletion.


