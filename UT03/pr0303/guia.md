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
 no lo hemos dado
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
 no se puede hacer
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
##

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
 ## 18
 no se puede hacer