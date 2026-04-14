---
title: Ataques de capa L2 y L3 Parte 2
date: 2026-04-14 00:00:00
categories: [Networking]
tags: [Networking]
---

# Resumen

En este post documenta la realización de los ataques DHCP Rogue server, Starvation y
STP Root Claim dentro de un laboratorio controlado de GNS3, con el objetivo de conocer y
comprender vulnerabilidades que pueden afectar redes mal configuradas, como estas
vulnerabilidades pueden ser explotadas y medidas para poder mitigarlas.

# Objetivo

El objetivo de este laboratorio es,como mencioné en la parte anterior, conocer ataques y amenazas a las que se puede enfrentan una red empresarial si no se configura con la debida seguridad, así como aprender técnicas o métodos en los que podemos mitigar dichos ataques.

# Topología

![alt text](assets\Images_L2_L3_part2\image-1.png)

![alt text](assets\Images_L2_L3_part2\image-2.png)

# Herramientas y parámetros utilizados

Para llevar a cabo los ataques realizados durante esta práctica se utilizaron las herramientas
Yersinia para DHCP starvation y STP claim root y se utilizó ettercap para el ataque Rogue
DHCP:

## Yersinia

Yersinia es un framework diseñado para aprovecharse de debilidades en protocolos de red para
realizar ataques de capa 2. En esta práctica fue utilizado para realizar los ataques DHCP
Starvation y STP claim root utilizando los siguientes parámetros:

· G -> Para elegir el protocolo.
· X -> Para listar los ataques de ese protocolo.
· 1-5 -> Para elegir alguno de los ataques.

## Ettercap

Es una suite de ataques en LANs que incluye rastreo de conexiones en vivo, filtrado de
contenido sobre la marcha y muchas otras funciones. En está práctica fue utilizada para llevar a
cabo el ataque DHCP rogue server utilizando el apartado de DHCP Spoofing y añadiendo el pool
de IPs a proveer. 

# Desmotración

## DHCP Starvation Attack

El ataque consiste en que nosotros como atacantes inundamos el servidor DHCP con solicitudes DHCP para así agotar todas las direcciones disponibles en el pool de DHCP, denegando al servicio a usuarios legítimos de la red.

Desde nuestra máquina atacante podemos comprobar que estamos dentro del rango de la
red:

![alt text](assets\Images_L2_L3_part2\image-3.png)

Iniciamos Yersinia:

![alt text](assets\Images_L2_L3_part2\image-4.png)

Elegimos dhcp y ejecutamos el ataque:

![alt text](assets\Images_L2_L3_part2\image-5.png)

![alt text](assets\Images_L2_L3_part2\image-6.png)

![alt text](assets\Images_L2_L3_part2\image-7.png)

![alt text](assets\Images_L2_L3_part2\image-8.png)

Resultado del ataque:

![alt text](assets\Images_L2_L3_part2\image-9.png)

## Rogue DHCP Attack

Este ataque consiste en configurar un servidor DHCP falso en una red para asignar direcciones IP y configuraciones maliciosas a los clientes. permitiéndonos redirigir el tráfico, realizar ataques Man-in-the-Middle o interrumpir la conectividad de la red legítima.

Dentro de Ettercap seleccionamos esta opción y configuramos la información del server:

![alt text](assets\Images_L2_L3_part2\image-10.png)

![alt text](assets\Images_L2_L3_part2\image-11.png)

Luego de iniciar el ataque, si intentamos añadir un equipo a la red, este tomará una IP de
nuestro dhcp server en lugar del legitimo:

![alt text](assets\Images_L2_L3_part2\image-12.png)

## STP Claim Root Attack

Consiste en enviar BPDUs falsificadas con prioridad baja para que nuestro equipo sea elegido como el root bridge. Para este ataque volveremos a utilizar Yersinia pero esta vez utilizando el protocolo stp:

Antes de realizar el ataque, al ejecutar el comando show spanning-tree podemos confirmas que este el root actual:

![alt text](assets\Images_L2_L3_part2\image-13.png)

En la máquina atacante elegimos la opción 4 para iniciar el ataque:

![alt text](assets\Images_L2_L3_part2\image-14.png)

Y luego de un momento si volvemos a confirmar en el sw, veremos que ya este no es el root
bridge indicando que el ataque se realizó exitosamente:

![alt text](assets\Images_L2_L3_part2\image-15.png)

# Medidas de mitigación

## DHCP Rogue server y Starvation Attacks

En el caso de estos ataques podemos implementar segmentación de vlans, port security
para limitar el número de MACs por puerto y habilitar DHCP Snooping para filtrar y validar
el tráfico DHCP.

## STP Root Claim Attack

Para mitigar este ataque podemos configurar root guard en los puertos donde no debería
haber un root bridge, BPDU Guard para desactivar puertos que pertenecen a hosts si
aparece una BPDU en alguno de ellos y directamente configurar la prioridad de los
switches. 