<div align="center">

<img width="480" height="270" alt="image" src="https://github.com/user-attachments/assets/d138c331-e4c4-4bdd-b272-ff8dc570cb79" />

</div>

---

# Ansible Continuous Deployment (CD) POC – Nginx Deployment

---

## Author Table

| Author | Created On | Version | Last Edited On | L0 Reviewer | L1 Reviewer | L2 Reviewer |
|:-------|:-----------|:--------|:---------------|:------------|:------------|:------------|
| Sahil Butola | 30-09-2026 | 1.0 | 30-09-2026 | Divya M. / Vishal | Aayush Verma | Varun / Mahesh |

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Objective](#2-objective)
3. [Prerequisites](#3-prerequisites)
4. [Setup & Environment Assumptions](#4-setup--environment-assumptions)
5. [Architecture](#5-architecture)
6. [Implementation](#6-implementation)
7. [Validation](#7-validation)
8. [Observations](#8-observations)
9. [Troubleshooting](#9-troubleshooting)
10. [Security Considerations](#10-security-considerations)
11. [Result](#11-result)
12. [Conclusion](#12-conclusion)
13. [Contact Information](#13-contact-information)
14. [References](#14-references)

---

## 1. Introduction

This POC demonstrates how **Ansible** can be used to automate the deployment of **Nginx** on an AWS EC2 instance.

AWS CLI is used for AWS authentication and EC2 verification, while Ansible uses **AWS Dynamic Inventory** to automatically discover the target EC2 instance based on its AWS tag — no static IP is hardcoded anywhere.

| Field | Details |
|-------|---------|
| **Project** | Nginx Deployment using Ansible |
| **Tools** | Ansible, AWS CLI |
| **Target Platform** | AWS EC2 (Ubuntu) |
| **Region** | `ap-south-1` |
| **Deployment Method** | SSH |

---

## 2. Objective

 -Configure AWS CLI authentication. 
 -Identify the target EC2 instance using AWS CLI. 
 -Configure Ansible AWS Dynamic Inventory. 
 -Dynamically discover EC2 instances using AWS tags. 
 -Establish SSH connectivity between Ansible and the EC2 instance. 
 -Install and configure Nginx. 
 -Deploy a test HTML page. 
 -Validate Nginx configuration. 
 -Verify the deployed application. 
 -Demonstrate Ansible idempotency. 

---

## 4. Prerequisites

| Component | Details |
|-----------|---------|
| Cloud | AWS EC2 |
| OS | Ubuntu |
| Automation | Ansible + `amazon.aws` collection |
| CLI | AWS CLI |
| Python | Python 3, `boto3`, `botocore` |
| Target Tag | `Role=webserver` |
| Region | `ap-south-1` |
| Access | SSH private key + Sudo |

---

## 5. Setup & Environment Assumptions

Steps that are easy to skip because the deployment "looks" ready without them.

| # | What to do | Command / File | Why it's needed |
|---|---|---|---|
| 1 | Install Ansible, AWS CLI, and Python AWS libraries | `sudo apt update && sudo apt install -y ansible awscli python3-boto3 python3-botocore` | Nothing else works without these — commands would fail with "command not found" or import errors |
| 2 | Install the AWS Ansible collection | `ansible-galaxy collection install amazon.aws` | The `amazon.aws.aws_ec2` inventory plugin isn't built into Ansible — without it, `ansible-inventory` errors with "plugin not found" |
| 3 | Verify installs | `ansible --version` / `aws --version` | Confirms the control machine is ready before proceeding |
| 4 | Configure AWS CLI credentials | `aws configure` (set region `ap-south-1`) | Both AWS CLI and the `boto3`-based inventory plugin read from this configuration |

---

## 6. Architecture

<img width="1186" height="718" alt="image" src="https://github.com/user-attachments/assets/3d3bf614-2c87-4b20-9cdf-9ffa8679575a" />


| Component | Responsibility |
|-----------|----------------|
| AWS CLI | AWS authentication and EC2 verification |
| Ansible | Deployment and configuration automation |
| AWS Dynamic Inventory | Automatically discovers EC2 instances |
| SSH | Connects Ansible to the target EC2 |
| Nginx | Web server used as the deployment target |
| AWS EC2 | Deployment environment |

**Final Workflow**

```text
AWS CLI Configuration → AWS Authentication → Find EC2 via AWS CLI
        ↓
Ansible Dynamic Inventory → Discover Role=webserver
        ↓
SSH Connectivity → Ansible Ping → Playbook Syntax Check
        ↓
Install Nginx → Start/Enable → Deploy HTML Page
        ↓
Validate Nginx → Restart Nginx
        ↓
HTTP Validation → Idempotency Test
```

---

## 7. Implementation

### 7.1 AWS CLI Configuration & Verification

Configure AWS CLI:

```bash
aws configure
```

Set `Default region: ap-south-1` along with the access key/secret key.

Verify authentication:

```bash
aws sts get-caller-identity
```

Verify the target EC2 by its `Role` tag:

```bash
aws ec2 describe-instances \
  --region ap-south-1 \
  --filters \
  "Name=tag:Role,Values=webserver" \
  "Name=instance-state-name,Values=running" \
  --query 'Reservations[].Instances[].{ID:InstanceId,IP:PublicIpAddress,Role:Tags[?Key==`Role`]|[0].Value}' \
  --output table
```

<details>
<summary> Screenshot: AWS CLI instance verification output</summary>
<img width="1001" height="269" alt="Screenshot 2026-09-30 at 12 02 09 PM" src="https://github.com/user-attachments/assets/13d35f32-8a8b-4e8f-95b1-4e7e973fb3c9" />
</details>

---

### 7.2 Dynamic Inventory Configuration

<details>
<summary> Screenshot: Project Structure</summary>
<img width="1001" height="269" alt="Screenshot 2026-09-30 at 12 02 26 PM" src="https://github.com/user-attachments/assets/dad83a65-d356-4222-be6d-93c0f284b258" />
</details>

`inventory/aws_ec2.yml`:

```yaml
plugin: amazon.aws.aws_ec2

regions:
  - ap-south-1

filters:
  tag:Role:
    - webserver
  instance-state-name:
    - running

keyed_groups:
  - key: tags.Role

compose:
  ansible_user: "'ubuntu'"
```

| Configuration | Purpose |
|---------------|---------|
| `amazon.aws.aws_ec2` | Uses AWS EC2 Dynamic Inventory |
| `regions` | Defines the AWS region |
| `tag:Role` | Finds EC2 instances with `Role=webserver` |
| `instance-state-name` | Only selects running instances |
| `keyed_groups` | Creates Ansible groups from EC2 tags |
| `ansible_user` | Defines the SSH user as `ubuntu` |

Validate:

```bash
ansible-inventory -i inventory/aws_ec2.yml --graph
```

<details>
<summary> Screenshot: Dynamic inventory graph output</summary>
<img width="1001" height="269" alt="Screenshot 2026-09-30 at 12 02 45 PM" src="https://github.com/user-attachments/assets/b42c8dff-eac5-4b34-83e1-f83a5e3fbc27" />
</details>

---

### 7.3 SSH Access & Connectivity Test

Set key permissions and test SSH manually:

```bash
chmod 400 /home/ubuntu/sahil-key.pem
ssh -i /home/ubuntu/sahil-key.pem ubuntu@<PUBLIC-IP>
```

Test Ansible connectivity:

```bash
ansible -i inventory/aws_ec2.yml _webserver -m ping --private-key /home/ubuntu/sahil-key.pem
```

<details>
<summary> Screenshot: Playbook execution output</summary>
<img width="1001" height="226" alt="Screenshot 2026-09-30 at 12 05 23 PM" src="https://github.com/user-attachments/assets/8ab70bda-02b5-4bb4-83a3-47f108a7dfab" />
</details>

---

### 7.4 Ansible Playbook

`playbook.yml`:

```yaml
---
- name: Deploy Nginx
  hosts: _webserver
  become: true

  tasks:

    - name: Install Nginx
      ansible.builtin.apt:
        name: nginx
        state: present
        update_cache: true

    - name: Ensure Nginx is running
      ansible.builtin.service:
        name: nginx
        state: started
        enabled: true

    - name: Deploy test page
      ansible.builtin.copy:
        content: |
          <!DOCTYPE html>
          <html>
          <head>
              <title>Ansible CD POC</title>
          </head>
          <body>
              <h1>Deployment Successful</h1>
              <p>NGINX deployed using Ansible Continuous Deployment.</p>
          </body>
          </html>
        dest: /var/www/html/index.html
        owner: root
        group: root
        mode: '0644'

    - name: Validate Nginx configuration
      ansible.builtin.command:
        cmd: nginx -t
      changed_when: false

    - name: Restart Nginx
      ansible.builtin.service:
        name: nginx
        state: restarted
```

| Task | Purpose |
|------|---------|
| Install Nginx | Installs Nginx if not already installed |
| Ensure Nginx is running | Starts Nginx and enables it at boot |
| Deploy test page | Places the application HTML page in the Nginx web root |
| Validate Nginx configuration | Checks whether the Nginx configuration is valid |
| Restart Nginx | Applies the deployment |

---

### 7.5 Execution Steps

1. Configure AWS CLI (`aws configure`) and verify with `aws sts get-caller-identity`.
2. Verify the target EC2 using `aws ec2 describe-instances` filtered on `Role=webserver`.
3. Create `inventory/aws_ec2.yml` and validate with `ansible-inventory --graph`.
4. Set SSH key permissions (`chmod 400 /home/ubuntu/sahil-key.pem`) and test manual SSH login.
5. Run `ansible ... -m ping` to confirm Ansible connectivity.
6. Syntax-check the playbook: `ansible-playbook --syntax-check -i inventory/aws_ec2.yml playbook.yml`.
7. Deploy: `ansible-playbook -i inventory/aws_ec2.yml playbook.yml --private-key /home/ubuntu/sahil-key.pem`.
8. Validate Nginx service and HTTP response (see Section 8).
9. Re-run step 7 once more to confirm idempotency.

<details>
<summary> Screenshot: Playbook execution output</summary>
<img width="1440" height="513" alt="Screenshot 2026-09-30 at 12 06 22 PM" src="https://github.com/user-attachments/assets/c3dedfa1-1e57-4781-8ccb-a3751a668a4e" />
</details>

---

## 8. Validation

| Check | Result |
|-------|--------|
| AWS CLI Authentication | Account identity returned |
| Target EC2 Identified (AWS CLI) | Instance found, tagged `Role=webserver` |
| AWS Dynamic Inventory | EC2 discovered under `_webserver` group |
| SSH Connectivity | `pong` (SUCCESS) |
| Ansible Syntax Check | Passed (`playbook: playbook.yml`) |
| Nginx Installation | Successful |
| Nginx Service | `active` |
| HTTP Response | `Deployment Successful` |
| Idempotency (2nd run) | No unnecessary changes |

<details>
<summary>Screenshot: Nginx service & HTTP validation output</summary>
<img width="656" height="182" alt="Screenshot 2026-09-30 at 12 07 20 PM" src="https://github.com/user-attachments/assets/0a83d812-dc89-4f3b-b950-ad24fa7e9709" />
<img width="833" height="348" alt="Screenshot 2026-09-30 at 12 08 27 PM" src="https://github.com/user-attachments/assets/2b492672-c4cb-4d38-bf59-f74870c9d29e" />
</details>

---

## 9. Observations

* AWS CLI is used purely for authentication and manual EC2 verification — Ansible's own dynamic inventory does the actual discovery.
* Dynamic inventory removes the need for hardcoded IPs — targets are discovered purely by AWS tag.
* Idempotency means running the same playbook multiple times results in the same desired state without repeatedly making unnecessary changes.
* Deployment errors are visible directly in the Ansible execution output (`failed=1`), and the playbook can simply be re-run after a fix.

---

## 10. Troubleshooting

| Issue | Solution |
|-------|----------|
| No hosts matched (`_webserver` group empty) | Verify EC2 is tagged `Role=webserver` and in `running` state |
| `aws sts get-caller-identity` fails | Re-run `aws configure` and check the access/secret key |
| SSH connection refused | Check EC2 Security Group allows port 22 from the control machine |
| Ansible syntax check fails | Validate YAML indentation in `playbook.yml` |
| Nginx not active after deploy | Re-run playbook; check `nginx -t` output for config errors |
| HTTP check fails | Confirm port 80 is open in Security Group and Nginx is running |

---

## 11. Security Considerations

* Do not commit `sahil-key.pem` to Git.
* Keep private key permissions restricted: `chmod 400 /home/ubuntu/sahil-key.pem`.
* Do not hardcode AWS access keys inside the inventory or playbook — use AWS CLI configuration or environment-based credentials.
* Use IAM permissions scoped appropriately for the POC.
* Restrict SSH access through the EC2 Security Group.
* Do not expose unnecessary ports.

---

## 12. Result

```text
AWS CLI Auth: SUCCESS (identity returned)
Target EC2: Found (Role=webserver)
SSH Test: SUCCESS (ping: pong)
Deployment: Nginx installed, active, page deployed
HTTP: Deployment Successful
Idempotency: No unnecessary changes on re-run
```

<details>
<summary> Screenshot: Final deployment result</summary>
<img width="1195" height="554" alt="Screenshot 2026-09-30 at 12 09 14 PM" src="https://github.com/user-attachments/assets/75ab5b41-571c-4866-8a23-fec01c363d38" />
</details>

---

## 13. Conclusion

This POC demonstrates an Ansible-based deployment workflow for an AWS EC2 environment. AWS CLI handles authentication and manual EC2 verification, while Ansible Dynamic Inventory automatically discovers the target instance using its AWS tag. Ansible then connects to the Ubuntu EC2 instance over SSH and deploys Nginx along with a test application page.

The POC does not depend on Jenkins, Terraform, or Docker, and focuses only on the core Ansible deployment process.

---

## 14. Contact Information

| Name | Email Address |
|------|---------------|
| Sahil Butola | sahil.butola.snaatak@mygurukulam.co |

---

## 15. References

| Resource | Link |
|---|---|
| Ansible AWS EC2 Inventory Plugin | [amazon.aws.aws_ec2 Documentation](https://docs.ansible.com/ansible/latest/collections/amazon/aws/aws_ec2_inventory.html) |
| Ansible Documentation | [Ansible Docs](https://docs.ansible.com/) |
| Ansible Installation on Ubuntu System| [Ansible-installation](https://docs.ansible.com/projects/ansible/latest/installation_guide/installation_distros.html)|
| AWS CLI | [AWS CLI Documentation](https://docs.aws.amazon.com/cli/) |
| AWS Installation on Ubuntu System|[aws-cli-installation](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) |
| AWS EC2 | [AWS EC2 Documentation](https://docs.aws.amazon.com/ec2/) |
| Nginx | [Nginx Documentation](https://nginx.org/en/docs/) |
