# TryHackMe Fundamentals: Networking, Lan essentials & Linux

> **Author:** Abrham Addis  
> **Topic:** Linux Fundamentals, Networking Architecture, LAN Essentials & Network Recon  
> **Course:** INSA Cyber Security & CTC Fundamentals  

---

## 📑 Overview & Objectives

In this writeup, we cover the foundational building blocks required for offensive and defensive operations across two primary pillars:
1. **Linux Operating System Architecture:** File system hierarchy, permissions, ownership, process tracking, and secure remote administration.
2. **Networking Fundamentals & LAN Essentials:** OSI/DoD reference models, IP addressing, subnetting, critical infrastructure protocols (ARP, DHCP, DNS, TCP/UDP), and packet analysis with Nmap and Wireshark.

---

## Part 1: Linux Fundamentals

### 1.1 Linux Filesystem Hierarchy Standard (FHS)
Unlike Windows which organizes storage around drive letters (`C:`, `D:`), Linux represents all files, devices, and memory structures as a single unified tree starting at the root directory `/`.

Key system directories:
* `/bin` & `/usr/bin`: Core user command binaries (e.g., `ls`, `grep`, `cat`, `nmap`).
* `/sbin` & `/usr/sbin`: Administrative binaries used by root (e.g., `iptables`, `fdisk`, `ifconfig`).
* `/etc`: System-wide configuration files (e.g., `/etc/passwd`, `/etc/shadow`, `/etc/network/interfaces`, `/etc/ssh/sshd_config`).
* `/var`: Variable data files such as system logs (`/var/log/syslog`, `/var/log/auth.log`), mail, and web server roots (`/var/www/html`).
* `/proc`: Virtual pseudo-filesystem providing an interface to kernel data structures, process statuses, and hardware runtime info (e.g., `/proc/cpuinfo`, `/proc/1/cgroup`).
* `/dev`: Device nodes representing hardware components (e.g., disk storage `/dev/sda`, terminals `/dev/pts/*`, pseudo-devices `/dev/null`, `/dev/urandom`).
* `/home`: User personal working directories.
* `/tmp`: Temporary storage cleared on system reboot, writable by all users (frequent staging area for reverse shells and privesc scripts).

### 1.2 File Permissions and Access Control
Linux enforces security through discretionary access control (DAC) based on three privilege tiers:
* **User (u):** The owner of the file.
* **Group (g):** Users belonging to the file's assigned group.
* **Others (o):** Everyone else.

Permissions breakdown:
| Permission | Character | Octal Value | Description on Files | Description on Directories |
|:---|:---:|:---:|:---|:---|
| **Read** | `r` | `4` | View contents | List entries (`ls`) |
| **Write** | `w` | `2` | Modify contents | Create/delete entries |
| **Execute** | `x` | `1` | Run as binary/script | Traverse into folder (`cd`) |

#### Modifying Permissions (`chmod`) & Ownership (`chown`):
```bash
# Set owner read/write/execute, group read/execute, others no access (750)
chmod 750 /opt/custom_script.sh

# Symbolic permission assignment (add execution for user and group)
chmod ug+x exploit.py

# Transfer ownership of file to user and group
sudo chown abrham:security_team findings.txt
```

#### Special Permissions (SUID, SGID, Sticky Bit):
* **SUID (Set User ID - `4000`):** File executes with the privileges of the file owner rather than the executing user (frequent vector for privilege escalation when misconfigured, e.g., GTFOBins).
  ```bash
  # Finding SUID binaries on a compromised machine:
  find / -perm -u=s -type f 2>/dev/null
  ```
* **Sticky Bit (`1000`):** Applied to directories (like `/tmp`) so only the file owner can delete or rename their files, even if others have write access.

### 1.3 Process Lifecycle and Management
Every active program runs as an isolated process assigned a unique **Process ID (PID)**.
* **PID 1:** The initialization process (`systemd` or `init`) that spawns all subsequent userland services.
* **Process states:** Running (`R`), Sleeping (`S`), Stopped (`T`), Zombie (`Z`).

```bash
# View active processes with hierarchy and users
ps aux | grep -i nmap

# Interactive process monitoring
top -d 1  # or btop/htop

# Terminating unresponsive or rogue processes
kill -15 <PID>   # SIGTERM (Graceful shutdown)
kill -9 <PID>    # SIGKILL (Forced immediate termination)

# Running tasks in the background
./scan_network.sh &
jobs -l
fg %1
```

