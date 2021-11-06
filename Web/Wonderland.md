# Capture the flags

### Obtain the flag in user.txt

* Try `nmap`. I got `ssh` and `http`.

```
┌──(root💀kali)-[~/box]
└─# nmap -sV 10.10.178.210
Starting Nmap 7.91 ( https://nmap.org ) at 2021-11-06 17:41 +07
Nmap scan report for 10.10.178.210
Host is up (0.22s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 19.08 seconds
```

* Try `dirsearch`. I got 2 interesting sites.

```
┌──(root💀kali)-[~/box]
└─# dirsearch -u 10.10.178.210

  _|. _ _  _  _  _ _|_    v0.4.1
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 30 | Wordlist size: 10877

Output File: /root/.dirsearch/reports/10.10.178.210/_21-11-06_17-53-35.txt

Error Log: /root/.dirsearch/logs/errors-21-11-06_17-53-35.log

Target: http://10.10.178.210/

[17:53:35] Starting: 
[17:53:38] 301 -    0B  - /%2e%2e//google.com  ->  /google.com
...
[17:54:27] 301 -    0B  - /img  ->  img/
[17:54:42] 301 -    0B  - /r  ->  r/
```

* Main page

```
┌──(root💀kali)-[~/box]
└─# curl 10.10.178.210
<!DOCTYPE html>
<head>
    <title>Follow the white rabbit.</title>
    <link rel="stylesheet" type="text/css" href="/main.css">
</head>
<body>
    <h1>Follow the White Rabbit.</h1>
    <p>"Curiouser and curiouser!" cried Alice (she was so much surprised, that for the moment she quite forgot how to speak good English)</p>
    <img src="/img/white_rabbit_1.jpg" style="height: 50rem;">
</body> 

┌──(root💀kali)-[~/box]
└─# wget 10.10.178.210/img/white_rabbit_1.jpg                             
--2021-11-06 18:01:06--  http://10.10.178.210/img/white_rabbit_1.jpg
Connecting to 10.10.178.210:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1993438 (1.9M) [image/jpeg]
Saving to: ‘white_rabbit_1.jpg’

white_rabbit_1.jpg                                   100%[======================================================================================================================>]   1.90M  1.11MB/s    in 1.7s    

2021-11-06 18:01:08 (1.11 MB/s) - ‘white_rabbit_1.jpg’ saved [1993438/1993438]

┌──(root💀kali)-[~/box]
└─# steghide info white_rabbit_1.jpg                                                                                                                                                                            1 ⨯
"white_rabbit_1.jpg":
  format: jpeg
  capacity: 99.2 KB
Try to get information about embedded data ? (y/n) y
Enter passphrase: 
  embedded file "hint.txt":
    size: 22.0 Byte
    encrypted: rijndael-128, cbc
    compressed: yes
                                                                                                                                                                                                                    
┌──(root💀kali)-[~/box]
└─# steghide extract -sf white_rabbit_1.jpg
Enter passphrase: 
wrote extracted data to "hint.txt".
                                                                                                                                                                                                                    
┌──(root💀kali)-[~/box]
└─# cat hint.txt      
follow the r a b b i t
```

* Follow the rabbit

```
┌──(root💀kali)-[~/box]
└─# curl 10.10.178.210/r/a/b/b/i/t/
<!DOCTYPE html>

<head>
    <title>Enter wonderland</title>
    <link rel="stylesheet" type="text/css" href="/main.css">
</head>

<body>
    <h1>Open the door and enter wonderland</h1>
    <p>"Oh, you’re sure to do that," said the Cat, "if you only walk long enough."</p>
    <p>Alice felt that this could not be denied, so she tried another question. "What sort of people live about here?"
    </p>
    <p>"In that direction,"" the Cat said, waving its right paw round, "lives a Hatter: and in that direction," waving
        the other paw, "lives a March Hare. Visit either you like: they’re both mad."</p>
    <p style="display: none;">alice:HowDothTheLittleCrocodileImproveHisShiningTail</p>
    <img src="/img/alice_door.png" style="height: 50rem;">
</body>
```

* SSH connection

```
alice@wonderland:~$ ls
root.txt  walrus_and_the_carpenter.py              
alice@wonderland:~$ cat root.txt
cat: root.txt: Permission denied
alice@wonderland:~$ ls -l /root/user.txt
-rw-r--r-- 1 root root 32 May 25  2020 /root/user.txt
alice@wonderland:~$ cat /root/user.txt
thm{"Curiouser and curiouser!"}
```

> *Answer:* thm{"Curiouser and curiouser!"}

### Escalate your privileges, what is the flag in root.txt?
```
alice@wonderland:~$ sudo -l
[sudo] password for alice: 
Matching Defaults entries for alice on wonderland:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User alice may run the following commands on wonderland:
    (rabbit) /usr/bin/python3.6 /home/alice/walrus_and_the_carpenter.py
alice@wonderland:~$ cat walrus_and_the_carpenter.py
import random
poem = """The sun was shining on the sea,
Shining with all his might:
He did his very best to make
The billows smooth and bright —
And this was odd, because it was
The middle of the night.

...

"O Oysters," said the Carpenter.
"You’ve had a pleasant run!
Shall we be trotting home again?"
But answer came there none —
And that was scarcely odd, because
They’d eaten every one."""

for i in range(10):
    line = random.choice(poem.split("\n"))
    print("The line was:\t", line)
alice@wonderland:~$ python3 walrus_and_the_carpenter.py 
The line was:    Walked on a mile or so,
The line was:    Pepper and vinegar besides
The line was:    I wish you were not quite so deaf —
The line was:    
The line was:    There were no birds to fly.
The line was:    The eldest Oyster winked his eye,
The line was:    Meaning to say he did not choose
The line was:    
The line was:    Such quantities of sand:
The line was:    They thanked him much for that.
```
> *Answer:* 
