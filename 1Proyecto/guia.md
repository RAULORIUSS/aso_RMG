```bash
#!/bin/bash
 
# Pedir la red en formato CIDR
read -p "Introduce la red en formato CIDR (ej: 192.168.1.0/24): " red
read -p "Introduce el rango de puertos a escanear (ej: 1-1024): " rango_puerto
read -p "Introduce el nombre del archivo de salida: " archivo_salida
# iniciar escaneo tiempo
start_time=$(date +%s) 
# Iniciar archivo de salida
echo "Escaneo de red - $(date)" > "$archivo_salida"
 
# Función para identificar el sistema operativo
function detectar_so {
    if [ "$1" -le 64 ]; then
        echo "Linux"
    elif [ "$1" -le 128 ]; then
        echo "Windows"
    else
        echo "Desconocido"
    fi
}
 
# Escaneo de red
echo "Escaneando la red $red..."
for IP in $(seq 1 254); do
    TARGET="${red%.*}.$IP"
    ping -c 1 -W 1 "$TARGET" &> /dev/null
    if [ $? -eq 0 ]; then
        echo "Equipo detectado: $TARGET"
        echo "Equipo detectado: $TARGET" >> "$archivo_salida"
 
        # Determinar TTL y Sistema Operativo
        TTL=$(ping -c 1 "$TARGET" | grep 'ttl=' | awk -F'ttl=' '{print $2}' | cut -d ' ' -f1)
        SO=$(detectar_so "$TTL")
        echo "Sistema Operativo: $SO"
        echo "Sistema Operativo: $SO" >> "$archivo_salida"
 
        # Escaneo de puertos
        echo "Escaneando puertos abiertos en $TARGET..."
        for PORT in $(seq $(echo $rango_puerto | cut -d '-' -f1) $(echo $rango_puerto | cut -d '-' -f2)); do
            nc -zv -w 1 "$TARGET" "$PORT" &> /dev/null
            if [ $? -eq 0 ]; then
                SERVICE=$(awk -F, -v port="$PORT" '$2 == port {print $3}' tcp.csv)
                echo "Puerto abierto: $PORT ($SERVICE)"
                echo "Puerto abierto: $PORT ($SERVICE)" >> "$archivo_salida"
            fi
        done
    fi
done
 
echo "Escaneo completado. Resultados almacenados en $archivo_salida."
# fin escaneo tiempo
end_time=$(date +%s)
echo "Tiempo de escaneo: $(($end_time - $start_time)) segundos" >> "$archivo_salida"
```
[Volver](../../index.md)