### 1.4 Secure Remote Administration (SSH)
The Secure Shell (SSH) protocol operates on TCP port 22, utilizing asymmetric cryptography for host authentication and session key exchange.

```bash
# Generate high-security Ed25519 keypair
ssh-keygen -t ed25519 -C "security-audit"

# Connect using private identity key
ssh -i ~/.ssh/id_ed25519 -p 22 operator@10.10.10.50

# Transfer file securely over encrypted tunnel
scp -P 22 ./proof.txt operator@10.10.10.50:/home/operator/
```

---

## Part 2: Network Fundamentals & LAN Essentials

### 2.1 The Reference Models: OSI vs. TCP/IP
Networking relies on standardized abstraction layers to separate physical transport from application logic.

| Layer | OSI Model | TCP/IP Model | Protocol Data Unit (PDU) | Key Protocols / Standards |
|:---:|:---|:---|:---:|:---|
| **7** | Application | Application | Data | HTTP, HTTPS, SSH, DNS, DHCP, FTP |
| **6** | Presentation | Application | Data | SSL/TLS, ASCII, JPEG |
| **5** | Session | Application | Data | RPC, NetBIOS |
| **4** | Transport | Transport | Segment (TCP) / Datagram (UDP) | TCP, UDP |
| **3** | Network | Internet | Packet | IPv4, IPv6, ICMP, ARP, IPsec |
| **2** | Data Link | Network Access | Frame | Ethernet (802.3), Wi-Fi (802.11), MAC |
| **1** | Physical | Network Access | Bits | Fiber, Copper, Radio Frequencies |

### 2.2 Local Area Network (LAN) & IP Addressing
A Local Area Network connects systems within a bounded physical area (home, office, campus).
* **MAC Address (Layer 2):** 48-bit hardware identifier burned into the Network Interface Card (NIC), e.g., `00:1A:2B:3C:4D:5E`.
* **IPv4 Address (Layer 3):** 32-bit logical identifier structured in 4 octets (e.g., `192.168.1.10`).

#### CIDR Notation & Subnetting:
A subnet mask divides an IP address into **Network Bits** and **Host Bits**.
* `/24` (Subnet: `255.255.255.0`): 24 network bits, 8 host bits → $2^8 - 2 = 254$ usable hosts.
* `/16` (Subnet: `255.255.0.0`): 16 network bits, 16 host bits → $2^{16} - 2 = 65,534$ usable hosts.

Private IPv4 Address Ranges (RFC 1918):
* Class A: `10.0.0.0` – `10.255.255.255` (`10.0.0.0/8`)
* Class B: `172.16.0.0` – `172.31.255.255` (`172.16.0.0/12`)
* Class C: `192.168.0.0` – `192.168.255.255` (`192.168.0.0/16`)

### 2.3 Core LAN Protocols

#### 1. Address Resolution Protocol (ARP)
Bridges Layer 3 to Layer 2 by mapping a known IP address to a physical MAC address within the broadcast domain.
* **Mechanism:** Host broadcasts `Who has 192.168.1.1? Tell 192.168.1.50`. The gateway replies unicast with its MAC.
* **Security Risk:** ARP has no authentication; hosts accept unsolicited replies, enabling **ARP Spoofing / Man-In-The-Middle (MITM)** attacks.
```bash
# View local ARP table
arp -a  # or: ip neighbor show
```

#### 2. Dynamic Host Configuration Protocol (DHCP)
Automates IP assignment, default gateway configuration, and DNS server distribution. Operates via the **DORA** four-way handshake:
1. **Discover:** Client broadcasts request for an IP (`DHCPDISCOVER`).
2. **Offer:** Server offers reserved configuration (`DHCPOFFER`).
3. **Request:** Client accepts the offer (`DHCPREQUEST`).
4. **Acknowledge:** Server confirms lease (`DHCPACK`).

#### 3. Domain Name System (DNS)
Translates human-readable domain names into routable IP addresses over UDP port 53 (TCP for large zone transfers).
```bash
# Query specific DNS records
dig +short A github.com @8.8.8.8
nslookup -type=MX bdu.edu.et
```

