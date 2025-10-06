# Ubuntu VM Setup
Date: 2025-10-04

Steps performed:
- Updated Ubuntu packages
- Installed tools: net-tools, curl, wget, vim, git
- Installed and started Apache web server
- Verified service via http://127.0.0.0
- 
Username: ubuntu
Password: LabPass123!

Next:
- Create Kali VM to perform nmap scan and traffic capture


## 2025-10-05 — Kali created & connectivity verified; prepared for nmap + capture
**Goal:** Create Kali VM and verify network connectivity to Ubuntu target (prepare for nmap + packet capture).

**Environment / IPs**
- VirtualBox network: `NatNetwork` (NAT Network)
- Ubuntu server IP: `127.0.0.0`  
- Kali attacker IP: `127.0.0.1`  
- Host OS: Windows; VM storage path: `E:\VirtualBox`
- 
Username: kali
Password: LabPass123!

**Actions performed**
- Created VirtualBox VM: `Kali-Attacker`
  - OS: Kali Linux (Installer ISO)
  - RAM: 2048 MB
  - Disk: 20 GB VDI (dynamically allocated)
- Attached Adapter 1 of both Ubuntu and Kali to the same VirtualBox NAT Network (`NatNetwork`)
- Booted Kali and confirmed it runs and is reachable.

**Verification**
- From Kali:
  ```bash
  ip -4 addr show
  ping 127.0.0.0
