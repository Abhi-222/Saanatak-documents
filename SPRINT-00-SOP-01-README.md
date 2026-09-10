# Common Stack | Operating System | Linux | SOP for Sysctl

---

# Author Table

| **Author** | **Created On** | **Version** |**Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ---------- | -------------- | ----------- |------------------ | --------------- | --------------- | --------------- |
| Sahil      | 24-08-26       | 1.1         | 07-09-26          | `Vishal/ Divya M`| `Aayush Verma`| `Mahesh Kumar / Varun` |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What is sysctl](#2-what-is-sysctl)
3. [Why sysctl is Used](#3-why-sysctl-is-used)
4. [Key Features of sysctl](#4-key-features-of-sysctl)
5. [Prerequisites and System Requirements](#5-Prerequisites-and-System-requirements)
6. [Viewing, Applying, and Persisting Kernel Parameters](#6-viewing-applying-and-persisting-kernel-parameters)
7. [Quick Commands](#7-quick-commands)
8. [Use Cases](#8-use-cases)
9. [Troubleshooting](#9-troubleshooting)
10. [Best Practices](#10-best-practices)
11. [Conclusion](#11-conclusion)
12. [Contact Information](#12-contact-information)
13. [References](#13-references)

---

# 1. Introduction

sysctl is a Linux tool used to view and change kernel settings — for performance tuning (like memory and networking behavior) or security hardening — without needing a reboot.

This SOP covers three things: how to view current kernel settings, how to apply a change safely, and how to persist that change so it survives a reboot.

---

# 2. What is sysctl

sysctl is a Linux tool that lets you view and change kernel settings while the system is running — no reboot needed. It covers things like memory usage, networking, and security-related behavior.
A change made with sysctl only affects the system until it restarts. To make it stick permanently, you also need to save it in a config file under /etc/sysctl.d/.

---

# 3. Why sysctl is Used

- **Performance tuning** — speed things up for your specific workload, like making the server less likely to swap memory to disk, or handle more network traffic.
- **Security hardening** — turn on extra protections, like defenses against certain types of network attacks.
- **Safe to test** — you can try a change right away and see what happens. If you don't save it, it disappears on the next reboot — so testing carries no long-term risk.

---

# 4. Key Features of sysctl

- **Live changes** — a setting takes effect right away, while the server is still running. No restart needed to see the change work.
- **Can be made permanent** — by default, a change disappears on reboot. If you want it to stay, you save it to a file, and it'll still be there next time the server starts.
- **Check anytime** — you can look up any current setting whenever you want, without changing anything, just to see what's active.
- **Covers a lot** — one tool handles many different kinds of settings: how the network behaves, how memory is managed, and various security-related options.

---

# 5. Prerequisites & System Requirements

| **Requirement**   | **Minimum**                                                                              |
| ----------------- | --------------------------------------------------------------------------------------------- |
| OS                | Any Linux distribution with `sysctl` available (pre-installed on most distros via `procps`)   |
| Backup Space      | Minimal — just enough free space to back up the existing `sysctl.conf` and `sysctl.d` directory before making changes |

---


# 6. Viewing, Applying, and Persisting Kernel Parameters
 
## 6.1 View Kernel Parameters
 
### Step 6.1.1: View kernel parameters
 
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

 
### Step 6.1.2: Search for a parameter by keyword
 
Useful when the exact parameter name is not known in advance.
 
```bash
sudo sysctl -a | grep swappiness
```
 
<details>
<summary><strong>Screenshot - keyword search output</strong></summary>
<img width="825" height="95" alt="Screenshot 2026-09-05 at 2 16 03 AM" src="https://github.com/user-attachments/assets/d810ff2b-1465-4b4b-be32-7a055e85c2f5" />
</details>

---
 
 ## 6.2 Apply Kernel Parameters (Temporary)
 
Temporary changes take effect immediately at runtime but do **not** survive a reboot. Always validate here before persisting (7.3).
 
### Step 6.2.1: Apply a parameter
 
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
 
 
### Step 6.2.2: Verify the change
 
```bash
sysctl vm.swappiness
```
 
**Note:** This change is runtime-only — a reboot alone reverts it, so there's no persistence risk yet.
 
<details>
<summary><strong>Screenshot - verified runtime value</strong></summary>
<img width="825" height="147" alt="Screenshot 2026-09-05 at 2 19 29 AM" src="https://github.com/user-attachments/assets/1def1d73-ec7a-4adf-a912-db677ba20d95" />
</details>

 
## 6.3 Persist Kernel Parameters (Permanent)
 
Once the temporary change is validated, persist it so it survives a reboot.
 
### Step 6.3.1: Create a dedicated config file
 
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

 
### Step 6.3.2: Apply, reload, and confirm it survives a reboot
 
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

# 7. Quick Commands

| **Action** | **Command** |
|---------|-----------------|
| View    | `sysctl <parameter>` |
| Apply   | `sudo sysctl -w <parameter>=<value>` |
| Persist | `sudo sysctl --system` | 

 
---

# 8. Use Cases

- **Server swapping too much memory** — if an application is slowing down because the server keeps moving memory to disk (swap) instead of keeping it in RAM, lowering vm.swappiness tells the kernel to avoid swapping unless it really has to. Try it live first, confirm performance improves, then save it so it stays after a reboot.
- **Server needs to route network traffic** — if this server is meant to act as a router or gateway (forwarding traffic between two networks instead of just handling its own), net.ipv4.ip_forward needs to be turned on — it's off by default. Enable it, confirm routing actually works, then save it.
- **Security review flags a missing protection** — if a security audit calls out that the server isn't protected against a specific type of network attack (like a SYN flood), turning on net.ipv4.tcp_syncookies adds that protection at the kernel level. Enable it, confirm nothing else breaks, then save it.

---

# 9. Troubleshooting

| **Issue** | **Cause** | **Solution** |
|-----------|-----------|--------------|
| `sysctl -p` returns "No such file or directory" | Typo in parameter name, or parameter removed in a newer kernel | Confirm the exact name with `sudo sysctl -a \| grep <keyword>` |
| Value reverts after reboot | Change was only applied with `sysctl -w`, never persisted | Repeat the Persist steps (7.3) and confirm with `sysctl --system` |
| Same parameter set in multiple `.conf` files | Duplicate entries across `sysctl.conf` and `sysctl.d/*.conf` | The last-loaded file wins — search with `grep -r "<param>" /etc/sysctl.conf /etc/sysctl.d/` and remove duplicates |
 


---

# 10. Best Practices

- **Be careful with security settings** — things like tcp_syncookies protect the server from attacks. Only turn them off if you have a good reason and someone else has reviewed it.
Always check after a reboot — a setting can look fine right after you save it, but the only real proof it worked is checking again after the server restarts.

- **Always check after a reboot** — a setting can look fine right after you save it, but the only real proof it worked is checking again after the server restarts.
- **Test before you save it** — try sysctl -w first, see how it behaves, before writing it into a file.
- **Use your own config file** — don't edit the system's default file directly; create a separate file so your changes are easy to find and remove later.
- **Don't set the same value in two places** — avoids confusion about which config file actually wins.
---

# 11. Conclusion

This SOP covers three things: viewing current kernel settings, applying a change safely, and making that change permanent so it survives a reboot — on any Linux server. Following these steps keeps changes safe, consistent, and easy to confirm.

---

# 12. Contact Information

| **Name** | **Email** |
| -------- | --------- |
| Sahil    | [sahil.butola.snaatak@mygurukulam.co](mailto:sahil.butola.snaatak@mygurukulam.co) ( |

---

# 13. References

| **Topic**                                                                                                    | **Description**                                  |
| ------------------------------------------------------------------------------------------------------------ | ------------------------------------------------ |
| [sysctl man page](https://man7.org/linux/man-pages/man8/sysctl.8.html)                                       | `sysctl` command reference                       |
| [sysctl.d man page](https://man7.org/linux/man-pages/man5/sysctl.d.5.html)                                   | `sysctl.d` configuration reference               |
| [Ubuntu Server documentation](https://ubuntu.com/server/docs)                                                | Example distribution-specific documentation (Ubuntu Server) |

