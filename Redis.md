
<p align="center">
  <img width="1500" height="1000" alt="image" src="https://github.com/user-attachments/assets/efb92d42-cb23-444b-b13a-0e431b5ffa92" />
</p>

# Redis Documentation

| Author | Created on | Version   | Last updated by | Last edited on |
| ------ | ---------- | --------- | ---------------- | -------------- |
| Sahil  | 16-09-26   | version 1 | Sahil            | 16-09-26       |

## Introduction

Redis (Remote Dictionary Server) is an open-source, in-memory data structure store used as a database, cache, message broker, and streaming engine. It stores data in memory rather than on disk, which makes read and write operations extremely fast compared to traditional disk-based databases. Redis supports a wide range of data structures such as strings, hashes, lists, sets, sorted sets, bitmaps, hyperloglogs, streams, and geospatial indexes.

## Purposes

Redis is commonly used to solve the following problems:

- **Caching** — reducing load on a primary database by storing frequently accessed data in memory (e.g., session data, API responses, computed query results).
- **Session Management** — storing user session data for web applications in a fast, shared store accessible across multiple app servers.
- **Message Broker / Pub-Sub** — enabling real-time messaging between services using Redis Pub/Sub or Streams.
- **Rate Limiting** — using atomic counters (INCR, EXPIRE) to throttle API requests.
- **Leaderboards / Real-time Analytics** — using Sorted Sets to maintain ranked data such as gaming leaderboards.
- **Queueing** — using Lists or Streams as lightweight job/task queues.

## Key features

- **In-memory storage** — data is stored in RAM, giving sub-millisecond read/write latency.
- **Rich data structures** — Strings, Hashes, Lists, Sets, Sorted Sets, Bitmaps, HyperLogLogs, Streams, Geospatial indexes.
- **Persistence options** — RDB (point-in-time snapshots) and AOF (append-only file logging) for durability.
- **Replication** — supports master-replica replication for read scalability and failover.
- **High Availability** — Redis Sentinel provides automatic failover and monitoring.
- **Clustering** — Redis Cluster allows horizontal scaling by sharding data across multiple nodes.
- **Atomic operations** — operations like INCR/DECR are atomic, useful for counters and locks.
- **Pub/Sub messaging** — built-in publish/subscribe messaging pattern.
- **Lua scripting** — supports server-side scripting for complex atomic operations.
- **TTL support** — keys can be set to expire automatically, ideal for caching.

## Getting Started

### Pre-requisites

| License Type | Description                                                     | Commercial Use | Open Source |
| ------------- | ---------------------------------------------------------------- | --------------- | ----------- |
| BSD 3-Clause  | Redis (up to 7.2) is free and open for public use and modification. Redis 7.4+ moved to RSALv2/SSPLv1 dual license for source-available use; check version-specific terms before commercial redistribution. | Yes             | Yes         |

### Software Overview

| Software | Version |
| -------- | ------- |
| Redis    | 8.0.5 (default via Ubuntu 26.04 "Resolute" universe repo) |

### System Requirement

| Requirement              | Minimum Recommendation                     |
| ------------------------- | ------------------------------------------- |
| Processor/Instance Type   | Dual-Core / T2.medium instance              |
| RAM                       | 2 GB minimum (size to dataset + overhead)   |
| ROM (Disk Space)          | 10 GB or higher (for RDB/AOF persistence)   |
| OS Required               | Linux — Ubuntu 26.04 LTS "Resolute" (also works on 22.04/24.04, CentOS/RHEL 7+) |

### Important Ports

| Ports | Description                                                                 |
| ----- | ---------------------------------------------------------------------------- |
| 22    | Used to establish an SSH connection to the server and access a shell.       |
| 6379  | Default Redis server port used by clients to connect to the Redis instance. |
| 16379 | Used by Redis Cluster for node-to-node bus communication (client port + 10000). |
| 26379 | Default port for Redis Sentinel.                                             |

## Dependencies

### Run-time Dependency

| Run-time Dependency | Version | Description |
| --------------------- | ------- | ------------ |
| glibc                  | 2.17+   | Standard C library required to run the Redis binary on Linux. |
| systemd                | Any recent | Used to manage the Redis service (start/stop/enable on boot). |

### Other Dependency

| Other Dependency | Version | Description |
| ------------------ | ------- | ------------ |
| gcc / build-essential | Latest (repo) | Required only if compiling Redis from source. |
| tcl                    | 8.5+          | Required to run Redis's own test suite when building from source. |

## How to Setup/Install Redis

### Step-by-step Installation Instruction

**On Ubuntu 26.04 (Resolute):**

Redis is available directly from Ubuntu's default `universe` repository — no third-party source needs to be added.

```
sudo apt update
sudo apt install redis-server redis-tools -y
```

This installs the `redis-server` package for the daemon and `redis-tools`, which provides `redis-cli` and related client utilities. The package also creates `/etc/redis/redis.conf` and registers the `redis-server.service` systemd unit automatically, pre-configured with `--supervised systemd --daemonize no`.

**On CentOS/RHEL:**

