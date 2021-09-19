# Reconnaissance

Scan the box, how many ports are open?
```
# nmap -Pn <machine ip>
...
PORT     STATE SERVICE
21/tcp   open  ftp
22/tcp   open  ssh
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
3128/tcp open  squid-http
3333/tcp open  dec-notes
...
```
> *Answer*: 6

What version of the squid proxy is running on the machine?
```
# nmap -sV <machine ip>
...
3128/tcp open  http-proxy  Squid http proxy 3.5.12
...
```
> *Answer*: 3.5.12

How many ports will nmap scan if the flag -p-400 was used?
> *Answer*: 400

Using the nmap flag -n what will it not resolve?
> *Answer*: DNS

What is the most likely operating system this machine is running?
```
# nmap -sV -O <machine ip>
...
3333/tcp open  http        Apache httpd 2.4.18 ((Ubuntu))
...
```
> *Answer*: Ubuntu

What port is the web server running on?
```
# nmap -sV <machine ip>
...
3333/tcp open  http        Apache httpd 2.4.18 ((Ubuntu))
...
```
> *Answer*: 3333


# Locating directories

What is the directory that has an upload form page?
```
# dirsearch -u "http://<machine ip>:3333/"
...
[14:17:17] 301 -  320B  - /internal  ->  http://10.10.117.6:3333/internal/
...
```
> *Answer*: /internal/

# Compromise the webserver

Try upload a few file types to the server, what common extension seems to be blocked?
> *Answer*: .php

Run this attack, what extension is allowed?
> *Answer*: .phtml

What is the name of the user who manages the webserver?
```
$ cat /etc/passwd
...
bill:x:1000:1000:,,,:/home/bill:/bin/bash
```
> *Answer*: bill

What is the user flag?
```
$ ls -la /home/bill
...
-rw-r--r-- 1 bill bill   33 Jul 31  2019 user.txt
$ cat /home/bill/user.txt
8bd7992fbe8a6ad22a63361004cfcedb
> *Answer*: 8bd7992fbe8a6ad22a63361004cfcedb

# Privilege Escalation

On the system, search for all SUID files. What file stands out?
```
$ find / -perm -u=s -type f | grep -v "Permission denied"
...
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
> *Answer*: /bin/systemctl

Become root and get the last flag (/root/root.txt)
