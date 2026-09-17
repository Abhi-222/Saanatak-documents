# POC: Notification Worker Documentation

---

# Author Table

| **Author** | **Created On** | **Version** | **Last Updated** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| :--------- | :------------- | :---------- | :--------------- | :-------------- | :-------------- | :-------------- |
| Sahil      | 16-08-26       | 1.0         | 17-09-26         | `Vishal/Divya M`| `Aayush Verma`  | `Mahesh Kumar / Varun` |

---

## Purpose

The Notification Worker is a Python-based service that reads employee records from Elasticsearch and sends salary-slip email notifications over SMTP. It supports both external and scheduled execution modes.

This POC validates the end-to-end deployment and notification flow on a single AWS EC2 instance. MailHog is used as a local SMTP server to capture and verify outgoing emails without sending real emails.

---

## Pre-requisites

Before starting, ensure the following are available.

* Ubuntu/Linux system or AWS EC2 instance
* Python 3.14.4
* Git 2.53.0
* OpenJDK 21.0.12
* Elasticsearch 7.17.29
* MailHog v1.0.1
* Access to the Notification Worker repository:
  `https://github.com/OT-MICROSERVICES/notification-worker.git`

**AWS-specific:** the EC2 instance security group must allow inbound access on:

* Port `8025` — MailHog Web UI for reviewer demonstration
* Port `22` — SSH access

Port `9200` does not need to be publicly exposed because Elasticsearch is accessed locally from the EC2 instance.

<details>
<summary>Screenshot: EC2 security group inbound rules</summary>

_Add screenshot here_

</details>

---

## System Requirements

| Hardware Specification | POC Configuration  |
| ----------------------- | ------------------ |
| Processor              | x86_64 / amd64     |
| Instance Type          | m7i.xlarge         |
| RAM                    | 7.6 GiB            |
| Swap                   | 0 B                |
| Disk                   | 19 GiB             |
| OS                     | Ubuntu 26.04.1 LTS |

The POC was successfully executed on a single AWS EC2 instance with the above configuration.

---

## Dependencies

### Build time Dependency

| Name   | Version               | Description                              |
| ------ | --------------------- | ---------------------------------------- |
| Python | 3.14.4                | Runtime for `notification_api.py`        |
| pip    | Installed with Python | Installs Python dependencies             |
| Git    | 2.53.0                | Used to clone the application repository |

### Run time Dependency

| Name             | Version | Description                                     |
| ---------------- | ------- | ------------------------------------------------|
| Elasticsearch    | 7.17.29 | Stores employee records read by the worker      |
| MailHog          | v1.0.1  | Local SMTP server used to capture outgoing mail |
| config-with-yaml | 0.1.0   | Loads `config.yaml`                             |
| elasticsearch    | 7.8.0   | Python client for Elasticsearch                 |
| emails           | 0.6     | Python email-sending library                    |
| schedule         | 0.6.0   | Handles scheduled execution                     |

> Note: the Python Elasticsearch client version is defined by the application's `requirements.txt`; it was not independently selected for this POC.

### Other Dependency

| Name | Version         | Description                   |
| ---- | --------------- | ------------------------------|
| Java | OpenJDK 21.0.12 | Runtime used by Elasticsearch |

---

## Important Ports

| Inbound Traffic | Description           |
| ---------------- | ---------------------- |
| 9200              | Elasticsearch API      |
| 1025              | MailHog SMTP listener  |
| 8025              | MailHog Web UI / API   |

| Outbound Traffic | Description                          |
| ------------------ | -------------------------------------- |
| 1025                | Notification Worker → MailHog SMTP    |
| 9200                | Notification Worker → Elasticsearch   |

For the POC, Elasticsearch and MailHog SMTP are accessed locally on the EC2 instance.

---

## Others

