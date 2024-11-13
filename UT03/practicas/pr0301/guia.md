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
#!/bin/bash
echo "Introduce el nombre del directorio:"
read directorio

if [[ -d "$directorio" ]];
        then
                if [[ -w "$directorio" ]];
                        then
                                echo "El directorio $directorio si existe y si tiene permisos de escritura"
                else
                        echo  "El directorio $direcdtorio si existe pero no tiene permisos de escritura."
                fi
        else 
                echo "El directorio  $directorio  no existe por tanto vamos a crearlo"
                mkdir "$directorio"
                echo "El directorio ha sido creado."
fi
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
## 8
```bash
#!/bin/bash
espacio-libre=$(df / | grep '/' | awk '{print $5}' | sed 's/%//' )

if  (( espacio-libre < 10 ));
        then
                echo "CUIDADO!!: El espacio del disco es menor al 10%"
        else
                echo "Tiene suficiente espacio en el disco"
fi
```
## 9
```bash
#!/bin/bash
echo "Elige un número:"echo "1."echo "2."echo "3."read numeroif [ "$numero" -eq 1 ];thenecho "Has elegido el número 3"elif [ "$numero" -eq 2 ];thenecho "Has elegido el número 5"elif [ "$numero" -eq 3 ];thenecho "Has elegido el número 7"elseecho "Número incorrecto. Elige otro número"fi
```
## 10
```bash
#!/bin/bash
echo "Introduce tu edad:"read edadif (( edad < 18 ));thenecho "Eres menor de edad"elif (( edad >= 18 && edad <= 65 ));thenecho "Eres adulto"elseecho "Eres mayor de 65 años"fi
```
## 11
```bash
#!/bin/bash
echo "Introduce un nombre de archivo:"read archivo
if [ -e "$archivo" ];then                linea=$(wc -l  < "$archivo")echo "El archivo $archivo consta de $linea lineas"elseecho "El archivo $archivo no existe."fi
```