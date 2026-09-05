# Common Stack | Operating System | Linux | SOP for Sysctl

---

# Author Table

| **Author** | **Created On** | **Version** |**Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ---------- | -------------- | ----------- |------------------ | --------------- | --------------- | --------------- |
| Sahil      | 24-08-26       | 1.0         | 03-09-26          | `Divya M`      | `Aayush Verma`   | `Mahesh Kumar / Varun` |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [Purpose](#2-purpose)
3. [What is sysctl](#3-what-is-sysctl)
4. [Why sysctl is Used](#4-why-sysctl-is-used)
5. [Key Features of sysctl](#5-key-features-of-sysctl)
6. [Prerequisites](#6-prerequisites)
   - [6.1 Access & Permissions](#61-access--permissions)
   - [6.2 System Requirements](#62-system-requirements)
7. [View Kernel Parameters](#7-view-kernel-parameters)
8. [Apply Kernel Parameters (Temporary)](#8-apply-kernel-parameters-temporary)
9. [Persist Kernel Parameters (Permanent)](#9-persist-kernel-parameters-permanent)
10. [Rollback Procedure](#10-rollback-procedure)
11. [Validation](#11-validation)
12. [Use Cases](#12-use-cases)
13. [Troubleshooting](#13-troubleshooting)
14. [Quick Commands](#14-quick-commands)
15. [Best Practices](#15-best-practices)
16. [Conclusion](#16-conclusion)
17. [Contact Information](#17-contact-information)
18. [References](#18-references)

---

# 1. Introduction

This SOP provides a structured guide to viewing, applying, and persisting **kernel parameter changes** using `sysctl` on **Linux servers**, for the purpose of performance or security tuning. It applies across most major distributions and is not tied to a specific OS version.

It covers the required checks, configuration steps, verification, validation, rollback, and troubleshooting needed to keep kernel-level tuning consistent, auditable, and reversible across environments.

---

# 2. Purpose

The purpose of this SOP is to provide a standardized procedure for:

- Viewing current kernel parameter values under `/proc/sys/` using `sysctl`
- Applying a parameter change temporarily (runtime only, safe to test)
- Persisting an approved change permanently via `/etc/sysctl.d/`
- Rolling back a change safely when the restored config does not, by itself, reset the live kernel value
- Validating that a configured parameter is active and survives a reboot

These procedures help maintain **system stability, security posture, performance, and operational consistency**.

---

# 3. What is sysctl

`sysctl` is a Linux command-line utility (and kernel interface) used to view and modify kernel parameters at runtime, without requiring a reboot. It reads and writes values exposed under the `/proc/sys/` virtual filesystem, covering areas such as networking, virtual memory, file handles, and security-related kernel behavior.

Values changed with `sysctl` at runtime only affect the live, running kernel. For a change to survive a reboot, the parameter must also be persisted in a configuration file under `/etc/sysctl.d/` or `/etc/sysctl.conf`.

---

# 4. Why sysctl is Used

- **Performance tuning** — adjust kernel behavior (memory management, swappiness, network buffers) for a specific workload without recompiling the kernel.
- **Security hardening** — enable protections such as SYN cookies, disable IP forwarding on non-routing hosts, restrict core dumps, and similar controls.
- **Safe runtime testing** — validate a parameter change live before committing it permanently, with no persistence risk if the value is wrong.
- **Operational consistency** — persisted config files make tuning auditable, versionable, and reproducible across servers.

---

# 5. Key Features of sysctl

| **Feature**                  | **Description**                                                                          |
| -------------------------------| ---------------------------------------------------------------------------------------------|
| Runtime + persistent modes     | Change kernel parameters live at runtime (`sysctl -w`) or persist them across reboots (`/etc/sysctl.d/`) |
| Live introspection             | View any active kernel parameter via `sysctl -a` or directly under `/proc/sys/`               |
| Modular configuration           | `/etc/sysctl.d/*.conf` isolates custom tuning from OS defaults                                |
| Reload without reboot           | `sysctl -p` / `sysctl --system` re-applies persisted config without restarting the system      |
| Broad scope                     | Covers networking, memory management, file system limits, and kernel/security behavior         |

---

# 6. Prerequisites

This SOP has no strict OS version requirement — it works on virtually any Linux distribution that includes `sysctl` (part of the `procps` package), including **Ubuntu, Debian, RHEL/CentOS/Rocky, Fedora, and similar systems**.

### 6.1 Access & Permissions

| **Prerequisite**   | **Details**                                                                                        |
| ------------------ | --------------------------------------------------------------------------------------------------- |
| Access             | SSH/terminal access with `sudo`/root privileges to view, modify, and persist kernel parameters      |
| Out-of-band Access | Console/out-of-band access recommended, in case a networking parameter change causes an SSH lockout |

### 6.2 System Requirements

| **Requirement**   | **Minimum**                                                                              |
| ----------------- | --------------------------------------------------------------------------------------------- |
| OS                | Any Linux distribution with `sysctl` available (pre-installed on most distros via `procps`)   |
| Permissions       | `sudo`/root access where required                                                             |
| Backup Space      | Minimal — just enough free space to back up the existing `sysctl.conf` and `sysctl.d` directory before making changes |

---

# 7. View Kernel Parameters

## Step 7.1: View kernel parameters

List all active parameters, check one specific parameter, or read a value directly from the proc filesystem:

```bash
sudo sysctl -a                          # list all parameters
sysctl net.ipv4.ip_forward              # view one specific parameter
cat /proc/sys/net/ipv4/ip_forward       # read directly from proc
```

<details>
<summary><strong>Screenshot - full parameter list (sysctl -a)</strong></summary>

<img width="825" height="608" alt="Screenshot 2026-09-05 at 2 14 45 AM" src="https://github.com/user-attachments/assets/b74a9777-e748-47b0-988c-0596e4249e6d" />

</details>

<details>
<summary><strong>Screenshot - specific parameter value</strong></summary>

<img width="825" height="95" alt="Screenshot 2026-09-05 at 2 15 47 AM" src="https://github.com/user-attachments/assets/9bfa162a-928a-40ce-a53c-357522bde63c" />

</details>

---

## Step 7.2: Search for a parameter by keyword

Useful when the exact parameter name is not known in advance.

```bash
sudo sysctl -a | grep swappiness
```

<details>
<summary><strong>Screenshot - keyword search output</strong></summary>

<img width="825" height="95" alt="Screenshot 2026-09-05 at 2 16 03 AM" src="https://github.com/user-attachments/assets/d810ff2b-1465-4b4b-be32-7a055e85c2f5" />

</details>

---

# 8. Apply Kernel Parameters (Temporary)

Temporary changes take effect immediately at runtime but do **not** survive a reboot. Always validate here before persisting (Section 9).

## Step 8.1: Backup configuration and apply a parameter

```bash
sudo cp /etc/sysctl.conf /etc/sysctl.conf.bak_$(date +%F)
sudo cp -r /etc/sysctl.d /etc/sysctl.d.bak_$(date +%F)
sudo sysctl -w vm.swappiness=10
```

Expected output:

```text
vm.swappiness = 10
```

Take the backup **before** making any change — it's required for a clean rollback later (Section 10).

<details>
<summary><strong>Screenshot - backup and applied value</strong></summary>

<img width="825" height="147" alt="Screenshot 2026-09-05 at 2 18 21 AM" src="https://github.com/user-attachments/assets/35bfcbfa-ed97-408f-9453-9bbac5562857" />

</details>

---

## Step 8.2: Verify the change

```bash
sysctl vm.swappiness
```

**Note:** This change is runtime-only — a reboot alone reverts it, so there's no persistence risk yet. To write directly to the kernel instead of using `sysctl -w`, use `echo 10 | sudo tee /proc/sys/vm/swappiness`.

<details>
<summary><strong>Screenshot - verified runtime value</strong></summary>

<img width="825" height="147" alt="Screenshot 2026-09-05 at 2 19 29 AM" src="https://github.com/user-attachments/assets/1def1d73-ec7a-4adf-a912-db677ba20d95" />

</details>

---

# 9. Persist Kernel Parameters (Permanent)

Once the temporary change is validated, persist it so it survives a reboot.

## Step 9.1: Create a dedicated config file

```bash
sudo nano /etc/sysctl.d/99-custom-tuning.conf
```

```text
vm.swappiness = 10
net.ipv4.ip_forward = 1
fs.file-max = 100000
```

Use a dedicated file under `/etc/sysctl.d/` instead of editing `/etc/sysctl.conf` directly — this isolates custom tuning from OS defaults and makes rollback a single file operation.

<details>
<summary><strong>Screenshot - config file contents</strong></summary>

<img width="825" height="147" alt="Screenshot 2026-09-05 at 2 20 08 AM" src="https://github.com/user-attachments/assets/0f8d53f8-8af4-42c1-947a-493a8b7c7929" />

</details>

---

## Step 9.2: Apply and reload

```bash
sudo sysctl -p /etc/sysctl.d/99-custom-tuning.conf
sudo sysctl --system
```

<details>
<summary><strong>Screenshot - config applied and reloaded</strong></summary>

<img width="825" height="346" alt="Screenshot 2026-09-05 at 2 21 25 AM" src="https://github.com/user-attachments/assets/279c2842-7734-46c1-94e6-eaea36039f58" />

</details>

---

# 10. Rollback Procedure

If a persisted change causes unexpected behaviour, revert as follows. **Restoring or deleting config files alone does not reset the live kernel value** — the rollback has two parts.

| **Step** | **Action**                                               | **Command**                                                                |
| -------- | ---------------------------------------------------------- | ---------------------------------------------------------------------------- |
| 1        | Remove the change file                                    | `sudo rm -f /etc/sysctl.d/99-custom-tuning.conf`                             |
| 2        | Find the original value from backup                       | `grep -rE "vm.swappiness\|ip_forward\|file-max" /etc/sysctl.d.bak_<date>/`   |
| 3        | Manually re-push the original value into the live kernel  | `sudo sysctl -w vm.swappiness=<original_value>`                              |
| 4        | Reload config                                              | `sudo sysctl --system`                                                       |
| 5        | Reboot and confirm the value holds long-term               | `sudo reboot`                                                                |

> [!NOTE]
> If `grep` finds no value for a parameter in the backup, it was only ever a kernel default — use the common Linux default for most distributions (`vm.swappiness=60`, `net.ipv4.ip_forward=0`), or for `fs.file-max`, let it recalculate on reboot rather than guessing a fixed number.

<details>
<summary><strong>Screenshot - rollback confirmation</strong></summary>

<img width="907" height="417" alt="Screenshot 2026-09-05 at 2 24 20 AM" src="https://github.com/user-attachments/assets/f733fc64-49d4-43eb-9279-3e99f997807c" />

</details>

---

# 11. Validation

```bash
sysctl vm.swappiness net.ipv4.ip_forward fs.file-max
```

<details>
<summary><strong>Screenshot - validation output</strong></summary>

<img width="713" height="95" alt="Screenshot 2026-09-05 at 2 44 16 AM" src="https://github.com/user-attachments/assets/0b163dbe-9af8-4bdf-bce3-d89b2edd1602" />

</details>

**Expected:** Values match the intended settings — immediately after applying, after `sudo sysctl --system`, and again after `sudo reboot`.

### Final Validation Checklist

| **Validation**                               | **Expected Result**                                   |
| --------------------------------------------- | --------------------------------------------------------- |
| `sysctl <parameter>` immediately after apply | Matches the intended value                                |
| `sysctl <parameter>` after `sysctl --system` | Matches the persisted config file                         |
| `sysctl <parameter>` after reboot            | Still matches — confirms true persistence                 |
| Dependent services (networking, DB, app)     | Healthy post-reboot                                       |
| Screenshots                                   | Attached at their respective placeholders as evidence      |

---

# 12. Use Cases

| **Scenario**                                          | **Commands / Actions**                                                                                      |
| ----------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| Application under memory pressure, excessive swapping | Lower `vm.swappiness` (Step 8.1), validate app behavior, then persist (Section 9)                            |
| Server needs to act as a router/gateway               | Set `net.ipv4.ip_forward = 1` temporarily (Step 8.1), validate routing, then persist                        |
| Application hitting "too many open files" system-wide | Raise `fs.file-max` (Step 8.1/9.1) after confirming the process-level `ulimit` is not the actual bottleneck |
| Security hardening review flags SYN flood exposure    | Set `net.ipv4.tcp_syncookies = 1` and persist under `/etc/sysctl.d/`                                        |
| A prior tuning change needs to be safely undone       | Follow the two-part Rollback Procedure (Section 10) — file removal **and** manual live re-push              |

---

# 13. Troubleshooting

| **Issue**                                          | **Cause**                                                                              | **Solution**                                                                                                      |
| -------------------------------------------------- | ---------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| `sysctl -p` returns "No such file or directory"    | Typo in parameter name, or parameter removed in a newer kernel                           | Confirm the exact name with `sudo sysctl -a \| grep <keyword>`                                                    |
| Value reverts after reboot                         | Change was only applied with `sysctl -w`, never persisted                                | Repeat the Persist steps (Section 9) and confirm with `sysctl --system`                                           |
| Value doesn't change after restoring backup config | Restoring/removing a config file does not reset the already-running kernel value         | Manually re-push with `sudo sysctl -w <param>=<value>` (Section 10)                                               |
| Permission denied                                  | Command run without `sudo`                                                               | Re-run with `sudo`                                                                                                |
| Network drops after a `net.*` parameter change     | Incorrect value for this host/environment (e.g. forwarding disabled on a routing host)   | Use console/out-of-band access to roll back immediately                                                           |
| Same parameter set in multiple `.conf` files       | Duplicate entries across `sysctl.conf` and `sysctl.d/*.conf`                             | The last-loaded file wins — search with `grep -r "<param>" /etc/sysctl.conf /etc/sysctl.d/` and remove duplicates |

---

# 14. Quick Commands

| **Task**                                     | **Command**                                                     |
| ------------------------------------------------ | -------------------------------------------------------------------- |
| List all kernel parameters                       | `sudo sysctl -a`                                                     |
| View a specific parameter                         | `sysctl <parameter>`                                                 |
| Read a parameter directly from proc               | `cat /proc/sys/<path>`                                                |
| Search parameters by keyword                      | `sudo sysctl -a \| grep <keyword>`                                    |
| Backup existing config                            | `sudo cp /etc/sysctl.conf /etc/sysctl.conf.bak_$(date +%F)`           |
| Apply a parameter at runtime                      | `sudo sysctl -w <parameter>=<value>`                                  |
| Write directly to proc (alternative)              | `echo <value> \| sudo tee /proc/sys/<path>`                           |
| Create a dedicated persistent config file          | `sudo nano /etc/sysctl.d/99-custom-tuning.conf`                       |
| Apply a specific config file                       | `sudo sysctl -p /etc/sysctl.d/99-custom-tuning.conf`                  |
| Reload all sysctl config files                     | `sudo sysctl --system`                                                |
| Remove a persisted change                          | `sudo rm -f /etc/sysctl.d/99-custom-tuning.conf`                      |

---

# 15. Best Practices

| **Best Practice**                            | **Description**                                                                                              |
| --------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| Always test with `sysctl -w` first           | Validate the value at runtime before persisting — no reboot risk if it's wrong                               |
| Use `/etc/sysctl.d/` over `sysctl.conf`      | Isolates custom tuning from OS defaults, making review and rollback a single file operation                  |
| Backup before every change                   | `sysctl.conf` and `sysctl.d` should be backed up before any modification, not just once                      |
| Treat rollback as two steps                  | Config file removal **and** a manual `sysctl -w` re-push — file removal alone is not a complete rollback     |
| Avoid disabling security parameters casually | `tcp_syncookies`, `randomize_va_space`, etc. should only be changed with clear justification and peer review |
| Validate after every reboot                  | Some persistence issues only surface after a real reboot, not just `sysctl --system`                         |

---

# 16. Conclusion

This SOP provides a standardized approach to viewing, applying, and persisting kernel parameters using `sysctl` on Linux servers, regardless of distribution.

Following these procedures helps administrators maintain **performance, security posture, and operational stability**, while providing a consistent, evidence-backed approach to configuration, validation, rollback, and troubleshooting. In particular, treating rollback as a two-part process — config removal plus a manual live re-push — closes a gap that a file-only rollback would otherwise miss.

---

# 17. Contact Information

| **Name** | **Email** |
| -------- | --------- |
| Sahil    | [sahil.butola.snaatak@mygurukulam.co](mailto:sahil.butola.snaatak@mygurukulam.co) ( |

---

# 18. References

| **Topic**                                                                                                    | **Description**                                  |
| ------------------------------------------------------------------------------------------------------------ | ------------------------------------------------ |
| [sysctl man page](https://man7.org/linux/man-pages/man8/sysctl.8.html)                                       | `sysctl` command reference                       |
| [sysctl.d man page](https://man7.org/linux/man-pages/man5/sysctl.d.5.html)                                   | `sysctl.d` configuration reference               |
| [Ubuntu Server documentation](https://ubuntu.com/server/docs)                                                | Example distribution-specific documentation (Ubuntu Server) |
| [Application Template](https://github.com/OT-MICROSERVICES/documentation-template/wiki/Application-Template) | Documentation format/index followed for this SOP |
| [Software Template](https://github.com/OT-MICROSERVICES/documentation-template/wiki/Software-Template)       | Documentation format/index followed for this SOP |
