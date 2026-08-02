# DNS Investigation Report
**Author:** Nemugui
**Date:** July 2026
**Tools Used:** dig, nslookup, curl

---

## What is DNS?
DNS (Domain Name System) = the internet's contact list.
Converts human-readable domain names into IP addresses.

-You type: google.com
-DNS finds: 142.251.10.138
-Browser connects to that IP

---

## DNS Record Types

-| Record | Purpose | Example |
-|--------|---------|---------|
-| A | Domain → IPv4 address | google.com → 142.251.10.138 |
-| AAAA | Domain → IPv6 address | google.com → 2607:f8b0::200e |
-| MX | Mail server for domain | google.com → aspmx.l.google.com |
-| NS | Nameserver for domain | google.com → ns1.google.com |
-| CNAME | Alias to another domain | www → google.com |
-| TXT | Text information | SPF, verification records |
-| PTR | IP → Domain (reverse) | 8.8.8.8 → dns.google |

---

## DNS Tools

### dig — detailed DNS lookup
```bash
dig google.com              # basic A record lookup
dig google.com MX           # find mail servers
dig google.com NS           # find nameservers
dig google.com TXT          # find text records
dig google.com ANY          # get all records
dig -x 8.8.8.8              # reverse lookup (IP to domain)
dig @1.1.1.1 google.com     # use specific DNS server
dig @8.8.8.8 google.com     # use Google DNS
```

### nslookup — basic DNS lookup
```bash
nslookup google.com         # basic lookup
nslookup -type=MX google.com # mail server lookup
```

---

## Investigation 1 — Google DNS Analysis

### Command
```bash
dig google.com
```

### Results
- 142.251.10.138
- 142.251.10.102
- 142.251.10.113
- 142.251.10.101
- 142.251.10.139
- 142.251.10.100

### Analysis
Google returned 6 IP addresses — this is called **load balancing:**
- Google has thousands of servers worldwide
- Multiple IPs returned so browser picks one
- If one server fails, others handle the traffic
- This is why Google never goes down

---

## Investigation 2 — DNS Server Comparison

### Command
```bash
dig @192.168.110.1 google.com | grep "Query time"
dig @8.8.8.8 google.com | grep "Query time"
dig @1.1.1.1 google.com | grep "Query time"
```

### Results
-| DNS Server | Response Time | Notes |
-|------------|--------------|-------|
-| 192.168.110.1 (Router/ISP) | 63ms | Slowest — forwards to ISP |
-| 8.8.8.8 (Google DNS) | 55ms | Middle — large infrastructure |
-| 1.1.1.1 (Cloudflare DNS) | 11ms | Fastest — optimized for speed |

### Conclusion
Switched to Cloudflare DNS (1.1.1.1) for:
- Faster response times (11ms vs 63ms)
- Better privacy (doesn't log queries like ISP)
- More reliable uptime

### How to change DNS in Kali
```bash
sudo nano /etc/resolv.conf
# Change to:
nameserver 1.1.1.1
nameserver 8.8.8.8
```

---

## Investigation 3 — Reverse DNS Lookup

### Command
```bash
dig -x 172.217.26.238
```

### Results
- bom05s09-in-f14.1e100.net
- ncmnlh-ak-in-f14.1e100.net
- nrt12s51-in-f14.1e100.net

### Analysis
- `1e100.net` = Google's infrastructure domain
  (1e100 = scientific notation for googol = Google)
- `bom` = Mumbai datacenter (closest to Philippines)
- `nrt` = Tokyo/Narita datacenter
- Google serves the Philippines from Mumbai and Tokyo

### Key Learning
-Reverse DNS doesn't always return the friendly domain name.
-Large companies like Google use separate infrastructure
-domains (1e100.net) for their actual servers.

---

## Investigation 4 — Nameserver Lookup

### Command
```bash
dig facebook.com NS
```

### Results
- facebook.com NS a.ns.facebook.com
- facebook.com NS b.ns.facebook.com
- facebook.com NS c.ns.facebook.com
- facebook.com NS d.ns.facebook.com

### Zone Transfer Attempt
```bash
dig axfr @a.ns.facebook.com facebook.com
# Result: Transfer failed (properly secured)
```

### Analysis
- Facebook uses 4 nameservers for redundancy
- Zone transfer blocked — good security practice
- Misconfigured servers allow zone transfers
- Zone transfer reveals ALL DNS records (huge security risk)

---

## DNS Security Attacks

-| Attack | How It Works | Defense |
-|--------|-------------|---------|
-| DNS Spoofing | Fake DNS responses redirect to malicious IP | DNSSEC |
-| DNS Cache Poisoning | Corrupt DNS cache with fake records | DNSSEC, short TTL |
-| DNS Hijacking | Change DNS settings to attacker's server | Monitor DNS settings |
-| DNS Tunneling | Hide data inside DNS queries | Monitor DNS traffic |
-| Zone Transfer | Get ALL DNS records from misconfigured server | Restrict zone transfers |

---

## My Current DNS Configuration
-Local DNS: 192.168.110.1 (router → ISP)
-Current DNS: 1.1.1.1 (Cloudflare — faster and private)
-Backup DNS: 8.8.8.8 (Google)
-ISP: [Philippine ISP]

---

## Key Takeaways

- DNS = internet's phone book
- Multiple IPs per domain = load balancing
- Cloudflare 1.1.1.1 = fastest DNS
- Reverse DNS reveals infrastructure info
- Zone transfers = dangerous if misconfigured
- DNS attacks are extremely common
- Always use encrypted DNS when possible (DoH/DoT)

---

*Investigation by Nemugui | Phase 2 — Networking*
