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
[Volver](../index.md)

