---
title: Instalación y Configuración de XDR Wazuh
featured_image: assets/Instalacion Wazuh/Portada.png
date: 2026-04-14 00:00:00
categories: [Blue Team]
tags: [XDR]
---

# Resumen

En este proyecto veremos la implementación del XDR/SIEM Wazuh en un entorno de laboratorio, con el objetivo de simular un entorno real de monitoreo de seguridad donde se puede detectar actividad sospechosa y evaluar la seguridad de sistemas windows.

# Requisitos


- **Una VM con Linux (Ubuntu Desktop, o Server)**

Para esto lo que vamos a hacer es ir a https://ubuntu.com/download y descargamos el archivo .iso de Ubuntu server o Desktop, cualquiera de las dos opciones funciona. En mi caso descargué Ubuntu server.

![alt text](assets/Instalacion Wazuh/image-1.png)

Luego, utilizamos un hipervisor para crear la máquina virtual en la que instalaremos Ubuntu, yo utilicé VMware.

![alt text](assets/Instalacion Wazuh/image-2.png)

Ya creada la máquina seguimos el proceso de instalación de Ubuntu e instalamos Gnome con la herramienta tasksel.

![alt text](assets/Instalacion Wazuh/image-3.png)

![alt text](assets/Instalacion Wazuh/image-4.png)


- **Una VM de windows 10**

Vamos a https://www.microsoft.com/en-us/software-download/windows10 e instalar la herramienta que brinda Microsoft para obtener el archivo .iso de Windows 10. Luego, creamos una VM e instalamos el sistema

![alt text](assets/Instalacion Wazuh/image-5.png)

# Procedimiento

## 1. **Configure ambas VM en modo Bridge y valide que sean alcanzables via ping**

Para esto vamos a la máquina correspondiente y entramos a configuraciones de la máquina virtual, luego al apartado de “Network Adaptor” y en “Network connection” seleccionamos la opción “Bridged”. Esto lo hacemos en cada
máquina:

**Windows 10**

![alt text](assets/Instalacion Wazuh/image-6.png)
![alt text](assets/Instalacion Wazuh/image-7.png)

**Ubuntu Server**

![alt text](assets/Instalacion Wazuh/image-8.png)
![alt text](assets/Instalacion Wazuh/image-9.png)

**Ping entre máquinas**

![alt text](assets/Instalacion Wazuh/image-10.png)
![alt text](assets/Instalacion Wazuh/image-11.png)

## 2. **Descargar e instalar Wazuh en la MV de Linux**

Para descargar Wazuh server en la máquina Ubuntu se utiliza, se utiliza el siguiente comando:

curl -sO https://packages.wazuh.com/4.9/wazuh-install.sh && sudo bash ./wazuh-install.sh -a

![alt text](assets/Instalacion Wazuh/image-12.png)

Al terminar la instalación se facilitan las credenciales para poder iniciar sesión desde un navegador con la url “https:<Ip de la máquina>:443”.

![alt text](assets/Instalacion Wazuh/image-13.png)

## 3. **Instalar y configurar del agente de Wazuh en la VM de Windows (validar que los puertos y servicios que utiliza el agente de wazuh esten permitidos en el firewall de windows)**

Lo primero es ir a https://documentation.wazuh.com/current/installation-guide/wazuh-agent/index.html e instalar el windows installer:

![alt text](assets/Instalacion Wazuh/image-14.png)

Luego abrimos una ventana de powershell como administrador, se cambia a la carpeta donde descargamos el archivo con el comando cd, seleccionamos el archivo y escribimos el siguiente comando:

```
./wazuh-agent-4.9.0-1.msi /q WAZUH_MANAGER="10.0.0.2"
``` 

Sustituyendo la ip de ejemplo con la ip de nuestro server. Y por último usamos el comando:

```
NET START Wazuh
```

![alt text](assets/Instalacion Wazuh/image-15.png)

Y si refrescamos en el servidor podemos ver como ya hay un agente activo.

![alt text](assets/Instalacion Wazuh/image-16.png)

# Probando las opciones de control y detección que brinda Wazuh. 

## File integrity monitoring

Es un proceso de seguridad que se utiliza para supervisar la integridad de los archivos de sistemas y aplicaciones.

Para su configuración debemos de especificar en el archivo de configuración de Wazuh (C:/Program Files (x86)/ossec-agent/ossec.conf) los directorios o archivos que el modulo FIM debe de monitorear de la siguiente manera:

```
<syscheck>
 <directories> <FILEPATH_OF_MONITORED_FILE> </directories>
 <directories> <FILEPATH_OF_MONITORED_DIRECTORY> </directories>
</syscheck>
```

Luego se reinicia el agente Wazuh con permisos de administrador:

```
Restart-Service -Name Wazuh
```
![alt text](assets/Instalacion Wazuh/image-17.png)


> [NT.]
> En mi caso yo especifiqué la ruta C:/Users/20240855/Desktop.

![alt text](assets/Instalacion Wazuh/image-18.png)

Entonces, si creo o modifico un archivo en el directorio que le especifiqué me notificaría en el dashboard de Wazuh, por ejemplo:

![alt text](assets/Instalacion Wazuh/image-19.png)

![alt text](assets/Instalacion Wazuh/image-20.png)

## Security Configuration Assessment

SCA realiza análisis para detectar configuraciones incorrectas y exposiciones en los puntos finales monitoreados y recomienda acciones de reparación. 

Para acceder a esta función hacemos clic en la opción “Configuration Assessment”:

![alt text](assets/Instalacion Wazuh/image-21.png)

Hace una serie de pruebas para al final darnos un “score de seguridad”, indicando las medidas de seguridad que pasó el agente y las que falló y en caso de pasar esto último, nos da recomendaciones para poder mejorar la seguridad del sistema en ese aspecto.

![alt text](assets/Instalacion Wazuh/image-22.png)

![alt text](assets/Instalacion Wazuh/image-23.png)

## Vulnerability Detection

![alt text](assets/Instalacion Wazuh/image-24.png)

## System inventory

Contiene información sore los activos registrados como agentes dentro de la infraestructura, utilizando esta opción podemos ver datos como interfaces de red, configuración de red, puertos, procesos, especificaciones del dispositivo, entre otras.

![alt text](assets/Instalacion Wazuh/image-25.png)
![alt text](assets/Instalacion Wazuh/image-26.png)
![alt text](assets/Instalacion Wazuh/image-27.png)
