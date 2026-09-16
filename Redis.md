# Redis Documentation

<p align="center">
  <img width="700" height="700" alt="Redis Icon" src="https://github.com/user-attachments/assets/efb92d42-cb23-444b-b13a-0e431b5ffa92" />
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

The purpose of this document is to provide a structured guide for Redis installation, configuration, basic CLI operations, maintenance, monitoring, disaster recovery, and high availability on AWS EC2.

It covers the required prerequisites, system requirements, important ports, installation steps, configuration parameters, and verification of a working Redis instance.

---

# 2. Key Features

| **Feature** | **Description** |
| --- | --- |
| In-Memory Storage | Data is stored in RAM, giving sub-millisecond read/write latency. |
| Rich Data Structures | Supports Strings, Hashes, Lists, Sets, Sorted Sets, Bitmaps, HyperLogLogs, Streams, and Geospatial indexes. |
| Persistence Options | RDB (point-in-time snapshots) and AOF (append-only file logging) for durability. |
| Replication | Supports master-replica replication for read scalability and failover. |
| High Availability | Redis Sentinel provides automatic failover and monitoring. |
| Clustering | Redis Cluster allows horizontal scaling by sharding data across multiple nodes. |
| Atomic Operations | Operations like INCR/DECR are atomic, useful for counters and locks. |
| AWS Support | Can be deployed on AWS EC2 instances. |

---

# 3. Getting Started

## Prerequisites

Before installing Redis, ensure the following prerequisites are available:

| **Requirement** | **Details / Verification** |
| --- | --- |
| AWS EC2 | EC2 instance with a supported Linux distribution. |
| Operating System | Ubuntu 26.04 LTS "Resolute" (also works on 22.04/24.04). |
| Network Connectivity | Client applications should be able to reach the instance on the required port. |
| Sudo Access | Required for package installation and configuration. |
| Security Group | Required Redis port must be allowed. |

---

## Software Overview

| **Component / Command** | **Purpose** |
| --- | --- |
| `redis-server` | Main Redis database service/daemon. |
| `redis.conf` | Main Redis configuration file. |
| `redis-cli` | Command-line interface for executing Redis commands. |
| `redis-tools` | Package providing `redis-cli` and related client utilities. |
| `systemctl` | Used to start, stop, enable, and check the Redis service. |

---

## System Requirements

The setup is performed on AWS EC2 using Ubuntu.

| **Requirement** | **Environment** |
| --- | --- |
| Platform | AWS EC2 |
| Instance Type | m7i.xlarge (4 vCPU) |
| OS | Ubuntu 26.04 LTS "Resolute" |
| Redis Version | 8.0.5 (default via Ubuntu 26.04 "Resolute" universe repo) |
| RAM | 2 GB minimum (size to dataset + overhead) |
| Disk Space | 10 GB or higher (for RDB/AOF persistence) |

> For production deployments, hardware sizing should be based on workload, dataset size, and expected throughput.

---

## Important Ports

| **Port** | **Protocol** | **Purpose** |
| --- | --- | --- |
| `22` | TCP | Used to establish an SSH connection to the server and access a shell. |
| `6379` | TCP | Default Redis server port used by clients to connect to the Redis instance. |

> Port `6379` should be restricted to trusted clients or application networks via the security group.

---

# 4. Dependencies

Redis on Ubuntu 26.04 is installed directly from the default `universe` repository — no external packages need to be added beforehand.

| **Dependency** | **Purpose** |
| --- | --- |
| `apt` | Installs and manages the Redis packages. |
| `systemd` | Manages the Redis service. |

---

# 5. How to Setup/Install Redis

## 5.1 Update Package Index

```bash
sudo apt update
```

---

## 5.2 Install Redis

```bash
sudo apt install redis-server redis-tools -y
```

This installs the `redis-server` package for the daemon and `redis-tools`, which provides `redis-cli` and related client utilities. The package also creates `/etc/redis/redis.conf` and registers the `redis-server.service` systemd unit automatically, pre-configured with `--supervised systemd --daemonize no`.

---

## 5.3 Start and Enable Redis

Enable the service:

```bash
sudo systemctl enable redis-server
```

Start the service:

```bash
sudo systemctl start redis-server
```

Check the service:

```bash
sudo systemctl status redis-server --no-pager
```

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

---

# Configuration

The main Redis configuration file is:

```text
/etc/redis/redis.conf
```

Edit the configuration:

```bash
sudo nano /etc/redis/redis.conf
```

Example configuration changes:

```conf
bind 127.0.0.1 -::1

requirepass YourStrongPassword

appendonly yes

maxmemory 512mb
maxmemory-policy allkeys-lru
```

After editing the configuration file, restart Redis to apply changes:

```bash
sudo systemctl restart redis-server
```

---

# 6. Basic Redis CLI Operations

After installing Redis, basic commands can be used to verify connectivity, set and retrieve keys, and confirm expiry behavior.

## 6.1 Connect to Redis

Connect to the Redis CLI:

```bash
redis-cli
```

## 6.2 Set a Key

Set a test key:

```bash
SET service_status "SUCCESS"
```

## 6.3 Get a Key

Retrieve the key:

```bash
GET service_status
```

## 6.4 Set a Key with Expiry

Set a key with a TTL (in seconds):

```bash
SET session_token "abc123" EX 60
```

