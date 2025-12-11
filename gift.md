level = easy 
target_IP = 192.168.150.206

## recon

```sh
nmap 
Nmap scan report for gift.lan (192.168.150.206)
Host is up (0.00026s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.3 (protocol 2.0)
| ssh-hostkey: 
|   3072 2c1b3627e54c527b3e10944139efb295 (RSA)
|   256 93c11e32240e34d9020effc39c599bdd (ECDSA)
|_  256 81ab36ecb12b5cd28655120c510027d7 (ED25519)
80/tcp open  http    nginx
|_http-title: Site doesn't have a title (text/html).
MAC Address: 08:00:27:1A:6C:38 (Oracle VirtualBox virtual NIC)
```


