1. Consultar la configuración de red
ip addr


ip -brief address


nmcli networking on


nmcli connection show


nmcli connection up "Conexión cableada 1"


2. Comprobar la conexión entre las laptops
Desde Ubuntu/Kubuntu:
ping -c 4 192.168.20.217


Desde la otra laptop:
ping -c 4 192.168.20.143


Estas fueron las direcciones IP que utilizamos durante las pruebas de conexión.
3. Actualizar Ubuntu e instalar Samba
sudo apt update


sudo apt upgrade -y


sudo apt install samba smbclient -y


Comprobar la versión instalada:
smbd --version


4. Crear la carpeta compartida
Primero utilizamos una carpeta dentro del usuario:
mkdir -p ~/Compartida


Crear un archivo de prueba:
echo "Archivo de prueba de Samba" > ~/Compartida/prueba.txt


Comprobar su contenido:
ls -l ~/Compartida


Posteriormente utilizamos una ubicación dedicada para Samba:
sudo mkdir -p /srv/samba/Compartida


Asignar permisos al usuario:
sudo chown victoro:victoro /srv/samba/Compartida


sudo chmod 2770 /srv/samba/Compartida


5. Crear el usuario de Samba
sudo smbpasswd -a victoro


Este comando solicita establecer una contraseña para acceder a la carpeta compartida.

Consultar los usuarios registrados:
sudo pdbedit -L


6. Configurar el archivo smb.conf
Abrir el archivo de configuración:
sudo nano /etc/samba/smb.conf


Agregar al final del archivo:
[Compartida]
   path = /srv/samba/Compartida
   browseable = yes
   read only = no
   writable = yes
   valid users = victoro
   create mask = 0660
   directory mask = 0770


Guardar con:
- Ctrl + O
- Enter
- Ctrl + X
7. Comprobar la configuración de Samba
sudo testparm


sudo testparm -s


8. Activar y reiniciar Samba
sudo systemctl enable --now smbd


sudo systemctl restart smbd


Comprobar su funcionamiento:
sudo systemctl status smbd


Consultar si está activo:
systemctl is-active smbd


9. Configurar el firewall
Consultar el estado:
sudo ufw status


Permitir conexiones de Samba:
sudo ufw allow samba


10. Probar el funcionamiento de Samba
Consultar las carpetas compartidas localmente:
smbclient -L localhost -U victoro


Conectarse a la carpeta compartida mediante la IP:
smbclient //192.168.20.143/Compartida -U victoro


11. Comandos adicionales de diagnóstico
Consultar permisos de la carpeta:
ls -ld /srv/samba/Compartida


Consultar archivos:
ls -l /srv/samba/Compartida


Consultar registros del servicio:
sudo journalctl -u smbd


Consultar los puertos en escucha:
sudo ss -tulpn | grep smb


Consultar la dirección IP:
hostname -I



12. Comandos para Windows (PowerShell)
Consultar la configuración de red:
ipconfig


Probar conexión con Ubuntu:
ping 192.168.20.143


Comprobar el puerto de Samba:
Test-NetConnection 192.168.20.143 -Port 445


13. Acceder a la carpeta compartida desde Windows
En el explorador de archivos de Windows, escribir en la barra de direcciones:
\\192.168.20.143\Compartida


Cuando solicite credenciales:
Campo	Valor
Usuario	victoro
Contraseña	La contraseña creada con smbpasswd
