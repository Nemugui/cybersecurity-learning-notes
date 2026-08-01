# Phase 1 — Computer Fundamentals Notes
**Status:** ✅ Complete
**Duration:** July 2026


## 1. How Computers Process Data

### Computer Components and its function
- CPU = is the one who process and execute everything, it is the brain of the computer.
- RAM = is the temporary storage it stores data from your drive (HDD/SSD), Holds active programs that the CPU is currently working on.
- STORAGE/ROM = stores everything, apps file, data on your computer
- OS = is the one who manage all computers resources
- KERNEL = is the one who established connection between your components and your software

### The Kitchen Analogy
- CPU = the chef (does all thinking and calculations)
- RAM = the counter (temporary fast storage, cleared on shutdown)
- Storage = the pantry (permanent slow storage, survives shutdown)
- OS = the restaurant manager (controls everything)
- Kernel = the building itself (core of the OS)

### Technical Details
- CPU measured in GHz (speed) and cores (parallel tasks)
- RAM = volatile memory (erased when power off)
- Storage = non-volatile (HDD mechanical, SSD faster)
- When you open Chrome: Storage → RAM → CPU processes it
  
### Cybersecurity Relevance
- Malware hides in RAM (fileless malware)
- Forensics analysts extract RAM dumps for evidence
- Buffer overflow attacks target how programs use memory
- Kernel exploits = Ring 0 access = full system control


---

## 2. Binary, Hexadecimal and Number Systems

### Binary (Base-2)
- Computers only understand 0 and 1 (off/on)
- Everything stored as combinations of 0s and 1s
- 1 bit = single 0 or 1
- 8 bits = 1 byte = one character

### Hexadecimal (Base-16)
- Shorthand for binary using 0-9 and A-F
- 4 bits = exactly 1 hex digit
- Example: 11001010 binary = CA hex

### Conversion Table
- 0000 = 0 | 0100 = 4 | 1000 = 8 | 1100 = C |
- 0001 = 1 | 0101 = 5 | 1001 = 9 | 1101 = D |
- 0010 = 2 | 0110 = 6 | 1010 = A | 1110 = E |
- 0011 = 3 | 0111 = 7 | 1011 = B | 1111 = F |

### Units
- 1 bit = single 0 or 1
- 8 bits = 1 Byte
- 1024 Bytes = 1 KB
- 1024 KB = 1 MB
- 1024 MB = 1 GB
- 1024 GB = 1 TB

### In Addition
We have:
- Decimal
- Binary
- Hexadecimal
- Octal

### Cybersecurity Relevance
- Memory addresses shown in hex (0x7FFE1234)
- MAC addresses written in hex
- Malware analysis requires reading hex dumps
- Encryption keys often in hex format

---

## 3. Operating System Basics

### What is an OS?
The manager between you and the hardware. Without it,
hardware is just expensive metal doing nothing.

### Three Major OS
| OS | Made By | Used For | Security |
|----|---------|----------|----------|
| Windows | Microsoft | Desktops, offices | Most targeted |
| macOS | Apple | Creative, dev | More secure |
| Linux | Community | Servers, security | Most flexible |

### The Kernel
- Core of every OS — Windows, macOS, Linux all have one
- Manages: CPU, RAM, devices, security boundaries
- Ring 0 = kernel level = complete control
- Ring 3 = user level = limited access
- Kernel exploit = jumping from Ring 3 to Ring 0

### Four Main Job of Kernel
- Process Management
- Memory Management
- Device Management
- Security and Access control

### Key Differences
- Windows: C:\ drives, .exe files, Registry
- Linux: / root, no extensions needed, everything is a file
- macOS: Unix-based, similar terminal to Linux

### Cybersecurity Relevance
- Most malware targets Windows (most common)
- Kali Linux is the standard pentesting OS
- Kernel exploits are the most powerful attacks
- Registry is where malware hides for persistence

### More 
- Registry = Settings are stored in a giant database of system configuration
- Software = refers to the program and all the app that makes your hardware do something useful
- Hardware = refers to the tangible things that makes your computer or other device such as TV, COMPUTER to work
  
---

## 4. File Systems and Permissions

### What is File System
- It is the organizational system that labels every box and group them into sections and keeps a map of where everything is.

### File Systems
- Windows → NTFS
- macOS → APFS
- Linux → ext4
- USB → FAT32 / exFAT

### Linux Directory Structure
- / → root (everything starts here)
- /home → user folders
- /etc → configuration files
- /var → logs and variable data
- /bin → essential programs
- /tmp → temporary files (malware loves here)
- /root → admin user home folder

### Linux Permissions
- Read 4(r) - View the file content
- Write 2(w) - Modify or Delete the file
- Execute 1(x) - Can run the file as a program
-  (-) = file, d = directory