### 2.4 Transport Layer: TCP vs. UDP

* **Transmission Control Protocol (TCP):** Connection-oriented, guaranteed delivery, sequenced packets, flow and congestion control.
  * **Three-Way Handshake:**
    1. Client $\rightarrow$ Server: `SYN` (Synchronize sequence number)
    2. Server $\rightarrow$ Client: `SYN-ACK` (Acknowledge and synchronize)
    3. Client $\rightarrow$ Server: `ACK` (Acknowledge connection established)
  * **Four-Way Teardown:** `FIN` $\rightarrow$ `ACK` $\rightarrow$ `FIN` $\rightarrow$ `ACK`.

* **User Datagram Protocol (UDP):** Connectionless, best-effort delivery, minimal overhead, no retransmissions. Ideal for real-time traffic (DNS, DHCP, VoIP, streaming, NTP).

### 2.5 Common Network Services & Default Ports
| Port | Protocol | Service | Description |
|:---:|:---:|:---|:---|
| `21` | TCP | FTP | File Transfer Protocol (Cleartext) |
| `22` | TCP | SSH | Secure Shell Remote Login |
| `23` | TCP | Telnet | Unencrypted Remote Terminal |
| `25` | TCP | SMTP | Simple Mail Transfer Protocol |
| `53` | UDP/TCP | DNS | Domain Name System |
| `67/68`| UDP | DHCP | Dynamic Host Configuration Protocol |
| `80` | TCP | HTTP | Hypertext Transfer Protocol (Cleartext) |
| `110`| TCP | POP3 | Post Office Protocol |
| `123`| UDP | NTP | Network Time Protocol |
| `143`| TCP | IMAP | Internet Message Access Protocol |
| `443`| TCP | HTTPS | HTTP over TLS/SSL |
| `445`| TCP | SMB | Server Message Block (Windows File Sharing) |
| `3389`| TCP | RDP | Remote Desktop Protocol |

---

## Part 3: Network Reconnaissance & Traffic Analysis

### 3.1 Network Mapping with Nmap
Nmap (Network Mapper) identifies active hosts, open ports, running services, and operating system signatures.

```bash
# 1. Fast Host Discovery (Ping Sweep on LAN)
nmap -sn 192.168.1.0/24

# 2. Stealth SYN Scan with Service & OS Detection
sudo nmap -sS -sV -O -p- --min-rate 1000 10.10.10.50

# 3. Default Safe Vulnerability Scripts Scan
nmap -sC -sV -p 21,22,80,443 -oN initial_recon.txt 10.10.10.50

# 4. UDP Port Scan (Common Target Services)
sudo nmap -sU -p 53,67,123,161 --top-ports 50 10.10.10.50
```

### 3.2 Packet Capture & Inspection (Wireshark & tcpdump)
Analyzing raw packet captures enables validation of protocol handshakes, identification of unencrypted credentials, and detection of anomalous traffic.

```bash
# Capture packets on wireless/ethernet interface filtering for specific host
sudo tcpdump -i wlp2s0 host 10.10.10.50 and port 80 -w traffic_capture.pcap

# Read and inspect capture file in terminal
tcpdump -r traffic_capture.pcap -nn -X | head -n 30
```

#### Key Wireshark Display Filters:
* `ip.addr == 192.168.1.50`: Filter traffic to/from target IP.
* `tcp.flags.syn == 1 and tcp.flags.ack == 0`: Filter initial SYN connection attempts.
* `http.request.method == "POST"`: Locate form submissions, login parameters, and API interactions.
* `dns.flags.response == 1`: Inspect resolved domain responses.
* `arp`: Monitor ARP broadcast requests and replies for poisoning detection.

---

## 🎯 Summary & Key Takeaways

1. **Linux System Administration:** A thorough understanding of permissions (`chmod`, `chown`), directory hierarchy (`/proc`, `/etc`, `/var/log`), and SUID execution is essential for both system hardening and privilege escalation.
2. **Network Protocol Mechanics:** The interaction between Layer 2 (ARP/MAC), Layer 3 (IP/Routing), and Layer 4 (TCP Handshakes/UDP) forms the foundation of all communication across enterprise and lab environments.
3. **Reconnaissance Methodology:** Always begin with non-intrusive host discovery before advancing to focused port scans, service enumeration, and full packet dissection.
