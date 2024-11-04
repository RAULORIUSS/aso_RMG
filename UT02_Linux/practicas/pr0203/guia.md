# 1
## 1.1
usamos la red 172.18.0.XX
## 1.2
```bash
albvaro:x:1001:1001:,,,:/home/albvaro:/bin/bash
belenana:x:1002:1002:,,,:/home/belenana:/bin/bash
sora:x:1003:1003:,,,:/home/sora:/bin/bash
```
## 1.3
realizo los pasos necesarios para que se puedan conectar
## 1.4.1
creo las claves publicas y privadas
```bash
raul@ubuntu2204:/$ ssh-keygen -t rsa -b 1024
Generating public/private rsa key pair.
Enter file in which to save the key (/home/raul/.ssh/id_rsa): 
Created directory '/home/raul/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/raul/.ssh/id_rsa
Your public key has been saved in /home/raul/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:uQd6wX1I2pHZG+E/yX1cvkVy+Zo712OQRs1svjY2d/8 raul@ubuntu2204.localdomain
The key's randomart image is:
+---[RSA 1024]----+
|            .    |
|           = .  .|
|          = + =.+|
|       . * o * &o|
|        S + + O B|
|       . + . + =+|
|      . o . . +.o|
|       . .    .X=|
|              =oE|
+----[SHA256]-----+
```
## 1.4.2
comparto mi clave publica con el otro equipo
```bash
raul@ubuntu2204:/$ scp /home/raul/.ssh/id_rsa.pub raul@172.18.0.3:/home/raul
The authenticity of host '172.18.0.3 (172.18.0.3)' can't be established.
ED25519 key fingerprint is SHA256:AqHnMIIJ/eyo+M0CH2/WBd22ZANS8hzixNO1kMsk+OE.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.18.0.3' (ED25519) to the list of known hosts.
raul@172.18.0.3's password: 
id_rsa.pub                                                   100%  241    98.8KB/s   00:00    
raul@ubuntu2204:/$ ssh raul@172.18.0.3
raul@172.18.0.3's password:
```
```bash
raul@ubuntu2204:~$ ls
id_rsa.pub
raul@ubuntu2204:~$ ls -l
total 4
-rw-r--r-- 1 raul raul 241 Oct 14 11:28 id_rsa.pub
raul@ubuntu2204:~$ mkdir .ssh
raul@ubuntu2204:~$ mv /home/raul/id_rsa.pub /home/raul/.ssh/authoriced_keys
raul@ubuntu2204:~$ ls
raul@ubuntu2204:~$ cd .ssh
raul@ubuntu2204:~/.ssh$ ls
authoriced_keys
raul@ubuntu2204:~/.ssh$
raul@ubuntu2204:~/.ssh$ mv authoriced_keys authorized_keys
raul@ubuntu2204:~/.ssh$ exit
logout
Connection to 172.18.0.3 closed.
```
y compruebo que me puedo conectar sin que me pida contrasela
```bash
raul@ubuntu2204:/$ ssh raul@172.18.0.3
Last login: Mon Oct 14 11:44:00 2024 from 172.18.0.4
raul@ubuntu2204:~$ 
```

[Volver](../../index.md)