| Others               | Description                                                                                                                                                                                                                                                                                                                                                                       |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| POC-only code change | The application originally used `"tls": True` for SMTP. This was changed to `"tls": False` so the worker could communicate with MailHog's non-TLS SMTP endpoint. The original file was backed up as `notification_api.py.bak`. This change is specific to the POC and should not be carried into a production environment without validating the production SMTP configuration. |

---

## Architecture

```text
┌─────────────────────┐
│    Elasticsearch    │
│    :9200            │
│ employee-management │
└──────────┬──────────┘
           │
     reads employees
           │
           ▼
┌─────────────────────┐
│ Notification Worker │
│ notification_api.py │
└──────────┬──────────┘
           │
      SMTP :1025
           │
           ▼
┌─────────────────────┐
│      MailHog        │
│ SMTP :1025          │
│ UI/API :8025        │
└──────────┬──────────┘
           │
           ▼
     Email visible
      in browser
```

---

## Dataflow Diagram

The Notification Worker reads employee email IDs from the `employee-management` index in Elasticsearch.

The worker creates a salary-slip notification and sends it through SMTP. For this POC, MailHog acts as the SMTP server and captures the email so that the notification can be verified without sending a real email.

**Flow:**

```text

|Elasticsearch| → |Notification Worker| → |SMTP| → |MailHog| → |Captured Email|
   
```
---

## Step-by-step installation of Notification Worker

### Step 1: Installation of software dependencies

### Elasticsearch 7.17.29

Download the Elasticsearch Debian package:

```bash
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.17.29-amd64.deb
```

Install the package:

```bash
sudo dpkg -i elasticsearch-7.17.29-amd64.deb
```

Configure Elasticsearch in:

```text
/etc/elasticsearch/elasticsearch.yml
```

The POC configuration was:

```yaml
cluster.name: notification-poc
node.name: notification-node-1
network.host: 127.0.0.1
http.port: 9200
discovery.type: single-node
```

Enable and start Elasticsearch:

```bash
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch
```

Verify:

```bash
curl -s http://127.0.0.1:9200
```

<details>
<summary>Screenshot: Elasticsearch running / curl response</summary>
<img width="574" height="315" alt="Screenshot 2026-09-17 at 7 00 08 AM" src="https://github.com/user-attachments/assets/79f17b52-21b7-41ce-be84-64cb373f627f" />
</details>

### Java

OpenJDK 21.0.12 was used for the POC.

Verify:

```bash
java -version
```

<details>
<summary>Screenshot: Elasticsearch running / curl response</summary>
<img width="717" height="88" alt="Screenshot 2026-09-17 at 7 01 21 AM" src="https://github.com/user-attachments/assets/f4b1f0bc-cc68-431c-b7f2-abd8660e0669" />
</details>

### MailHog

MailHog v1.0.1 was installed using the Linux AMD64 binary.

The `go install` method was not used because the required dependency required a newer Go version than the Go version available on the EC2 instance.

After downloading the binary:

```bash
chmod +x MailHog_linux_amd64
sudo mv MailHog_linux_amd64 /usr/local/bin/mailhog
```

Verify:

```bash
mailhog --help
```

Start MailHog:

```bash
mailhog > /tmp/mailhog.log 2>&1 &
```

Verify the listeners:

```bash
sudo ss -lntp | grep -E ':(1025|8025)\b'
```


<details>
<summary>Screenshot: MailHog listeners (ss output)</summary>
<img width="892" height="88" alt="Screenshot 2026-09-17 at 7 01 57 AM" src="https://github.com/user-attachments/assets/37a9139b-7be6-48f9-a44b-eaf13c1a9c50" />
</details>

---

### Step 2: Build / Artifact Generation

Clone the repository:

```bash
git clone https://github.com/OT-MICROSERVICES/notification-worker.git
cd notification-worker
```

Create a Python virtual environment:

```bash
python3 -m venv venv
source venv/bin/activate
```

Install application dependencies:

```bash
pip install -r requirements.txt
```

Verify:

