# Linux CLI Cheat Sheet
**Author:** Nemugui
**Updated:** July 2026

---

## Navigation
```bash
pwd                  # where am I right now?
ls                   # list files in current folder
ls -la               # list with details + hidden files
cd foldername        # go into folder
cd ..                # go back one level
cd ~                 # go to home folder
cd /                 # go to root
```

---

## File Operations
```bash
cat file.txt         # read file contents
touch file.txt       # create empty file
mkdir foldername     # create folder
mkdir -p a/b/c       # create nested folders at once
cp file1 file2       # copy file1 to file2
cp file1 folder/     # copy file into folder
mv file1 file2       # rename file1 to file2
mv file1 folder/     # move file into folder
rm file.txt          # delete file (permanent!)
rm -r foldername     # delete folder and contents
```

---

## Reading and Writing
```bash
cat file.txt                    # display file contents
echo "text"                     # print text to screen
echo "text" > file.txt          # write text to file (overwrite)
echo "text" >> file.txt         # append text to file
echo -e "line1\nline2"          # print with newlines
cat f1.txt f2.txt > combined.txt # combine two files
```

---

## Searching
```bash
grep "word" file.txt            # find word in file
grep -i "word" file.txt         # case insensitive search
grep -r "word" folder/          # search recursively
grep -v "word" file.txt         # show lines WITHOUT word
find / -name "file.txt"         # find file on entire system
find folder/ -name "*.txt"      # find all txt files in folder
find / -name "*.txt" 2>/dev/null # hide permission errors
ls -la | grep "^d"   # filters only directory
ls -la | grep "^-"   # filters only files
```

---

## System Information
```bash
whoami                # current username
pwd                   # current directory
hostname              # computer name
uname -a              # system information
id                    # user ID and groups
echo $PATH            # show PATH variable
echo $?               # exit code of last command
```

---

## Process Management
```bash
ps aux                # show all running processes
ps aux | grep python  # find specific process
ps aux | wc -l        # counts lines, tell you total number of processes running
kill 1234             # stop process with PID 1234
kill -9 1234          # force stop process
top -bn1              # is a Linux command used to display a real-time, one-time snapshot of running processes, CPU usage, and memory usage
htop                  # better live process monitor
```

---

## Network Commands
```bash
ip addr               # show network interfaces and IPs
ip addr show eth0     # show specific interface
ip route show         # show routing table
ping google.com       # test connectivity
ping -c 4 google.com   # ping exactly 4 times
curl ifconfig.me      # show your public IP
curl -I http://site   # show HTTP headers only
curl -v http://site   # verbose — show everything
curl -L http://site   # follow the link to the final destination.
curl -s http://example.com   # It prints only the raw content returned by the server—nothing else.
curl -Lv http://site
curl -Lo output.html http://site
curl -sL http://site
traceroute google.com # show network path
ss -tulnp             # show active connections
```

---

## Package Management
```bash
sudo apt update              # refresh software list
sudo apt upgrade             # update all software
sudo apt install nmap        # install a program
sudo apt remove nmap         # uninstall a program
sudo apt search keyword      # search for software
which nmap                   # find where program is installed
```

---

## Permissions
```bash
ls -la                       # show permissions
chmod 755 file               # change permissions (numeric)
chmod +x file                # make file executable
chmod -w file                # remove write permission
chown user:group file        # change file owner
sudo command                 # run as administrator
sudo -i                      # switch to root shell
```

---

## Command Chaining
```bash
command1 && command2    # run 2 only if 1 succeeded
command1 || command2    # run 2 only if 1 failed
command1 ; command2     # always run both
command1 | command2     # pipe output of 1 into 2
command > file          # save output to file (overwrite)
command >> file         # append output to file
command 2>/dev/null     # hide error messages
```

---

## Wildcards
```bash
*.txt                   # any file ending in .txt
file*                   # any file starting with "file"
folder/*.txt            # txt files in folder
folder/*/*.txt          # txt files one level deep
```

---

## Useful Shortcuts
```bash
Ctrl+C     # cancel current command
Ctrl+Z     # pause current command
Ctrl+L     # clear screen (same as clear)
Ctrl+A     # go to beginning of line
Ctrl+E     # go to end of line
Tab        # autocomplete command or filename
↑ arrow    # previous command
!!         # repeat last command
sudo !!    # repeat last command as sudo
```

---

## Cybersecurity Specific
```bash
# First commands after system access
whoami                          # who am I?
id                              # what groups am I in?
ip addr                         # what network am I on?
ps aux                          # what's running?
find / -perm -4000 2>/dev/null  # find SUID files

# Search for sensitive files
find / -name "*.conf" 2>/dev/null
find / -name "*.txt" 2>/dev/null
grep -r "password" /etc/ 2>/dev/null

# Network reconnaissance
nmap -sn 192.168.1.0/24        # find live hosts
nmap -sV target_ip             # detect services
nmap -p 80,443 target_ip       # scan specific ports
dig +short,+stats
dig -x 8.8.8.8                 # reverse lookup
nslookup
dnsrecon

# Log analysis
cat /var/log/auth.log           # authentication logs
grep "Failed" /var/log/auth.log # find failed logins
grep "root" /var/log/auth.log   # find root attempts
```

---

## Additional i just discovered

```bash
curl -s ifconfig.me # sis a flag and means silence

```
---

## Exit Codes
```bash
echo $?    # check last command result
# 0 = success
# 1 = not found / failed
# 2 = error
```

---

*Cheat sheet by Nemugui — updated as I learn more*
