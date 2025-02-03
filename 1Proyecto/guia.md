# Documentación del Script de Análisis de Red

## Descripción del Proyecto
Este script en Bash permite analizar una red local para detectar:

Equipos conectados.

Puertos abiertos.

El sistema operativo estimado de cada equipo mediante el TTL.
La dirección MAC de los dispositivos.
Toda la información se almacena en un archivo de texto para su posterio

## Comandos Utilizados y Explicación

### 1 Solicitar la dirección de red en formato CIDR
```bash
read -p "Ingrese la red en formato CIDR (ejemplo: 192.168.1.0/24): " RED_CIDR
```

### 2  Validar el formato CIDR

```bash
if [[ ! $RED_CIDR =~ ^([0-9]{1,3}\.){3}[0-9]{1,3}/(8|16|24)$ ]]; then
    echo "Formato incorrecto. Use el formato CIDR con máscaras /8, /16 o /24."
    exit 1
fi
```
Se usa una expresión regular para validar la IP y la máscara de red.
### 3 Extraer IP base y máscara
```bash
IP_BASE=$(echo "$RED_CIDR" | cut -d '/' -f1)
MASK=$(echo "$RED_CIDR" | cut -d '/' -f2)
```
cut -d '/' -f1: Extrae la parte de la dirección IP.

cut -d '/' -f2: Extrae la máscara de subred.
### 4 Generar el rango de IPs a escanear
Dependiendo de la máscara (/8, /16, /24), se define el rango de direcciones:
```bash
if [ "$MASK" == "8" ]; then
    SUBNET="$i1"
    RANGO_INICIO=1
    RANGO_FIN=254
    SEGMENTOS=3
elif [ "$MASK" == "16" ]; then
    SUBNET="$i1.$i2"
    RANGO_INICIO=1
    RANGO_FIN=254
    SEGMENTOS=2
elif [ "$MASK" == "24" ]; then
    SUBNET="$i1.$i2.$i3"
    RANGO_INICIO=1
    RANGO_FIN=254
    SEGMENTOS=1
else
    echo "Máscara no soportada. Solo se permite /8, /16 o /24."
    exit 1
fi
```
### 5  Escaneo de dispositivos con ping
```bash
if ping -c 1 -W 1 "$IP" &> /dev/null; then
```
ping -c 1: Envía solo un paquete.

-W 1: Espera 1 segundo para la respuesta.

&> /dev/null: Oculta la salida para mejorar el rendimiento.

### 6 Obtener la dirección MAC
```bash
sleep 1
MAC=$(arp -n "$IP" 2>/dev/null | awk '/ether/ {print $3}')
if [[ -z "$MAC" ]]; then
    MAC=$(ip neigh show "$IP" | awk '{print $5}')
fi
```
arp -n: Lista dispositivos en la caché ARP.

ip neigh show: Alternativa moderna a arp.

sleep 1: Da tiempo para que la tabla ARP se actualice.
### 7 Detección del Sistema Operativo
```bash
TTL=$(ping -c 1 "$IP" | awk -F 'ttl=' '/ttl=/ {print $2}' | awk '{print $1}')
if [[ -n "$TTL" ]]; then
    if [[ "$TTL" -le 64 ]]; then
        SO="Linux"
    elif [[ "$TTL" -le 128 ]]; then
        SO="Windows"
    else
        SO="Desconocido"
    fi
else
    SO="No determinado"
fi
```
ping -c 1 "$IP" | awk -F 'ttl=' '/ttl=/ {print $2}' | awk '{print $1}': Extrae el TTL.

Se usa el valor TTL:

Linux: TTL ≈ 64.

Windows: TTL ≈ 128.

Si es superior, se marca como desconocido.

### 8 Escaneo de puertos abiertos
```bash
for PUERTO in $(seq 1 1024); do
    nc -zv -w1 "$IP" "$PUERTO" 2>&1 | grep -q "succeeded" && echo "    - $PUERTO abierto"
done
```
nc -zv -w1 $IP $PUERTO:

-z: Solo escaneo, no envía datos.

-v: Modo detallado.

-w1: Espera 1 segundo por respuesta.

## Codigo completo




