# 1
## La tarea se ejecuta cada hora
0 * * * * 
## La tarea se ejecuta los domingos cada 3 horas
0 */3 * * 0
## La tarea se ejecuta a las 12 de la mañana los días pares del mes.
0 12 */2 * * 
## La tarea se ejecuta el primer día de cada mes a las 8 de la mañana y  a  las 8 de la tarde.
0 8,20 1 * * 
## La tarea se ejecuta cada media hora de lunes a viernes.
*/30 * * * 1-5
## La tarea se ejecuta cada cuarto de hora, entre las 3 y las 8, de lunes a  viernes, durante todo el mes de agosto
*/15 3-8 * 8 1-5
## La tarea se ejecuta cada 90 minutos
0 */1 * * * 
# 2
systemctl status cron
# 3
que cada 15 minutos durante las horas 1,2y3 de la mañana se va a ejecutar sobreescribiendo el archivo test
# 4
/etc/crontab
# 5
los ficheros cron.allow y cron.deny
# 6
30 7 * * 1 /ruta/del/script.sh
# 7
crontab -l
para ver las tareas y 
crontab -r para suprimirla
# 8
*/2 * * * * ps -ef  >> /tmp/ps_result
# 9
crontab -l
# 10 
cat /tmp/ps_result
# 11. 
bash
sudo adduser asir2
echo "asir2" | sudo tee -a /etc/cron.deny
# 12
su asir2
crontab -e
# 13 
5 0 * * rm -r /tmp
# 14
0 9 * 7-9 1-5 date +'%T' > /tmp/file; who >> /tmp/file
# 15
cron.d:
Este directorio contiene archivos de configuración para tareas de cron que no encajan en las categorías de tareas diarias, semanales, mensuales, etc. 
cron.allow:
si existe solo los usuarios que están listados en dicho fichero pueden crear, editar, mostrar o eliminar ficheros crontab.
cron.deny:
Si existe solo cron.deny, los usuarios en esa lista no podrán usar cron, pero todos los demás sí.
cron.daily:
Este directorio contiene scripts o archivos que se ejecutan automáticamente una vez al día
cron.hourly:
En este directorio se almacenan scripts o tareas que se ejecutan una vez por hora. 
cron.monthly:
Este directorio contiene scripts o archivos que se ejecutan una vez al mes. 
