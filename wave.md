target-ip = 192.168.150.232
# recon

```sh
nmap 
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2 (protocol 2.0)
| ssh-hostkey: 
|   256 07e9c82259a5004115fa260f7dd329ff (ECDSA)
|_  256 c7818e0649338f1a883b829e27f3721e (ED25519)
80/tcp open  http    nginx 1.22.1
|_http-title: Site doesn't have a title (text/html).
| http-robots.txt: 1 disallowed entry 
|_/backup
|_http-server-header: nginx/1.22.1
MAC Address: 08:00:27:A7:07:44 (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