```bash
#!/bin/bash

# Verificar si el script se ejecuta como root
if [[ $EUID -ne 0 ]]; then
   echo "Este script debe ejecutarse como root." 
   exit 1
fi

# Solicitar la red en formato CIDR
read -p "Ingrese la red en formato CIDR (ejemplo: 192.168.1.0/24): " RED_CIDR

# Validar el formato CIDR
if [[ ! $RED_CIDR =~ ^([0-9]{1,3}\.){3}[0-9]{1,3}/(8|16|24)$ ]]; then
    echo "Formato incorrecto. Use el formato CIDR con máscaras /8, /16 o /24."
    exit 1
fi

# Extraer la dirección base y la máscara
IP_BASE=$(echo "$RED_CIDR" | cut -d '/' -f1)
MASK=$(echo "$RED_CIDR" | cut -d '/' -f2)

# Determinar el rango de direcciones según la máscara
IFS='.' read -r i1 i2 i3 i4 <<< "$IP_BASE"
if [ "$MASK" == "8" ]; then
    SUBNET="$i1"
    RANGO_INICIO=1
    RANGO_FIN=254
    SEGMENTOS=3
elif [ "$MASK" == "16" ]; then
    SUBNET="$i1.$i2"
    RANGO_INICIO=1
    RANGO_FIN=254
    SEGMENTOS=2
elif [ "$MASK" == "24" ]; then
    SUBNET="$i1.$i2.$i3"
    RANGO_INICIO=1
    RANGO_FIN=254
    SEGMENTOS=1
else
    echo "Máscara no soportada. Solo se permite /8, /16 o /24."
    exit 1
fi

# Archivo de salida
USUARIO=$(whoami)
ARCHIVO_SALIDA="${USUARIO}_escaneo_red.txt"
INICIO=$(date +%s)

echo "Iniciando escaneo de red en $RED_CIDR..."
echo "Fecha de escaneo: $(date)" > "$ARCHIVO_SALIDA"
echo "Escaneando red: $RED_CIDR" >> "$ARCHIVO_SALIDA"
echo "---------------------------------" >> "$ARCHIVO_SALIDA"

# Escaneo de red con ping
for i in $(seq "$RANGO_INICIO" "$RANGO_FIN"); do
    if [ "$SEGMENTOS" -eq 3 ]; then
        IP="$SUBNET.$i.1.1"
    elif [ "$SEGMENTOS" -eq 2 ]; then
        IP="$SUBNET.$i.1"
    else
        IP="$SUBNET.$i"
    fi

    if ping -c 1 -W 1 "$IP" &> /dev/null; then
        echo "Equipo detectado: $IP" | tee -a "$ARCHIVO_SALIDA"
        
        # Pequeña pausa para que la caché ARP se actualice
        sleep 1

        # Obtener dirección MAC con arp o ip neigh
        MAC=$(arp -n "$IP" 2>/dev/null | awk '/ether/ {print $3}')
        if [[ -z "$MAC" ]]; then
            MAC=$(ip neigh show "$IP" | awk '{print $5}')
        fi
        if [[ -z "$MAC" ]]; then
            MAC="No disponible"
        fi
        echo "  MAC: $MAC" | tee -a "$ARCHIVO_SALIDA"

        # Detectar sistema operativo por TTL
        TTL=$(ping -c 1 "$IP" | awk -F 'ttl=' '/ttl=/ {print $2}' | awk '{print $1}')
        if [[ -n "$TTL" ]]; then
            if [[ "$TTL" -le 64 ]]; then
                SO="Linux"
            elif [[ "$TTL" -le 128 ]]; then
                SO="Windows"
            else
                SO="Desconocido"
            fi
        else
            SO="No determinado"
        fi
        echo "  Sistema operativo: $SO (TTL=$TTL)" | tee -a "$ARCHIVO_SALIDA"
        
        # Escaneo de puertos abiertos (1-1024)
        echo "  Puertos abiertos:" | tee -a "$ARCHIVO_SALIDA"
        for PUERTO in $(seq 1 1024); do
            nc -zv -w1 "$IP" "$PUERTO" 2>&1 | grep -q "succeeded" && echo "    - $PUERTO abierto" | tee -a "$ARCHIVO_SALIDA"
        done
        echo "---------------------------------" >> "$ARCHIVO_SALIDA"
    fi
done

# Medir tiempo total de ejecución
FIN=$(date +%s)
DURACION=$((FIN - INICIO))
echo "Escaneo completado en $DURACION segundos." | tee -a "$ARCHIVO_SALIDA"
echo "Resultados guardados en $ARCHIVO_SALIDA"
```


## Dificultades Encontradas y Soluciones
### Problema 1: arp no encontrado
Causa: Algunas distribuciones modernas no incluyen arp por defecto.
🔹 Solución: Se reemplazó con ip neigh show

### Problema 2: Detección incorrecta del Sistema Operativo
 Causa: La extracción del TTL con grep no era precisa en algunos sistemas.
🔹 Solución: Se usó awk para capturar el TTL de manera más confiable.


[Volver](../index.md)

