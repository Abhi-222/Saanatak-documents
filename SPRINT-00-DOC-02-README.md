# Common Stack | Ansible | Role | Jinja Templating

---

# Author Table

| **Author** | **Created On** | **Version** | **Last Updated By** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ---------- | -------------- | ----------- | -------------------- | ------------------- | ---------------- | ---------------- | ---------------- |
| Sahil | 31-08-2026 | 1.0 | Sahil | 31-08-2026 | `Diviya M` | `Ayush Verma` | `Mahesh Kumar / Varun` |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What is an Ansible Role](#2-what-is-an-ansible-role)
3. [Why Roles and Jinja Templating are Required](#3-why-roles-and-jinja-templating-are-required)
4. [Key Features of Ansible Roles and Jinja Templating](#4-key-features-of-ansible-roles-and-jinja-templating)
5. [Acceptance Criteria - Concept](#5-acceptance-criteria---concept)
6. [Contact Information](#6-contact-information)
7. [References](#7-references)

---

# 1. Introduction

Ansible is an open-source automation tool used for configuration management, application deployment, and infrastructure orchestration. A Role is Ansible's standard way of organizing automation content into a reusable, self-contained structure. Jinja Templating is the engine Ansible uses to generate configuration files dynamically at deploy time, based on variables and host facts. Together, Roles and Jinja Templates form the core building blocks of any production-grade Ansible automation, and are the foundation used in this stack's PostgreSQL and infrastructure automation work.

---

# 2. What is an Ansible Role

A Role is a standardized directory layout that groups related automation content — tasks, handlers, templates, files, variables, and metadata — into a single reusable unit. Instead of writing one large playbook, a Role allows the same automation to be applied consistently across different projects and environments simply by including the role name.

A Jinja Template is a file (with a `.j2` extension) that contains a mix of static configuration content and dynamic placeholders. When Ansible deploys a template using the `template` module, it substitutes variables, evaluates conditionals, and renders a final configuration file specific to the target host.

---

# 3. Why Roles and Jinja Templating are Required

Roles and Jinja Templating are required because they solve several common automation challenges:

- **Reusability** — a role written once (e.g. for PostgreSQL) can be applied to any number of hosts or environments without rewriting logic.
- **Consistency** — the fixed directory structure means every engineer organizes automation the same way, making handovers and reviews easier.
- **Dynamic configuration** — Jinja allows one template to produce different output per host, such as different `primary_conninfo` values for a primary versus a replica node.
- **Separation of logic and data** — tasks define what happens, while variables (in `defaults` or `vars`) define the specifics, keeping automation environment-agnostic.
- **OS/environment independence** — combined with conditionals and host facts, the same role can branch its behavior across different operating systems or roles without duplicating code.

---

# 4. Key Features of Ansible Roles and Jinja Templating

| **Feature** | **Description** |
| ----------- | ---------------- |
| Standard Directory Structure | tasks, handlers, templates, files, vars, defaults, and meta folders are auto-discovered by Ansible without explicit includes. |
| Variable Precedence | defaults hold low-priority, overridable values; vars hold role-internal, higher-priority values; extra vars override everything. |
| Template Module | Renders `.j2` files with variable substitution and pushes the result to the target host, unlike the file module which copies content as-is. |
| Variable Substitution | Double curly braces insert a variable's value directly into the rendered file. |
| Conditionals | If/else blocks allow a template to render different content depending on a variable or host fact, such as node role or OS family. |
| Loops | For blocks allow a template to iterate over a list, useful for generating repeated configuration blocks. |
| Filters | Modify a variable's value inline during rendering, such as supplying a default value when one is not set. |
| Handlers | Triggered only when a related task reports a change, commonly used to restart a service after its configuration file is updated. |

---

# 5. Acceptance Criteria - Concept

| **Criterion** | **Description** |
| -------------- | ---------------- |
| Role Structure | Can explain the standard role directory layout and why Ansible auto-discovers it without explicit include statements. |
| Variable Precedence | Can distinguish between defaults and vars, and explain where each sits in Ansible's overall variable precedence order. |
| Template vs Copy | Can explain why the template module is used for .j2 files instead of the copy module. |
| Conditional Rendering | Can describe how a Jinja conditional block can change rendered output based on a host variable, such as OS family or replication role. |
| Practical Mapping | Can map the concept to a real use case, such as generating a different postgresql.conf per primary/replica node. |

---

# 6. Contact Information

| **Name** | **Email** |
| -------- | --------- |
| Sahil | `sahil.butola.snaatak@mygurukulam.co` |

---

# 7. References

| **Topic** | **Description** |
| --------- | ---------------- |
| [Ansible Roles Documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html) | Official documentation for structuring and using Ansible Roles. |
| [Jinja2 Templating Documentation](https://jinja.palletsprojects.com/) | Official documentation for the Jinja2 templating engine used by Ansible. |
