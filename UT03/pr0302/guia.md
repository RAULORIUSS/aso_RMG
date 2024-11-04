# Comando case
## 1
```bash
#!/bin/bash
read -p "escoja el primer numero" numero1
read -p "escoja el segundo numero" numero2
read -p "escoja la operacion a realizar" operacion

resulsuma=$( expr $numero1 + $numero2 )
resulresta=$( expr $numero1 - $numero2 )
resulmult=$( expr $numero1 \* $numero2 )
resuldiv=$( expr $numero1 / $numero2 )
case $operacion in
 suma)
  echo "tu resultado es $resulsuma";;
 resta)
  echo "tu resultado es $resulresta";;
 multiplicar)
  echo "tu resultado es $resulmult";;
 dividir)
  echo "tu resultado es $resuldiv";;
esac
```
## 2
```bash
#!/bin/bash
read -p "escoja un dia de la semana" dia
case $dia in
 lunes | martes | miercoles | jueves | viernes)
  echo "a trabajar";;
 sabado | domingo)
  echo "a descansar";;
esac
```
## 3
```bash
#!/bin/bash
read -p "que nota saco uste: " nota
case $nota in
   [0-4])
      echo "uste suspendio";;
   5)
      echo "no sabe ni uste como aprobo";;
   6)
      echo "bien";;
   [7-8])
      echo "notable";;
   9 | 10)
      echo "sobresaliente";;
esac
```
## 4
```bash
#!/bin/bash
read -p "que mes es: " estacion
case $estacion in
   diciembre | enero | febrero)
      echo "invierno";;
   marzo | abril | mayo )
      echo "primavera";;
   junio | julio | agosto)
      echo "verano";;
   septiembre |octubre | noviembre)
      echo "verano";;
esac
```
## 5
```bash
#!/bin/bash
read -p "que extension quiere saber" extension
case $extension in
   .txt)
      echo "texto";;
   .jpg)
      echo "imagen";;
   .pdf)
      echo "documento";;
   .mp3)
      echo "musica";;
esac
```
## 6
```bash
#!/bin/bash
read -p "escoja el tipo de temperatura que quieras ingresar" temp1
read -p "escoja la temperatura que quieras ingresar" num

CK=$( expr $num + 273)
CF=$( expr ( expr $num \* 1.8) + 32)
KC=$( expr $num - 273)
KF=$( expr ( expr $KC \* 1.8) + 32)
FC=$( expr ( expr $num - 32) / 1.8)
FK=$( expr ( expr ( expr $num - 32) / 1.8) + 273)

case $temp1 in
    [Cc]elsius)
       read -p "a que lo quieres pasar" temp2
         case $temp2 in
            [Kk]elvin)


;;
    [Kk]elvin)
       echo "tu resultado es $resulresta";;
   [Ff]ahrenheit)
      echo "tu resultado es $resulmult";;
esac
```