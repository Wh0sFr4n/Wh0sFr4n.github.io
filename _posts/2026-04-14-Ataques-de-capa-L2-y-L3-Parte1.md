---
title: Ataques de capa L2 y L3 Parte 1
date: 2026-04-14 00:00:00
categories: [Networking]
tags: [Networking]
---

# Resumen

Este post documenta la realización de los ataques ARP Spoofing y CDP Flooding dentro de un laboratorio práctico controlado en gns3, con el con el objetivo de conocer y comprender vulnerabilidades que pueden afectar redes mal configuradas, y como las mismas pueden ser explotadas y medidas para poder mitigarlas.

# Objetivo

El objetivo de la realización de los ataques a continuación es conocer ataques y amenazas a las que se puede enfrentan una red empresarial si no se configura con la debida seguridad, así como aprender técnicas o métodos en los que podemos mitigar dichos ataques.

# Topologia 

![Image](\assets\Images_L2_L3_part1\image-1.png)


![alt text](assets\Images_L2_L3_part1\image-2.png)

# Herramientas y Parámetros utilizados

Para llevar a cabo los ataques fueron utilizadas las herramientas Bettercap y Yersinia para ARP
Spoofing y CDP Flooding respectivamente. Adicionalmente se utilizó wireshark para sniffear
tráfico de la red:

## Bettercap

Bettercap es una herramienta muy utilizada que permite a testers de penetración y hackers éticos
llevar a cabo varios tipos de ataques MitM contra una red y manipular tráfico en tiempo real. En
está práctica fue utilizada para realizar un ataque de ARP Spoofing utilizando los siguientes
parámetros:

· Net.probe on -> Para detectar dispositivos en la red.
· Ticker on -> Organizar la información sobre los dispositivos de la red de una manera más
legible.
· Set arp.spoof.targets -> Especificar una victima.
· arp.spoof on -> Iniciar el ataque

## Yersinia

Yersinia es un framework diseñado para aprovecharse de debilidades en protocolos de red para
realizar ataques de capa 2. En esta práctica fue utilizado para aprocharse del protocolo CDP
(Cisco Discovery Protocol) para realizar un ataque de denegación de servicio utilizando los
siguientes parámetros:

· G -> cdp – Para elegir el protocolo.
· X -> Para listar los ataques de ese protocolo.
· 1 -> Para iniciar el ataque cdp flooding.

## Wireshark

Wireshark es un "sniffer" de red que permite capturar y examinar tráfico.

# Demostración Técnica

## MitM mediante ARP SPOOFING

El protocolo ARP (Address Resolution Protocol) es un protocolo que se utiliza para resolver
direcciones IPv4 a direcciones MAC en una red local, donde el dispositivo que desea
comunicarse envía un broadcast preguntando quién es el dueño de la IP deseada, siendo este el
que responde con su dirección MAC. Debido a la falta de autenticación un atacante puede
enviara respuestas ARP falsas para suplantar al equipo veridico para así poder interceptar,
modificar y redirigir el tráfico.

En esta demostración se utilizaron dos dispositivos finales (un atacante y una victima). Primero,
se verificó la IP de la máquina victima y se utilizó el comando traceroute para identificar la ruta
que seguirían los paquetes dirigidos un destino externo (8.8.8.8):

![alt text](assets\Images_L2_L3_part1\image-3.png)

![alt text](assets\Images_L2_L3_part1\image-4.png)

Luego, se verificó la IP de la máquina atacante, comprobando que se encuentra en mismo rango
que la victima:

![alt text](assets\Images_L2_L3_part1\image-5.png)

Desde la máquina atacante se inició la herramienta bettercap con permisos privilegiados y luego
se utilizaron los comandos “net.probe on” y “ticker on” para identificar dispositivos activos en la
red y organizar la información recolectada respectivamente:

![alt text](assets\Images_L2_L3_part1\image-6.png)

![alt text](assets\Images_L2_L3_part1\image-7.png)

![alt text](assets\Images_L2_L3_part1\image-8.png)

pues se utilizó “set arp.spoof.targets 10.8.55.4” para indicar que la .4 sería nuestra victima y
“arp.spoof on” para iniciar el ataque:

![alt text](assets\Images_L2_L3_part1\image-9.png)

Una vez iniciado el ataque, utilizando wireshark se pudo identificar el tráfico ARP enviado desde
la máquina atacante reclamando la IP del router de la red y pasando su MAC address.

![alt text](assets\Images_L2_L3_part1\image-10.png)

Por último, se pudo confirmar que el ataque se realizó exitosamente al volver a realizar un
traceroute, esta vez con la diferencia de que antes de llegar al router, el tráfico de la victima pasa
por nuestra máquina atacante:

![alt text](assets\Images_L2_L3_part1\image-11.png)

## DoS mediante CDP Flooding

CDP es un protocolo de red propietario de cisco que permite a dispositivos de ese vendor
descubir y anunciar información a otros vecinos directos a través de paquetes periódicos, el
problema recide es que un atacante puede inundar a estos dispositivos con mensajes CDP
generando un consumo excesivo de CPU/memoria causando así denegación de servicios.

Para realizar este ataque, desde la máquina atacante se inició la heramienta yersinia desde el
modo interactivo:

![alt text](assets\Images_L2_L3_part1\image-12.png)

Luego, se presiona la letra G para elegir el protocolo, en este caso se seleccionó CDP:

![alt text](assets\Images_L2_L3_part1\image-13.png)

Se presiona la letra X para entrar al panel de ataque y luego la tecla 1 para iniciar el ataque CDP
flooding:

![alt text](assets\Images_L2_L3_part1\image-14.png)

Despues de iniciar el ataque, si abrimos wireshark podemos ver el tráfico CDP enviado:

![alt text](image-15.png)

Por último, al acceder al switch de la red podemos ver que nos muestra el siguiente mensaje, que
nos indica un uso intensivo de la CPU y por mas que intentamos acceder a la terminal el
dispositivo simplemente no responde.

![alt text](assets\Images_L2_L3_part1\image-16.png)

# Medidas de mitigación

## ARP Spoofing

Para evitar este ataque podemos segmentar la red en diferentes vlans para así reducir el radio de
impacto, utilizar port security para limitar las MACs por puerto y habilitar Dynamic ARP
Inspection en switches para filtrar y bloquear mensajes ARP falsos.

## CDP Flooding

Para evitar el CDP Flooding podemos configurar port security para evitar que los host generen
una cantidad exagerada de paquetes, de la misma forma podemos limitar el tráfico broadcast y
multicast en puertos de acceso y desactivar este protocolo donde no sea necesario.