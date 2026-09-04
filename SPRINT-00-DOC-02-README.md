# Ansible | Role | Jinja Templating (Concept)

---

# Author Table

| Author | Created On | Version | Last Updated | L0 Reviewer | L1 Reviewer | L2 Reviewer |
|--------|------------|---------|---------------|-------------|-------------|-------------|
| Sahil | 05-09-26 | v1.0 | 05-09-26 | `Diviya M` | `Aayush Verma` | `Mahesh Kumar / Varun` |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [Purpose](#2-purpose)
3. [What is Jinja Templating](#3-what-is-jinja-templating)
4. [Why Jinja Templating is Used](#4-why-jinja-templating-is-used)
5. [Key Features / Concepts](#5-key-features--concepts)
6. [Jinja Templating in Ansible Roles](#6-jinja-templating-in-ansible-roles)
7. [Example](#7-example)
8. [Best Practices](#8-best-practices)
9. [Conclusion](#9-conclusion)
10. [Contact Information](#10-contact-information)
11. [References](#11-references)

---

# 1. Introduction

Jinja is the templating engine used by Ansible to generate dynamic content — configuration files, variable values, and conditional logic — at runtime, based on variables and facts gathered from the target hosts.

This document explains the concept of Jinja templating and how it is used within the context of an Ansible Role.

---

# 2. Purpose

The purpose of this document is to explain:

- What Jinja templating is
- Why it is used in Ansible
- The core concepts and syntax behind it
- How Jinja templates fit into the structure of an Ansible Role

---

# 3. What is Jinja Templating

Jinja is a templating language for Python that Ansible uses to dynamically generate text — most commonly configuration files — by combining a static template with variable data supplied at runtime.

A Jinja template is a normal text file (commonly given a `.j2` extension in Ansible) that contains placeholders, expressions, and control statements alongside regular static content. When Ansible renders the template — for example, using the `template` module — it substitutes those placeholders with actual values (variables, facts, or the results of expressions) to produce a final file.

---

# 4. Why Jinja Templating is Used

- **Dynamic configuration generation** — the same template can produce different output files depending on the variables supplied for each host or environment.
- **Separation of logic and static content** — configuration structure stays in the template, while values that change (hostnames, ports, credentials, feature flags) are supplied as variables.
- **Reusability** — a single template can be reused across multiple environments (dev, staging, production) or multiple hosts, avoiding duplicated config files.
- **Consistency** — reduces manual editing of configuration files, lowering the chance of human error across environments.
- **Conditional and repetitive logic** — templates can include loops and conditionals, letting a single file represent variations that would otherwise require separate static files.

---

# 5. Key Features / Concepts

| **Concept** | **Description** |
|-------------|------------------|
| Variables (`{{ }}`) | Insert a variable's value into the rendered output, e.g. `{{ app_port }}` |
| Conditionals (`{% if %}`) | Include or exclude parts of the template based on a condition |
| Loops (`{% for %}`) | Repeat a block of the template for each item in a list or dictionary |
| Filters (`\|`) | Transform a variable's value before it's rendered, e.g. `{{ app_name \| upper }}` |
| Comments (`{# #}`) | Add notes inside the template that are not included in the rendered output |
| `.j2` file extension | The common naming convention Ansible uses to identify Jinja template files |
| `template` module | The Ansible module that renders a `.j2` file using the current variables and copies the result to the target host |

---

# 6. Jinja Templating in Ansible Roles

Within the standard Ansible Role directory structure, Jinja templates live in the `templates/` directory:

```
roles/
└── role_name/
    ├── tasks/
    ├── handlers/
    ├── templates/
    │   └── config_file.conf.j2
    ├── vars/
    ├── defaults/
    └── meta/
```

A task inside the role then uses the `template` module to render a file from `templates/` and place it on the target host, using variables defined in `vars/`, `defaults/`, inventory, or passed in when the role is invoked.

---

# 7. Example

A simplified Jinja template for an application config file:

```jinja2
# {{ ansible_managed }}
app_name={{ app_name }}
app_port={{ app_port }}

{% if enable_debug %}
debug_mode=true
{% endif %}

allowed_hosts:
{% for host in allowed_hosts %}
  - {{ host }}
{% endfor %}
```

With variables such as `app_name: myapp`, `app_port: 8080`, `enable_debug: true`, and a list for `allowed_hosts`, Ansible renders this template into a fully populated configuration file on the target host.

---

# 8. Best Practices

| **Best Practice** | **Description** |
|--------------------|------------------|
| Keep logic minimal in templates | Complex logic is easier to maintain in variables or tasks than deeply nested `{% if %}` / `{% for %}` blocks |
| Use descriptive variable names | Makes templates self-explanatory without needing to trace back to `vars/` or `defaults/` |
| Store templates in `templates/` | Follow the standard Role directory structure so templates are easy to locate |
| Use `{{ ansible_managed }}` | Add a header comment noting the file is managed by Ansible, so it isn't edited manually |
| Validate rendered output | Test templates against representative variable sets before rolling out to production |

---

# 9. Conclusion

Jinja templating allows Ansible Roles to generate dynamic, environment-specific configuration files from a single reusable template. By separating static structure from variable data, it improves consistency, reduces duplication, and makes configuration management easier to maintain across multiple hosts and environments.

---

# 10. Contact Information

| Name | Email ID |
|------|----------|
| Sahil | `sahil.butola.snaatak@mygurukulam.co` |

---

# 11. References

| Resource | Description |
|----------|--------------|
| [Ansible Templating (Jinja2)](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_templating.html) | Official Ansible documentation on Jinja templating |
| [Jinja Documentation](https://jinja.palletsprojects.com/) | Official Jinja templating engine documentation |
| [Ansible Roles](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html) | Official documentation on Ansible Role structure |
| [Ansible `template` Module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/template_module.html) | Official documentation for the `template` module |
