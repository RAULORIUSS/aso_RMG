# Ejercicios condicional if

## 1
```bash
#!/bin/bash
read -p "Introduce un número: " numero
if [ numero % 2 == 0 ]
 then
    echo "El número es par."
else
    echo "El número es impar."
fi
```
## 2
```bash
#!/bin/bash
read -p "Introduce la ruta del archivo: " ruta_archivo
if [ -e "$ruta_archivo" ]; then
    if [ -r "$ruta_archivo" ]; then
        echo "El archivo existe y tiene permisos de lectura."
    else
        echo "El archivo existe pero no tiene permisos de lectura."
    fi
else
    echo "El archivo no existe."
fi
```
## 3
```bash
#!/bin/bash
read -p "Introduce el primer número: " numero1
read -p "Introduce el segundo número: " numero2

if  [ $numero1 -gt $numero2 ]
 then
 echo "el numero1 es mas grande que el numero2"
elif [ $numero1 -lt $numero2 ]
 then
 echo "el numero1 es mas pequeño que el numero2"
else
 echo "el numero1 es igual  que el numero2"
fi

```
## 4
```bash
#!/bin/bash
read -p "dime tu contraseña: " pass
secret=1234
if  [ $pass = $secret ]
 then
echo "tu contraseña es correcta"
else
echo "tu contraseña no es correcta"
fi
```
## 5
```bash

```
## 6
```bash
#!/bin/bash
if [ $USER = "root" ]
then
echo "este usuario es el root"
else
echo "este usuario no es el root"
fi
```
## 7
```bash
#!/bin/bash
read -p "Que nota sacaste: " nota
if [ $nota -ge 5 ]
then
echo "aprobado"
else 
echo "suspenso"
fi
```
