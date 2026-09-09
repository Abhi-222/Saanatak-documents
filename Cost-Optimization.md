# Cost Optimization Designing — Cost Tag Reports via Cost Explorer

## Introduction

This document covers the design and usage of **cost tag–based reports** in AWS Cost Explorer as part of the broader Cost Optimization initiative. It explains what cost allocation tags are, why they matter for cost visibility, how the reporting workflow operates end to end, and the best practices to follow when adopting this approach across teams and projects.

## What 

Cost allocation tags are key-value labels (e.g. `Environment: Production`, `Project: PaymentsAPI`, `Owner: TeamX`) attached to AWS resources. Once activated in the Billing and Cost Management console, these tags become available as a dimension inside **AWS Cost Explorer**, allowing costs to be grouped, filtered, and reported by tag instead of only by service or account.

A **Cost Tag Report** is simply a Cost Explorer view (or saved/exported report) that groups spend by one or more tag keys — giving a breakdown of "who is spending what, where."

## Why

- Raw AWS bills only show cost by service and account, which doesn't answer "which team or project is driving this cost."
- Tag-based reporting enables **chargeback/showback** to individual teams, projects, or environments.
- It surfaces cost anomalies at a granular level (e.g. a specific project's dev environment spiking).
- It supports data-driven cost optimization decisions — rightsizing, decommissioning idle resources, budget forecasting — by attributing cost to an owner.
- It builds the foundation for automated budget alerts and cost governance tied to tags.

## Workflow

![Cost Tag Reports Workflow](./workflow-diagram.svg)

The process flows in two stages: **tagging setup** (steps 1–4) followed by **Cost Explorer reporting** (steps 5–8).

1. **Define Tags** — Standardize a tagging taxonomy (e.g. `Owner`, `Environment`, `Project`, `CostCenter`).
2. **Apply Tags** — Attach the agreed tags to AWS resources (EC2, RDS, S3, etc.), ideally enforced via Terraform/IaC or tag policies.
3. **Activate Cost Allocation Tags** — Enable the tags as "User-Defined Cost Allocation Tags" in the Billing and Cost Management console.
4. **Tag Propagation** — Allow up to 24 hours for tags to reflect in Cost Explorer and billing data.
5. **Open Cost Explorer** — Group the cost view by the relevant tag key.
6. **Apply Filters** — Narrow by date range, service, or linked account as needed.
7. **Save / Export** — Save the view as a reusable custom report, or export to CSV for offline analysis.
8. **Analyze & Share** — Review cost by team/project/environment and distribute the report to stakeholders.

## Advantages

- Clear cost attribution down to team, project, or environment level.
- No additional tooling cost — built natively into AWS Cost Explorer.
- Reports can be saved and reused, reducing repetitive manual analysis.
- Supports both ad-hoc investigation and recurring reporting cadences.
- Forms the basis for future automation (budgets, anomaly detection, alerts tied to tags).

## Best Practices

- Agree on a **standard tag taxonomy** before rollout — avoid inconsistent casing or free-text tag values.
- Enforce tagging at resource creation time (IaC modules, tag policies, SCPs) rather than relying on manual tagging after the fact.
- Activate only the tags that are actually needed for cost reporting to keep Cost Explorer views clean.
- Re-check and re-activate tags whenever new resource types or teams are onboarded.
- Schedule recurring exports (weekly/monthly) for stakeholders instead of ad-hoc pulls.
- Combine tag-based grouping with linked account and service filters for the clearest breakdown in multi-account setups.

## Conclusion

Cost tag–based reporting via AWS Cost Explorer provides a lightweight, native way to move from account-level billing visibility to team/project-level cost accountability. With a consistent tagging taxonomy and a routine reporting workflow, this becomes a repeatable input into ongoing cost optimization efforts.

## Contact Information

| Role | Name | Contact |
|---|---|---|
| Author | Sahil | *(add email/Slack handle)* |
| Reviewer | *(add name)* | *(add email/Slack handle)* |

## References

- AWS Documentation — Using Cost Allocation Tags
- AWS Documentation — Analyzing Your Costs with AWS Cost Explorer
- AWS Billing and Cost Management User Guide
