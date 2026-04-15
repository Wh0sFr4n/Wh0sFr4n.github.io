---
title: Laboratorios Active Directory y de implementación de AAA mediante NPS.
date: 2026-04-15 00:00:01
categories: [Networking] [OS]
tags: [Active Directory]
---

# Laboratorio Active Directory

Link: https://youtu.be/pFX71I7SRXc

En este laboratorio se implementaron las siguientes configuraciones:

1. Servidor con Windows Server 2016 R2, este es el primer domain controller principal, tendrá los
siguientes roles: AD, DNS y DHCP Server. La dirección IP de este servidor es reservada y la
de los demás servidores. El DHCP entregara direcciones IP a los pcs con Windows 10.

3. 2 PCs Windows 10, estas pertenecen al dominio, y reciben las ip por el dhcp server.

4. 5 unidades organizativas, estas pueden son: Dirección de Tecnología, Dirección Administrativa,
Dirección Legal, Dirección de Comunicaciones y Dirección de Gestión Humana. Cada unidad organizativa
debe tiene 2 usuarios.

5. GPOs que restrinjan lo siguiente: a los usuarios de Dirección Administrativa,
Dirección Legal, Dirección de Comunicaciones y Dirección de Gestión Humana: bloqueo del panel de
control,bloqueo del CMD, RUN. Asignan un fondo de pantalla.

6. Se configuró roaming profile para todos los usuarios. Esto consiste en que la carpeta de Documentos este redirigida a una carpeta en el file server. La carpeta debe crearse automáticamente sino existe y solo el usuario dueño de la data puede acceder a dichos archivos. No importa en qué pc haga login el usuario sus archivos tienen que estar en la carpeta Documentos.

7. En el File Server, se crearon 5 carpetas por cada unidad organizativa, los usuarios de cada dirección solo deben tener permisos para acceder a la carpeta que le corresponde, ósea gestión humana no puede ver las carpetas de tecnología, ni de la dirección administrativa, ni de las demás unidades organizativas. Los usuarios de Dirección Administrativa, Dirección Legal y Dirección de Gestión Humana no pueden guardar archivos de audio, video, ejecutables, entre otros, solo podrán guardar archivos de documentos, textos. Los de Dirección de Comunicaciones pueden guardar archivos de documentos, textos, audio y video, losdemás están prohibidos. Los de tecnología no tendrán ninguna restricción en el file server.

8. GPO para todos los usuarios: la hora es provista por el domain controller, la cuenta de los usuarios debe de bloquearse con 3 intentos fallidos, la clave tiene que ser robusta.

9. Se instaló la herramienta de verificación de vulnerabilidades OpenVAS. Esta entrega un informe de todas las vulnerabilidades encontradas en las máquinas virtuales creadas, con su posible solución y/o
recomendación.

10. Se creó una máquina virtual en Google Cloud con sistema operativo Linux, el cuál permite el acceso utilizando un certificado desde uno de los equipos virtuales que hemos
creado en este proyecto.

# Laboratorio de implementación de AAA mediante NPS

Link: https://youtu.be/K3LmiOmvgUs

implementación de AAA mediante NPS para autenticación en equipos Cisco.