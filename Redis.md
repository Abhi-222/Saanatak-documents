
<p align="center">
  <img width="1500" height="1000" alt="image" src="https://github.com/user-attachments/assets/efb92d42-cb23-444b-b13a-0e431b5ffa92" />
</p>

# OT MS Understanding | Redis | Detailed documentation

## Author Table

| **Author** | **Created on** | **Version** | **Last edited on**  | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ---------- | -------------- | ----------- | ------------------- |---------------- | ----------------|---------------- |
| Sahil      | 10-09-26       | 1.0         | 10-09-26            | `Vishal/Divya M`|   `Aayush Verma`| `Mahesh Kumar / Varun`|

## Table of Contents

1. [Introduction](#1-introduction)
2. [Purpose](#2-purpose)
3. [Key Features](#3-key-features)
4. [Getting Started](#4-getting-started)
   - 4.1 [Pre-requisites](#41-pre-requisites)
   - 4.2 [Software Overview](#42-software-overview)
   - 4.3 [System Requirements](#43-system-requirements)
   - 4.4 [Important Ports](#44-important-ports)
5. [Dependencies](#5-dependencies)
   - 5.1 [Run-time Dependency](#51-run-time-dependency)
   - 5.2 [Other Dependency](#52-other-dependency)
6. [How to Setup/Install Redis](#6-how-to-setupinstall-redis)
   - 6.1 [Step-by-step Installation Instructions](#61-step-by-step-installation-instructions)
   - 6.2 [Configuration](#62-configuration)
7. [Maintenance](#7-maintenance)
8. [Monitoring](#8-monitoring)
9. [Disaster Recovery](#9-disaster-recovery)
10. [High Availability](#10-high-availability)
11. [Troubleshooting](#11-troubleshooting)
12. [FAQs](#12-faqs)
13. [Contact Information](#13-contact-information)
14. [References](#14-references)

---

## 1. Introduction

Redis (**RE**mote **DI**ctionary **S**erver) is an open-source, in-memory data structure store used as a database, cache, message broker, and streaming engine. Because data is held primarily in RAM, Redis delivers sub-millisecond read/write latency, which makes it a common choice for caching layers, session stores, real-time analytics, and pub/sub messaging in microservices architectures.

<!-- Screenshot placeholder: Redis architecture / logo banner -->

## 2. Purpose

Redis is used in the OT-Microservices ecosystem for scenarios such as:

- **Caching** – reducing database load by storing frequently accessed data (e.g., API responses, session tokens) in memory.
- **Session management** – storing user session state for stateless application servers.
- **Rate limiting** – using atomic counters (`INCR`, `EXPIRE`) to throttle API requests.
- **Pub/Sub messaging** – lightweight event broadcasting between microservices.
- **Leaderboards / real-time analytics** – using Sorted Sets for ranking and counting use cases.
- **Message queues** – using Lists or Streams for lightweight job queues.

## 3. Key Features

| Feature | Description |
|---|---|
| In-memory storage | Data is stored in RAM, giving very low read/write latency compared to disk-based databases. |
| Rich data structures | Supports Strings, Hashes, Lists, Sets, Sorted Sets, Bitmaps, HyperLogLogs, Geospatial indexes, and Streams. |
| Persistence options | Supports RDB (point-in-time snapshots) and AOF (append-only file) persistence to recover data after restarts. |
| Replication | Master-replica replication for read scaling and failover. |
| High Availability | Redis Sentinel provides automatic failover; Redis Cluster provides sharding and HA at scale. |
| Atomic operations | Commands like `INCR`, `SETNX` are atomic, making Redis reliable for counters and locks. |
| Pub/Sub | Built-in publish/subscribe messaging system. |
| Lua scripting | Server-side scripting for complex atomic operations. |
| Transactions | `MULTI`/`EXEC` support for grouping commands. |

## 4. Getting Started

### 4.1 Pre-requisites

### 4.2 Software Overview

| Software | Version |
|---|---|
| Redis | 8.0.x (latest stable line as of documentation date) |

### 4.3 System Requirements

| Requirement | Minimum Recommendation |
|---|---|
| Processor/Instance Type | Dual-Core / t2.medium instance (or higher for production workloads) |
| RAM | 4 GB or higher (Redis is memory-bound — size RAM to dataset size plus overhead) |
| ROM (Disk Space) | 10 GB or higher (for persistence files — RDB/AOF) |
| OS Required | Linux (Ubuntu 20.04+/Amazon Linux 2 or later recommended) |

### 4.4 Important Ports

| Port | Description |
|---|---|
| 22 | Used to establish an SSH connection to the EC2 instance for setup and access. |
| 6379 | Default Redis server port used for client connections. |
| 16379 | Default Redis Cluster bus port (used for node-to-node communication when Cluster mode is enabled; typically the client port + 10000). |
| 26379 | Default Redis Sentinel port (used when Sentinel is configured for high availability). |

## 5. Dependencies

### 5.1 Run-time Dependency

| Run-time Dependency | Version | Description |
|---|---|---|
| glibc | System default (Ubuntu 20.04+) | Standard C library required to run the Redis server binary. |
| systemd | System default | Used to manage the Redis service (start/stop/enable on boot). |

### 5.2 Other Dependency

| Other Dependency | Version | Description |
|---|---|---|
| redis-tools / redis-cli | Matches server version | Command-line client used to interact with and administer Redis. |
| build-essential, tcl | Latest (apt) | Required only if compiling Redis from source. |

## 6. How to Setup/Install Redis

### 6.1 Step-by-step Installation Instructions

**Install via apt (Ubuntu/Debian):**

```
sudo apt update
sudo apt install redis-server -y
```

**Start and enable the service:**

```
sudo systemctl start redis-server
sudo systemctl enable redis-server
```

**Verify installation:**

```
redis-cli ping
```

A healthy instance responds with `PONG`.

<!-- Screenshot placeholder: terminal output of redis-cli ping showing PONG -->

### 6.2 Configuration

The main configuration file is located at `/etc/redis/redis.conf`. Common settings to review/change:

| Setting | Purpose |
|---|---|
| `bind` | Restricts which network interfaces Redis listens on (avoid `0.0.0.0` in production without protection). |
| `port` | Port Redis listens on (default `6379`). |
| `requirepass` | Sets an authentication password — should always be set outside local dev environments. |
| `maxmemory` | Caps memory usage; works with `maxmemory-policy` to control eviction. |
| `maxmemory-policy` | Eviction strategy when `maxmemory` is reached (e.g., `allkeys-lru`, `volatile-ttl`). |
| `appendonly` | Enables AOF persistence (`yes`/`no`). |
| `save` | Defines RDB snapshot intervals. |

After editing the configuration file, restart the service:

```
sudo systemctl restart redis-server
```

<!-- Screenshot placeholder: redis.conf key settings highlighted -->

## 7. Maintenance

```
# For Update
sudo apt update && sudo apt upgrade redis-server -y

# To upgrade software version
sudo apt install redis-server=<version> -y

# For restart
sudo systemctl restart redis-server

# To check service status
sudo systemctl status redis-server
```

## 8. Monitoring

After installation, verify Redis is running correctly and monitor its health using the following:

| Command | Purpose |
|---|---|
| `redis-cli ping` | Confirms the server is up and responding. |
| `redis-cli info` | Returns detailed server stats (memory, clients, persistence, replication). |
| `redis-cli info memory` | Shows current memory usage — useful for catching memory pressure early. |
| `redis-cli monitor` | Streams all commands processed by the server in real time (use with caution in production — performance impact). |
| `redis-cli --latency` | Measures round-trip latency to the server. |

Log files are typically located at `/var/log/redis/redis-server.log` and should be checked first when investigating issues.

<!-- Screenshot placeholder: redis-cli info output -->

## 9. Disaster Recovery

Disaster recovery processes ensure continuity and recoverability of Redis data in the event of node failure or data loss.

- **RDB snapshots**: Point-in-time backups saved to disk (`dump.rdb`) at configured intervals — restore by placing the file in the data directory and restarting Redis.
- **AOF (Append Only File)**: Logs every write operation; provides more durable recovery with minimal data loss compared to RDB alone.
- **Regular backup copies**: Copy `dump.rdb`/`appendonly.aof` files off-instance (e.g., to S3) on a schedule.
- **Replica promotion**: In a replicated setup, a replica can be promoted to master if the primary node fails.

Best practice is to combine RDB + AOF persistence with automated, off-host backups.

## 10. High Availability

Redis provides HA through two main mechanisms:

- **Redis Sentinel**: Monitors master and replica nodes, and automatically performs failover by promoting a replica to master if the current master becomes unavailable.
- **Redis Cluster**: Shards data across multiple nodes and provides automatic failover per shard, allowing Redis to scale horizontally while remaining available if individual nodes fail.

For production microservices, at minimum a master + one or more replicas with Sentinel is recommended to avoid a single point of failure.

## 11. Troubleshooting

| Issue | Cause | Resolution |
|---|---|---|
| `Could not connect to Redis` | Redis service not running, or bind/firewall blocking the port | Check `sudo systemctl status redis-server`; verify port 6379 is open and `bind`/`protected-mode` settings are correct |
| `NOAUTH Authentication required` | `requirepass` is set but client did not authenticate | Run `redis-cli -a <password>` or `AUTH <password>` after connecting |
| High memory usage / OOM | No `maxmemory` limit set, or eviction policy not configured | Set `maxmemory` and an appropriate `maxmemory-policy` |
| Data lost after restart | Persistence (RDB/AOF) not enabled | Enable `appendonly yes` and/or configure `save` intervals |
| Slow commands | Use of blocking or O(N) commands (e.g., `KEYS *`) on large datasets | Use `SCAN` instead of `KEYS`, and review `redis-cli --latency` / slow log |

<!-- Screenshot placeholder: error output for common issue, if applicable -->

## 12. FAQs

- **Is Redis free to use?**
  - Yes, Redis Open Source is free to use; however, licensing terms changed starting with version 7.4 (RSALv2/SSPLv1/AGPLv3 tri-license) — review terms for commercial/managed-service use cases.

- **Can Redis be deployed on all cloud platforms?**
  - Yes, Redis can be self-hosted on any cloud provider (AWS EC2, Azure VM, GCP Compute) or consumed as a managed service (e.g., AWS ElastiCache for Redis).

- **Does Redis persist data by default?**
  - Partially — RDB snapshotting is enabled by default with periodic saves; AOF must be explicitly enabled for stronger durability.

- **Is Redis single-threaded?**
  - The core command execution is single-threaded (per shard), which is why Redis avoids costly O(N) blocking commands in production; I/O threading was introduced in later versions to improve throughput.

## 13. Contact Information

| Name | Email address |
|---|---|
| Sahil | [sahil.butola.snaatak@mygurukulam.co](mailto:sahil.butola.snaatak@mygurukulam.co) |

## 14. References

| Links | Description |
|---|---|
| https://redis.io/docs/latest/ | Official Redis documentation |
| https://redis.io/docs/latest/operate/oss_and_stack/install/install-redis/ | Official Redis installation guide |
| https://redis.io/docs/latest/operate/rs/release-notes/ | Redis release notes and version history |
| https://github.com/OT-MICROSERVICES/documentation-template/wiki/Software-Template | Documentation format followed from this template |
