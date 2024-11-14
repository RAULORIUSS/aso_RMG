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

CK=$(( $num + 273 ))
CF=$((  $(( $num * 2 )) + 32 ))
KC=$(( $num - 273 ))
KF=$(( $((  $KC * 2 )) + 32 ))
FC=$((  $((  $num - 32 )) / 2 ))
FK=$(( $((  $((  $num - 32 )) / 2 )) + 273 ))

case $temp1 in
    [Cc]elsius)
       read -p "a que lo quieres pasar" temp2
         case $temp2 in
            [Kk]elvin)
               echo "tu resultado es $CK";;
            [Ff]ahrenheit)
                echo "tu resultado es $CF";;
         esac ;;
     [Kk]elvin)
       read -p "a que lo quieres pasar" temp2
         case $temp2 in
            [Cc]elsius)
               echo "tu resultado es $KC";;
            [Ff]ahrenheit)
                echo "tu resultado es $CF";;
         esac ;;
   [Ff]ahrenheit)
       read -p "a que lo quieres pasar" temp2
         case $temp2 in
            [Cc]elsius)
               echo "tu resultado es $FC";;
            [Kk]elvin)
                echo "tu resultado es $FK";;
         esac ;;
esac
```
## 7
```bash
#!/bin/bash
read -p "introduce tu servicio: " serv
read -p "que quieres hacer: " op
   case $op in
      iniciar)
         systemctl start $serv
         echo "$serv se ha iniciado";;
      reiniciar)
         systemctl stop $serv
         echo "$serv se ha reiniciado";;
      detener)
         systemctl restart $serv
         echo "$serv se ha detenido";;
   esac

```
## 8
```bash
#!/bin/bash
read -p "introduce numero de metros: " metro
read -p "introduce a que unidad lo quieres pasar: " uni

km=$(( $metro / 1000 ))
cm=$(( $metro * 100 ))
mm=$(( $metro * 1000 ))
   case $uni in
      [Kk]ilometros)
         echo "$km";;
      [Cc]entimetros)
         echo "$cm";;
      [Mm]ilimetros)
         echo "$mm";;
   esac
```

[Volver](../../index.md)