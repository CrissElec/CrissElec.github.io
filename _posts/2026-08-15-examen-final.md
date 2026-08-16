---
title: EXAMEN FINAL
date: 2026-08-14 23:00:00 -0500
categories: [Ciberseguridad]
tags: [examen final, github, blog, MITREATT&CK]

---
# Capstone Threat-Informed — Turla / Snake


Item 08: María Cristina Ochante León
APT asignado: Turla / Snake
Técnica asignada: T1505.003 — Web Shell


## ACTO 1 - La inteligencia dirige (OpenCTI del curso)


### Investigamos el perfil de Turla/Snake


Turla es un grupo de ciberespionaje atribuido al Servicio Federal de Seguridad de Rusia (FSB). Según la información registrada en OpenCTI, sus actividades se remontan al menos a 2004 y han afectado a víctimas de más de 50 países, incluyendo organizaciones gubernamentales, embajadas, militares, instituciones educativas y de investigación, así como empresas farmacéuticas. El grupo es conocido por emplear campañas de watering hole y spearphishing, además de herramientas y malware desarrollados internamente, como Uroboros.


![Importación](/assets/imag6/fig1.png)


se observa:


 Intrusion Set (1), ID (2), alias (3) y motivación (4*)


_* Motivación registrada: OpenCTI establece la motivación primaria como Unknown. No obstante, el grupo es descrito como un actor de ciberespionaje, por lo que su actividad observable se encuentra orientada al espionaje y obtención de información._


### 5–8 TTPs clave (Attack Patterns) que usa el grupo.


Se requiere de 5–8 TTPs de Turla/Snake que se encuentren en el OpenCTI.

Para ello en el OpenCTI buscamos dentro Turla / Snake → Attack Patterns (1), accedemos y observamos el conjunto de técnicas relacionadas con el grupo .


![Importación](/assets/imag6/fig2.png)

| # | ID	| Nombre	| Táctica	|
|---|-----|---------|---------|
| 1 |T1505.003 |	Web Shell |	Persistence	|
| 2 |T1078 |	Valid Accounts |	Persistence / Priv. Esc. / Initial Access	|
| 3 |T1027 |	Obfuscated Files or Information |	Defense Evasion |
| 4 |T1036 |	Masquerading |	Defense Evasion |
| 5 |T1055 |	Process Injection |	Priv. Esc. / Defense Evasion |	
| 6 |T1110 |	Brute Force |	Credential Access	|


### IoCs / Indicators asociados y Malware / herramientas del grupo.


Al consultar la sección Observations especificamente en _Indicators (2)_ dentro de la ficha del grupo Turla en OpenCTI, la tabla se encuentra completamente vacía.

En el caso de la sección _Malware (1)_, se identifican 17 familias de malware (Uroboros, TinyTurla, PowerStallion, Penquin, Mosquito, LunarWeb, LunarMail, LunarLoader, LightNeuron y otros) 

Ademas, se registra 13 herramientas asociadas (visibles en la sección Arsenal → Tools del menú lateral).

![Importación](/assets/imag6/fig4.png)


## ACTO 2 — Plan de ataque mapeado a ATT&CK


### Tabla 1 — TTPs con análogo reproducible en Metasploitable3

|Táctica|Técnica ATT&CK | Servicio/objetivo en Metasploitable3|	Herramienta|
|-------|---------------|-------------------------------------|------------|
|Persistence|	T1505.003 – Web Shell|WAMP/WordPress (puerto 80)|	Metasploit: exploits/unix/webapp/wp_admin_shell_upload|
|-------|---------------|-------------------------------------|------------|
|Credential| Access	T1110 – Brute Force|WordPress wp-login.php (puerto 80)|	Metasploit: módulo auxiliar de fuerza bruta HTTP/WordPress|
|-------|---------------|-------------------------------------|------------|
|Defense Evasion|T1027 – Obfuscated Files or Information|Payload PHP subido dentro del plugin falso|Encoders de msfvenom / ofuscación del payload del módulo|
|-------|---------------|-------------------------------------|------------|
|Defense Evasion|T1036 – Masquerading|Plugin malicioso disfrazado de plugin legítimo de WordPress|El propio módulo wp_admin_shell_upload|
|-------|---------------|-------------------------------------|------------|

### Tabla 2 — TTPs descartadas (sin análogo reproducible)


