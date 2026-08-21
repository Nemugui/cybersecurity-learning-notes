# Phase 2 — Networking Notes
**Status:** ✅ Complete
**Duration:** July - August 2026

---

## 1. What the Internet Actually Is

- Internet = physical cables connecting the world
- Data travels as **packets** (small chunks)
- Each packet has: source IP, destination IP, sequence number
- Underwater submarine cables connect countries
- Journey happens in milliseconds

---

## 2. IP Addresses

### IPv4
- Format: 192.168.110.2 (32 bits, 4 numbers 0-255)
- Only 4.3 billion addresses exist
- We ran out — that's why IPv6 was created

### IPv6
- Format: 2001:0db8:85a3::8a2e:0370:7334
- Essentially unlimited addresses
- Internet slowly transitioning to this

### Private vs Public

Private (inside your network):
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16 ← your home network

Public (visible to internet):
Everything else
My public IP: [REDACTED]

### My Network

Kali VM: 192.168.110.2
Laptop: 192.168.110.73
Router: 192.168.110.1
LG TV: 192.168.110.86
Network: 192.168.110.0/24


---

## 3. OSI Model

| Layer | Name         | What it does            | Examples          |
|-------|--------------|-------------------------|-------------------|
| 7     | Application  | User-facing protocols   | HTTP, DNS, SSH    |
| 6     | Presentation | Encryption, compression | SSL/TLS           |
| 5     | Session      | Session management      | Login sessions    |
| 4     | Transport    | Ports, reliability      | TCP, UDP          |
| 3     | Network      | IP addressing, routing  | IP, ICMP          |
| 2     | Data Link    | MAC addresses, switches | Ethernet, WiFi    |
| 1     | Physical     | Cables, signals         | Fiber, WiFi radio |

### TCP vs UDP

TCP → reliable, confirmed delivery, slower
→ used for: web, email, file transfer

UDP → fast, no confirmation
→ used for: gaming, video calls, DNS


### Memory trick (top to bottom):
**A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing

---

## 4. DNS — Domain Name System

DNS = internet's contact list
→ converts domain names to IP addresses

### Resolution Chain

Browser cache
↓ (if not found)
OS cache
↓ (if not found)
ISP Resolver
↓ (if not found)
Root DNS servers (13 worldwide)
↓ redirects to
TLD nameserver (.com, .ph, .net)
↓ redirects to
Authoritative nameserver
↓ returns IP
ISP Resolver caches it
↓
Your browser connects


### DNS Record Types

A → domain → IPv4
AAAA → domain → IPv6
MX → mail server
NS → nameserver
CNAME → alias
TXT → text info (SPF, verification)
PTR → IP → domain (reverse lookup)


### TTL (Time To Live)

→ timer set by domain owner
→ controls how long cache is valid
→ when expired → fresh lookup happens
→ low TTL = faster propagation
→ high TTL = less DNS server load


### DNS Performance (my test results)

Router/ISP DNS (192.168.110.1) → 63ms
Google DNS (8.8.8.8) → 55ms
Cloudflare DNS (1.1.1.1) → 11ms ← fastest


### Key Commands
```bash
dig google.com              # basic lookup
dig google.com MX           # mail servers
dig google.com NS           # nameservers
dig -x 8.8.8.8              # reverse lookup
dig @1.1.1.1 google.com     # use specific DNS
nslookup google.com         # basic lookup
```

### DNS Attacks

DNS Spoofing → fake DNS responses
Cache Poisoning → corrupt DNS cache
DNS Hijacking → control someone's DNS
DNS Tunneling → hide data in DNS queries
Zone Transfer → get ALL DNS records (axfr)


---

## 5. DHCP, NAT, Subnetting

### DHCP

DHCP = automatic IP address assignment
DORA process:
D → Discover (client broadcasts)
O → Offer (server offers IP)
R → Request (client accepts)
A → Acknowledge (server confirms)

Gives you: IP, subnet mask, gateway, DNS, lease time

### NAT

NAT = Network Address Translation
→ translates private IP to public IP
→ allows many devices to share one public IP
→ your router does this automatically

Private: 192.168.110.2 → Public: [your ISP IP]


### Subnetting

/24 = 255.255.255.0 = 254 usable hosts
Network address = first IP (192.168.110.0)
Broadcast = last IP (192.168.110.255)
Usable = 192.168.110.1 to 192.168.110.254

Formula:
Host bits = 32 - subnet size
Usable hosts = 2^(host bits) - 2



---

## 6. Protocols

| Protocol | Port | Encrypted | Purpose |
|----------|------|-----------|---------|
| HTTP     | 80 | ✗ | Web (unsafe) |
| HTTPS    | 443 | ✓ | Web (safe) |
| FTP      | 20,21 | ✗ | File transfer |
| SSH      | 22 | ✓ | Remote access |
| Telnet   | 23 | ✗ | Remote (dangerous) |
| SMTP     | 25,587 | Optional | Send email |
| POP3     | 110 | ✗ | Receive email (download) |
| IMAP     | 143 | ✗ | Receive email (sync) |
| DNS | 53 | ✗ | Domain resolution |
| DHCP | 67,68 | ✗ | IP assignment |
| RDP | 3389 | ✓ | Windows remote desktop |
| SMB | 445 | Optional | Windows file sharing |
| SNMP | 161 | ✗ | Device management |
| LDAP | 389 | ✗ | Directory services |

