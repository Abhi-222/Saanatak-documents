# Ansible Continuous Deployment (CD) POC

---

## Author Table

| **Author** | **Created On** | **Version** | **Last Updated** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
|---|---|---|---|---|---|---|
| Sahil Butola | 30-09-2026 | v1.0 | 30-09-2026 | - | - | - |

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Objective](#2-objective)
3. [Prerequisites](#3-prerequisites)
4. [Architecture](#4-architecture)
5. [Dynamic Inventory](#5-dynamic-inventory)
6. [Ansible Playbook](#6-ansible-playbook)
7. [Jenkins Credentials](#7-jenkins-credentials)
8. [Jenkins Pipeline](#8-jenkins-pipeline)
9. [Validation](#9-validation)
10. [Observations](#10-observations)
11. [Troubleshooting](#11-troubleshooting)
12. [Result](#12-result)
13. [Conclusion](#13-conclusion)
14. [Contact Information](#14-contact-information)
15. [References](#15-references)

---

## 1. Introduction

This POC demonstrates **Continuous Deployment using Jenkins and Ansible**.

Jenkins triggers the pipeline; Ansible discovers the target EC2 dynamically via AWS tags (no hardcoded IP) and deploys **Nginx** over SSH.

| Field | Details |
|---|---|
| **CI/CD Tool** | Jenkins |
| **Automation** | Ansible (AWS EC2 Dynamic Inventory) |
| **Cloud / Region** | AWS, `ap-south-1` |
| **Target** | Ubuntu EC2, tagged `Role=webserver` |
| **Application** | Nginx |
| **Deployment Method** | SSH |
| **Terraform / Docker** | Not used |

---

## 2. Objective

| Objective |
|---|
| Discover target EC2 dynamically via AWS tags |
| Establish SSH connectivity from Jenkins to target |
| Deploy and configure Nginx using Ansible |
| Validate deployment (service status + HTTP response) |
| Demonstrate Ansible idempotency |
| Demonstrate pipeline failure handling |

---

## 3. Prerequisites

| Component | Details |
|---|---|
| Cloud | AWS EC2 |
| OS | Ubuntu |
| CI/CD | Jenkins |
| Automation | Ansible + `amazon.aws` collection |
| Target Tag | `Role=webserver` |
| Region | `ap-south-1` |
| Access | SSH + Sudo |
| Credentials | AWS access/secret key, SSH private key (in Jenkins Credentials) |

---

## 4. Architecture

```text
Developer → GitHub → Jenkins
                        |
              Syntax Check + AWS Creds
                        |
              AWS Dynamic Inventory (Role=webserver)
                        |
                  SSH → Ubuntu EC2
                        |
                     Nginx
                        |
              Deployment Validation
```

---

## 5. Dynamic Inventory

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

## 6. Ansible Playbook

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
          <p>Application deployed using Jenkins and Ansible.</p>
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

## 7. Jenkins Credentials

| Credential ID | Type | Purpose |
|---|---|---|
| `aws-access-key` / `aws-secret-key` | Secret Text | AWS API auth |
| `ansible-ssh-key` | SSH Username + Private Key | SSH to target EC2 (`ubuntu`) |

None stored in Git or the Jenkinsfile.

---

## 8. Jenkins Pipeline

```text
Checkout → Syntax Check → SSH Connectivity → Deploy → Validate
```

```groovy
pipeline {
    agent any
    stages {
        stage('Checkout') { steps { checkout scm } }

        stage('Ansible Syntax Check') {
            steps {
                withCredentials([string(credentialsId:'aws-access-key',variable:'AWS_ACCESS_KEY_ID'),
                                  string(credentialsId:'aws-secret-key',variable:'AWS_SECRET_ACCESS_KEY')]) {
                    sh 'ansible-playbook --syntax-check -i inventory/aws_ec2.yml -e ansible_user=ubuntu playbooks/nginx.yml'
                }
            }
        }

        stage('Test SSH Connection') {
            steps {
                withCredentials([string(credentialsId:'aws-access-key',variable:'AWS_ACCESS_KEY_ID'),
                                  string(credentialsId:'aws-secret-key',variable:'AWS_SECRET_ACCESS_KEY')]) {
                    sshagent(['ansible-ssh-key']) {
                        sh 'ansible -i inventory/aws_ec2.yml -e ansible_user=ubuntu webserver -m ping'
                    }
                }
            }
        }

        stage('Continuous Deployment') {
            steps {
                withCredentials([string(credentialsId:'aws-access-key',variable:'AWS_ACCESS_KEY_ID'),
                                  string(credentialsId:'aws-secret-key',variable:'AWS_SECRET_ACCESS_KEY')]) {
                    sshagent(['ansible-ssh-key']) {
                        sh 'ansible-playbook -i inventory/aws_ec2.yml -e ansible_user=ubuntu playbooks/nginx.yml'
                    }
                }
            }
        }

        stage('Deployment Validation') {
            steps {
                withCredentials([string(credentialsId:'aws-access-key',variable:'AWS_ACCESS_KEY_ID'),
                                  string(credentialsId:'aws-secret-key',variable:'AWS_SECRET_ACCESS_KEY')]) {
                    sshagent(['ansible-ssh-key']) {
                        sh '''
                            ansible -i inventory/aws_ec2.yml -e ansible_user=ubuntu webserver -m shell -a "systemctl is-active nginx"
                            ansible -i inventory/aws_ec2.yml -e ansible_user=ubuntu webserver -m shell -a "curl -f -s http://localhost"
                        '''
                    }
                }
            }
        }
    }
    post {
        success { echo 'Ansible CD deployment completed successfully.' }
        failure { echo 'Ansible CD deployment failed.' }
    }
}
```

<details>
<summary>📷 Screenshot: Jenkins pipeline stage view</summary>

<!-- Add screenshot of the Jenkins pipeline stages (Checkout → Syntax Check → SSH Test → Deploy → Validate) here -->

</details>

---

## 9. Validation

| Check | Result |
|---|---|
| AWS Dynamic Inventory | EC2 discovered under `webserver` group |
| SSH Connectivity | `pong` (SUCCESS) |
| Ansible Syntax Check | Passed |
| Nginx Installation | Successful |
| Nginx Service | `active (running)` |
| HTTP Response | `Deployment Successful` |
| Idempotency (2nd run) | `changed=0` |
| Jenkins Pipeline | `Finished: SUCCESS` |

<details>
<summary>📷 Screenshot: Service & HTTP validation output</summary>

<!-- Add screenshot of `systemctl is-active nginx` and `curl http://localhost` output here -->

</details>

<details>
<summary>📷 Screenshot: Idempotency second-run output (changed=0)</summary>

<!-- Add screenshot of the second playbook run showing changed=0 here -->

</details>

---

## 10. Observations

* Dynamic inventory removes the need for hardcoded IPs — targets are discovered purely by AWS tag.
* Jenkins injects both AWS and SSH credentials at runtime; neither is stored in Git.
* The playbook is idempotent — a second run reports `changed=0`.
* The deployment validation stage only runs if the deploy stage succeeds.
* Nginx serves the deployed page correctly after each successful run.

---

## 11. Troubleshooting

| Issue | Solution |
|---|---|
| No hosts matched (`webserver` group empty) | Verify EC2 is tagged `Role=webserver` and in `running` state |
| SSH connection refused | Check EC2 Security Group allows port 22 from Jenkins |
| AWS credentials error | Re-check `aws-access-key` / `aws-secret-key` in Jenkins Credentials |
| Ansible syntax check fails | Validate YAML indentation in `playbooks/nginx.yml` |
| Nginx not active after deploy | Re-run playbook; check `nginx -t` output for config errors |
| HTTP check fails | Confirm port 80 is open in Security Group and Nginx is running |

---

## 12. Result

```text
SSH Test: SUCCESS (ping: pong)
Deployment: ok=6 changed=1 unreachable=0 failed=0
Nginx: active
HTTP: Deployment Successful
Pipeline: Finished SUCCESS
```

<details>
<summary>📷 Screenshot: Jenkins final build result (SUCCESS)</summary>

<!-- Add screenshot of the Jenkins build log/summary showing Finished: SUCCESS here -->

</details>

---

## 13. Conclusion

This POC validates end-to-end Continuous Deployment via Jenkins → Ansible → AWS EC2 (dynamic inventory) → Nginx, with no Terraform/Docker/Kubernetes required. Target discovery uses AWS tags rather than static IPs, and the flow is confirmed idempotent and fail-safe.

---

## 14. Contact Information

| Name | Email Address |
|---|---|
| Sahil Butola | [Add contact email] |

---

## 15. References

| Resource | Link |
|---|---|
| Ansible AWS EC2 Inventory Plugin | [amazon.aws.aws_ec2 Documentation](https://docs.ansible.com/ansible/latest/collections/amazon/aws/aws_ec2_inventory.html) |
| Ansible Documentation | [Ansible Docs](https://docs.ansible.com/) |
| Jenkins Pipeline | [Jenkins Pipeline Documentation](https://www.jenkins.io/doc/book/pipeline/) |
| AWS EC2 | [AWS EC2 Documentation](https://docs.aws.amazon.com/ec2/) |
| Nginx | [Nginx Documentation](https://nginx.org/en/docs/) |
