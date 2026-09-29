# Comandos utilizados – Práctica de monitoreo de salud en Linux

## 1. Crear el script

Primero creé el archivo `monitor_salud.sh` utilizando Gedit:

```bash
gedit ~/monitor_salud.sh
```

Dentro del archivo coloqué el siguiente script:

```bash
#!/bin/bash

# Archivo donde se guardará la información
ARCHIVO="$HOME/salud_computadora.txt"

# Encabezado del registro
echo "=============================================" >> "$ARCHIVO"
echo "       SALUD DE MI COMPUTADORA" >> "$ARCHIVO"
echo "Fecha: $(date)" >> "$ARCHIVO"
echo "=============================================" >> "$ARCHIVO"

# Información de la memoria RAM
echo "--- MEMORIA RAM ---" >> "$ARCHIVO"
free -h >> "$ARCHIVO"

# Espacio utilizado y disponible del disco
echo "--- ESPACIO DEL DISCO ---" >> "$ARCHIVO"
df -h "$HOME" >> "$ARCHIVO"

# Carga del sistema
echo "--- CARGA DEL PROCESADOR ---" >> "$ARCHIVO"
uptime >> "$ARCHIVO"

# Tiempo que lleva encendida la computadora
echo "--- TIEMPO ENCENDIDA ---" >> "$ARCHIVO"
uptime -p >> "$ARCHIVO"

# Separador entre registros
echo "" >> "$ARCHIVO"
```

## 2. Dar permisos de ejecución

Después de guardar el script, le di permisos para poder ejecutarlo:

```bash
chmod +x ~/monitor_salud.sh
```

## 3. Ejecutar el script para comprobar que funciona

Ejecuté el script manualmente para comprobar que generara correctamente el archivo de información:

```bash
~/monitor_salud.sh
```

## 4. Consultar el archivo de salud

Para abrir el archivo generado utilizando Gedit:

```bash
gedit ~/salud_computadora.txt
```

También puedo consultar su contenido directamente desde la terminal:

```bash
cat ~/salud_computadora.txt
```

## 5. Configurar Crontab

Para configurar la ejecución automática del script cada 2 minutos, abrí el administrador de tareas programadas de mi usuario:

```bash
crontab -e
```

Dentro de Crontab agregué la siguiente línea:

```bash
*/2 * * * * /home/victoro/monitor_salud.sh
```

Esta configuración hace que el script se ejecute automáticamente cada 2 minutos.

## 6. Comprobar la configuración de Crontab

Para verificar que la tarea quedó registrada correctamente utilicé:

```bash
crontab -l
```

## 7. Consultar nuevamente la información registrada

Finalmente, puedo revisar los datos que el script ha recopilado mediante:

```bash
gedit ~/salud_computadora.txt
```

O directamente desde la terminal:

```bash
cat ~/salud_computadora.txt
```
