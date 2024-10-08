# GUIA

## 1.1 

```bash
vagrant@ubuntu2204:~$ cd /home
vagrant@ubuntu2204:/home$ ls
vagrant
vagrant@ubuntu2204:/home$ cd vagrant
vagrant@ubuntu2204:~$ ls
vagrant@ubuntu2204:~$ mkdir pr0201
vagrant@ubuntu2204:~$ cd pr0201
vagrant@ubuntu2204:~/pr0201$ mkdir dir1
vagrant@ubuntu2204:~/pr0201$ mkdir dir2
vagrant@ubuntu2204:~/pr0201$ ls
dir1  dir2
vagrant@ubuntu2204:~/pr0201$ ls -l
drwxrwxr-x 2 vagrant vagrant 4096 Oct  1 07:07 dir1
```
## 1.2
```bash
vagrant@ubuntu2204:~/pr0201$ chmod a-w dir2
vagrant@ubuntu2204:~/pr0201$ ls -l
total 8
drwxrwxr-x 2 vagrant vagrant 4096 Oct  1 07:07 dir1
dr-xr-xr-x 2 vagrant vagrant 4096 Oct  1 07:07 dir2
```
## 1.3
```bash
vagrant@ubuntu2204:~/pr0201$ chmod 551 dir2
```
## 1.4
```bash
dr-xr-x--x 2 vagrant vagrant 4096 Oct  1 07:42 dir2
```
## 1.5 
```bash
vagrant@ubuntu2204:~/pr0201$ mkdir dir2/dir21
mkdir: cannot create directory ‘dir2/dir21’: Permission denied
```
## 1.6
```bash
vagrant@ubuntu2204:~/pr0201$ chmod 751 dir2
vagrant@ubuntu2204:~/pr0201$ mkdir dir2/dir21
vagrant@ubuntu2204:~/pr0201$ cd dir2
vagrant@ubuntu2204:~/pr0201/dir2$ ls -l
total 4
drwxrwxr-x 2 vagrant vagrant 4096 Oct  1 07:46 dir21
```
## 2.1 Notacion simbolica

rwxrwxr-x :chmod u+rwx,g+rwx,o+rx /file
rwxr--r-- :chmod u+rwx,g+r--,o+r-- /file
r--r----- :chmod u+r,g+r,o-r /file
rwxr-xr-x :chmod u+rwx,g+rx,o+rx /file
r-x--x--x :chmod u+rx,g+x,o+x /file
-w-r----x :chmod u-w,g+r,o+x /file
-----xrwx :chmod u-r,g-r,o+rwx /file
r---w---x :chmod u+r,g+w,o+x /file
-w------- :chmod u-w,g-r,o-r /file
rw-r----- :chmod u+rw,g+r,o-r /file
rwx--x--x :chmod u+rwx,g+x,o+x /file
## 2.2 Notacion octal

rwxrwxrwx :chmod 777 /file
--x--x--x :chmod 111 /file
r---w---x :chmod 421 /file
-w------- :chmod 200 /file
rw-r----- :chmod 640 /file
rwx--x--x :chmod 711 /file
rwxr-xr-x :chmod 755 /file
r-x--x--x :chmod 511 /file
-w-r----x :chmod 241 /file
-----xrwx :chmod 017 /file
# 3 El bit setgid

## 3.1 
```bash
vagrant@ubuntu2204:~$ sudo groupadd asir
vagrant@ubuntu2204:~$ sudo useradd  -G asir rmg1
vagrant@ubuntu2204:~$ sudo useradd  -G asir rmg2
```
## 3.2
```bash
sudo mkdir /compartido
sudo chown root:asir /compartido
```
## 3.3
```bash
sudo chmod 770 /compratido
```
## 3.4
```bash
sudo chmod g+s /compartido
ls -ld /compartido
```
## 3.5
```bash
vagrant@ubuntu2204:~$ su rmg1
Password: 
$ cd /compartido
$ touch fichero1    
$ echo "esto contiene el fichero1" > fichero1
$ ls -l /compartido/fichero1
-rw-rw-r-- 1 rmg1 asir 26 Oct  7 10:19 /compartido/fichero1
```
## 3.6
```bash
$ cat fichero1
esto contiene el fichero1
$ su rmg2
Password:
$ cd /compartido
$ echo "esto lo añadio el 2" >fichero1
$ cat fichero1
esto lo añadio el 2
```
## 3.7
### 3.7.1
El bit setgid asegura que cualquier archivo o subdirectorio creado dentro de un directorio here el grupo propietario del directorio, en lugar del grupo predeterminado del usuario que crea el archivo. Esto es útil en entornos colaborativos porque garantiza que todos los archivos creados dentro de un directorio compartido pertenezcan al grupo del directorio, facilitando la colaboración sin que sea necesario modificar los permisos o grupos manualmente cada vez que se crea un archivo.
### 3.7.2
Si el bit setgid no está habilitado, los archivos y subdirectorios creados dentro del directorio heredarán el grupo del usuario que los crea. Esto puede causar problemas en entornos colaborativos, ya que otros usuarios del grupo original podrían no tener los permisos necesarios para acceder o modificar esos archivos, lo que podría ralentizar el trabajo en equipo y requerir ajustes manuales en los permisos o los grupos de los archivos.
## 3.8
```bash
vagrant@ubuntu2204:~$ sudo userdel  rmg1
vagrant@ubuntu2204:~$ sudo userdel  rmg2
vagrant@ubuntu2204:~$ sudo rm -r  /compartido
```
# 4 El sticky bit
## 4.1
```bash
sudo mkdir /compartido
sudo chmod 777 /compartido
```
## 4.2
```bash
vagrant@ubuntu2204:~$ sudo useradd -m rmg1
vagrant@ubuntu2204:~$ sudo useradd -m rmg2
vagrant@ubuntu2204:~$ sudo passwd rmg1
vagrant@ubuntu2204:~$ sudo passwd rmg2
```
## 4.3
```bash
vagrant@ubuntu2204:~$ su rmg1
Password: 
$ touch /compartido/fichero1.txt
$ exit
vagrant@ubuntu2204:~$ su rmg2
Password: 
$ rm /compartido/fichero1.txt
rm: remove write-protected regular empty file '/compartido/fichero1.txt'?
```
## 4.4
```bash
vagrant@ubuntu2204:~$ sudo chmod +t /compartido
vagrant@ubuntu2204:~$ ls -ld /compartido
drwxrwxrwt 2 root root 4096 Oct  8 07:55 /compartido
```
## 4.5
```bash
vagrant@ubuntu2204:~$ su rmg1
Password: 
$ touch /compartido/archivo2.txt
$ exit
vagrant@ubuntu2204:~$ su rmg2
Password: 
$ rm /compartido/archivo2.txt
rm: remove write-protected regular empty file '/compartido/archivo2.txt'? y
rm: cannot remove '/compartido/archivo2.txt': Operation not permitted
```
## 6.1
### 6.1.1
El sticky bit en un directorio restringe la eliminación o renombramiento de archivos dentro de ese directorio. Específicamente, solo el propietario del archivo (o el usuario root) puede eliminar o renombrar archivos en un directorio con sticky bit, incluso si otros usuarios tienen permisos de escritura en ese directorio.
### 6.1.2
El propietario del fichero.
O tener privilegios de superusuario (root).