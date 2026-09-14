# Common Stack | Ansible | Role | Jinja Templating

---

# Author Table

| Author | Created On | Version | Last Updated  | L0 Reviewer | L1 Reviewer | L2 Reviewer |
|--------|------------|---------|---------------|-------------|-------------|-------------|
| Sahil  | 31-08-26   | 1.0     | 06-09-26      | `Vishal/Divya M` | `Aayush Verma` | `Mahesh Kumar / Varun` |
| Sahil  | 11-09-26   | 1.1     | 13-09-26      | `Vishal/Divya M` | `Aayush Verma` | `Mahesh Kumar / Varun` |


---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What is Jinja Templating](#2-what-is-jinja-templating)
3. [Why Jinja Templating is Used](#3-why-jinja-templating-is-used)
4. [Key Features](#4-key-features--concepts)
5. [Jinja Templating in Ansible Roles](#5-jinja-templating-in-ansible-roles)
6. [Advantages and Disadvantages](6-advantages-and-disadvantages)
7. [Use Cases](#7-use-cases)
8. [Best Practices](#8-best-practices)
9. [Conclusion](#9-conclusion)
10. [Contact Information](#10-contact-information)
11. [References](#11-references)

---

# 1. Introduction

A config file often needs to look almost the same across many servers, just with a few values changed — like the hostname or port. Jinja lets you write that file once, as a template with blanks in it, and Ansible fills in the right values for each server automatically.

This document explains what Jinja templating is and how it's used inside an Ansible Role.

---

# 2. What is Jinja Templating

Jinja is a templating language — it lets you write a text file with "blanks" in it, and then automatically fill in those blanks with real values.

In Ansible, a Jinja template is just a normal text file (usually named with a .j2 extension, like nginx.conf.j2) that mixes regular content with placeholders like {{ app_port }}. When Ansible runs — using the template module — it replaces each placeholder with an actual value (a variable you defined, a fact about the server, etc.) and saves the result as a real, finished file.

---

# 3. Why Jinja Templating is Used

- **One template, many outputs** — the same template can produce a different file for each server, just by changing the variables.
- **Keeps things separate** — the file's structure stays in the template, while things that change (hostnames, ports, passwords, feature flags) live in variables instead.
- **Reuse instead of repeat** — one template can be used for dev, staging, and production, so you're not maintaining several nearly-identical files.
- **Fewer mistakes** — less manual copy-pasting and editing means less chance of a typo breaking one environment.
- **Handles variations automatically** — templates can include simple logic (if this, then that; repeat this for each item), so one file can cover cases that would otherwise need several separate files.
---

# 4. Key Features 

- **Variable** substitution — automatically fills placeholders with real values (like a hostname or port) instead of you typing them in by hand for every server.
- **Conditional logic**  — a template can include or skip parts of a file depending on whether a condition is true, so one file can handle multiple cases.
- **Loops**  — a single block can repeat itself to generate a list of items (like multiple allowed hosts), without writing each line manually.
- **Format-agnostic**  — works on any plain text file, not just configs — YAML, JSON, shell scripts, and more.
- **Native Ansible integration**  — reads host variables and facts directly, with no extra setup or plugins required.

---

# 5. Jinja Templating in Ansible Roles
 
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
 
A task inside the role then uses the `template` module to render a file from `templates/` and place it on the target host, using variables defined in `vars/`, `defaults/`, inventory, or passed in when the role is invoked. For example, a task in `tasks/main.yml` might look like this:
 
```yaml
- name: Render application config file
  template:
    src: config_file.conf.j2
    dest: /etc/myapp/config_file.conf
    owner: myapp
    group: myapp
    mode: "0644"
```
 
`src` points to the template inside the role's `templates/` directory, and `dest` is where the rendered file lands on the target host.
 
---
 
## Examples
 
Before looking at one combined template, here's each construct in isolation — a minimal template and its rendered output.
 
### Variable Substitution
 
**Template (`app.conf.j2`):**
```jinja2
app_name={{ app_name }}
app_port={{ app_port }}
```
 
**Variables:**
```text
app_name=myapp
app_port=8080
```
 
Each `{{ variable_name }}` is replaced directly with the value Ansible has for that variable — no logic involved, just substitution.
 
---
 
### Conditional
 
**Template (`app.conf.j2`):**
```jinja2
app_name={{ app_name }}
{% if enable_debug %}
debug_mode=true
{% endif %}
```
 
**Case 1 — `enable_debug: true`:**
```text
app_name=myapp
debug_mode=true
```
 
**Case 2 — `enable_debug: false`:**
```text
app_name=myapp
```
 
The `{% if %} ... {% endif %}` block adds or removes the line entirely based on the condition — there's no leftover blank line or placeholder when the condition is false, the line simply isn't rendered.
 
---
 
### Loop
 
**Template (`app.conf.j2`):**
```jinja2
allowed_hosts:
{% for host in allowed_hosts %}
  - {{ host }}
{% endfor %}
```
 
**Variables:**
```text
allowed_hosts:
  - server1
  - server2
  - server3
```
 
The `{% for %} ... {% endfor %}` block repeats once per item in the list. Add or remove hosts in the variable and the output grows or shrinks automatically — the template itself never changes.
 
---
 
### Putting It Together
 
Real templates usually combine all three constructs in one file. Here's the earlier example, now that each piece has been seen on its own:
 
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
 
**Variables:**
```text
# Ansible managed
app_name=myapp
app_port=8080
 
debug_mode=true
 
allowed_hosts:
  - server1
  - server2
```
 
`{{ ansible_managed }}` is a built-in Ansible variable — it doesn't need to be defined anywhere, and typically renders as a note that the file is auto-generated, so no one edits it by hand on the server.
 
---

# 6. Advantages and Disadvantages

|Advantages | Disadvantages |
|-----------|---------------|
| Reduces duplication — one template can serve many hosts or environments instead of maintaining separate static files |  Templates can become hard to read if logic grows too complex, especially with deeply nested `{% if %}` / `{% for %}` blocks |
| Keeps configuration structure and variable data cleanly separated | Syntax errors in a template are only caught at render time, not before the playbook runs |
| Syntax is readable and quick to learn, especially for anyone familiar with Python-style expressions |  Debugging rendered output usually requires re-running the playbook (e.g., with `--check`/`--diff`) rather than testing the template in isolation |
| Tightly integrated with Ansible — works directly with the `template` module, host variables, and gathered facts |  Not a general-purpose templating tool outside the Python/Ansible ecosystem |
| Supports conditionals and loops, so a single file can represent many variations of a config |  Overusing logic inside templates can blur the intended separation between configuration and code |

---

# 7. Use Cases

- **Environment-specific** configuration files — rendering nginx.conf.j2 differently for dev, staging, and production using host-specific variables.
- **Injecting credentials or secrets** — populating a config template with database connection details sourced from vars//Ansible Vault.
- **Conditional feature flags** — enabling or disabling debug mode or specific modules based on a variable.
- **Templating systemd unit files** — generating a service file with a variable port, working directory, or run-as user.
- **Inventory-driven output** — producing a list of allowed hosts, load balancer members, or cluster nodes by looping over an inventory group.

---

# 8. Best Practices

- **Keep templates simple** — if a template needs a lot of {% if %}/{% for %} blocks, that logic often belongs elsewhere.
- **Name variables clearly** — app_port is easier to understand than p1.
- **Mark the file as auto-generated** — add {{ ansible_managed }} at the top so no one edits it by hand.
- **Test before rolling out** — try the template with sample values first, so mistakes show up before production.

---

# 9. Conclusion

Jinja templating lets you write one file and reuse it for every server, filling in the right values automatically. This means less repeated work, fewer mistakes, and configs that are easier to manage across all your environments.

---

# 10. Contact Information

| Name | Email ID |
|------|----------|
| Sahil | [sahil.butola.snaatak@mygurukulam.co](mailto:sahil.butola.snaatak@mygurukulam.co) |

---

# 11. References

| References | Links        |
|------------|--------------|
| Official Ansible documentation on Jinja templating | [Ansible Templating (Jinja2)](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_templating.html) | 
| Official Jinja templating engine documentation | [Jinja Documentation](https://jinja.palletsprojects.com/) | 
| Official documentation on Ansible Role structure | [Ansible Roles](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html) | 