```
sudo yum install epel-release -y
sudo yum install redis -y
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

A response of `PONG` confirms Redis is installed and running correctly.

**Verify the exact package/version installed:**

```
apt list --installed | grep redis
redis-server --version
```

### Configuration

Redis is configured primarily through the `redis.conf` file, typically located at `/etc/redis/redis.conf`.

Common configuration changes:

- **Bind address** — restrict which interfaces Redis listens on:
  ```
  bind 127.0.0.1 -::1
  ```
- **Set a password (requirepass):**
  ```
  requirepass YourStrongPassword
  ```
- **Persistence mode** — enable AOF for durability:
  ```
  appendonly yes
  ```
- **Max memory and eviction policy:**
  ```
  maxmemory 512mb
  maxmemory-policy allkeys-lru
  ```

After editing the configuration file, restart Redis to apply changes:

```
sudo systemctl restart redis-server
```

## Maintenance

Follow these commands to maintain the Redis service:

```
# For Update (Ubuntu/Debian)
sudo apt update && sudo apt upgrade redis-server -y

# To check installed/upgraded software version
redis-server --version

# For restart
sudo systemctl restart redis-server
```

Periodic maintenance tasks should also include reviewing memory usage (`INFO memory`), checking for slow queries (`SLOWLOG GET`), and rotating/compacting AOF files (`BGREWRITEAOF`).

## Monitoring

After installation, confirm Redis is running and healthy using the following:

```
# Check service status
sudo systemctl status redis-server

# Confirm Redis is responsive
redis-cli ping

# View real-time server statistics
redis-cli info

# Monitor commands in real time
redis-cli monitor
```

Key metrics to watch: `used_memory`, `connected_clients`, `keyspace_hits` / `keyspace_misses`, `evicted_keys`, and `rejected_connections`. In case of issues, check logs at `/var/log/redis/redis-server.log` for errors or warnings.

## Disaster Recovery

- **RDB Snapshots** — Redis periodically saves point-in-time snapshots of the dataset to disk (`dump.rdb`). Snapshot frequency is configured via `save` directives in `redis.conf`.
- **AOF (Append Only File)** — logs every write operation; on restart, Redis replays the AOF to reconstruct the dataset with minimal data loss.
- **Backup strategy** — regularly copy `dump.rdb` / AOF files to a separate storage location (e.g., S3) outside the Redis host.
- **Restore process** — stop Redis, replace the `dump.rdb` (or AOF file) in the configured data directory, then restart the service to reload the data.
- Best practice: combine RDB + AOF ("hybrid persistence") for both fast restarts and minimal data loss.

## High Availability

- **Redis Sentinel** — monitors master and replica nodes, automatically promotes a replica to master on failure, and notifies clients of the topology change.
- **Replication** — a master node replicates writes to one or more replica nodes, which can serve read traffic and act as failover candidates.
- **Redis Cluster** — shards data across multiple master nodes (with replicas for each shard), providing both horizontal scalability and automatic failover, with no single point of failure.
- For production workloads, running at least 3 Sentinel nodes (for quorum) or a properly sized Redis Cluster is recommended over a single standalone instance.

## Troubleshooting

- **Redis service fails to start** — check `/var/log/redis/redis-server.log` for port conflicts or permission issues on the data directory; verify `redis.conf` syntax.
- **`Could not connect to Redis` errors** — confirm the service is running (`systemctl status redis-server`) and that the `bind` directive/firewall rules allow the client's IP.
- **`NOAUTH Authentication required` error** — the `requirepass` directive is set; clients must authenticate using `AUTH <password>` or the `-a` flag with `redis-cli`.
- **High memory usage / OOM kills** — review `maxmemory` and `maxmemory-policy`; without an eviction policy, Redis can consume all available RAM.
- **Data loss after restart** — verify persistence is enabled (`appendonly yes` and/or `save` snapshot rules); an unconfigured instance runs in memory-only mode.

## FAQs

- **Is Redis free to use?**
  - Redis versions through 7.2 are open-source (BSD 3-Clause). From Redis 7.4 onward, Redis Ltd. moved core Redis to a dual RSALv2/SSPLv1 source-available license — free to use in most cases, but commercial redistribution/hosting scenarios should be reviewed against current license terms.

- **Can Redis data be persisted, since it's an in-memory store?**
  - Yes. Redis supports RDB snapshots and AOF logging so data can survive restarts and crashes.

- **Can Redis be deployed on any cloud platform?**
  - Yes, Redis can be deployed on AWS, Azure, GCP, or on-premises; managed offerings (e.g., AWS ElastiCache for Redis) are also available.

- **Does Redis support clustering for large datasets?**
  - Yes, Redis Cluster shards data across multiple nodes to scale horizontally beyond a single machine's memory.

## Contact Information

| Name  | Email address        |
| ----- | --------------------- |
| Sahil | sahil@mygurukulam.co |

## References

| Links                                              | Descriptions                                    |
| ---------------------------------------------------- | ------------------------------------------------ |
| https://redis.io/docs/latest/                        | Official Redis documentation                     |
| https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/ | Reference for Redis persistence (RDB/AOF) details |
| https://redis.io/docs/latest/operate/oss_and_stack/management/sentinel/    | Reference for Redis Sentinel / High Availability  |
| https://github.com/OT-MICROSERVICES/documentation-template/wiki/Software-Template | Software Template this document follows          |
