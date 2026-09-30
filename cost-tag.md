<img width="2000" height="900" alt="image" src="https://github.com/user-attachments/assets/7347c555-a9f3-4656-84e6-15773f0dcb20" />

# Cost Optimization | Implementation | Cost Tag Reports via AWS Cost Explorer

---

## Author Table

| **Author** | **Created on** | **Version** | **Last edited on** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ---------- | -------------- | ----------- | ------------------ | --------------- | --------------- | --------------- |
| Sahil | 30-09-26 | 1.0 | 30-09-26 | Vishal/Divya M | Aayush Verma | Mahesh Kumar / Varun |

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Prerequisites](#2-prerequisites)
3. [Implementation Workflow](#3-implementation-workflow)
4. [Step-by-Step Implementation](#4-step-by-step-implementation)
   - [4.1 Select an AWS Resource](#41-select-an-aws-resource)
   - [4.2 Apply Cost Allocation Tags](#42-apply-cost-allocation-tags)
   - [4.3 Activate Cost Allocation Tags](#43-activate-cost-allocation-tags)
   - [4.4 Configure the Cost Tag Report](#44-configure-the-cost-tag-report)
   - [4.5 Apply Filters](#45-apply-filters)
   - [4.6 Save and Export the Report](#46-save-and-export-the-report)
5. [Validation and Evidence](#5-validation-and-evidence)
6. [Troubleshooting](#6-troubleshooting)
7. [Conclusion](#7-conclusion)
8. [Contact Information](#8-contact-information)
9. [References](#9-references)

---

# 1. Introduction

This document gives the step-by-step procedure for creating **cost tag–based reports in AWS Cost Explorer**, following the design in the previous sprint's document "Organizing and tracking costs using AWS cost allocation tags".

The implementation uses an existing AWS resource (an EC2 instance) and is performed manually through the AWS Management Console. No new infrastructure is created.

---

# 2. Prerequisites

| Requirement | Description |
| ----------- | ----------- |
| AWS Account | Active account with Cost Explorer enabled |
| AWS Resource | Existing resource, such as an EC2 instance |
| Permissions | Billing and Cost Management access, and permission to tag resources and activate cost allocation tags |
| Account Type | A management account, or a standalone account that is not a member of an organization |

---

# 3. Implementation Workflow

The workflow has two stages: tagging setup (steps 1 to 4) and Cost Explorer reporting (steps 5 to 10).

<p align="center">
<img width="420" alt="Cost tag report implementation workflow" src="images/cost-tag-report-workflow.png" />
</p>

---

# 4. Step-by-Step Implementation

## 4.1 Select an AWS Resource

1. Log in to the AWS Management Console and open **EC2**.
2. Go to **Instances** and select an existing instance.

<!-- Screenshot placeholder: selected EC2 instance -->

---

## 4.2 Apply Cost Allocation Tags

Apply the standard tags defined in the previous sprint.

| Tag Key | Example Value | Purpose |
| ------- | ------------- | ------- |
| Environment | Dev | Identifies the environment |
| Project | CostOptimization | Identifies the project |
| Owner | Sahil | Identifies the owner |
| CostCenter | CC001 | Identifies the cost center |

1. Open the instance and go to the **Tags** section.
2. Choose **Manage tags** and add the four key-value pairs.
3. Save, then confirm the tags are visible on the instance.

<!-- Screenshot placeholder: tags on the EC2 instance -->

---

## 4.3 Activate Cost Allocation Tags

Tags must be activated as user-defined cost allocation tags before Cost Explorer can use them.

1. Open **Billing and Cost Management** and choose **Cost Allocation Tags**.
2. Select the tag keys Environment, Project, Owner and CostCenter.
3. Choose **Activate** and confirm they show as active.

**Timing:** tag keys can take up to 24 hours to appear on this page after tagging, and up to another 24 hours to activate. Validate the report only after the tags are active.

**Historical cost:** activation applies only to cost incurred afterwards by default. To cover earlier periods, a management account user can choose **Backfill tags** on the same page and select a start month (up to 12 months back). The tag must already have been assigned to the resource in that period.

<!-- Screenshot placeholder: activated Cost Allocation Tags -->

---

## 4.4 Configure the Cost Tag Report

1. Open **Cost Explorer** from Billing and Cost Management.
2. Set the time period (for example, last 7 days) and granularity to **Daily**.
3. Under **Group by**, select **Tag**, then choose **Project**.

The report now shows cost per Project tag value, plus a separate entry for cost without the tag.

<!-- Screenshot placeholder: Cost Explorer grouped by Project tag -->

---

## 4.5 Apply Filters

1. Add a tag filter: **Environment = Dev**.
2. Optionally filter by service, region or linked account.

The report now shows project-level cost for the Dev environment only.

<!-- Screenshot placeholder: Environment = Dev filter applied -->

---

## 4.6 Save and Export the Report

1. Verify the date range, granularity, tag grouping and filters.
2. Save the report as **Cost Tag Report - Project** (or per the organization's naming convention).
3. Use the download option to export the data as CSV for offline analysis and stakeholder sharing.

<!-- Screenshot placeholder: saved report and exported CSV -->

---

# 5. Validation and Evidence

| Validation | Expected Result | Screenshot for Jira ticket |
| ---------- | --------------- | -------------------------- |
| Resource tags | All four tags present on the resource | Tagged resource |
| Cost Allocation Tags | Required tags active | Activated tags page |
| Group by tag | Costs grouped by Project tag values | Cost Explorer configuration and final breakdown |
| Tag filter | Environment = Dev applied | Applied filter |
| Saved / exported report | Report saved and CSV exported | Saved report or CSV |

Actual cost values depend on the account's billing data.

---

# 6. Troubleshooting

| Problem | Cause | Resolution |
| ------- | ----- | ---------- |
| Tag not visible in Cost Allocation Tags or Cost Explorer | Tag applied recently, not activated, or not yet processed | Wait up to 24 hours, activate, then wait again for activation |
| Cost shown as untagged | Resource lacks the tag, tag was not active when cost was incurred, or service does not support the tag | Check resource tags and activation status |
| Earlier cost not tagged | Activation is not applied to past periods by default | Request a backfill (up to 12 months) |
| Cost Allocation Tags page not accessible | Member account of an organization | Use the management account or a standalone account |

---

# 7. Conclusion

This implementation provides a native AWS way to analyze cost by standardized resource tags. Following the previous sprint's workflow, it produces a saved and exportable Cost Explorer report, and lays the foundation for project, environment, owner and cost-center cost visibility in the Cost Optimization initiative.

---

# 8. Contact Information

| Name | Contact |
| ---- | ------- |
| Sahil | [sahil.butola.snaatak@mygurukulam.co](mailto:sahil.butola.snaatak@mygurukulam.co) |

---

# 9. References

| Resource | Link |
| -------- | ---- |
| Using user-defined cost allocation tags | [AWS Documentation](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/custom-tags.html) |
| Activating user-defined cost allocation tags | [AWS Documentation](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/activating-tags.html) |
| Backfill cost allocation tags | [AWS Documentation](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-allocation-backfill.html) |
| Analyzing Your Costs with AWS Cost Explorer | [AWS Documentation](https://docs.aws.amazon.com/cost-management/latest/userguide/ce-what-is.html) |
