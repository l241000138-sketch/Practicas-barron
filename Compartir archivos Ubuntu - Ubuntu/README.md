# Compartir archivos Ubuntu/Kubuntu - Ubuntu/Kubuntu

## Objetivo
Configurar una carpeta compartida mediante Samba (SMB) entre dos equipos Ubuntu/Kubuntu conectados por Ethernet, verificar la conectividad IP y comprobar el acceso autenticado y la transferencia de archivos.

## Topología usada como ejemplo
En la práctica realizada se utilizaron estas direcciones

| Equipo | Función | IP de ejemplo |
|---|---|---|
| Vicpapas-HP (Kubuntu) | Servidor Samba | `192.168.20.143` |
| ed-thinkpad (Kubuntu) | Cliente | `192.168.20.217` |

Máscara: `255.255.255.0` (`/24`). En una conexión Ethernet directa no se requiere puerta de enlace ni DNS para comunicarse entre esos dos equipos. Si tu práctica usó las funciones invertidas, intercambia las IP y ejecuta los comandos en el equipo que corresponda.

## Estructura
- `Codigo/`: scripts Bash comentados.
- `Terminal/`: espacio para evidencias reales de ejecución.
- `Reporte/`: reporte técnico en PDF.

## Requisitos
- Dos equipos Ubuntu/Kubuntu conectados por Ethernet.
- Direcciones IP únicas en la misma subred.
- Acceso a una cuenta con permisos `sudo` en el servidor.
- Samba y `smbclient` (el script de configuración instala los paquetes si faltan).
- Usuario local existente en el servidor. Se creará/actualizará su contraseña Samba.

## Procedimiento

### 1. Configurar IP
Configura IPv4 manual en ambos equipos. Ejemplo:
- Servidor: `192.168.20.143/24`
- Cliente: `192.168.20.217/24`
- Puerta de enlace y DNS: vacíos para esta conexión aislada.

Comprueba las direcciones con:
```bash
ip -brief address
```

### 2. Probar conectividad
En el cliente ejecuta:
```bash
bash Codigo/verificar_red.sh 192.168.20.143
```
Sustituye la IP por la dirección real del servidor. Un resultado de `0% packet loss` indica que el destino respondió a ICMP; no demuestra por sí solo que SMB esté configurado.

### 3. Configurar el servidor Samba
Copia la carpeta `Codigo` al equipo servidor o sitúate en la carpeta del proyecto. Ejecuta como usuario normal:
```bash
bash Codigo/configurar_samba.sh victoro Compartida
```
Sustituye `victoro` por el usuario local que será autorizado. El script instala Samba, crea `~/Compartida`, respalda `smb.conf` antes de agregar el bloque, solicita una contraseña Samba y reinicia `smbd`. No uses el nombre de usuario de ejemplo si no existe en ese equipo.

### 4. Verificar servidor
En el servidor:
```bash
bash Codigo/verificar_samba.sh victoro
```
También puedes validar manualmente:
```bash
sudo testparm -s
sudo systemctl status smbd
smbclient -L localhost -U victoro
```
En la lista debe aparecer `Compartida`.

### 5. Acceder desde el cliente
En Dolphin presiona `Ctrl+L` y escribe:
```text
smb://192.168.20.143/Compartida
```
O desde terminal:
```bash
smbclient //192.168.20.143/Compartida -U victoro
```
Usa la IP real del servidor y la contraseña creada con `smbpasswd`. No escribas la contraseña en el comando ni la guardes en el README.

### 6. Probar escritura
Desde el cliente crea un archivo en la carpeta compartida, por ejemplo `archivo_desde_cliente.txt`. En el servidor comprueba:
```bash
ls -l ~/Compartida
```
La presencia del archivo confirma que el cliente pudo escribir en el recurso, siempre que los permisos de carpeta y Samba lo permitan.

## Explicación de comandos principales
- `ip -brief address`: muestra interfaces y direcciones IP de forma compacta.
- `ping -c 4 IP`: envía cuatro solicitudes ICMP para probar conectividad.
- `apt-get install samba smbclient`: instala el servidor SMB y la herramienta cliente.
- `install -d`: crea el directorio con propietario, grupo y permisos definidos.
- `smbpasswd -a usuario`: registra o habilita la contraseña de autenticación Samba.
- `testparm -s`: valida y muestra la configuración efectiva de Samba.
- `systemctl enable --now smbd`: habilita e inicia el servicio.
- `systemctl restart smbd`: aplica cambios reiniciando el servicio.
- `smbclient -L localhost -U usuario`: enumera los recursos publicados localmente.
- `ufw allow samba`: permite el perfil de Samba en UFW si el firewall está activo.

## Solución de problemas
1. **No responde el ping:** confirma cable, enlace Ethernet, IPs distintas, máscara `/24` y que ambas interfaces estén activas.
2. **Responde el ping pero no abre el recurso:** verifica `sudo testparm -s`, `systemctl status smbd` y `smbclient -L localhost -U usuario`.
3. **No aparece `Compartida`:** confirma que el bloque `[Compartida]` esté en `/etc/samba/smb.conf` y que `path` exista.
4. **Acceso denegado:** comprueba el usuario Samba (`sudo pdbedit -L`), contraseña, `valid users` y permisos Linux del directorio.
5. **Firewall:** si UFW está activo, `sudo ufw allow samba`.
6. **Credenciales antiguas en el cliente:** elimina la conexión guardada en el gestor de contraseñas o vuelve a conectarte indicando el usuario correcto.

## Seguridad y observaciones
El recurso requiere autenticación y no habilita acceso invitado. No se recomienda activar SMB1 ni desactivar el firewall por completo. En una red de laboratorio aislada, las IP estáticas facilitan las pruebas, pero no deben duplicarse con las de otros dispositivos de la red.

## Conclusiones técnicas
La práctica integra varias capas: Ethernet proporciona el enlace físico; IPv4 y la máscara identifican los equipos dentro de la subred; ICMP permite comprobar alcance; `smbd` publica el recurso mediante SMB; y Samba valida las credenciales y aplica sus reglas de acceso. Un ping exitoso no garantiza que el servicio SMB esté disponible, por lo que se verificaron por separado conectividad, servicio, configuración y permisos. La transferencia exitosa desde el cliente confirma tanto el acceso al recurso como los permisos de escritura efectivos.
