## 1
```bash
#!/bin/bash
read -p "Escribe el nombre del archivo" archivo
if [[ -f $archivo ]];
        then 
        read -p "Escribe la palabra clave:" palabra
        grep "$palabra" "$archivo"
else
        echo "Archivo desconocido"
fi
```
## 2
```bash
#!/bin/bash
read -p "Escribe el nombre del archivo:" archivo
if [[ -f $archivo ]];
         then
         num_palabras=$(wc -w < "$archivo")
        echo "El archivo tiene $num_palabras palabras."
else
        echo "Archivo desconocido."
fi
```
## 3
```bash
#!/bin/bash
read -p "Escribe el nombre del archivo de texto:" archivo
suma=0
if [[ -f $archivo ]];
         then
         while IFS= read -r numero;     
         do
                 suma=$((suma + numero))
         done < "$archivo"
  echo "La suma es: $suma"
else
  echo "Archivo desconocido."
fi
```
## 4
```bash
#!/bin/bash
read -p "Escribe el nombre del archivo que se desea analizar:" archivo
read -p "Escribe el NIF del alumno: " nif

if [[ -f $archivo ]];
         then
                 grep "$nif" "$archivo" | while IFS=,
                 read -r nif nombre apellidos ciclo modulo nota;
                 do
                         echo "El alumno $nombre $apellidos tiene una calificación de $nota puntos en el módulo $modulo."
                 done
        else
                 echo "El archivo $archivo no existe."
fi
```
## 5
```bash
#!/bin/bash
read -p "Escribe un nombre de archivo:" archivo
read -p "Indica el número mínimo de caracteres:" n

if [[ -f $archivo ]];
         then
         while IFS= 
                 read -r linea; 
                 do
                         if [[ ${#linea} -gt $n ]];     
                         then
                                 echo "$linea"
                         fi
                 done < "$archivo"
        else
                 echo "Archivo desconocido."
fi
```
## 6
```bash
#!/bin/bash
for archivo in "$@";
         do
                if [[ -f $archivo ]];
                 then
                         num_lineas=$(wc -l < "$archivo")
                         num_palabras=$(wc -w < "$archivo")
                         num_caracteres=$(wc -m < "$archivo")
                                 echo "Archivo: $archivo"
                                 echo "Líneas: $num_lineas, Palabras: $num_palabras, Caracteres: $num_caracteres"
                                 echo "--------------------------------------"
                 else
                         echo "El archivo $archivo no existe."
                 fi
        done
```


[Volver](../../index.md)