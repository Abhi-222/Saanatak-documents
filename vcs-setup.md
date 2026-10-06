# VCS Implementation | Setup Notification for Code Events

## 1. Author Table

| Author       | Created On  | Version | Last Edited On | L0 Reviewer | L1 Reviewer | L2 Reviewer |
| ------------ | ----------- | ------- | -------------- | ----------- | ----------- | ----------- |
| Sahil Butola | 07-Oct-2026 | 1.0     | 07-Oct-2026    |  Divya M. / Vishal | Aayush Verma | Mahesh / Varun Kumar |

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

</details>

<details>
<summary> Screenshot:Slack branch-created notification</summary>

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

</details>


<details>
<summary> Screenshot:Slack PR notification</summary>

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

</details>


<details>
<summary> Screenshot:Slack commit notification </summary>

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

</details>


<details>
<summary> Screenshot:Slack branch-deletion notification </summary>

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

Event-by-event screenshots should be attached as evidence before marking the acceptance criteria completely **PASS**.

