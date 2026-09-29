# Práctica: Monitoreo de la salud del equipo mediante Crontab

## Objetivos

El objetivo principal de esta práctica fue crear un script en Linux capaz de obtener información sobre el estado de la computadora y ejecutarlo automáticamente cada dos minutos utilizando `crontab`.

También se buscó aprender a:

* Crear y ejecutar scripts de Bash.
* Obtener información sobre los recursos del sistema.
* Guardar información en un archivo de texto dentro de la carpeta personal.
* Programar tareas automáticas mediante `crontab`.
* Comprobar que una tarea programada se está ejecutando correctamente.

## Desarrollo de la práctica

Primero se creó un script llamado `monitor_salud.sh` utilizando el editor de texto `gedit`. El script se encargó de obtener diferentes datos del equipo, como el uso de la memoria RAM, el espacio disponible del disco, la carga del sistema y el tiempo que lleva encendida la computadora.

Para crear el archivo se utilizó:

```bash
gedit ~/monitor_salud.sh
```

El símbolo `~` representa la carpeta personal del usuario, por lo que el archivo se creó directamente dentro de `/home/victoro/`.

Después de crear el script, fue necesario darle permisos para poder ejecutarlo:

```bash
chmod +x ~/monitor_salud.sh
```

El comando `chmod` permite modificar los permisos de un archivo y `+x` agrega el permiso de ejecución.

Para probar que el script funcionara correctamente se ejecutó:

```bash
~/monitor_salud.sh
```

El script generó un archivo llamado `salud_computadora.txt`, donde se almacenó la información obtenida del sistema.

Para consultar el contenido del archivo desde la terminal se utilizó:

```bash
cat ~/salud_computadora.txt
```

También fue posible abrirlo utilizando `gedit`:

```bash
gedit ~/salud_computadora.txt
```

## Comandos utilizados para obtener información del sistema

Uno de los comandos utilizados fue:

```bash
free -h
```

Este comando muestra información sobre la memoria RAM. La opción `-h` significa que los valores se muestran en un formato fácil de leer, utilizando unidades como MB o GB.

Para consultar el espacio disponible en el disco se utilizó:

```bash
df -h "$HOME"
```

El comando `df` muestra información sobre el espacio utilizado y disponible en los sistemas de archivos. La opción `-h` permite mostrar los valores de manera más comprensible.

También se utilizó:

```bash
uptime
```

Este comando muestra cuánto tiempo lleva encendido el sistema y proporciona información sobre la carga promedio del equipo.

Para mostrar solamente el tiempo que lleva encendida la computadora se utilizó:

```bash
uptime -p
```

## Configuración de Crontab

Después de comprobar que el script funcionaba correctamente, se utilizó `crontab` para automatizar su ejecución.

Se abrió el archivo de tareas programadas con:

```bash
crontab -e
```

Dentro del archivo se agregó la siguiente línea:

```bash
*/2 * * * * /home/victoro/monitor_salud.sh
```

Esta expresión indica que el script debe ejecutarse cada dos minutos.

La estructura básica de `crontab` utiliza cinco campos:

```text
minuto hora día-mes mes día-semana
```

En este caso, `*/2` en el campo de los minutos significa que la tarea se ejecutará cada dos minutos. Los demás `*` indican que no existe una restricción específica para la hora, día, mes o día de la semana.

Para comprobar que la tarea quedó registrada se utilizó:

```bash
crontab -l
```

Este comando muestra las tareas programadas del usuario actual.

## Archivo generado

Toda la información recopilada por el script se guardó en:

```text
/home/victoro/salud_computadora.txt
```

Este archivo permite consultar los datos obtenidos por el script y comprobar que la tarea automática se está ejecutando.

## Conclusiones técnicas

Con esta práctica se comprendió cómo Linux puede utilizar scripts de Bash para automatizar tareas de administración y monitoreo del sistema. El uso de comandos como `free`, `df` y `uptime` permite obtener información importante sobre los recursos de la computadora.

También se aprendió a utilizar `chmod` para asignar permisos de ejecución y `crontab` para programar procesos automáticamente. Esto permite que una tarea se ejecute sin necesidad de iniciarla manualmente cada vez.

Finalmente, la práctica permitió comprender que un sistema Linux puede automatizar tareas periódicas de manera sencilla mediante scripts y servicios propios del sistema. En este caso, el monitoreo se realiza cada dos minutos y los resultados se almacenan en un archivo dentro de la carpeta personal del usuario.
