# Scripts y comandos utilizados

## 1. Crear el script de monitoreo

Para crear el archivo del script utilizando el editor de texto gedit se utilizó el siguiente comando:

```bash
gedit ~/monitor_linux.sh
```

Dentro del archivo se escribió el siguiente código:

```bash
#!/bin/bash

# Archivo donde se guardarán los registros
ARCHIVO="$HOME/monitor_linux.log"

# Ciclo continuo del monitoreo
while true
do
    # Obtener fecha y hora actual
    FECHA=$(date '+%Y-%m-%d %H:%M:%S')

    # Contar el número total de procesos activos
    PROCESOS=$(ps -e --no-headers | wc -l)

    # Obtener la memoria RAM disponible
    RAM=$(free -h | awk '/Mem:/ {print $7}')

    # Guardar los datos en el archivo de bitácora
    echo "Fecha: $FECHA | Procesos activos: $PROCESOS | RAM disponible: $RAM" >> "$ARCHIVO"

    # Esperar 5 segundos antes del siguiente registro
    sleep 5
done
```

## 2. Dar permisos de ejecución al script

Después de guardar el archivo, se le dieron permisos de ejecución mediante:

```bash
chmod +x ~/monitor_linux.sh
```

## 3. Crear el archivo del servicio de systemd

Para crear el archivo que permite administrar el script como un servicio se utilizó:

```bash
sudo gedit /etc/systemd/system/monitor-linux.service
```

Dentro del archivo se colocó:

```ini
[Unit]
Description=Monitor de procesos y memoria RAM
After=multi-user.target

[Service]
Type=simple
ExecStart=/home/victoro/monitor_linux.sh
Restart=always
RestartSec=1

[Install]
WantedBy=multi-user.target
```

## 4. Recargar la configuración de systemd

Después de crear el servicio se utilizó:

```bash
sudo systemctl daemon-reload
```

Este comando permite que systemd vuelva a leer sus archivos de configuración y reconozca el nuevo servicio.

## 5. Habilitar el servicio

Para configurar el servicio para que pueda iniciarse automáticamente con el sistema:

```bash
sudo systemctl enable monitor-linux.service
```

## 6. Iniciar el servicio

Para iniciar el demonio:

```bash
sudo systemctl start monitor-linux.service
```

## 7. Comprobar el estado del servicio

Para comprobar que el servicio se encuentra funcionando:

```bash
systemctl status monitor-linux.service
```

El resultado esperado es:

```text
Active: active (running)
```

## 8. Consultar la bitácora

Para visualizar los datos registrados por el script:

```bash
cat ~/monitor_linux.log
```

También se puede observar la información en tiempo real mediante:

```bash
tail -f ~/monitor_linux.log
```

Para salir de la visualización en tiempo real se utiliza:

```text
Ctrl + C
```

## 9. Obtener el PID del proceso

Para conocer el PID que actualmente está utilizando el demonio:

```bash
systemctl show -p MainPID --value monitor-linux.service
```

## 10. Realizar la prueba con kill -9

Después de obtener el PID, se utilizó el siguiente comando, sustituyendo `PID` por el número correspondiente:

```bash
sudo kill -9 PID
```

Por ejemplo:

```bash
sudo kill -9 28071
```

## 11. Comprobar que systemd reinició el proceso

Después de ejecutar `kill -9`, se volvió a consultar el PID:

```bash
systemctl show -p MainPID --value monitor-linux.service
```

Si el servicio está configurado correctamente, aparecerá un PID diferente, demostrando que systemd volvió a iniciar automáticamente el proceso.

También se puede comprobar mediante:

```bash
systemctl status monitor-linux.service
```

## 12. Detener el servicio

Para detener temporalmente el demonio:

```bash
sudo systemctl stop monitor-linux.service
```

## 13. Desactivar el inicio automático

Para evitar que el servicio se inicie automáticamente al encender la computadora:

```bash
sudo systemctl disable monitor-linux.service
```

## 14. Detener y desactivar el servicio

Para realizar ambas acciones:

```bash
sudo systemctl stop monitor-linux.service
```

```bash
sudo systemctl disable monitor-linux.service
```

## 15. Volver a activar el servicio

Si se desea utilizar nuevamente el demonio:

```bash
sudo systemctl enable monitor-linux.service
```

Después se inicia:

```bash
sudo systemctl start monitor-linux.service
```

De esta manera, el script queda configurado como un servicio administrado por systemd, ejecutándose en segundo plano, registrando información cada 5 segundos y reiniciándose automáticamente en caso de que el proceso sea terminado.