```bash
pip list | grep -E 'emails|elasticsearch|config-with-yaml|schedule'
```
<details>
<summary>Screenshot: MailHog listeners (ss output)</summary>
<img width="1087" height="119" alt="Screenshot 2026-09-17 at 7 04 29 AM" src="https://github.com/user-attachments/assets/a142327e-6fe2-4af5-96f1-3e807272cbf4" />
</details>


---

### Step 3: Application Deployment

### Verify Elasticsearch

Confirm Elasticsearch is running:

```bash
curl -s http://127.0.0.1:9200
```

<details>
<summary>Screenshot: Elasticsearch running / curl response</summary>
<img width="574" height="315" alt="Screenshot 2026-09-17 at 7 00 08 AM" src="https://github.com/user-attachments/assets/79f17b52-21b7-41ce-be84-64cb373f627f" />
</details>

### Create Test Employee Data

Create the `employee-management` index and add a test employee:

```bash
curl -X PUT "http://127.0.0.1:9200/employee-management/_doc/1" \
  -H 'Content-Type: application/json' \
  -d '{
    "employee_id": "POC001",
    "name": "POC Employee",
    "email_id": "employee01@example.com"
  }'
```

Add a second employee for multiple-recipient testing:

```bash
curl -X PUT "http://127.0.0.1:9200/employee-management/_doc/2" \
  -H 'Content-Type: application/json' \
  -d '{
    "employee_id": "POC002",
    "name": "POC Employee 2",
    "email_id": "employee02@example.com"
  }'
```

Verify the document count:

```bash
curl -s http://127.0.0.1:9200/employee-management/_count
```


<details>
<summary>Screenshot: Employee documents created / count response</summary>
<img width="1120" height="48" alt="Screenshot 2026-09-17 at 7 06 24 AM" src="https://github.com/user-attachments/assets/c59e207d-d865-44a6-bbf8-5e4b92f12a87" />
</details>

---

### Start MailHog

```bash
mailhog > /tmp/mailhog.log 2>&1 &
```

Verify:

```bash
sudo ss -lntp | grep -E ':(1025|8025)\b'
```

<details>
<summary>Screenshot: Employee documents created / count response</summary>
<img width="906" height="87" alt="Screenshot 2026-09-17 at 7 07 50 AM" src="https://github.com/user-attachments/assets/7c2e8d95-49a0-456d-8e8d-8ed744d8b420" />
</details>

---

### Configure the Worker

Update `config.yaml`:

```yaml
---
smtp:
  from: "test@example.com"
  username: "test"
  password: "test"
  smtp_server: "127.0.0.1"
  smtp_port: "1025"

elasticsearch:
  username: "elastic"
  password: "elastic"
  host: "127.0.0.1"
  port: 9200
```

For this POC, Elasticsearch security was not enabled, so the username and password values are not required for the local Elasticsearch connection.

Set the configuration file path:

```bash
export CONFIG_FILE=$HOME/notification-worker/config.yaml
```

Verify:

```bash
echo $CONFIG_FILE
```

<details>
<summary>Screenshot: Employee documents created / count response</summary>
<img width="981" height="87" alt="Screenshot 2026-09-17 at 7 13 07 AM" src="https://github.com/user-attachments/assets/e38fa7b2-2dfd-4e47-87a7-cda1da91264f" />
</details>

---

### Run the Notification Worker

Run external mode:

```bash
python3 notification_api.py --mode external
```

The application:

1. Loads `config.yaml`
2. Connects to Elasticsearch
3. Reads employee records from `employee-management`
4. Gets the `email_id`
5. Creates the salary-slip notification
6. Sends the notification through SMTP
7. MailHog captures the email

<details>
<summary>Screenshot: Terminal output of `notification_api.py --mode external`</summary>
<img width="1440" height="176" alt="Screenshot 2026-09-17 at 7 26 32 AM" src="https://github.com/user-attachments/assets/fdd96431-1184-4025-a6b7-f89c244d2b76" />
</details>

