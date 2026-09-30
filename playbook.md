<div align="center">

<img width="252" height="80" alt="Ansible + AWS logo" src="https://via.placeholder.com/252x80?text=Ansible+%2B+AWS" />

</div>

---

# Ansible Continuous Deployment (CD) POC

---

## Author Table

| Author | Created On | Version | Last Updated By | Last Edited On | L0 Reviewer | L1 Reviewer | L2 Reviewer |
|:-------|:-----------|:--------|:----------------|:---------------|:------------|:------------|:------------|
| Sahil Butola | 30-09-2026 | 1.0 | Sahil Butola | 30-09-2026 | Divya M. / Vishal | Aayush Verma | Varun / Mahesh  | 

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Objective](#2-objective)
3. [Prerequisites](#3-prerequisites)
4. [Setup & Environment Assumptions](#4-setup--environment-assumptions)
5. [Architecture](#5-architecture)
6. [Implementation](#6-implementation)
   - 6.1 [Dynamic Inventory Configuration](#61-dynamic-inventory-configuration)
   - 6.2 [Ansible Playbook](#62-ansible-playbook)
   - 6.3 [Execution Steps](#63-execution-steps)
7. [Validation](#7-validation)
8. [Observations](#8-observations)
9. [Troubleshooting](#9-troubleshooting)
10. [Result](#10-result)
11. [Conclusion](#11-conclusion)
12. [Contact Information](#12-contact-information)
13. [References](#13-references)

---

## 1. Introduction

This POC demonstrates **Continuous Deployment using Ansible**, run directly from the command line — no CI/CD orchestrator is required.

Ansible discovers the target EC2 dynamically via AWS tags (no hardcoded IP) and deploys **Nginx** over SSH.

| Field | Details |
|-------|---------|
| **Automation** | Ansible (AWS EC2 Dynamic Inventory) |
| **Cloud / Region** | AWS, `ap-south-1` |
| **Target** | Ubuntu EC2, tagged `Role=webserver` |
| **Application** | Nginx |
| **Deployment Method** | SSH |

---

## 2. Objective

- Discover target EC2 dynamically via AWS tags 
- Establish SSH connectivity to the target 
- Deploy and configure Nginx using Ansible 
- Validate deployment (service status + HTTP response) 
- Demonstrate Ansible idempotency 
- Demonstrate failure handling on the command line 

---

## 3. Prerequisites

| Component | Details |
|-----------|---------|
| Cloud | AWS EC2 |
| OS | Ubuntu |
| Automation | Ansible (installed locally / on a control node) + `amazon.aws` collection |
| Target Tag | `Role=webserver` |
| Region | `ap-south-1` |
| Access | SSH + Sudo |
| Credentials | AWS access/secret key (local environment), SSH private key |

---

## 4. Setup & Environment Assumptions

This section covers the Ansible-side setup this POC specifically depends on — steps that are easy to skip because the deployment "looks" ready without them.

| # | What to do | Command / File | Why it's needed |
|---|---|---|---|
| 1 | Install Ansible on the control machine | `sudo apt update && sudo apt install ansible -y` | Nothing else works without this — `ansible`/`ansible-playbook` would fail with "command not found" |
| 2 | Install the AWS collection | `ansible-galaxy collection install amazon.aws` | The `amazon.aws.aws_ec2` inventory plugin isn't built into Ansible — without it, `ansible-inventory` errors with "plugin not found" |
| 3 | Install Python AWS libraries | `pip3 install boto3 botocore` | The collection depends on these to talk to AWS; missing them looks like an AWS login problem rather than a missing package |
| 4 | AWS credentials | `export AWS_ACCESS_KEY_ID=...` / `export AWS_SECRET_ACCESS_KEY=...` | `boto3` picks these up automatically from the shell environment |
| 5 | Skip SSH host-key prompt | `ansible.cfg` → `host_key_checking = False` | A brand-new EC2 triggers an interactive "yes/no" SSH prompt that would block a non-interactive run |
| 6 | Retry SSH on freshly launched instances | `ansible.cfg` → `retries = 3` | A "running" EC2 may still take 30–60s before SSH is actually ready; retries avoid false failures |

`ansible.cfg` (covers points 5 and 6):

```ini
[defaults]
host_key_checking = False

[ssh_connection]
retries = 3
```

---

## 5. Architecture

```text
Developer
    |
    v
AWS Dynamic Inventory (Role=webserver)
    |
    v
SSH → Ubuntu EC2
    |
    v
  Nginx
    |
    v
Deployment Validation
```

---

## 6. Implementation

### 6.1 Dynamic Inventory Configuration

`inventory/aws_ec2.yml`:

```yaml
plugin: amazon.aws.aws_ec2
regions:
  - ap-south-1
filters:
  tag:Role: webserver
  instance-state-name: running
keyed_groups:
  - key: tags.Role
compose:
  ansible_user: ubuntu
```

Discovers running EC2 instances tagged `Role=webserver` in `ap-south-1` — no static IPs needed.

<details>
<summary>📷 Screenshot: Dynamic inventory graph output</summary>

<!-- Add screenshot of `ansible-inventory -i inventory/aws_ec2.yml --graph` output here -->

</details>

---

### 6.2 Ansible Playbook

`playbooks/nginx.yml`:

```yaml
---
- name: Deploy Nginx Application
  hosts: webserver
  become: true
  tasks:
    - name: Install Nginx
      ansible.builtin.apt:
        name: nginx
        state: present
        update_cache: true

    - name: Ensure Nginx running & enabled
      ansible.builtin.service:
        name: nginx
        state: started
        enabled: true

    - name: Deploy application page
      ansible.builtin.copy:
        content: |
          <h1>Deployment Successful</h1>
          <p>Application deployed using Ansible.</p>
        dest: /var/www/html/index.html

    - name: Validate config & restart
      block:
        - ansible.builtin.command: nginx -t
          changed_when: false
        - ansible.builtin.service:
            name: nginx
            state: restarted
```

---

### 6.3 Execution Steps

1. Tag the target EC2 instance with `Role=webserver` in AWS.
2. Set AWS credentials in the shell: `export AWS_ACCESS_KEY_ID=...` and `export AWS_SECRET_ACCESS_KEY=...`.
3. Confirm the target is discoverable:
   ```bash
   ansible-inventory -i inventory/aws_ec2.yml --graph
   ```
4. Test SSH connectivity:
   ```bash
   ansible -i inventory/aws_ec2.yml -e ansible_user=ubuntu webserver -m ping
   ```
5. Run a syntax check:
   ```bash
   ansible-playbook --syntax-check -i inventory/aws_ec2.yml -e ansible_user=ubuntu playbooks/nginx.yml
   ```
6. Deploy:
   ```bash
   ansible-playbook -i inventory/aws_ec2.yml -e ansible_user=ubuntu playbooks/nginx.yml
   ```
7. Validate:
   ```bash
   ansible -i inventory/aws_ec2.yml -e ansible_user=ubuntu webserver -m shell -a "systemctl is-active nginx"
   ansible -i inventory/aws_ec2.yml -e ansible_user=ubuntu webserver -m shell -a "curl -f -s http://localhost"
   ```
8. Re-run step 6 once more to confirm idempotency (`changed=0`).

<details>
<summary>📷 Screenshot: Command-line deployment output</summary>

<!-- Add screenshot of the ansible-playbook run here -->

</details>

---

## 7. Validation

| Check | Result |
|---|---|
| AWS Dynamic Inventory | EC2 discovered under `webserver` group |
| SSH Connectivity | `pong` (SUCCESS) |
| Ansible Syntax Check | Passed |
| Nginx Installation | Successful |
| Nginx Service | `active (running)` |
| HTTP Response | `Deployment Successful` |
| Idempotency (2nd run) | `changed=0` |

<details>
<summary>📷 Screenshot: Service & HTTP validation output</summary>

<!-- Add screenshot of `systemctl is-active nginx` and `curl http://localhost` output here -->

</details>

<details>
<summary>📷 Screenshot: Idempotency second-run output (changed=0)</summary>

<!-- Add screenshot of the second playbook run showing changed=0 here -->

</details>

---

## 8. Observations

* Dynamic inventory removes the need for hardcoded IPs — targets are discovered purely by AWS tag.
* The playbook is idempotent — a second run reports `changed=0`.
* Nginx serves the deployed page correctly after each successful run.

---

## 9. Troubleshooting

| Issue | Solution |
|-------|----------|
| No hosts matched (`webserver` group empty) | Verify EC2 is tagged `Role=webserver` and in `running` state |
| SSH connection refused | Check EC2 Security Group allows port 22 from the control machine |
| AWS credentials error | Re-check `AWS_ACCESS_KEY_ID` / `AWS_SECRET_ACCESS_KEY` in the shell environment |
| Ansible syntax check fails | Validate YAML indentation in `playbooks/nginx.yml` |
| Nginx not active after deploy | Re-run playbook; check `nginx -t` output for config errors |
| HTTP check fails | Confirm port 80 is open in Security Group and Nginx is running |

---

## 10. Result

```text
SSH Test: SUCCESS (ping: pong)
Deployment: ok=6 changed=1 unreachable=0 failed=0
Nginx: active
HTTP: Deployment Successful
```

<details>
<summary>📷 Screenshot: Final deployment result</summary>

<!-- Add screenshot of the final ansible-playbook run showing success here -->

</details>

---

## 11. Conclusion

This POC validates end-to-end deployment via Ansible → AWS EC2 (dynamic inventory) → Nginx, run entirely from the command line. Target discovery uses AWS tags rather than static IPs, and the flow is confirmed idempotent and fail-safe.

---

## 12. Contact Information

| Name | Email Address |
|------|---------------|
| Sahil Butola | [Add contact email] |

---

## 13. References

| Resource | Link |
|----------|------|
| Ansible AWS EC2 Inventory Plugin | [amazon.aws.aws_ec2 Documentation](https://docs.ansible.com/ansible/latest/collections/amazon/aws/aws_ec2_inventory.html) |
| Ansible Documentation | [Ansible Docs](https://docs.ansible.com/) |
| AWS EC2 | [AWS EC2 Documentation](https://docs.aws.amazon.com/ec2/) |
| Nginx | [Nginx Documentation](https://nginx.org/en/docs/) |
