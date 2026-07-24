# Network Reconnaissance & Assessment Report

## Target Overview
* **Target Host:** `scanme.nmap.org`
* **IP Address:** `45.33.32.156`
* **Scan Type:** TCP Connect Scan (`-sT`) with Version Detection (`-sV`)
* **OS Fingerprint:** Linux Kernel (`cpe:/o:linux:linux_kernel`)

---

## Discovered Services & Enumeration Table

| Port | State | Service | Service Version | Risk & Security Analysis |
| :--- | :--- | :--- | :--- | :--- |
| **22/tcp** | Open | SSH | `OpenSSH 6.6.1p1 (Ubuntu)` | Remote management endpoint. Requires strict key-based authentication and routine security patching. |
| **80/tcp** | Open | HTTP | `Apache httpd 2.4.7 (Ubuntu)` | Web server active. Requires web application scanning and enforcement of TLS/HTTPS redirection. |
| **9929/tcp** | Open | nping-echo | `Nping echo` | Network utility port used for packet ping/echo testing. |
| **31337/tcp** | Open | tcpwrapped | `tcpwrapped` | Connection accepted but immediately closed by TCP wrapper rules or local firewall. |

---

## Recommendations & Hardening Checklist
1. **Software Patching:** Audit and update OpenSSH and Apache software packages to patch known historical CVE vulnerabilities associated with older release builds.
2. **Access Control:** Restrict management services (SSH) to explicit internal/admin IP ranges using local firewalls (`ufw` / `iptables`).
3. **Encrypted Transport:** Enforce TLS/SSL encryption for web services operating on HTTP port 80.
