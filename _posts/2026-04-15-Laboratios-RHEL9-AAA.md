---
title: Laboratorios sobre administración de Red Hat Enterprise Linux 9 e implementación de servidor AAA utilizando FreeRadius.
date: 2026-04-14 00:00:01
categories: [Networking] [OS]
tags: [Active Directory]
---

# Laboratorios sobre administración de Red Hat Enterprise Linux 9 e 

Cree una serie de videos documentando la configuración básica de servidores red hat linux (Gestión de usuarios, parametros de red, uso de cron, ssh, servidor HTTP, correos, impresión, etc.).

- Lab 1: https://youtube.com/playlist?list=PLeak1OuZXLPfZpSAHIR_ZgKhX51hDmP_F&si=l56vwgmIOYLjlsW1

- Lab 2: https://youtube.com/playlist?list=PLeak1OuZXLPfvcwj1roqCr2MNEQEqwuqV&si=2V22BTp_5xMmwk1D

- Lab 3: https://youtube.com/playlist?list=PLeak1OuZXLPeuQuIhFbWcobJhjrk28ciu&si=Dic4ek-Ex9B1iaJt

- Lab 4: https://youtube.com/playlist?list=PLeak1OuZXLPdA7eKf6Cgvi1fI8Zi-pqDO&si=6CnufAcZym3RV_pM

- Lab 5: https://youtube.com/playlist?list=PLeak1OuZXLPcNGhdkbadfa2kZnJ2_fajE&si=mj-k7n9CWUzkF0GN

- Lab 6: https://youtube.com/playlist?list=PLeak1OuZXLPcyv9DQ52TVBNR4nxmjg-T7&si=5TG0GfJo0Ll5lAOj

- Lab 7: https://youtube.com/playlist?list=PLeak1OuZXLPcGDkxZZPUPYKuyur39y0Cm&si=S_A7fyByYcmfkgnC

- Lab 8: https://youtube.com/playlist?list=PLeak1OuZXLPdtqI02KMYDAO4TKBG-yHDh&si=9LS6_JieVvyxqHmP

- Lab 9: https://youtube.com/playlist?list=PLeak1OuZXLPc9jT2u8AnlP5Lg1HMA2kVk&si=d9ugEUKDMVjgVcu1


# implementación de servidor AAA utilizando FreeRadius

## Requisitos

### Configuración del entorno virtual:

- Software de virtualización, como VirtualBox o VMware.
- VM de Ubuntu Server.
- Proceda a instalar el Sistema operativo Ubuntu Server
- Configura tu tarjeta de red en modo Bridge o host only para comunicarte con tu maquina host
- Realiza una prueba de Ping para validar que existe comunicacion entre ambos equipos

2- Instalación del servidor AAA:

- Procede a Instalar La aplicacion FreeRadius en la máquina de Ubuntu
Server
- Edita el archivo de configuración del servidor RADIUS y agrega las reglas
necesarias para la autorización de los usuarios
3- Pruebas de autenticacion del Servidor AAA:

- Se Utilizó el appliance (Firewall FortiVM) para probar la autenticación en el servidor AAA.

- Intenta autenticarte con las cuentas de usuario que configuraste previamente y verifica que el servidor responda correctamente.


## Procedimiento

### 1.Instalación del servidor AAA:

- Instalando freeradius:

![alt text](assets/AAA freradius/image-1.png)

- Comprobando que el servicio esté activo:

![alt text](assets/AAA freradius/image-2.png)

- Agregando al cliente (FortiVM) al archivo de configuración de FreeRadius (/etc/freeradius/3.0/clients.conf):

![alt text](assets/AAA freradius/image-3.png)

![alt text](assets/AAA freradius/image-4.png)

- Añadiendo un usuario

![alt text](assets/AAA freradius/image-5.png)

![alt text](assets/AAA freradius/image-6.png)

- Reiniciando y viendo estatus del servicio:

![alt text](assets/AAA freradius/image-7.png)

### 2- Pruebas de autenticacion del Servidor AAA

- Desplegando appliance FortiVM en vmware

![alt text](assets/AAA freradius/image-8.png)

![alt text](assets/AAA freradius/image-9.png)

- Asignación de IP estática a FortiVM y habilitamos ping, https y ssh en el port1 para que esté en la misma subred que el servidor FreeRadius.

![alt text](assets/AAA freradius/image-10.png)

- Configurando puerta de enlace predeterminada:

![alt text](assets/AAA freradius/image-11.png)

- Verificando conectividad con servidor Freeradius:

![alt text](assets/AAA freradius/image-12.png)

- Accediendo a la interfaz de administración:

![alt text](assets/AAA freradius/image-13.png)

![alt text](assets/AAA freradius/image-14.png)

- Agregando servidor FreeRadius a la FortiVM:

![alt text](assets/AAA freradius/image-15.png)

![alt text](assets/AAA freradius/image-16.png)

- Test de credenciales de usuario:

![alt text](assets/AAA freradius/image-17.png)

- Creando un grupo de usuarios para que utilicen el server Radius:

![alt text](assets/AAA freradius/image-18.png)

- Configurando permisos administrativos del grupo “Radius-users”:

![alt text](assets/AAA freradius/image-19.png)

- Prueba de autenticación con el usuario “fran” contraseña “fran”:

![alt text](assets/AAA freradius/image-20.png)

![alt text](assets/AAA freradius/image-21.png)