# Common Stack | Operating System | Linux | SOP for Sysctl

---

# Author Table

| **Author** | **Created On** | **Version** |**Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ---------- | -------------- | ----------- |------------------ | --------------- | --------------- | --------------- |
| Sahil      | 24-08-26       | 1.1         | 07-09-26          | `Vishal/ Divya M`| `Aayush Verma`| `Mahesh Kumar / Varun` |

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

sysctl is a Linux tool used to view and change kernel settings — for performance tuning (like memory and networking behavior) or security hardening — without needing a reboot.

This SOP covers three things: how to view current kernel settings, how to apply a change safely, and how to persist that change so it survives a reboot.

---


# 3. What is sysctl

sysctl is a Linux tool that lets you view and change kernel settings while the system is running — no reboot needed. It covers things like memory usage, networking, and security-related behavior.
A change made with sysctl only affects the system until it restarts. To make it stick permanently, you also need to save it in a config file under /etc/sysctl.d/.

---

# 4. Why sysctl is Used

- **Performance tuning** — speed things up for your specific workload, like making the server less likely to swap memory to disk, or handle more network traffic.
- **Security hardening** — turn on extra protections, like defenses against certain types of network attacks.
- **Safe to test** — you can try a change right away and see what happens. If you don't save it, it disappears on the next reboot — so testing carries no long-term risk.

---

# 5. Key Features of sysctl

- **Live changes** — a setting takes effect right away, while the server is still running. No restart needed to see the change work.
- **Can be made permanent** — by default, a change disappears on reboot. If you want it to stay, you save it to a file, and it'll still be there next time the server starts.
- **Check anytime** — you can look up any current setting whenever you want, without changing anything, just to see what's active.
- **Covers a lot** — one tool handles many different kinds of settings: how the network behaves, how memory is managed, and various security-related options.

---

# 6. Prerequisites & System Requirements

| **Requirement**   | **Minimum**                                                                              |
| ----------------- | --------------------------------------------------------------------------------------------- |
| OS                | Any Linux distribution with `sysctl` available (pre-installed on most distros via `procps`)   |
| Permissions       | `sudo`/root access where required                                                             |
| Backup Space      | Minimal — just enough free space to back up the existing `sysctl.conf` and `sysctl.d` directory before making changes |

---


 # 7. Viewing, Applying, and Persisting Kernel Parameters
 
## 7.1 View Kernel Parameters
 
### Step 7.1.1: View kernel parameters
 
List all active parameters, or check one specific parameter:
 
```bash
sudo sysctl -a                          # list all parameters
sysctl net.ipv4.ip_forward              # view one specific parameter
```
 
<details>
<summary><strong>Screenshot - full parameter list (sysctl -a)</strong></summary>
<img width="825" height="608" alt="Screenshot 2026-09-05 at 2 14 45 AM" src="https://github.com/user-attachments/assets/b74a9777-e748-47b0-988c-0596e4249e6d" />
</details>
<details>
<summary><strong>Screenshot - specific parameter value</strong></summary>
<img width="825" height="95" alt="Screenshot 2026-09-05 at 2 15 47 AM" src="https://github.com/user-attachments/assets/9bfa162a-928a-40ce-a53c-357522bde63c" />
</details>

 
### Step 7.1.2: Search for a parameter by keyword
 
Useful when the exact parameter name is not known in advance.
 
```bash
sudo sysctl -a | grep swappiness
```
 
<details>
<summary><strong>Screenshot - keyword search output</strong></summary>
<img width="825" height="95" alt="Screenshot 2026-09-05 at 2 16 03 AM" src="https://github.com/user-attachments/assets/d810ff2b-1465-4b4b-be32-7a055e85c2f5" />
</details>

---
 
 ## 7.2 Apply Kernel Parameters (Temporary)
 
Temporary changes take effect immediately at runtime but do **not** survive a reboot. Always validate here before persisting (7.3).
 
### Step 7.2.1: Apply a parameter
 
```bash
sudo sysctl -w vm.swappiness=10
```
 
Expected output:
 
```text
vm.swappiness = 10
```
 
<details>
<summary><strong>Screenshot - sysctl -w applied</strong></summary>
<img width="825" height="147" alt="Screenshot 2026-09-05 at 2 18 21 AM" src="https://github.com/user-attachments/assets/35bfcbfa-ed97-408f-9453-9bbac5562857" />
</details>
*Note: this screenshot was captured alongside a backup step that's no longer part of this SOP — if you retake it, a screenshot of just the `sysctl -w` command and its output is enough.*
 
 
### Step 7.2.2: Verify the change
 
```bash
sysctl vm.swappiness
```
 
**Note:** This change is runtime-only — a reboot alone reverts it, so there's no persistence risk yet.
 
<details>
<summary><strong>Screenshot - verified runtime value</strong></summary>
<img width="825" height="147" alt="Screenshot 2026-09-05 at 2 19 29 AM" src="https://github.com/user-attachments/assets/1def1d73-ec7a-4adf-a912-db677ba20d95" />
</details>

 
## 7.3 Persist Kernel Parameters (Permanent)
 
Once the temporary change is validated, persist it so it survives a reboot.
 
### Step 7.3.1: Create a dedicated config file
 
```bash
sudo nano /etc/sysctl.d/99-custom-tuning.conf
```
 
```text
vm.swappiness = 10
net.ipv4.ip_forward = 1
fs.file-max = 100000
```
 
Use a dedicated file under `/etc/sysctl.d/` instead of editing `/etc/sysctl.conf` directly — this isolates custom tuning from OS defaults.
 
<details>
<summary><strong>Screenshot - config file contents</strong></summary>
<img width="825" height="147" alt="Screenshot 2026-09-05 at 2 20 08 AM" src="https://github.com/user-attachments/assets/0f8d53f8-8af4-42c1-947a-493a8b7c7929" />
</details>

 
### Step 7.3.2: Apply, reload, and confirm it survives a reboot
 
```bash
sudo sysctl -p /etc/sysctl.d/99-custom-tuning.conf
sudo sysctl --system
```
 
<details>
<summary><strong>Screenshot - config applied and reloaded</strong></summary>
<img width="825" height="346" alt="Screenshot 2026-09-05 at 2 21 25 AM" src="https://github.com/user-attachments/assets/279c2842-7734-46c1-94e6-eaea36039f58" />
</details>
To confirm the change is truly persistent (not just applied for now), reboot and check the value again:
 
```bash
sudo reboot
sysctl vm.swappiness net.ipv4.ip_forward fs.file-max
```
  
*Note: no screenshot yet for this reboot-check step — add one when available.*
 
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
