
- Target_IP = 192.168.150.122
- Target_system = linux 

# recon 

```bash 
nmap

PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 9.2p1 Debian 2 (protocol 2.0)
| ssh-hostkey: 
|   256 10edddab26fdf49f281e8993f45816ab (ECDSA)
|_  256 433bd98c1244e992becf1a78fd333867 (ED25519)
80/tcp   open  http    Apache httpd 2.4.57 ((Debian))
|_http-title: Logan
|_http-server-header: Apache/2.4.57 (Debian)
3000/tcp open  ppp?
| fingerprint-strings: 
|   GenericLines, Help: 
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain; charset=utf-8
|     Connection: close
|     Request
|   GetRequest: 
|     HTTP/1.0 200 OK
|     Content-Type: text/html; charset=UTF-8
|     Set-Cookie: lang=en-US; Path=/; Max-Age=2147483647
|     Set-Cookie: i_like_gitea=ee0a4f649b595a0d; Path=/; HttpOnly
|     Set-Cookie: _csrf=jGnm43x0GhdgowlUbbyOo5iFk5k6MTY5NjkwMjUzMDk1MTA5NDM3Nw; Path=/; Expires=Wed, 11 Oct 2023 01:48:50 GMT; HttpOnly
|     X-Frame-Options: SAMEORIGIN
|     Date: Tue, 10 Oct 2023 01:48:50 GMT
|     <!DOCTYPE html>
|     <html lang="en-US" class="theme-">
|     <head data-suburl="">
|     <meta charset="utf-8">
|     <meta name="viewport" content="width=device-width, initial-scale=1">
|     <meta http-equiv="x-ua-compatible" content="ie=edge">
|     <title> Gitea: Git with a cup of tea </title>
|     <link rel="manifest" href="/manifest.json" crossorigin="use-credentials">
|     <meta name="theme-color" content="#6cc644">
|     <meta name="author" content="Gitea - Git with a cup of tea" />
|     <meta name="description" content="Gitea (Git with a cup of tea) is a painless
|   HTTPOptions: 
|     HTTP/1.0 404 Not Found
|     Content-Type: text/html; charset=UTF-8
|     Set-Cookie: lang=en-US; Path=/; Max-Age=2147483647
|     Set-Cookie: i_like_gitea=9320068247ed34a2; Path=/; HttpOnly
|     Set-Cookie: _csrf=U4fodUlJU1FRowVG2gs8Odz52k46MTY5NjkwMjUzNjA0Njg0NjA1Mw; Path=/; Expires=Wed, 11 Oct 2023 01:48:56 GMT; HttpOnly
|     X-Frame-Options: SAMEORIGIN
|     Date: Tue, 10 Oct 2023 01:48:56 GMT
|     <!DOCTYPE html>
|     <html lang="en-US" class="theme-">
|     <head data-suburl="">
|     <meta charset="utf-8">
|     <meta name="viewport" content="width=device-width, initial-scale=1">
|     <meta http-equiv="x-ua-compatible" content="ie=edge">
|     <title>Page Not Found - Gitea: Git with a cup of tea </title>
|     <link rel="manifest" href="/manifest.json" crossorigin="use-credentials">
|     <meta name="theme-color" content="#6cc644">
|     <meta name="author" content="Gitea - Git with a cup of tea" />
|_    <meta name="description" content="Gitea (Git with a c
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port3000-TCP:V=7.93%I=7%D=10/10%Time=6524AD83%P=x86_64-pc-linux-gnu%r(G
SF:enericLines,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20
SF:text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\
SF:x20Request")%r(GetRequest,2943,"HTTP/1\.0\x20200\x20OK\r\nContent-Type:
SF:\x20text/html;\x20charset=UTF-8\r\nSet-Cookie:\x20lang=en-US;\x20Path=/
SF:;\x20Max-Age=2147483647\r\nSet-Cookie:\x20i_like_gitea=ee0a4f649b595a0d
SF:;\x20Path=/;\x20HttpOnly\r\nSet-Cookie:\x20_csrf=jGnm43x0GhdgowlUbbyOo5
SF:iFk5k6MTY5NjkwMjUzMDk1MTA5NDM3Nw;\x20Path=/;\x20Expires=Wed,\x2011\x20O
SF:ct\x202023\x2001:48:50\x20GMT;\x20HttpOnly\r\nX-Frame-Options:\x20SAMEO
SF:RIGIN\r\nDate:\x20Tue,\x2010\x20Oct\x202023\x2001:48:50\x20GMT\r\n\r\n<
SF:!DOCTYPE\x20html>\n<html\x20lang=\"en-US\"\x20class=\"theme-\">\n<head\
SF:x20data-suburl=\"\">\n\t<meta\x20charset=\"utf-8\">\n\t<meta\x20name=\"
SF:viewport\"\x20content=\"width=device-width,\x20initial-scale=1\">\n\t<m
SF:eta\x20http-equiv=\"x-ua-compatible\"\x20content=\"ie=edge\">\n\t<title
SF:>\x20Gitea:\x20Git\x20with\x20a\x20cup\x20of\x20tea\x20</title>\n\t<lin
SF:k\x20rel=\"manifest\"\x20href=\"/manifest\.json\"\x20crossorigin=\"use-
SF:credentials\">\n\t<meta\x20name=\"theme-color\"\x20content=\"#6cc644\">
SF:\n\t<meta\x20name=\"author\"\x20content=\"Gitea\x20-\x20Git\x20with\x20
SF:a\x20cup\x20of\x20tea\"\x20/>\n\t<meta\x20name=\"description\"\x20conte
SF:nt=\"Gitea\x20\(Git\x20with\x20a\x20cup\x20of\x20tea\)\x20is\x20a\x20pa
SF:inless")%r(Help,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:
SF:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20
SF:Bad\x20Request")%r(HTTPOptions,206E,"HTTP/1\.0\x20404\x20Not\x20Found\r
SF:\nContent-Type:\x20text/html;\x20charset=UTF-8\r\nSet-Cookie:\x20lang=e
SF:n-US;\x20Path=/;\x20Max-Age=2147483647\r\nSet-Cookie:\x20i_like_gitea=9
SF:320068247ed34a2;\x20Path=/;\x20HttpOnly\r\nSet-Cookie:\x20_csrf=U4fodUl
SF:JU1FRowVG2gs8Odz52k46MTY5NjkwMjUzNjA0Njg0NjA1Mw;\x20Path=/;\x20Expires=
SF:Wed,\x2011\x20Oct\x202023\x2001:48:56\x20GMT;\x20HttpOnly\r\nX-Frame-Op
SF:tions:\x20SAMEORIGIN\r\nDate:\x20Tue,\x2010\x20Oct\x202023\x2001:48:56\
SF:x20GMT\r\n\r\n<!DOCTYPE\x20html>\n<html\x20lang=\"en-US\"\x20class=\"th
SF:eme-\">\n<head\x20data-suburl=\"\">\n\t<meta\x20charset=\"utf-8\">\n\t<
SF:meta\x20name=\"viewport\"\x20content=\"width=device-width,\x20initial-s
SF:cale=1\">\n\t<meta\x20http-equiv=\"x-ua-compatible\"\x20content=\"ie=ed
SF:ge\">\n\t<title>Page\x20Not\x20Found\x20-\x20\x20Gitea:\x20Git\x20with\
SF:x20a\x20cup\x20of\x20tea\x20</title>\n\t<link\x20rel=\"manifest\"\x20hr
SF:ef=\"/manifest\.json\"\x20crossorigin=\"use-credentials\">\n\t<meta\x20
SF:name=\"theme-color\"\x20content=\"#6cc644\">\n\t<meta\x20name=\"author\
SF:"\x20content=\"Gitea\x20-\x20Git\x20with\x20a\x20cup\x20of\x20tea\"\x20
SF:/>\n\t<meta\x20name=\"description\"\x20content=\"Gitea\x20\(Git\x20with
SF:\x20a\x20c");
MAC Address: 08:00:27:FE:36:6E (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

three ports 
open 22/tcp   open  ssh 80/tcp   open  http
3000/tcp open  ppp

 git on port 3000 

might be vulnerable to http request smuggling attack 



everything in here https://blog.csdn.net/xdeclearn/article/details/133750594

user and root.txt 
```
1290381293128301a root

24671329416324134234 user
```