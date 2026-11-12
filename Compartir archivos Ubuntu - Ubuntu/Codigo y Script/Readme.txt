1. Configuración y comprobación de red
En ambas laptops:

Consultar las interfaces y direcciones IP:
ip addr


Mostrar las interfaces de forma resumida:
ip -brief address


Activar la conexión de red:
sudo nmcli networking on


Consultar las conexiones disponibles:
nmcli connection show


Activar la conexión Ethernet:
sudo nmcli connection up "Conexión cableada 1"


Desactivar temporalmente WiFi para evitar conflictos:
sudo nmcli radio wifi off


2. Configuración de direcciones IP
En la interfaz gráfica configuramos IPv4 manualmente:
Laptop	Dirección IP	Máscara
Vicpapas-HP	192.168.20.143	255.255.255.0
ed-thinkpad	192.168.20.217	255.255.255.0

No utilizamos puerta de enlace ni DNS para esta conexión directa.
3. Comprobar la conexión entre laptops
Desde Vicpapas-HP:
ping -c 4 192.168.20.217


Desde ed-thinkpad:
ping -c 4 192.168.20.143


Consultar la ruta hacia la otra laptop:
ip route get 192.168.20.217


4. Descargar e instalar Samba
En la laptop que funcionará como servidor:

Actualizar los repositorios:
sudo apt update


Actualizar los paquetes instalados:
sudo apt upgrade -y


Instalar Samba y su cliente:
sudo apt install samba smbclient -y


Comprobar la versión instalada:
smbd --version


5. Crear la carpeta compartida
Crear el directorio:
mkdir -p ~/Compartida


Crear un archivo de prueba:
echo "Conexion Samba exitosa entre Kubuntu" > ~/Compartida/prueba.txt


Comprobar el contenido:
ls -l ~/Compartida


6. Crear el usuario de Samba
Agregar el usuario:
sudo smbpasswd -a victoro


Comprobar los usuarios registrados:
sudo pdbedit -L


7. Configurar Samba
Abrir el archivo de configuración:
sudo nano /etc/samba/smb.conf


Agregar al final:
[Compartida]
   path = /home/victoro/Compartida
   browsable = yes
   read only = no
   valid users = victoro
   create mask = 0664
   directory mask = 0775


Guardar con Ctrl + O, Enter y salir con Ctrl + X.
8. Comprobar la configuración de Samba
Validar el archivo:
sudo testparm


Mostrar la configuración resumida:
sudo testparm -s


9. Iniciar y habilitar Samba
Habilitar e iniciar el servicio:
sudo systemctl enable --now smbd


Reiniciar el servicio:
sudo systemctl restart smbd


Consultar su estado:
sudo systemctl status smbd


Comprobar si está activo:
systemctl is-active smbd


10. Configurar el firewall
Consultar el estado:
sudo ufw status


Permitir conexiones Samba si UFW está activo:
sudo ufw allow samba


11. Comprobar los recursos compartidos
Mostrar los recursos Samba locales:
smbclient -L localhost -U victoro


Conectarse al recurso compartido desde la otra laptop:
smbclient //192.168.20.143/Compartida -U victoro


12. Acceder desde Dolphin
En la laptop cliente, abre Dolphin y presiona Ctrl + L.

Escribe:
smb://192.168.20.143/Compartida


Introduce el usuario y contraseña Samba.
13. Comprobar la transferencia de archivos
Desde la laptop cliente crea un archivo llamado archivo_desde_cliente.txt dentro de la carpeta compartida.

Después, en el servidor ejecuta:
ls -l ~/Compartida


Leer el contenido del archivo de prueba:
cat ~/Compartida/prueba.txt


Comprobar los permisos de la carpeta:
ls -ld ~/Compartida



14. Comandos adicionales para diagnóstico
Si necesitas revisar posibles errores:

Consultar los últimos registros de Samba:
sudo journalctl -u smbd -n 30 --no-pager


Consultar si Samba escucha en sus puertos:
sudo ss -tulpn | grep -E ':139|:445'


Reiniciar el servicio:
sudo systemctl restart smbd