|Táctica|Técnica ATT&CK |Motivo de descarte|
|-------|---------------|-------------------------------------|
|Privilege Escalation / Defense Evasion|T1055 – Process Injection|Se descarta porque mi ataque es contra WordPress mediante HTTP y busca obtener una Web Shell. Process Injection es otro tipo de ataque, donde se introduce código dentro de otro proceso, y eso no corresponde al escenario que me asignaron.|
|-------|---------------|-------------------------------------|
|Initial Access|T1078 – Valid Accounts|Porque no se está empezando con una cuenta válida que ya haya sido comprometida. Si durante la prueba descubriera las credenciales mediante fuerza bruta (T1110), esas credenciales serían el resultado de la prueba y no una cuenta válida obtenida previamente|
|-------|---------------|-------------------------------------|

## Acto 3 — Ejecución

### Verificación del servicio objetivo

Se verificó la disponibilidad del servicio objetivo desde Kali Linux.

![Importación](/assets/imag6/fig5.png)

El resultado fue satisfactorio, con 0 % de pérdida de paquetes, confirmando que Kali puede comunicarse con el host objetivo.

Luego se verificó específicamente el puerto asignado:

![Importación](/assets/imag6/fig6.png)

El resultado muestra que el host 10.0.2.15 se encuentra disponible, pero el puerto TCP/80 aparece actualmente en estado closed. Por esta razón, la ejecución del módulo de Metasploit asignado se mantiene pendiente hasta verificar la disponibilidad del servicio WordPress/WAMP requerido por el escenario.

### Verificación de WordPress/WAMP

Sabemos:

       Objetivo: 10.0.2.15
       Técnica: T1505.003 — Web Shell
       Servicio asignado: WordPress/WAMP
       Puerto asignado: TCP/80
       TCP/80: actualmente cerrado


![Importación](/assets/imag6/fig7.png)

Se verificó la existencia local del módulo wp_admin_shell_upload.rb, correspondiente al archivo indicado en la asignación del examen.

### Revisar el código del módulo

![Importación](/assets/imag6/fig8.png)


Se revisó el código fuente del módulo para comprender su funcionamiento. El módulo automatiza las operaciones necesarias para interactuar con WordPress y realizar la carga del archivo que permitirá establecer la Web Shell. Esta revisión permite relacionar directamente la herramienta utilizada con la técnica T1505.003.


### Abrir Metasploit


Se cargó en Metasploit el módulo exploit/unix/webapp/wp_admin_shell_upload, correspondiente al archivo asignado. Posteriormente se consultaron sus opciones para identificar los parámetros necesarios para realizar la prueba contra el WordPress objetivo.

![Importación](/assets/imag6/fig9.png)

![Importación](/assets/imag6/fig10.png)


CONFIGURAR LO QUE YA SE CONOCE:

![Importación](/assets/imag6/fig11.png)

Se configuraron la dirección del objetivo (RHOSTS=10.0.2.15) y el puerto establecido para el servicio (RPORT=80). La consulta de las opciones del módulo permitió identificar que la explotación requiere además la ruta de WordPress y credenciales válidas para autenticarse en la aplicación.


### ACTO 4 - Acto 4 — Defensa

en este Acto 4, gira alrededor de detectar y evitar una Web Shell.

| Técnica ejecutada         | ¿Cómo la detectarías?                                                                                              | Mitigación                                                                                                                                                                           |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **T1505.003 — Web Shell** | Revisaría **logs del servidor web**, creación/modificación de archivos PHP y **creación de procesos** sospechosos. | **M1042 — Disable or Remove Feature or Program** y endurecimiento del servidor web: eliminar/deshabilitar funcionalidades innecesarias y restringir la subida/ejecución de archivos. |



### Evidencia defensiva 1

Visor de eventos (eventvwr):


Se revisó Windows Logs → Security para identificar _eventos de seguridad_ que puedan utilizarse para investigar actividad sospechosa y correlacionarla con una posible ejecución posterior a una Web Shell.


![Importación](/assets/imag6/fig12.png)


### Evidencia defensiva 2


Windows Firewall (firewall.cpl):

Se observa la configuración del firewall como medida de postura de seguridad destinada a reducir la superficie de exposición y controlar comunicaciones no autorizadas.

![Importación](/assets/imag6/fig13.png)

