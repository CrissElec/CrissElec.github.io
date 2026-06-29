---
title: "Lab 04 - Análisis de Emulación del Grupo APT OilRig (APT34)"

date: 2026-06-28 17:00:00 -0500
categories: [Cyberseguridad]
tags: [Tacticas, SMB, MITREATT&CK]
---

# Investigación de TTPs — OilRig (APT34)

## TTP T1592: Reconnaissance
DESCRIPCIÓN: Se recopila información técnica sobre los equipos de la organización objetivo antes del acceso inicial. 

EJECUCIÓN: El atacante emplea técnicas de reconocimiento pasivo y activo, como OSINT, consultas DNS, fingerprinting de sistemas, análisis de encabezados HTTP, escaneo de servicios expuestos y recopilación de información publicada por la organización.

PRODUCTO: Información del host, como sistema operativo, software, direcciones IP y configuraciones, que facilita la planificación del ataque.

## T1583: 	Resource Development

DESCRIPCIÓN: Se obtiene o prepara la infraestructura que será utilizada durante el ataque, como dominios, servidores o servicios en la nube.

EJECUCIÓN: El adversario registra dominios, configura servidores, adquiere direcciones IP o crea cuentas en servicios legítimos para alojar malware o establecer comunicación con la víctima.

PRODUCTO: Infraestructura lista para realizar phishing, alojar herramientas maliciosas y establecer canales de comando y control (C2).

## T1566.002: Initial Access

DESCRIPCIÓN: Utilizada para obtener acceso inicial mediante el envío de un enlace malicioso a una víctima específica. Su objetivo es inducir al usuario a interactuar con el enlace para comprometer el sistema o capturar credenciales.

EJECUCIÓN: El atacante envía un correo electrónico personalizado con un enlace que dirige a un sitio web que descarga malware cuando la víctima hace clic o accede a él.

PRODUCTO: Se obtiene acceso inicial al sistema o credenciales válidas que permiten continuar con las siguientes etapas del ataque.

## T1053.005: Persistence

DESCRIPCIÓN: Es utilizada para mantener la persistencia en el sistema mediante la creación o modificación de tareas programadas que ejecutan procesos de forma automática.

EJECUCIÓN: El atacante modifica tareas programadas del sistema para ejecutar malware o scripts en horarios específicos o durante el inicio del sistema o la sesión del usuario.

PRODUCTO: Mantiene el acceso persistente al sistema y permite ejecutar código malicioso automáticamente sin intervención del usuario.

## T1078.002: Privilege Escalation

DESCRIPCIÓN: Utiliza cuentas de dominio válidas para acceder a recursos de la red y ejecutar acciones con los permisos del usuario víctima.

EJECUCIÓN: El atacante obtiene credenciales válidas mediante phishing (robo de credenciales o fuerza bruta) y las utiliza para iniciar sesión en sistemas o servicios del dominio.

PRODUCTO: Acceso autorizado a recursos del dominio con los privilegios asociados a la cuenta comprometida, facilitando el movimiento dentro de la red y la escalada de privilegios.


## T1068: Privilege Escalation

DESCRIPCIÓN:Aprovecha vulnerabilidades del sistema o software para obtener privilegios más altos.

EJECUCIÓN: El atacante explota una vulnerabilidad local para elevar sus permisos en el sistema.

PRODUCTO: Obtiene privilegios elevados, como acceso de administrador o SYSTEM, lo que le permite controlar el sistema y ejecutar acciones restringidas.

## T1082: 	Discovery

DESCRIPCIÓN: Consiste en adquirir información del sistema vulnerable para conocer sus características y planificar acciones posteriores. Esta información puede incluir el sistema operativo, la versión, el hardware y otros detalles del equipo.

EJECUCIÓN: El atacante ejecuta comandos para obtener información sobre el equipo, como el sistema operativo, la arquitectura, la versión y el hardware instalado.

PRODUCTO: Se obtiene información detallada del sistema que facilita la toma de decisiones y la ejecución de las siguientes etapas del ataque.

## T1033: Discovery

DESCRIPCIÓN: Identifica el propietario o el usuario actual del sistema comprometido.

EJECUCIÓN: Ejecuta comandos o API del sistema (por ejemplo, whoami) para consultar la información del usuario.

PRODUCTO: Obtiene el nombre del usuario, propietario o sesión activa del sistema.

## T1016: Discovery

DESCRIPCIÓN: Descubre la configuración de red del sistema comprometido.

EJECUCIÓN: Ejecuta comandos como ipconfig, ifconfig o ip addr para consultar la configuración de red.

PRODUCTO: Obtiene direcciones IP, interfaces de red, DNS, puertas de enlace y otra información de la red.

## T1087: Discovery

DESCRIPCIÓN:Identifica las cuentas de usuario existentes en el sistema o la red.

EJECUCIÓN: Ejecuta comandos o consultas del sistema para enumerar cuentas de usuario.

PRODUCTO: Obtiene una lista de cuentas de usuario locales o de dominio.

## T1069: Discovery

DESCRIPCIÓN: Identifica los grupos de permisos y privilegios del sistema o dominio.

EJECUCIÓN: Ejecuta comandos o consultas para enumerar los grupos de usuarios y sus miembros.

PRODUCTO: Obtiene información sobre grupos de permisos y usuarios con privilegios.

## T1021.001: Lateral Movement

DESCRIPCIÓN: Utiliza el Protocolo de Escritorio Remoto (RDP) para acceder y moverse entre sistemas.

EJECUCIÓN: Se conecta a un equipo remoto mediante RDP usando credenciales válidas.

PRODUCTO: Obtiene acceso remoto interactivo al sistema objetivo.

## T1005: Collection

DESCRIPCIÓN: Recopila archivos y datos almacenados en el sistema comprometido.

EJECUCIÓN: Busca y copia archivos o información de interés del equipo local.

PRODUCTO: Obtiene datos locales para su posterior uso o exfiltración.

## T1074.001: Collection

DESCRIPCIÓN: Agrupa y prepara los datos recopilados en el sistema local antes de exfiltrarlos.

EJECUCIÓN: Copia o mueve archivos a una ubicación temporal dentro del equipo.

PRODUCTO: Obtiene un conjunto de datos organizado y listo para ser exfiltrado.


## T1071.001: Command and Control

DESCRIPCIÓN: Utiliza protocolos web para comunicarse con el servidor de comando y control (C2).

EJECUCIÓN: Envía y recibe datos mediante HTTP o HTTPS.

PRODUCTO: Establece un canal de comunicación con el servidor C2.

## T1572: Command and Control

DESCRIPCIÓN: Encapsula tráfico en otro protocolo para ocultar las comunicaciones.

EJECUCIÓN: Tuneliza el tráfico de red a través de un protocolo permitido.

PRODUCTO: Mantiene comunicaciones ocultas y evade controles de red.

## T1048.003	Exfiltration 

DESCRIPCIÓN: Exfiltra datos mediante protocolos que no son de comando y control.

EJECUCIÓN: Transfiere información usando protocolos de red distintos al canal C2.

PRODUCTO: Extrae datos del entorno comprometido.