---

## 7. Ports

Port = virtual door on a device (0-65535)

Ranges:
0-1023 → well-known (HTTP, SSH, FTP)
1024-49151 → registered (MySQL, RDP)
49152-65535 → dynamic (random source ports)

States:
Open → service listening, accepting connections
Closed → nothing listening, actively rejected
Filtered → firewall blocking, no response


### Must Know Ports

20 → FTP data
21 → FTP control
22 → SSH
23 → Telnet
25 → SMTP
53 → DNS
67/68 → DHCP
69 → TFTP
80 → HTTP
110 → POP3
139 → NetBIOS
143 → IMAP
161 → SNMP
443 → HTTPS
445 → SMB
1433 → MSSQL
3306 → MySQL
3389 → RDP


---

## 8. Firewalls, Routers, Switches

### Firewall

Security guard checking all traffic
→ allows or blocks based on rules
→ logs suspicious activity

Types:
Packet filtering → checks IP/port only
Stateful → tracks connection state
NGFW → inspects content (Layer 7)

Actions:
ALLOW → let through
BLOCK → silently drop (better for security)
REJECT → drop and notify sender
LOG → record without blocking

Linux tools: iptables, ufw

### Router

Post office sorting facility
→ forwards packets between networks
→ uses IP addresses (Layer 3)
→ has routing table
→ your home router also does NAT, DHCP, WiFi


### Switch

Smart traffic director within network
→ sends data only to correct device
→ uses MAC addresses (Layer 2)
→ builds MAC address table
→ VLANs for network segmentation

Hub vs Switch:
Hub → sends to ALL devices (dumb, old)
Switch → sends to CORRECT device (smart)


### How they work together

Internet → Router → Firewall → Switch → Your devices


---

## 9. Wireshark

Network packet analyzer
→ captures all traffic on network
→ shows source, destination, protocol, content
→ industry standard tool

Two versions:
wireshark → GUI (graphical)
tshark → CLI (command line)


### Essential Filters
```bash
http                        # HTTP traffic only
dns                         # DNS traffic only
tcp.port == 80              # specific port
ip.addr == 192.168.110.1    # specific IP
icmp                        # ping traffic
arp                         # ARP traffic
```

### Key tshark Commands
```bash
sudo tshark -i eth0                    # capture live
sudo tshark -i eth0 -f "port 53"      # capture filter
sudo tshark -i eth0 -w file.pcap      # save to file
tshark -r file.pcap -Y "dns"          # read + filter
```

### What you CAN'T see

HTTPS → encrypted, content hidden
SSH → encrypted, commands hidden
→ this is why encryption matters


---

## 10. VPNs

VPN = encrypted tunnel to VPN server
→ hides real IP address
→ encrypts traffic from ISP
→ traffic appears from VPN server location

### VPN Protocols (best to worst)

WireGuard → fastest, modern, secure
OpenVPN → reliable, open source
IKEv2 → good for mobile
L2TP → older, less secure
PPTP → broken, never use

### What VPN protects

✓ ISP seeing your browsing
✓ Public WiFi attacks
✓ Basic IP tracking

✗ Malware on device
✗ Phishing attacks
✗ VPN provider itself
✗ DNS leaks


### Linux VPN Commands
```bash
sudo openvpn --config file.ovpn    # connect OpenVPN
sudo wg-quick up wg0               # connect WireGuard
sudo wg-quick down wg0             # disconnect
sudo wg show                       # check status
curl ifconfig.me                   # verify IP changed
```

---

## Phase 2 Real World Practice

### Devices Found on My Network
| IP | Device | Open Ports | Notes |
|----|--------|------------|-------|
| 192.168.110.1 | Ruijie Router | 53,80,443 | lighttpd web server |
| 192.168.110.73 | My Laptop (NEMU) | 2968 | Windows 11 |
| 192.168.110.86 | LG Smart TV | 8 ports | WebOS, HTTP on 8008 |
| 192.168.110.205 | TOGL21-045 | 135,139,445 | Windows device, SMB open |
| 192.168.110.214 | Redmi Note 13 Pro | none | Android, all ports closed |

### Security Findings

Finding 1: Router exposing firmware version in HTTP banner
Finding 2: LG TV has 8 open ports (large attack surface)
Finding 3: TOGL21-045 has SMB signing not required
→ vulnerable to NTLM relay attacks
Finding 4: Unknown device MAC randomization (Android)


### Tools Used This Phase

nmap → network scanning and reconnaissance
dig → DNS analysis and enumeration
tshark → packet capture and analysis
curl → web requests and header inspection
ping → connectivity testing
traceroute → network path analysis
ip addr → network interface info
arp -a → ARP table (learned MACs)
ipcalc → subnet calculation
macchanger → MAC address manipulation
ufw → firewall management
iptables → advanced firewall rules


---

*Notes by Nemugui | Phase 2 Complete ✅*
*Next: Phase 3 — Linux*