---

### Verify Notification

Check MailHog API:

```bash
curl -s http://127.0.0.1:8025/api/v1/messages
```

If a message appears in the response, the notification was successfully sent and captured.

For the browser-based demo, open:

```text
http://<EC2-PUBLIC-IP>:8025
```


<details>
<summary>Screenshot: MailHog Web UI showing captured email (reviewer demo)</summary>

<img width="1440" height="341" alt="Screenshot 2026-09-17 at 7 28 55 AM" src="https://github.com/user-attachments/assets/f9e3542c-54aa-40d8-a714-b8e703f21eeb" />

</details>

<details>
<summary>Screenshot: `curl` response from MailHog API showing captured message</summary>
<img width="1081" height="53" alt="Screenshot 2026-09-17 at 7 36 11 AM" src="https://github.com/user-attachments/assets/9287c428-e3ba-4ee6-a838-ecec8e873a9a" />
</details> 

---

### Scheduled Mode

The application also supports scheduled execution:

```bash
python3 notification_api.py --mode scheduled
```

The process remains running and waits for the scheduled execution time.

The current application code uses an hourly schedule:

```python
schedule.every().hour.do(send_mail_to_all_users)
```

**Known discrepancy:** Repository documentation mentions a monthly schedule, while the current implementation uses an hourly schedule. This should be reviewed with the application owner.

<details>
<summary>Screenshot: Terminal output of `notification_api.py --mode scheduled`</summary>
<img width="1081" height="341" alt="Screenshot 2026-09-17 at 7 31 51 AM" src="https://github.com/user-attachments/assets/11a9faf1-870e-42ef-be62-59181cbcafdd" />
</details>

Stop the running process with:

```text
Ctrl+C
```

---

### Quick Health-Check Sequence

Once the application is installed and configured:

**1. Check Elasticsearch data**

```bash
curl -s http://127.0.0.1:9200/employee-management/_count
```

Expected:

```json
{"count":2}
```

**2. Check MailHog**

```bash
curl -s http://127.0.0.1:8025/api/v1/messages
```

**3. Run notification**

```bash
python3 notification_api.py --mode external
```

**4. Verify email**

```bash
curl -s http://127.0.0.1:8025/api/v1/messages
```

A captured email should now appear.

---

## Monitoring

Production monitoring and automated metrics are **not implemented in this POC**.

Manual validation performed during the POC included:

* Elasticsearch availability check
* Elasticsearch document count check
* MailHog SMTP listener check
* MailHog Web UI/API verification
* Notification Worker execution
* Email delivery verification through MailHog
* Scheduled mode process verification

#### Metrics

| Parameter                  | Description                               | Priority | Threshold                             |
| -------------------------- | ----------------------------------------- | ---------| ------------------------------------- |
| Elasticsearch availability | Checks whether Elasticsearch is reachable | High     | Service must be reachable             |
| Employee document count    | Confirms employee records are available   | High     | At least 1 for testing                |
| MailHog SMTP listener      | Confirms SMTP endpoint is available       | High     | Port 1025 listening                   |
| Captured messages          | Confirms notification delivery            | High     | Message should appear after execution |

#### Health check

| Name                | Type   | InitialDelaySeconds | PeriodSeconds | TimeoutSeconds | SuccessThreshold | FailureThreshold |
| --------------------| -------| --------------------| --------------| -------------- | -----------------| -----------------|
| Elasticsearch       | Manual | N/A                 | N/A           | N/A            | 1                | 1                |
| MailHog             | Manual | N/A                 | N/A           | N/A            | 1                | 1                |
| Notification Worker | Manual | N/A                 | N/A           | N/A            | 1                | 1                |

---

## Logging

The Notification Worker output is displayed in the terminal during execution.

MailHog output was redirected to:

```text
/tmp/mailhog.log
```

MailHog logs can be checked using:

