# How do we load websites?

### What request verb is used to retrieve page content?
> *Answer:* GET

### What port do web servers normally listen on?
> *Answer:* 80

### What's responsible for making websites look fancy?
> *Answer:* CSS


<br><br>
# More HTTP - Verbs and request formats

### What verb would be used for a login?
> *Answer:* POST

### What verb would be used to see your bank balance once you're logged in?
> *Answer:* GET

### Does the body of a GET request matter? Yea/Nay
> *Answer:* Nay

### What's the status code for "I'm a teapot"?
> *Answer:* 418

### What status code will you get if you need to authenticate to access some content, and you're unauthenticated?
> *Answer:* 401


<br><br>
# Mini CTF

### What's the GET flag?
```
curl -X GET http://10.10.247.2:8081/ctf/get
```
> *Answer:* thm{162520bec925bd7979e9ae65a725f99f}

### What's the POST flag?
```
curl -X POST -d "flag_please" http://10.10.247.2:8081/ctf/post
or
curl -X POST --data "flag_please" http://10.10.247.2:8081/ctf/post
```
> *Answer:* thm{3517c902e22def9c6e09b99a9040ba09}

### What's the "Get a cookie" flag?
```
curl -X GET -I http://10.10.247.2:8081/ctf/getcookie
or 
curl -X GET --head http://10.10.247.2:8081/ctf/getcookie
```
> *Answer:* thm{91b1ac2606f36b935f465558213d7ebd}

### What's the "Set a cookie" flag?
```
curl -X GET -b "flagpls=flagpls" http://10.10.247.2:8081/ctf/sendcookie
or
curl -X GET --cookie "flagpls=flagpls" http://10.10.247.2:8081/ctf/sendcookie
```
> *Answer:* thm{c10b5cb7546f359d19c747db2d0f47b3}

