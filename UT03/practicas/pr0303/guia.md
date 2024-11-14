# ejercicios while, until y for
 
 ## 1
 ```bash
#!/bin/bash
for contador in {1..10}
do
   echo "$contador"
done
 ```
 ## 2
 ```bash
#!/bin/bash
sum=0

for contador in {1..50}
do
   sum=$(( $sum + $contador ))
done
echo "$sum"
 ```
 ## 3
 ```bash
#!/bin/bash
read -p  "dime un numero: " num

for i in {1..10}
   do
      resultado=$(( $num * $i ))
      echo "$resultado"
done
 ```
 ## 4
```bash
#!/bin/bash
read -p "Escribe una palabra: " palabra
for letra in $(echo $palabra | grep -o .); 
 do
  echo $letra
done
```
 ## 5
 ```bash
#!/bin/bash
i=2

while [ $i -le 20 ]
do
echo "$i"
i=$(( $i + 2 ))
done
 ```
 ## 6
```bash
#!/bin/bash
read -p "Escribe un número: " num
suma=0
while [ $num -gt 0 ]; 
 do
  digito=$((num % 10))
  suma=$((suma + digito))
  num=$((num / 10))
done
echo "La suma de los dígitos es: $suma"
```
 ## 7
 ```bash
#!/bin/bash

read -p "dime un numero: " num

until [ $num -eq 0 ]
do
   echo "$num"
   num=$(( $num - 1 ))
done
 ```
 ## 8
 ```bash
#!/bin/bash
echo "Escribe el nombre del directorio:"
read directorio
if [ -d "$directorio" ];
        then
for archivo in *;
 do
  if [[ $archivo == *.txt ]];
         then
         echo $archivo
  fi
done
else
 ```
 ## 9 
 ```bash
#!/bin/bash

read -p "dime un numero: " num
tot=1

for i in $( seq 1 $num )
do
   tot=$(( $i * $tot ))
done
echo "$tot"
 ```
## 10
```bash
#!/bin/bash
password="1234"
input=""
until [ "$input" == "$password" ];
 do
  read -sp "Introduce la contraseña: " input
  echo
done
echo "Contraseña correcta"
```
## 11
```bash
#!/bin/bash
numero=$((RANDOM % 10 + 1))
intento=0
while [ $intento -ne $numero ];
 do
  read -p "Adivina el número (1-10): " intento
  if [ $intento -eq $numero ];
         then
        echo "¡Correcto!"
  else
         echo "Vuelve a jugar"
  fi
done
```
## 12
```bash
#!/bin/bash
read -p "¿Dime un número?: " n
for ((i=1; i<=n; i++)); 
 do
  date
done
```
 ## 13
 ```bash
#!/bin/bash

read -p "introduce un numero: " num

suma=0
cont=0
while [ $num != "fin" ]
do
   suma=$(( $suma + $num ))
   cont=$(( $cont + 1 ))
   read -p "introduce un numero: " num
done
echo "has introducido $cont numeros"
echo "la suma es $suma"
echo  "y su promedio $(( $suma / $cont ))"
 ```
## 14
```bash
#!/bin/bash
read -p "Escribe una frase: " frase
num_palabra=$(echo $frase | wc -w)
echo "La frase tiene $num_palabra palabras"
```
 ## 15
 ```bash
#!/bin/bash
numero=$((RANDOM % 100 + 1))
intento=0
while [ $intento -ne $numero ];
 do
  read -p "Adivina el número (1-100): " intento
  if [ $intento -lt $numero ];
         then
         echo "El número es mayor"
  elif [ $intento -gt $numero ];
         then
         echo "El número es menor"
  else
    echo "¡Correcto!"
  fi
done
```
## 16
```bash
#!/bin/bash

ruta=$(pwd)

for f in $ruta/*
do
if [ -d $f ]
   then
      echo $f >> ./directorios.txt
   fi
done
```
## 17
```bash
#!/bin/bash
read -p "¿Número de  archivos?: " n
for ((x=1; x<=n; x++)); 
         do
         touch "archivo$i.txt"
done
echo "$n archivos creados"
```
 ## 18
```bash
#!/bin/bash
read -p "Escribe una cadena: " cadena
vocales=0
for letra in $(echo $cadena | grep -o .); 
         do
         if [[ $letra =~ [aeiouAEIOU] ]]; 
                 then
                 vocales=$((vocales + 1))
         fi
         done
echo "La cadena tiene $vocales vocales"
```
 ## 19
 ```bash
#!/bin/bash
numero=0
until [[ $numero -ge 1 && $numero -le 10 ]]; 
 do
  read -p "Introduce un número entre 1 y 10: " numero
done
echo "Número válido: $numero"
```
## 20
```bash
#!/bin/bash
mkdir -p backup
for archivo in *.txt; 
 do
  if [ -f "$archivo" ]; 
         then
         cp "$archivo" backup/
  fi
done
echo "Archivos .txt copiados a la carpeta backup"
```


[Volver](../../index.md)