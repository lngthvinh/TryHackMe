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
3128/tcp open  http-proxy  Squid http proxy 3.5.12
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

# Compromise the webserver

Try upload a few file types to the server, what common extension seems to be blocked?

Run this attack, what extension is allowed?

What is the name of the user who manages the webserver?

What is the user flag?

# Privilege Escalation

On the system, search for all SUID files. What file stands out?

Become root and get the last flag (/root/root.txt)
