
# Compartir archivos Ubuntu/Kubuntu - Windows 11

## 1. Objetivo

Establecer una conexión de red entre una computadora con Kubuntu y otra con Windows 11 mediante un cable Ethernet, utilizando Samba como servicio de compartición de archivos.

El propósito es permitir que Windows 11 acceda a una carpeta compartida alojada en Kubuntu, utilizando el protocolo SMB y autenticación mediante usuario y contraseña.

## 2. Descripción de la práctica

Se conectaron dos computadoras mediante un cable Ethernet.

La computadora con Kubuntu se configuró como servidor Samba, mientras que Windows 11 se utilizó como cliente para acceder a los archivos compartidos.

### Direcciones IP utilizadas

| Equipo | Dirección IP | Máscara |
|---|---|---|
| Kubuntu (servidor) | 192.168.20.217 | 255.255.255.0 |
| Windows 11 (cliente) | 192.168.20.143 | 255.255.255.0 |

Nombre del recurso compartido: Compartida.

Usuario Samba: victoro.

## 3. Procedimiento

1. Se conectaron ambas computadoras mediante un cable Ethernet.
2. Se configuraron direcciones IPv4 manuales dentro de la misma subred.
3. Se comprobó la comunicación mediante el comando ping.
4. Se instaló Samba en Kubuntu utilizando apt.
5. Se creó el directorio Compartida.
6. Se registró un usuario mediante smbpasswd.
7. Se modificó el archivo smb.conf para publicar el recurso compartido.
8. Se validó la configuración mediante testparm.
9. Se inició el servicio smbd.
10. Desde Windows 11 se accedió al recurso mediante la dirección de red.
11. Se realizó una prueba de transferencia de archivos.

## 4. Explicación de los comandos

- ip addr: muestra las interfaces de red y sus direcciones IP.
- ipconfig: muestra la configuración de red de Windows.
- ping: comprueba la comunicación entre los equipos.
- apt update: actualiza la información de los repositorios.
- apt install samba smbclient: instala las herramientas necesarias para compartir archivos.
- mkdir: crea el directorio compartido.
- smbpasswd: registra las credenciales de un usuario Samba.
- nano: permite editar el archivo de configuración de Samba.
- testparm: comprueba la sintaxis de smb.conf.
- systemctl: permite administrar el servicio smbd.
- smbclient: permite consultar y acceder a recursos compartidos SMB.
- ufw allow samba: permite las conexiones Samba mediante el firewall UFW.
- Test-NetConnection: comprueba la conectividad TCP hacia el puerto 445.
- net use: permite conectar y administrar unidades de red en Windows.

## 5. Fundamento técnico

Samba es una implementación de código abierto del protocolo SMB, utilizado para compartir archivos e impresoras entre diferentes sistemas operativos.

En esta práctica, Kubuntu proporciona el servicio mediante el proceso smbd, mientras que Windows 11 utiliza su cliente SMB integrado.

La comunicación se realiza mediante Ethernet e IPv4. La máscara de subred permite que ambos equipos se encuentren dentro de la misma red local.

El puerto TCP 445 es utilizado por SMB para establecer las conexiones de compartición de archivos.

La autenticación permite controlar qué usuarios pueden acceder al recurso compartido. Además, Samba utiliza los permisos del sistema de archivos Linux para determinar las operaciones que pueden realizarse sobre los archivos.

## 6. Análisis del comportamiento del sistema operativo

Durante la práctica se observó que ambos sistemas operativos pueden intercambiar archivos a pesar de utilizar diferentes entornos y herramientas de administración.

Kubuntu utiliza Samba para proporcionar el servicio de compartición, mientras que Windows 11 incorpora compatibilidad SMB de manera nativa.

La prueba de ping permitió comprobar que existía comunicación IP entre las computadoras. Sin embargo, una respuesta positiva no garantiza por sí sola que Samba esté funcionando, por lo que también fue necesario verificar el servicio y el puerto TCP 445.

Otro aspecto importante fue la configuración de permisos. Aunque Windows pudiera localizar la dirección IP del servidor, necesitaba que el recurso estuviera publicado correctamente y que las credenciales proporcionadas fueran válidas.

El uso de una red privada en Windows permitió habilitar las funciones de detección y compartición de archivos para ese perfil de red.

## 7. Resultados

Se realizaron pruebas de comunicación entre Windows 11 y Kubuntu mediante Ethernet.

Se verificó el funcionamiento del servicio Samba y la disponibilidad del recurso compartido denominado Compartida.

Desde Windows 11 se accedió a la carpeta alojada en Kubuntu utilizando su dirección IP y las credenciales correspondientes.

También se comprobó la transferencia de archivos mediante la creación de un documento desde Windows y su posterior visualización desde Kubuntu.

## 8. Conclusiones técnicas

La práctica permitió comprender cómo se establece una comunicación entre dos sistemas operativos diferentes mediante una red local y el protocolo SMB.

Se comprobó que Samba facilita la interoperabilidad entre Linux y Windows sin necesidad de instalar software adicional de compartición en Windows.

También se identificó la importancia de configurar correctamente las direcciones IP, los permisos de acceso, las credenciales y los servicios de red.

Finalmente, se concluye que la transferencia de archivos depende de la correcta interacción entre la infraestructura de red, el servicio Samba y los mecanismos de seguridad de ambos sistemas operativos.

## 9. Seguridad

El recurso compartido se configuró con autenticación mediante usuario y contraseña.

Se recomienda mantener activo el firewall, permitir únicamente los servicios necesarios y evitar habilitar el acceso anónimo cuando no sea necesario.

No se recomienda desactivar completamente el firewall ni habilitar versiones antiguas e inseguras del protocolo SMB.
