# VirtualBox Lab Setup — Ubuntu & Kali Connectivity Test

**Date:** October 2025  
**Author:** YU  
**Project Goal:**  
Build a small home lab using VirtualBox to simulate a real-world security environment.  
This stage focuses on creating and configuring two virtual machines (Ubuntu Server and Kali Linux) and ensuring network connectivity between them.

---

## 🧭 Objective
Set up a controlled virtual environment to use for future cybersecurity and networking experiments.  
The primary goals achieved so far:
1. Installed and configured VirtualBox on Windows host.
2. Created Ubuntu Server (target machine) and Kali Linux (attacker machine) VMs.
3. Configured both VMs on the same internal network (`NatNetwork`).
4. Verified that both VMs can communicate via ICMP (`ping` test).

---

## ⚙️ Environment Setup

### Host System
- **Operating System:** Windows 10/11  
- **Virtualization Platform:** Oracle VirtualBox  
- **VM Storage Location:** `E:\VirtualBox`  
- **Virtual Network Type:** NAT Network (`NatNetwork`)

### Virtual Machines

#### 🟩 Ubuntu Server (Target)
- **OS:** Ubuntu 22.04 LTS (Server)
- **Memory:** 2 GB  
- **Disk Size:** 25 GB (VDI, dynamically allocated)  
- **Network Adapter 1:** NAT Network (`NatNetwork`)  
- **Installed Tools:** `net-tools`, `curl`, `apache2`  
- **Purpose:** Acts as a target host and future web/log server.

#### 🟥 Kali Linux (Attacker)
- **OS:** Kali Linux 2025.1 (Installer ISO)
- **Memory:** 2 GB  
- **Disk Size:** 20 GB (VDI, dynamically allocated)  
- **Network Adapter 1:** NAT Network (`NatNetwork`)  
- **Purpose:** Used for penetration testing and scanning experiments.  
- **Installed Tools:** `net-tools`, `nmap` (to be installed next)

---

## 🔌 Network Configuration

| VM | Interface | IP Address | Network Mode | Notes |
|----|------------|-------------|---------------|-------|
| Ubuntu Server | `enp0s3` | `10.0.3.3` | NAT Network (`NatNetwork`) | Target machine |
| Kali Linux | `eth0` | `10.0.3.1` | NAT Network (`NatNetwork`) | Attacker machine |

✅ Both VMs are on the same subnet and can communicate directly.  
Example configuration:
Ubuntu IP: 10.0.3.3
Kali IP: 10.0.3.1

## 🧪 Connectivity Test

**From Kali:**
```bash
ping -c 4 10.0.3.3


Result:
64 bytes from 10.0.3.3: icmp_seq=1 ttl=64 time=0.431 ms
