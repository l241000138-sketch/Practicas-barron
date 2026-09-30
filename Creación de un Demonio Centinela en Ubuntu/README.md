# Implementación de un demonio de monitoreo en Linux

## Objetivos

El objetivo principal de esta práctica fue crear un script en Linux capaz de monitorear algunos recursos básicos del sistema y convertirlo en un servicio administrado por `systemd`.

El demonio debía ejecutarse de manera continua en segundo plano y registrar cada 5 segundos la fecha y hora, el número de procesos activos y la cantidad de memoria RAM disponible.

También se buscó comprobar el funcionamiento de `systemd` como administrador de servicios, especialmente su capacidad para reiniciar automáticamente un proceso cuando este termina de manera inesperada.

## Explicación de los comandos

Primero se creó el script utilizando **gedit** con el siguiente comando:

`gedit ~/monitor_linux.sh`

El script utiliza `date` para obtener la fecha y hora exacta. Para conocer el número de procesos activos se utilizó `ps -e`, junto con `wc -l` para contar las líneas obtenidas. Para consultar la memoria RAM disponible se utilizó el comando `free`.

El ciclo `while true` permite que el monitoreo se mantenga ejecutándose continuamente, mientras que `sleep 5` hace que el programa espere cinco segundos antes de realizar nuevamente el registro.

Después se utilizó:

`chmod +x ~/monitor_linux.sh`

Este comando agrega permisos de ejecución al script, permitiendo que pueda ejecutarse directamente.

Posteriormente se creó el archivo:

`/etc/systemd/system/monitor-linux.service`

Este archivo contiene la configuración necesaria para que `systemd` pueda administrar nuestro script como un servicio.

Después se utilizó:

`sudo systemctl daemon-reload`

Este comando hace que `systemd` vuelva a cargar sus archivos de configuración y reconozca el nuevo servicio.

Con:

`sudo systemctl enable monitor-linux.service`

se configuró el servicio para que pueda iniciarse automáticamente junto con el sistema.

Para iniciarlo manualmente se utilizó:

`sudo systemctl start monitor-linux.service`

Y para comprobar su estado:

`systemctl status monitor-linux.service`

Finalmente, se utilizó `kill -9` sobre el PID del proceso para comprobar la recuperación automática. La configuración:

`Restart=always`

indica que `systemd` debe intentar reiniciar el servicio cuando termine, mientras que:

`RestartSec=1`

establece una espera de un segundo antes de intentar iniciarlo nuevamente.

## Conclusiones técnicas

La práctica permitió comprobar cómo un script de Bash puede convertirse en un servicio administrado por `systemd`. De esta manera, el programa deja de depender de que el usuario lo ejecute manualmente y puede funcionar como un proceso en segundo plano.

También se comprobó la utilidad de `systemd` para controlar el ciclo de vida de los servicios. La prueba con `kill -9` permitió verificar que el proceso puede ser terminado de manera forzada, pero `systemd` detecta su finalización y genera nuevamente el servicio con un nuevo PID.

En conclusión, se logró implementar un demonio funcional capaz de registrar información básica del sistema cada 5 segundos y con un mecanismo de recuperación automática. Esto demuestra un uso práctico de Bash, procesos de Linux y administración de servicios mediante `systemd`.