Check remaining time-to-live:

```bash
TTL session_token
```

## 6.5 Delete a Key

```bash
DEL service_status
```

## 6.6 Verified Output

The commands were successfully verified on the EC2 instance:

```text
127.0.0.1:6379> SET service_status "SUCCESS"
OK
127.0.0.1:6379> GET service_status
"SUCCESS"
```

This confirms that **Redis is running, CLI connectivity is working, and keys can be set and retrieved successfully.**

---

# 7. Maintenance

Regular maintenance helps keep the Redis instance healthy and reliable.

| **Task** | **Command / Action** |
| --- | --- |
| Check Service | `sudo systemctl status redis-server` |
| Restart Service | `sudo systemctl restart redis-server` |
| Update Package | `sudo apt update && sudo apt upgrade redis-server -y` |
| Check Version | `redis-server --version` |
| Check CLI | `redis-cli ping` |
| Check Disk | `df -h` |
| Check System Resources | `free -h` / `nproc` |
| Check Logs | `sudo journalctl -u redis-server` |
| Rewrite AOF | `redis-cli BGREWRITEAOF` |
| Check Slow Queries | `redis-cli SLOWLOG GET` |

---

# 8. Monitoring

Monitoring helps identify performance problems, resource exhaustion, and service availability issues.

| **Metric / Check** | **Purpose** | **Command / Tool** |
| --- | --- | --- |
| Service Status | Verify service health | `sudo systemctl status redis-server` |
| Server Stats | View memory, clients, stats | `redis-cli info` |
| Live Command Monitor | Watch commands in real time | `redis-cli monitor` |
| Memory Usage | Detect memory pressure | `redis-cli info memory` / `free -h` |
| CPU Usage | Detect CPU saturation | `top` / `htop` |
| Disk Usage | Prevent storage exhaustion | `df -h` |
| Redis Port | Verify port 6379 | `ss -lntp \| grep 6379` |
| Service Logs | View recent logs | `journalctl -u redis-server -n 100` |
| Live Logs | Monitor logs continuously | `journalctl -u redis-server -f` |

Key metrics to watch: `used_memory`, `connected_clients`, `keyspace_hits` / `keyspace_misses`, `evicted_keys`, and `rejected_connections`.

---

# 9. Disaster Recovery

Disaster Recovery (DR) consists of processes, strategies, and tools used to recover Redis services and data after unexpected failures.

| **Failure Scenario** | **Protection Mechanism** |
| --- | --- |
| Instance Failure | RDB/AOF backups restored to a new instance |
| Disk Failure | Replicated data and off-instance backups |
| Data Corruption | Backup and restore |
| Accidental Deletion | Backup (RDB snapshot / AOF replay) |
| Availability Zone Failure | Multi-AZ deployment with replicas |
| Region Failure | Cross-region backup strategy |

RDB snapshots (`dump.rdb`) and AOF logs should be copied regularly to a separate storage location (e.g., S3) outside the Redis host. Restoring involves stopping Redis, replacing the data file in the configured data directory, and restarting the service.

---

# 10. High Availability

High Availability (HA) ensures that Redis remains accessible with minimal downtime even when individual infrastructure components fail.

| **HA Component** | **Recommendation** |
| --- | --- |
| Replication | Configure master-replica replication for read scalability and failover. |
| Sentinel | Use Redis Sentinel (minimum 3 nodes for quorum) for automatic failover and monitoring. |
| Clustering | Use Redis Cluster to shard data across nodes for horizontal scale and no single point of failure. |
| Availability Zones | Distribute replicas across AZs. |
| Backups | Maintain independent RDB/AOF backups. |
| Monitoring | Configure health monitoring and alerts. |
| Capacity | Maintain sufficient headroom for node failures. |

---

# 11. Conclusion

Redis provides high performance, flexibility, and in-memory speed for caching, session management, messaging, and real-time data use cases. A properly configured Redis deployment on AWS EC2 improves application responsiveness while maintaining data durability and availability.

---

# 12. FAQs

### Is Redis free to use?

Redis versions through 7.2 are open-source (BSD 3-Clause). From Redis 7.4 onward, Redis Ltd. moved core Redis to a dual RSALv2/SSPLv1 source-available license — free to use in most cases, but commercial redistribution/hosting scenarios should be reviewed against current license terms.

### Can Redis data be persisted, since it's an in-memory store?

Yes. Redis supports RDB snapshots and AOF logging so data can survive restarts and crashes.

### Does Redis support clustering for large datasets?

Yes. Redis Cluster shards data across multiple nodes to scale horizontally beyond a single machine's memory.

---

# 13. Contact Information

| Name | Email Address |
| --- | --- |
| Sahil | [sahil.butola.snaatak@mygurukulam.co](mailto:sahil.butola.snaatak@mygurukulam.co) |

---

# 14. References

| **Reference** | **Purpose** |
| --- | --- |
| [Redis Documentation](https://redis.io/docs/latest/) | Official Redis documentation |
| [Redis Persistence](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/) | Reference for Redis persistence (RDB/AOF) details |
| [Redis Sentinel](https://redis.io/docs/latest/operate/oss_and_stack/management/sentinel/) | Reference for Redis Sentinel / High Availability |
| [Software Template](https://github.com/OT-MICROSERVICES/documentation-template/wiki/Software-Template) | Software Template this document follows |