```bash
cat /tmp/mailhog.log
```

Elasticsearch logs can be checked using:

```bash
sudo journalctl -u elasticsearch
```

or:

```bash
sudo ls /var/log/elasticsearch/
```

---

## Disaster Recovery

Not applicable for this POC.

The setup uses a single EC2 instance and does not include production backup, replication or recovery mechanisms.

---

## High Availability

Not applicable for this POC.

The complete setup runs on a single EC2 instance without redundancy or failover configuration.

---

## Troubleshooting

| Issue                                           | Likely Cause                                                  | Resolution                                                                         |
| ----------------------------------------------- | ------------------------------------------------------------- | -----------------------------------------------------------------------------------|
| `ModuleNotFoundError: No module named 'emails'` | Python dependencies were not installed                        | Run `pip install -r requirements.txt`                                              |
| Elasticsearch fails to start                    | Elasticsearch configuration or system requirement issue       | Check `sudo systemctl status elasticsearch` and `sudo journalctl -u elasticsearch` |
| MailHog `go install` fails                      | Installed Go version does not satisfy dependency requirements | Use the MailHog Linux AMD64 binary                                                 |
| Can't view MailHog UI                           | EC2 security group does not allow port 8025                   | Allow port 8025 for the reviewer/demo source                                       |
| No email appears in MailHog                     | SMTP configuration or TLS mismatch                            | Verify SMTP port is `1025` and the POC configuration uses `tls: False`             |
| `_count` returns `0`                            | Employee document is missing                                  | Create a document in the `employee-management` index                               |
| Elasticsearch connection fails                  | Elasticsearch is not running or wrong host/port               | Verify `curl http://127.0.0.1:9200`                                                |
| MailHog SMTP connection fails                   | MailHog is not running                                        | Verify ports `1025` and `8025` using `ss`                                          |

---

## FAQs

### Does this POC send real emails?

No. MailHog captures outgoing emails locally, so the POC does not send the test notifications to an external SMTP service.

### Can this run on any cloud platform?

The POC was executed on AWS EC2, but the application itself is not dependent on AWS. A similar Linux-based setup can be used on another cloud platform.

### Why is MailHog used?

MailHog provides a local SMTP endpoint and web interface, allowing outgoing emails to be captured and verified safely during the POC.

### What is the notification flow?

```text
Elasticsearch
     ↓
Notification Worker
     ↓
SMTP
     ↓
MailHog
     ↓
Captured Email
```

### What is the difference between external and scheduled mode?

`external` executes the notification flow once and exits.

`scheduled` keeps the process running and waits for the configured schedule. The current application code uses an hourly schedule.

---

## Contact Information

| Name  | Email address |
| ------| --------------|
| Sahil | [sahil.butola.snaatak@mygurukulam.co](mailto:sahil.butola.snaatak@mygurukulam.co)|

---

## References

| Link                                                                                 | Description                                   |
| --------------------------------------------------------------------------------------- | -------------------------------------------------- |
| https://github.com/OT-MICROSERVICES/notification-worker                              | Source repository for Notification Worker     |
| https://github.com/OT-MICROSERVICES/documentation-template/wiki/Application-Template | Documentation template used for this document |
| https://github.com/mailhog/MailHog                                                   | MailHog project and reference                 |

---

## POC Result

The Notification Worker POC was successfully validated on AWS EC2.

The following end-to-end flow was successfully tested:

**Employee data in Elasticsearch → Notification Worker → SMTP → MailHog → Captured email**

The POC validated:

* Elasticsearch connectivity
* Employee data retrieval
* Single-recipient notification
* Multiple-recipient notification
* SMTP communication with MailHog
* Email capture and verification through MailHog
* Scheduled mode execution
* Browser-based demonstration of captured notifications

The POC successfully demonstrated that the Notification Worker can be configured and executed on a single AWS EC2 instance and that generated notifications can be verified without sending real emails.
