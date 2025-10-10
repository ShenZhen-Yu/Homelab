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


Artifacts to produce next (planned)

~/lab_artifacts/nmap_scan_results.txt (nmap output)

~/lab_artifacts/pcap_first200.txt (small pcap textual export)

~/lab_artifacts/pcap_summary_conversations.txt (tshark IP conv summary)

screenshots/ping_kali_to_ubuntu.png

Next steps

Install nmap on Kali and tcpdump/tshark on Ubuntu.

Run tcpdump on Ubuntu and run an nmap scan from Kali.

Save outputs, summarize with tshark, commit artifacts to repo (small text summaries only), and push.

## 2025-10-06 — Fixed APT repositories and Internet connectivity
**Issue:** Kali couldn’t install packages because APT had no sources and no Internet.
**Fix:** 
- Added Adapter 2 (NAT) in VirtualBox for Internet.
- Updated `/etc/apt/sources.list` to include:
  `deb http://http.kali.org/kali kali-rolling main contrib non-free non-free-firmware`
- Ran `sudo apt update` and installed `nmap`, `net-tools`, and `isc-dhcp-client`.

**Verification:**
- `ping http.kali.org` succeeded.
- `nmap --version` displayed installed version.
- `ifconfig` worked (from net-tools).


## 2025-10-06 — Ubuntu network setup (Internal + NAT)
**Goal:** Ensure Ubuntu VM has the correct network adapters for lab & internet access.

**What I did**
- Verified VirtualBox network adapters:
  - Adapter 1 = Internal Network (`lab-net` or `NatNetwork`) for VM-to-VM traffic.
  - Adapter 2 = NAT for internet / apt updates (optional if needed).
- Brought up the interface on Ubuntu and requested DHCP lease (if required).
- Verified connectivity with `ip -4 addr show` and `ping -c 3 8.8.8.8`.
- Confirmed Kali ↔ Ubuntu internal connectivity (`ping` from Kali succeeded).

**Commands used**
```bash
# show interfaces & IPs
ip -4 addr show

# (if interface was down)
sudo ip link set enp0s3 up         
sudo dhclient enp0s3               

# verify connectivity
ping -c 3 8.8.8.8
```

**Next:** Use `nmap` to scan the Ubuntu VM and capture packets on Ubuntu.


## 2025-10-06 — Fixed DHCP issue on Kali and Ubuntu (Internal Network)
**Issue:** Both Kali and Ubuntu VMs were unable to receive IPv4 addresses, showing `169.254.x.x` (APIPA address).
**Fix:**
- Verified VirtualBox settings to ensure both VMs used the same **Internal Network**.
- Configured **static IPs** for both VMs on the internal network (`10.0.3.5` for Ubuntu, `10.0.3.10` for Kali).
- Optionally, set up **dnsmasq** as a DHCP server on Kali for future DHCP needs.

**Verification:**
- `ip -4 addr show` showed valid IPs for both VMs.
- Successfully pinged Kali from Ubuntu and vice versa.
- Internet access verified on Kali.

**Next step:** Proceed with running nmap scan and capturing packets.


## 2025-10-06 — Working with Default Route and Internet Access for Kali and Ubuntu
**Issue:** Initially, Kali and Ubuntu could not access the Internet when `eth0` was set to `10.0.3.1`.
**Fix:**
- Set the **default gateway** for NAT network for both VMs to **`10.0.3.2`** (acting as the gateway for Internet access via NAT).
- **Kali** (`10.0.3.1`) and **Ubuntu** (`10.0.3.3`) use **`10.0.3.2`** as the default route to reach the **Internet**.

**Verification:**
- `ip route` shows **`10.0.3.2`** as the default gateway for both VMs.
- Successfully pinged **8.8.8.8** (Internet) from Kali and Ubuntu.
- Internal communication confirmed (ping between Kali and Ubuntu).

**Next step:** Proceed with running `nmap` scan and capturing packets.


## 2025-10-06 — Nmap Scan Results for Ubuntu

**Objective:** Scan Ubuntu (`10.0.3.3`) for open ports and services.

**Command Run:**
```bash
sudo nmap -sS 10.0.3.3
```
Results:

Port 80/tcp: open (HTTP)

Next steps: Continue with packet capture and document results.


## 2025-10-06 — Packet Capture Analysis on Ubuntu (Command-line with `tcpdump`)
**Objective:** Capture and analyze network traffic between Kali and Ubuntu using `tcpdump`.

### Capture Overview:
- **Tool Used:** tcpdump
- **Captured Interfaces:** `enp0s3` (internal network), `eth1` (NAT interface)

### Steps Taken:
1. **Captured ICMP traffic** between Kali and Ubuntu (`ping` test).
2. **Captured HTTP traffic** from Ubuntu (web server on port 80).
3. **Captured ICMP traffic** for internet access (`ping 8.8.8.8` from Kali).

### Key Findings:
- **ICMP Traffic:**
  - Ping requests and replies between Kali and Ubuntu were successfully captured.
- **HTTP Traffic:**
  - HTTP requests and responses between Ubuntu's web server and Kali were successfully captured.
- **Internet Traffic:**
  - Internet access traffic from Kali (via `10.0.3.2` as the gateway) was successfully captured.

### Next Steps:
- Analyze the **specific packet details** from the capture.
- Investigate if there are any unexpected behaviors in the captured traffic.