### Permission Numbers
- r = 4, w = 2, x = 1
- 7 = rwx (full access)
- 6 = rw- (read + write)
- 5 = r-x (read + execute)
- 4 = r-- (read only)
- 777 = dangerous (everyone full access)
- 755 = normal (owner full, others read+execute)

### 3 Types of User
- Owner = The person who owns the File.
- Group = A team of user sharing access.
- Others = Everyone else on the system.

### Windows ACL
ACL = Access Control list - list assign to all folder
- It simply defines WHO is allowed (or denied) access, and WHAT they are allowed to do with it.

- Full Control = Do Everything
- Modify = Read,Write,Delete
- Read & X = Read & Run
- Read = Read Only/View
- Write = Can only Modify/Change Content

### Quick Summary:
- File System = how storage is organize
- NTFS = New Technology File System (Windows File System)
- ext4 = Linux File System (Fourth Extended Filesystem)
- / = Root of Everything in Linux
- c:\ = Root of Windows
- Fat = File Allocation Table.

### Cybersecurity Relevance
- Misconfigured permissions = privilege escalation
- 777 files = red flag in security audits
- /tmp folder = common malware staging area
- Forensics: file timestamps reveal attack timeline

---

## 5. Basic Command Line Usage

### Why CLI Matters
- Every security tool runs in terminal
- Remote servers have no GUI
- Faster and more powerful than GUI
- Automation through scripting

### Essential Commands
```bash

# Navigation
pwd          → where am I?
ls           → what's here?
ls -la       → details + hidden files
cd folder    → go into folder
cd ..        → go back one level
cd ~         → go home
cd /         → go to root

# File Operations
cat file     → read file contents
touch file   → create empty file
mkdir folder → create folder
cp a b       → copy a and paste b
mv a b       → move or rename
rm file      → delete (permanent!)
rm -r folder → delete folder (rm dir)

# Search
grep "word" file     → find word in file
grep -r "word" dir/  → search recursively
find / -name "file"  → find file on system

# System
whoami       → current user
ps aux       → all running processes
kill PID     → stop a process
sudo command → run as admin
clear        → clean screen

# Packages
sudo apt update        → refresh software list
sudo apt install name  → install software
sudo apt remove
sudo shutdown now

# Chaining
&&   → run next if previous succeeded
;    → always run next command
|    → pipe output to next command
>    → save output to file (overwrite)
>>   → append output to file

# NMAP
nmap -sn 192.x.x.x - find all device on the network
nmap -sV 192.x.x.x - scan anything based on IP
     -sV -p 80 192.x.x.x - scan specific port only

dig @192.x.x.x name +short, +stats
dig 1.1.1.1 name
nslookup name 192.x.x.x
dnsrecon -d <domain> -nn <ip>
curl -v -L -I -o
curl ifconfig.me - displays your public ip

# Additional
clear -> to clean the terminal
rm -r -> delete the entire folder
grep -i -> case insensitive searching
-r -> recursive - search through entire folder
/etc/ - system wide configuration (user acc, network)
-i -> case insensitive
* -> everything
ps aux --sort=-%cpu | head -10 -> check the processor
ps aux --sort=-%mem | head -10 -> check the ram

```
### Path Types
- Absolute: /home/kali/Documents/file.txt
- Relative: Documents/file.txt (from current location)
- ../ → go up one level

### Cybersecurity Relevance
- whoami → first command after system compromise
- ps aux → find malicious processes
- find / -name "*.conf" → locate config files
- grep -r "password" → search for credentials
- chmod → change permissions

---

## 6. How Software is Installed and Runs

### Windows Installation
- .exe or .msi installer
- → copies files to C:\Program Files
- → writes to Registry
- → creates shortcuts
- → program ready

### Linux Installation
- sudo apt install program
- → downloads from repository
- → verifies integrity
- → copies to /usr/bin/
- → updates software database
- → ready to use anywhere

### Key Concepts
- Repository = official online software library
- PATH = folders Linux searches for programs
- Process = software while running (has PID)
- Daemon = background process always running
- Dependencies = other software a program needs

### Cybersecurity Relevance
- Malware installs itself like legitimate software
- Registry = where malware hides for persistence
- PATH manipulation = privilege escalation technique
- Rogue repositories = supply chain attacks
- Daemons = common attack targets (always running)

---

## Phase 1 Tools Used
- VirtualBox → VM hypervisor
- Kali Linux → security-focused Linux distro
- Terminal → command line interface

## Phase 1 Labs Completed
- [x] Set up VirtualBox with Kali Linux VM
- [x] Practiced all CLI navigation commands
- [x] Built folder structures with chained commands
- [x] Binary to hexadecimal conversion exercises
- [x] File permissions analysis with ls -la
- [x] Process analysis with ps aux
- [x] Package installation with apt

---
*Notes by Nemugui | Phase 1 Complete ✅*
