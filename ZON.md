

Machine_IP = **`192.168.0.116`**

## Recon 

```sql
 # nmap 

Nmap scan report for 192.168.0.116
Host is up (0.00094s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u1 (protocol 2.0)
| ssh-hostkey: 
|   256 dd83dacb45d3a8eac6be19034576438c (ECDSA)
|_  256 e55f7f25aac01804c44698b35da52b48 (ED25519)
80/tcp open  http    Apache httpd 2.4.57 ((Debian))
|_http-server-header: Apache/2.4.57 (Debian)
|_http-title: zon
MAC Address: 08:00:27:34:FB:BE (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 19.84 seconds

```

- open port = 80, 22 

```python
# Nikto 

+ Server: Apache/2.4.57 (Debian)
+ The anti-clickjacking X-Frame-Options header is not present.
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ DEBUG HTTP verb may show server debugging information. See http://msdn.microsoft.com/en-us/library/e8z01xdh%28VS.80%29.aspx for details.
+ OSVDB-3268: /images/: Directory indexing found.
+ OSVDB-3268: /images/?pattern=/etc/*&sort=name: Directory indexing found.
+ 6544 items checked: 0 error(s) and 4 item(s) reported on remote host
+ End Time:           2024-04-03 08:40:12 (GMT1) (25 seconds)

# Gobster 

choose.php
testimonial.php 
service.php 
upload.php 
blog.php 
index.php 
contact.php 
report.php 
```

