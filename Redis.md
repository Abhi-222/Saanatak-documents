# Redis Documentation

<p align="center">
  <img width="500" height="500" alt="Redis Icon" src="https://github.com/user-attachments/assets/efb92d42-cb23-444b-b13a-0e431b5ffa92" />
</p>

---
## Author Table

| **Author** | **Created on** | **Version** | **Last edited on**  | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer**       |
| ---------- | -------------- | ----------- | ------------------- |---------------- | ----------------|---------------------- |
| Sahil      | 10-09-26       | 1.0         | 10-09-26            | `Vishal/Divya M`|   `Aayush Verma`| `Mahesh Kumar / Varun`|

---

# Table of Contents

1. [Purpose](#1-purpose)
2. [Key Features](#2-key-features)
3. [Getting Started](#3-getting-started)
4. [Dependencies](#4-dependencies)
5. [How to Setup/Install Redis](#5-how-to-setupinstall-redis)
6. [Basic Redis CLI Operations](#6-basic-redis-cli-operations)
7. [Maintenance](#7-maintenance)
8. [Monitoring](#8-monitoring)
9. [Disaster Recovery](#9-disaster-recovery)
10. [High Availability](#10-high-availability)
11. [Conclusion](#11-conclusion)
12. [FAQs](#12-faqs)
13. [Contact Information](#13-contact-information)
14. [References](#14-references)

---

# 1. Purpose

This document explains how to install, configure, and manage Redis on AWS EC2. It covers everything needed to get Redis up and running — including setup steps, basic commands, day-to-day maintenance, monitoring, backup/recovery, and high availability — so that anyone on the team can follow it and deploy Redis confidently, even without prior experience.

---

# 2. Key Features

| **Feature** | **Description** |
| --- | --- |
| In-Memory Storage | Data is stored in RAM, giving sub-millisecond read/write latency. |
| Rich Data Structures | Supports Strings, Hashes, Lists, Sets, Sorted Sets, Bitmaps, HyperLogLogs, Streams, and Geospatial indexes. |
| Persistence Options | RDB (point-in-time snapshots) and AOF (append-only file logging) for durability. |
| High Availability & Clustering | Redis Sentinel provides automatic failover and monitoring; Redis Cluster enables horizontal scaling by sharding data across nodes. |
| Atomic Operations | Operations like INCR/DECR are atomic, useful for counters and locks. |

---

# 3. Getting Started

## Prerequisites

Before installing Redis, ensure the following prerequisites are available:

| **Requirement** | **Details / Verification** |
| ----------------| -------------------------- |
| AWS EC2 | EC2 instance with a supported Linux distribution. |
| Operating System | Ubuntu 26.04 LTS "Resolute Raccoon" (also works on 22.04/24.04). |
| Sudo Access | Required for package installation and configuration. |
| Security Group | Inbound rule allowing TCP port 6379 from trusted sources/clients. |
| Network Connectivity | Client applications must be able to reach the instance on port 6379. |

---

## Software Overview

| **Component / Command** | **Purpose** |
| ----------------------- | ----------- |
| `redis-server` | Main Redis database service/daemon. |
| `redis.conf` | Main Redis configuration file (`/etc/redis/redis.conf`). |
| `redis-cli` | Command-line interface for executing Redis commands (included in `redis-tools`). |
| `redis-tools` | Package providing client utilities: `redis-cli`, `redis-benchmark`, `redis-check-aof`, `redis-check-rdb`. |
| `systemctl` | Used to start, stop, enable, and check the Redis service. |

---

## System Requirements

The setup is performed on AWS EC2 using Ubuntu.

| **Requirement** | **Environment** |
| --- | --- |
| Platform | AWS EC2 |
| Instance Type | m7i.xlarge (4 vCPU, 16 GB RAM) |
| OS | Ubuntu 26.04 LTS "Resolute Raccoon" |
| Redis Version | 8.0.5 (default via Ubuntu 26.04 "Resolute Raccoon" universe repo) |
| RAM | 2 GB minimum for Redis workload (m7i.xlarge provides 16 GB headroom) |
| Disk Space | 10 GB or higher (for RDB/AOF persistence) |

> For production deployments, hardware sizing should be based on workload, dataset size, and expected throughput.

---

## Important Ports

| **Port** | **Protocol** | **Purpose** |
| -------- | ------------ | ----------- |
| `22`     | TCP          | Used to establish an SSH connection to the server and access a shell. |
| `6379`   | TCP          | Default Redis server port used by clients to connect to the Redis instance. |

> Port `6379` should be restricted to trusted clients or application networks via the security group.

---

# 4. Dependencies

Redis on Ubuntu 26.04 is installed directly from the default `universe` repository — no external packages need to be added beforehand.

| **Dependency** | **Purpose** |
| -------------- | ----------- |
| `apt`          | Installs and manages the Redis packages. |
| `systemd`      | Manages the Redis service. |
| `universe repository` | Ubuntu component that hosts the `redis-server`/`redis-tools` packages (usually enabled by default). |

---

# 5. How to Setup/Install Redis

## 5.1 Update Package Index

```bash
sudo apt update
```
<details>
<summary><strong>Screenshot - Package index updated</strong></summary>
<img width="1440" height="305" alt="Screenshot 2026-09-16 at 1 49 26 PM" src="https://github.com/user-attachments/assets/ea65f00d-1536-4f94-b6db-33631715e7d4" />
</details>

---

## 5.2 Install Redis

```bash
sudo apt install redis-server redis-tools -y
```
<details>
<summary><strong>Screenshot - Redis packages installed</strong></summary>
<img width="1261" height="626" alt="Screenshot 2026-09-16 at 1 52 16 PM" src="https://github.com/user-attachments/assets/ba833162-750f-4bfc-96a3-33dd53e7109b" />
</details>

This installs the `redis-server` package for the daemon and `redis-tools`, which provides `redis-cli` and related client utilities. The package also creates `/etc/redis/redis.conf` and registers the `redis-server.service` systemd unit automatically, pre-configured with `--supervised systemd --daemonize no`.

---

## 5.3 Start and Enable Redis

```bash
sudo systemctl enable redis-server
sudo systemctl start redis-server
sudo systemctl status redis-server --no-pager
```

<details>
<summary><strong>Screenshot - Redis service enabled and running</strong></summary>
<img width="866" height="343" alt="Screenshot 2026-09-16 at 1 52 51 PM" src="https://github.com/user-attachments/assets/8bf89bb3-d6b9-424a-bd5b-020fb3d84569" />
</details>

---

## 5.4 Verify Installation

```bash
redis-cli ping
```

A response of `PONG` confirms Redis is installed and running correctly.

Verify the exact package/version installed:

```bash
apt list --installed | grep redis
redis-server --version
```

<details>
<summary><strong>Screenshot - Redis ping and version verified</strong></summary>
<img width="816" height="155" alt="Screenshot 2026-09-16 at 1 53 23 PM" src="https://github.com/user-attachments/assets/837eae1f-7e9f-43c3-8d31-c3206b0f49dd" />
</details>

---

# Configuration

The main Redis configuration file is:

```text
/etc/redis/redis.conf
```

To Edit the configuration:

```bash
sudo nano /etc/redis/redis.conf
sudo systemctl restart redis-server

```

<!-- <details>
<summary><strong>Screenshot - redis.conf edited with requirepass and appendonly set</strong></summary>
<img width="802" height="571" alt="Screenshot 2026-09-16 at 2 01 42 PM" src="https://github.com/user-attachments/assets/5929659f-3943-4ba3-907e-14d7a2b573de" />
</details> -->

---

# 6. Basic Redis CLI Operations

After installing Redis, basic commands can be used to verify connectivity, set and retrieve keys, and confirm expiry behavior.

Connect to the Redis CLI

```bash
redis-cli 
```

Set a test key:

```bash
SET service_status "SUCCESS"
```


Retrieve the key:

```bash
GET service_status
```

Set a key with a TTL (in seconds):

```bash
SET session_token "abc123" EX 60
```

Check remaining time-to-live:

```bash
TTL session_token
```

Delete a Key

```bash
DEL service_status
```


<details>
<summary><strong>Screenshot - CLI SET/GET/TTL/DEL verified</strong></summary>
<img width="478" height="238" alt="Screenshot 2026-09-16 at 1 55 48 PM" src="https://github.com/user-attachments/assets/fbc778b8-625d-4395-ab72-9dae10676d13" />
</details>

---

# 7. Maintenance

Regular maintenance helps keep the Redis instance healthy and reliable.

| **Task**        | **Command / Action** |
| --------------- | -------------------- |
| Check Service   | `sudo systemctl status redis-server` |
| Restart Service | `sudo systemctl restart redis-server` |
| Update Package  | `sudo apt update && sudo apt upgrade redis-server -y` |
| Check Version   | `redis-server --version` |
| Check CLI       | `redis-cli ping` |
| Check Disk      | `df -h` |
| Check System Resources | `free -h` / `nproc` |
| Check Logs             | `sudo journalctl -u redis-server` |
| Rewrite AOF            | `redis-cli BGREWRITEAOF` |
| Check Slow Queries     | `redis-cli SLOWLOG GET` |

---

# 8. Monitoring

Monitoring helps identify performance problems, resource exhaustion, and service availability issues.

| **Metric / Check**   | **Purpose**                 | **Command / Tool** |
| -------------------- | ----------------------------| ------------------ |
| Service Status       | Verify service health          | `sudo systemctl status redis-server` |
| Server Stats         | View memory, clients, stats    | `redis-cli info` |
| Live Command Monitor | Watch commands in real time    | `redis-cli monitor` |
| Memory Usage         | Detect memory pressure         | `redis-cli info memory` / `free -h` |
| CPU Usage            | Detect CPU saturation          | `top` / `htop` |
| Disk Usage           | Prevent storage exhaustion     | `df -h` |
| Redis Port           | Verify port 6379               | `ss -lntp \| grep 6379` |
| Connected Clients    | Detect connection leaks/spikes | `redis-cli info clients` |
| Latency              | Measure command response time  | `redis-cli --latency` |
| Service Logs         | View recent logs               | `journalctl -u redis-server -n 100` |
| Live Logs            | Monitor logs continuously      | `journalctl -u redis-server -f` |

Key metrics to watch: `used_memory`, `connected_clients`, `keyspace_hits` / `keyspace_misses`, `evicted_keys`, and `rejected_connections`.

> `redis-cli monitor` streams every command in real time and can noticeably impact performance — avoid running it on high-throughput production instances for extended periods.

---

# 9. Disaster Recovery

Disaster Recovery (DR) consists of processes, strategies, and tools used to recover Redis services and data after unexpected failures.

| **Failure Scenario**      | **Protection Mechanism** |
| ------------------------- | ------------------------ |
| Instance Failure          | RDB/AOF backups restored to a new instance |
| Disk Failure              | Replicated data and off-instance backups |
| Data Corruption           | Backup and restore |
| Accidental Deletion       | Backup (RDB snapshot / AOF replay) |
| Availability Zone Failure | Multi-AZ deployment with replicas |
| Region Failure            | Cross-region backup strategy |

RDB snapshots (`dump.rdb`) and AOF logs should be copied regularly to a separate storage location (e.g., S3) outside the Redis host.

---

# 10. High Availability

High Availability (HA) ensures that Redis remains accessible with minimal downtime even when individual infrastructure components fail.

| **HA Component**   | **Recommendation** |
| ------------------ | ------------------ |
| Replication        | Configure master-replica replication for read scalability and failover. |
| Sentinel           | Use Redis Sentinel (minimum 3 nodes for quorum) for automatic failover and monitoring. |
| Clustering         | Use Redis Cluster to shard data across nodes for horizontal scale and no single point of failure. |
| Availability Zones | Distribute replicas across AZs. |
| Backups            | Maintain independent RDB/AOF backups. |
| Monitoring         | Configure health monitoring and alerts. |
| Capacity           | Maintain sufficient headroom for node failures. |

---

# 11. Conclusion

Redis provides high performance, flexibility, and in-memory speed for caching, session management, messaging, and real-time data use cases. A properly configured Redis deployment on AWS EC2 improves application responsiveness while maintaining data durability and availability. This document should serve as a reliable reference for setting up, operating, and maintaining Redis within the team, reducing ramp-up time for anyone new to the tool.

---

# 12. FAQs

### Is Redis free to use?

Redis versions through 7.2 are open-source (BSD 3-Clause). Redis 7.4–7.8 shipped under a dual RSALv2/SSPLv1 source-available license. Starting with Redis 8.0 (the version used in this doc), Redis Ltd. added AGPLv3 as a third licensing option — users can choose RSALv2, SSPLv1, or AGPLv3. AGPLv3 is OSI-approved, making Redis 8.0+ open source again. Free to use in all cases; commercial redistribution or managed-hosting scenarios should still be reviewed against RSALv2/SSPLv1 terms if AGPLv3 isn't the chosen option.

### Can Redis data be persisted, since it's an in-memory store?

Yes. Redis supports RDB snapshots and AOF logging so data can survive restarts and crashes.

### Does Redis support clustering for large datasets?

Yes. Redis Cluster shards data across multiple nodes to scale horizontally beyond a single machine's memory.

---

# 13. Contact Information

| Name  | Email Address |
| ----- | ------------- |
| Sahil | [sahil.butola.snaatak@mygurukulam.co](mailto:sahil.butola.snaatak@mygurukulam.co) |

---

# 14. References

| **Reference** | **Purpose** |
| --- | --- |
| [Redis Documentation](https://redis.io/docs/latest/) | Official Redis documentation |
| [Redis Persistence](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/) | Reference for Redis persistence (RDB/AOF) details |
| [Redis Sentinel](https://redis.io/docs/latest/operate/oss_and_stack/management/sentinel/) | Reference for Redis Sentinel / High Availability |
| [Redis Licenses](https://redis.io/legal/licenses/) | Official Redis licensing (RSALv2 / SSPLv1 / AGPLv3) |
| [Software Template](https://github.com/OT-MICROSERVICES/documentation-template/wiki/Software-Template) | Software Template this document follows |
