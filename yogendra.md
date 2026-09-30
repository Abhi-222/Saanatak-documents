# Ansible Playbook CI/CD

Automated deployment of an Ansible playbook to target EC2 instances using a Jenkins CD pipeline, with secure secrets handling via Jenkins Credentials and Ansible Vault.

![Ansible](https://img.shields.io/badge/Ansible-EE0000?logo=ansible&logoColor=white)
![Jenkins](https://img.shields.io/badge/Jenkins-D24939?logo=jenkins&logoColor=white)
![AWS EC2](https://img.shields.io/badge/AWS-EC2-FF9900?logo=amazonaws&logoColor=white)

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Components](#components)
- [Repository Structure](#repository-structure)
- [Prerequisites](#prerequisites)
- [Inventory](#inventory)
- [Playbook](#playbook)
- [Secrets Management](#secrets-management)
- [CD Pipeline](#cd-pipeline)
- [Validation](#validation)
- [Failure Handling](#failure-handling)
- [Security Practices](#security-practices)
- [Acceptance Criteria](#acceptance-criteria)
- [References](#references)
- [Document Info](#document-info)

## Overview

This project describes the Continuous Deployment (CD) process for automatically executing an Ansible playbook on target systems through a CI/CD pipeline.

It covers:

- Automated playbook execution
- Inventory-based target selection
- Secure secrets management
- Deployment validation

## Architecture

```text
Developer
    |
    v
Git Repository
    |
    v
Jenkins Pipeline
    |
    +---- Inventory
    +---- Credentials
    |
    v
Ansible Playbook
    |
    | SSH
    v
Target EC2
    |
    v
Application / Configuration
```

## Components

| Component           | Purpose                            |
| ------------------- | ---------------------------------- |
| Git                 | Stores Ansible configuration       |
| Jenkins             | Automates the CD pipeline          |
| Ansible             | Performs deployment/configuration  |
| Inventory           | Defines target systems             |
| Jenkins Credentials | Provides secure SSH authentication |
| EC2                 | Target deployment system           |

## Repository Structure

```text
.
├── inventory        # Target hosts and groups
├── playbook.yml     # Deployment tasks
├── Jenkinsfile      # CD pipeline definition
└── README.md
```

## Prerequisites

- A Jenkins server with Ansible installed on the agent that runs the job
- An SSH private key stored in **Jenkins Credentials**
- A target EC2 instance reachable over SSH from the Jenkins agent
- Python available on the target host (required by Ansible modules)

## Inventory

The inventory defines the systems against which the playbook executes.

```ini
[webserver]
web1 ansible_host=<TARGET_IP>

[webserver:vars]
ansible_user=ubuntu
```

The playbook references the inventory group using `hosts: webserver`.

## Playbook

The playbook contains the deployment tasks executed on the target systems.

```yaml
---
- name: Deploy Nginx
  hosts: webserver
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
```

> Ansible is agentless: the target system does not require an Ansible agent.

## Secrets Management

Sensitive credentials must **not** be stored directly in Git.

The SSH private key is stored in **Jenkins Credentials** and accessed by the pipeline using its credential ID.

```text
Jenkins Credentials
        |
        v
Jenkins Pipeline
        |
        v
Ansible
        |
       SSH
        |
        v
Target EC2
```

Ansible Vault can be used for sensitive variables such as passwords, tokens, or application credentials.

## CD Pipeline

The Jenkins pipeline performs the following stages:

```text
Checkout Repository
        |
        v
Load SSH Credential
        |
        v
Read Inventory
        |
        v
Execute Playbook
        |
        v
Validate Deployment
```

The deployment command is:

```bash
ansible-playbook -i inventory playbook.yml
```

### Automatic deployment flow

When the CD pipeline is triggered:

1. Jenkins checks out the repository.
2. Required SSH credentials are provided securely.
3. Ansible reads the inventory.
4. Ansible connects to the target using SSH.
5. The playbook executes deployment tasks.
6. Jenkins reports the deployment status.

### Example Jenkinsfile

An illustrative pipeline (adjust the credential ID to match your Jenkins setup):

```groovy
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Deploy') {
            steps {
                withCredentials([sshUserPrivateKey(
                    credentialsId: 'ec2-ssh-key',
                    keyFileVariable: 'SSH_KEY'
                )]) {
                    sh '''
                        ansible-playbook -i inventory playbook.yml \
                          --private-key "$SSH_KEY"
                    '''
                }
            }
        }

        stage('Validate') {
            steps {
                withCredentials([sshUserPrivateKey(
                    credentialsId: 'ec2-ssh-key',
                    keyFileVariable: 'SSH_KEY'
                )]) {
                    sh '''
                        ansible -i inventory webserver -m ping \
                          --private-key "$SSH_KEY"
                    '''
                }
            }
        }
    }
}
```

## Validation

Verify Ansible connectivity:

```bash
ansible -i inventory webserver -m ping
```

Expected output:

```text
web1 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

Verify the deployed service:

```bash
ansible -i inventory webserver -m shell \
  -a "systemctl is-active nginx"
```

Expected output:

```text
active
```

## Failure Handling

If an Ansible task fails, the playbook returns a non-zero exit status and Jenkins marks the deployment as **failed**.

Common causes:

- Invalid SSH credentials
- Target host unreachable
- Incorrect inventory
- Playbook/task failure
- Package or service failure

Check the Jenkins console output to identify the failed task.

## Security Practices

- Never commit private keys or passwords to Git.
- Store SSH credentials in Jenkins Credentials.
- Use Ansible Vault for sensitive variables.
- Restrict access to Jenkins credentials.
- Follow least-privilege access.
- Do not expose secrets in pipeline logs.

## Acceptance Criteria

| Requirement                  | Implementation                                                             |
| ---------------------------- | -------------------------------------------------------------------------- |
| Automatic playbook execution | Jenkins executes the Ansible playbook through the CD pipeline              |
| Target systems               | Ansible inventory defines deployment targets                               |
| Secrets management           | Jenkins Credentials securely provide SSH authentication                    |
| Detailed CD documentation    | CD workflow, inventory, execution, validation, and security are documented |

## References

| Reference                                                                                               | Description                    |
| ------------------------------------------------------------------------------------------------------- | ------------------------------ |
| [Ansible Documentation](https://docs.ansible.com/)                                                      | Official Ansible documentation |
| [Ansible Inventory Guide](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html) | Inventory configuration        |
| [Ansible Playbooks](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_intro.html)        | Playbook documentation         |
| [Ansible Vault](https://docs.ansible.com/ansible/latest/vault_guide/index.html)                         | Secrets management             |
| [Jenkins Documentation](https://www.jenkins.io/doc/)                                                    | Jenkins CI/CD documentation    |
| [Jenkins Credentials](https://www.jenkins.io/doc/book/using/using-credentials/)                         | Jenkins credential management  |

## Document Info

| Author       | Created On | Version | Last Updated | L0 Reviewer | L1 Reviewer | L2 Reviewer |
| ------------ | ---------- | ------- | ------------ | ----------- | ----------- | ----------- |
| Sahil Butola | 01-10-2026 | v1.0    | 01-10-2026   |             |             |             |
