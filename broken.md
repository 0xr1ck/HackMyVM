
brief`` 

target_IP = 192.168.0.105
## enum

```sql 

# nmap 
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 1b8df3e35664af54df10f839acadc92f (RSA)
|   256 77c1f3e46b960f1e5c242e4d3e4a0980 (ECDSA)
|_  256 8805ef7a0456f05962a5f84032248a17 (ED25519)
80/tcp open  http    nginx 1.14.2
| http-robots.txt: 1 disallowed entry 
|_/textpattern
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: nginx/1.14.2
MAC Address: 08:00:27:6E:FE:84 (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

# directories 

/textpattern 
/robots.txt 
/index.php            (Status: 200) [Size: 11604]                                          
/files                (Status: 301) [Size: 185] [--> http://broken.hmv/textpattern/files/] 
/themes               (Status: 301) [Size: 185] [--> http://broken.hmv/textpattern/themes/]
/css.php              (Status: 200) [Size: 0]                                              
/README.txt           (Status: 200) [Size: 1152]                                           
/INSTALL.txt          (Status: 200) [Size: 3080]                                           
/LICENSE.txt          (Status: 200) [Size: 15170]                                          
/rpc                  (Status: 301) [Size: 185] [--> http://broken.hmv/textpattern/rpc/]   
/HISTORY.txt          (Status: 200) [Size: 71287]                                          
/UPGRADE.txt
```


