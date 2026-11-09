# Notas de trabajo – Adquisición de un equipo comprometido

**Alumno:** Raúl Toledo Procopio
**Actividad:** Adquirir un equipo comprometido
**Fecha:** 08/11/2026
**Máquina virtual:** FORENSIC_10
**Sistema operativo:** Windows 7 de 64 bits

---

# 1. Colocación de la marca del investigador

**Hora de inicio:** [10:57]
**Hora de finalización:** [11:02]

**Marca utilizada:** `AFI-RaulToledo-20261108`

### Procedimiento

1. Abrí el Bloc de notas.
2. Escribí la marca del investigador y dejé el documento abierto sin guardar.
3. Creé en el escritorio el archivo `marca-raultoledo.txt`.
4. Introduje en él la misma marca.
5. Copié el archivo al USB encontrado.

### Evidencias

- [Captura del Bloc de notas con la marca](capturas/Marca1.png)
- [Captura del archivo de marca en el escritorio](capturas/Marca2.png)

---

# 2. Adquisición en vivo

## 2.1 Documentación del estado inicial

**Hora de inicio:** [11:04]
**Hora de finalización:** [11:04]

### Procedimiento

1. Documenté el estado inicial del equipo.
2. Registré la fecha y hora que mostraba Windows.
3. Consulté la hora real.
4. Calculé la diferencia horaria entre ambas.
5. Realicé una captura del estado inicial.

### Resultado y observaciones

- Fecha y hora del sistema: 08/11/2026, 11:04.
- Hora real consultada: 08/11/2026, 11:04.
- Diferencia horaria observada: ninguna.

### Evidencias

- [Captura del estado inicial](capturas/Inicial.png)
- [Captura de la hora del sistema](capturas/Hora.png)

## 2.2 Adquisición de la memoria RAM

**Hora de inicio:** [11:05]
**Hora de finalización:** [12:12]

La hora exacta de adquisición no se puede fijar con las evidencias disponibles.

### Herramienta utilizada

- WinPmem mini x64 RC2: `winpmem_mini_x64_rc2.exe` (527.640 bytes).
- SHA-256 del ejecutable utilizado: `A4D516B6FCAF3B5B1D4EE709CE86F8EABF1D8028B3A83101479B7568B933D21B`.
- La versión utilizada se identifica por el nombre del ejecutable como WinPmem mini x64 RC2.

### Motivo de elección

Se utilizó para adquirir en vivo la memoria física del sistema Windows 7 de 64 bits. La edición mini genera una imagen RAW; la captura documenta su ejecución correcta en FORENSIC_10 y la creación de `memoria.raw`.

### Procedimiento

1. Ejecuté desde el USB de respuesta `D:\Windows\memoria\winpmem_mini_x64_rc2.exe`, con privilegios de administrador.
2. Indiqué como destino `D:\Salida\memoria.raw`.
3. Esperé a que WinPmem terminara la adquisición y descargara el controlador.
4. Comprobé la existencia y el tamaño del volcado en PowerShell.
5. Calculé el SHA-256 de `memoria.raw` con `Get-FileHash`.

### Resultado y observaciones

- Archivo generado: `memoria.raw`
- Ubicación en el equipo adquirido: `D:\Salida\memoria.raw`
- Tamaño: 1.073.676.288 bytes
- Algoritmo de hash: SHA-256
- SHA-256: `39ded414bf9664489c100c796bc717ca13d2724e45dd016ebc2c0fd0dc0fad3f`
- Incidencias: la adquisición finalizó y se generó el archivo. Hay que conciliar las horas visibles en las capturas: la consola indica 10:11:25–10:16:48, la barra de tareas muestra aproximadamente 11:20–11:21, el archivo figura modificado a las 11:16 y la captura del hash está a las 12:11. El intervalo anotado arriba (11:05–12:12) abarca estas actividades, pero no permite confirmar por sí solo la hora exacta de adquisición.

### Evidencias

- [Captura de ejecución de WinPmem](capturas/winpmem.png)
- [Captura del volcado generado](capturas/winpmem2.png)
- [Captura del cálculo del hash](capturas/winpmem3.png)
- [Registro del hash de RAM](hashes/memoria-sha256.txt)

## 2.3 Recogida de datos volátiles

**Hora de inicio:** [12:45:16]
**Hora de finalización:** [12:45:26] (hora local UTC+01:00).

### Herramienta utilizada

- `respuesta-en-vivo2.ps1`, adaptación compatible con PowerShell 2.0.

### Motivo de elección

Se utilizó el script de respuesta en vivo para recopilar y registrar información volátil del sistema sin instalar herramientas adicionales en FORENSIC_10.

### Procedimiento

1. Intenté ejecutar el script original desde el USB de respuesta; la política de ejecución bloqueó su primera ejecución.
2. Tras permitir la ejecución, el script original falló por incompatibilidad con PowerShell 2.0.
3. Adapté el script y lo guardé como `respuesta-en-vivo2.ps1`.
4. Ejecuté la versión adaptada como administrador, con la salida en `D:\Salida\UD1_FORENSIC_10_20261108-114516`.
5. Comprobé los ficheros generados y sus hashes SHA-256.

### Información recopilada

- Procesos y procesos asociados a conexiones de red.
- Conexiones, puertos, configuración, rutas y cachés ARP/DNS.
- Sesiones, usuarios locales y grupo de administradores.
- Servicios, tareas programadas y elementos de inicio automático.
- Discos, volúmenes, dispositivos USB conectados y recursos compartidos.
- Fecha/hora, estado de arranque y sincronización.
- El portapapeles no se pudo recopilar: `Get-Clipboard` no está disponible en PowerShell 2.0.

### Resultado y observaciones

El registro `00-registro.txt` consigna la ejecución entre 12:45:16 y 12:45:26, hora local (UTC+01:00), y recoge el equipo como `FORENSE-06`, mientras que el nombre de la máquina virtual es `FORENSIC_10`. Se generaron 19 ficheros de resultados y `hashes-sha256.csv`; el inventario general de `D:\Salida` registra 283 archivos, 107 directorios y 1.280.290.051 bytes. Los hashes individuales están en `hashes-sha256.csv`. El registro también tiene SHA-256 `47d27908bc62a91476581f495f7a6f701deecbd880c7b803b8ccc2a0b1aa8129`, recogido en el manifiesto general.

### Incidencias

La política de ejecución bloqueó el primer intento. El script original no era compatible con PowerShell 2.0 y se adaptó. El portapapeles no se recopiló por la limitación indicada. El registro refleja UTC+01:00; no se conserva una comparación independiente de la hora del sistema con una fuente horaria externa.

### Evidencias

- [Captura de los resultados obtenidos](capturas/respuesta.png)

## 2.4 Captura del tráfico de red

**Hora de inicio:** [12:54]
**Hora de finalización:** [12:58]

### Herramienta utilizada

- VirtualBox (`VBoxManage`)

### Motivo de elección

Se utilizó la captura de red de VirtualBox para registrar tráfico de la máquina virtual desde el host, sin instalar software de captura en el sistema investigado.

### Procedimiento

1. Configuré la captura de tráfico de la interfaz de red de FORENSIC_10 desde VirtualBox.
2. Inicié la captura mientras la máquina virtual estaba en funcionamiento.
3. Detuve la captura y comprobé el archivo PCAP generado.

### Comandos utilizados

VBoxManage controlvm "FORENSIC_10" nictracefile1 "C:\Users\Raúl\Desktop\FORENSIC10-red.pcap"
VBoxManage controlvm "FORENSIC_10" nictrace1 on
VBoxManage controlvm "FORENSIC_10" nictrace1 off

### Resultado y observaciones

- Archivo: `Adquisición/FORENSIC_10-red.pcap`.
- Tamaño: 78.196 bytes.
- SHA-256: `885a477b09976ad8dee434720ceffabfaa91fc5fdbe408096d4e76da0ea3c329` (registrado en `hashes/hashes-raiz-sha256.txt`).

---

## 2.5 Adquisición del triaje

**Hora de inicio:** [14:50]
**Hora de finalización:** [15:31]

### Herramienta utilizada

- Herramienta: RawCopy.exe y utilidades nativas de Windows 7.
- Versión de RawCopy: 1.0.0.22 (versión indicada en la documentación de referencia; no verificada directamente en el ejecutable utilizado).
- Configuración o colección utilizada: adquisición manual de artefactos forenses mediante RawCopy y comandos nativos de Windows.
- Destino de los resultados: D:\Salida, ubicado en el USB de respuesta.

### Motivo de elección

Se eligió RawCopy como herramienta principal para obtener la tabla maestra de archivos NTFS ($MFT) de la máquina investigada mientras permanecía encendida.

Inicialmente se intentó utilizar KAPE, pero presentó un problema de compatibilidad relacionado con el protocolo de seguridad. También se intentó utilizar Velociraptor en su versión legacy, pero no pudo ejecutarse debido a la ausencia de la biblioteca api-ms-win-crt-stdio-1-1-0.dll. Se decidió no instalar componentes adicionales para evitar modificar el sistema investigado.

Por este motivo, se utilizó RawCopy para obtener la $MFT y se complementó la adquisición con comandos nativos de Windows 7 para recopilar información del sistema, registros de eventos, configuración de red y otros artefactos relevantes.

Esta alternativa permitió realizar un triaje selectivo sin efectuar una imagen completa del disco ni instalar componentes adicionales en la máquina investigada.

### Procedimiento

1. Preparé RawCopy.exe en el USB de respuesta, dentro de D:\Windows\triaje\RawCopy.
2. Utilicé D:\Salida como destino de los resultados, evitando guardarlos en el disco de la máquina investigada.
3. Abrí CMD como administrador y ejecuté RawCopy para obtener la tabla maestra de archivos NTFS.
4. Guardé las cinco colmenas principales del Registro de Windows.
5. Exporté los registros de eventos del sistema, aplicaciones, seguridad e instalación.
6. Recopilé artefactos relacionados con la ejecución de programas, tareas programadas, accesos directos, archivos temporales y elementos de inicio.
7. Recopilé información sobre procesos, servicios, usuarios, grupos locales, recursos compartidos y configuración de red.
8. Generé un inventario de los archivos recopilados y calculé el hash SHA-256 de la $MFT.
9. Comprobé que los resultados se encontraban en la carpeta de salida del USB.
10. Comprobé la presencia de mi marca en los resultados del triaje. Se localizó el acceso directo marca-raultoledo.lnk dentro de D:\Salida\Recent y el fichero original marca-raultoledo.txt en el escritorio, con un tamaño de 23 bytes. La búsqueda inicial con findstr se limitó a los archivos TXT y no encontró la cadena. Después, al buscar directamente en los artefactos binarios, encontré el nombre marca-raultoledo.txt y la cadena AFI-RaulToledo-20261108 dentro de la $MFT. Esto confirma la presencia de la marca en la memoria y en los metadatos del triaje; no implica que el contenido del fichero de marca se haya extraído como un archivo independiente del triaje.

### Configuración y comandos utilizados

Obtención de la tabla maestra de archivos NTFS:

D:\Windows\triaje\RawCopy\RawCopy.exe /FileNamePath:C:0 /OutputPath:D:\Salida

La operación tardó aproximadamente 25,42 segundos.

Exportación de las colmenas del Registro:

reg save HKLM\SYSTEM D:\Salida\SYSTEM.hiv /y
reg save HKLM\SOFTWARE D:\Salida\SOFTWARE.hiv /y
reg save HKLM\SAM D:\Salida\SAM.hiv /y
reg save HKLM\SECURITY D:\Salida\SECURITY.hiv /y
reg save HKCU D:\Salida\HKCU.hiv /y

Exportación de registros de eventos:

wevtutil epl System D:\Salida\System.evtx
wevtutil epl Application D:\Salida\Application.evtx
wevtutil epl Security D:\Salida\Security.evtx
wevtutil epl Setup D:\Salida\Setup.evtx

Recopilación de artefactos del sistema:

xcopy C:\Windows\Prefetch D:\Salida\Prefetch\ /E /I /H /K /Y
xcopy C:\Windows\System32\Tasks D:\Salida\Tasks\ /E /I /H /K /Y
xcopy "C:\ProgramData\Microsoft\Windows\Start Menu" "D:\Salida\StartMenu\" /E /I /H /K /Y
xcopy "C:\Users\Administrador\AppData\Roaming\Microsoft\Windows\Recent\*" "D:\Salida\Recent\" /E /I /H
xcopy C:\Windows\Temp D:\Salida\Temp\ /E /I /H /K /Y
copy C:\Windows\System32\drivers\etc\hosts D:\Salida\hosts
xcopy "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup" "D:\Salida\Startup\" /E /I /H /K /Y
xcopy "%USERPROFILE%\AppData\Local\Temp" "D:\Salida\Temp_Usuario\" /E /I /H /K /Y

Recopilación de información del sistema y de red:

ipconfig /all > D:\Salida\ipconfig-all.txt
netstat -ano > D:\Salida\netstat-ano.txt
route print > D:\Salida\rutas-red.txt
ipconfig /displaydns > D:\Salida\cache-dns.txt
tasklist /v > D:\Salida\procesos.txt
sc query type= service state= all > D:\Salida\servicios.txt
net user > D:\Salida\usuarios-locales.txt
net localgroup > D:\Salida\grupos-locales.txt
net share > D:\Salida\recursos-compartidos.txt

Inventario y verificación de integridad:

dir D:\Salida /s /a > D:\Salida\inventario-triaje.txt
certutil -hashfile D:\Salida\$MFT SHA256 > D:\Salida\MFT-sha256.txt

Comprobaciones de la marca:

dir D:\Salida /s /b | findstr /i "marca-raultoledo"
dir "%USERPROFILE%\Desktop\marca-raultoledo.txt"
findstr /s /i /m "AFI-RaulToledo-20261108" D:\Salida\*.txt

### Resultado y observaciones

- Herramienta utilizada: RawCopy.exe y utilidades nativas de Windows 7.
- Carpeta de salida: D:\Salida, ubicada en el USB de respuesta.
- Recuento inicial: 283 archivos y 107 directorios.
- Tamaño total en ese recuento: 1.280.290.051 bytes (aproximadamente 1,19 GiB).
- Archivo $MFT obtenido: sí.
- Tamaño de $MFT: 69.206.016 bytes.
- Hash SHA-256 de $MFT: `2f162bac298efadd414a969638bd1e764e734fc6a8ea804a28e3869169fff852`, almacenado en `D:\Salida\MFT-sha256.txt`.
- Inventario: almacenado en D:\Salida\inventario-triaje.txt.
- Marca: se localizó `marca-raultoledo.lnk` en `D:\Salida\Recent`. La búsqueda inicial limitada a archivos TXT no encontró la cadena. La búsqueda posterior en `$MFT` localizó el nombre `marca-raultoledo.txt` y la cadena `AFI-RaulToledo-20261108`. El archivo original de 23 bytes se comprobó en el escritorio, pero no se extrajo como fichero independiente del triaje.

### Incidencias

- KAPE no pudo ejecutarse por un problema de compatibilidad con el protocolo de seguridad.
- Velociraptor no pudo ejecutarse por la ausencia de una biblioteca del sistema. Se decidió no instalarla.
- Durante la copia de los accesos recientes se produjo un error de acceso al utilizar inicialmente la ruta del perfil. Se identificó la ruta real del usuario Administrador y se repitió la copia correctamente.
- La adquisición se completó utilizando RawCopy y herramientas nativas, sin instalar componentes adicionales.

### Evidencias

- [Captura de la herramienta RawCopy y su ubicación](capturas/rawcopy.png)
- [Captura de la ejecución de RawCopy y obtención de $MFT](capturas/rawcopy2.png)
- [Captura de la comprobación de la marca en el triaje](capturas/marca4.png)

## 2.6 Comprobación del cifrado

**Hora de inicio:** [15:55]
**Hora de finalización:** [15:55]

### Herramienta utilizada

- Encrypted Disk Detector (EDD) v3.1.0.
- Desarrollador: Magnet Forensics Inc.

### Motivo de elección

Se utiliza esta herramienta para comprobar si existen discos o volúmenes cifrados en el sistema antes de proceder al apagado. Esta comprobación se realiza mientras el equipo permanece encendido, ya que algunos volúmenes cifrados pueden encontrarse desbloqueados y permitir el acceso a sus datos.

### Procedimiento

1. Ejecuté Encrypted Disk Detector v3.1.0 con permisos de administrador.
2. La herramienta examinó los discos físicos conectados al sistema.
3. Comprobó los volúmenes lógicos y realizó una comprobación secundaria de BitLocker.
4. Revisó los procesos en ejecución para detectar herramientas relacionadas con el cifrado.
5. Registré el resultado obtenido.

### Resultado y observaciones

- Discos físicos detectados:
  - PhysicalDrive0: VBOX HARDDISK, 34 GB.
  - PhysicalDrive1: Kingston DataTraveler 70 USB, 62 GB.
- Volúmenes lógicos detectados:
  - C: NTFS, 34 GB.
  - D: NTFS, 62 GB.
- Volúmenes cifrados detectados: No.
- Comprobación secundaria de BitLocker: Realizada.
- Tecnologías comprobadas: TrueCrypt, PGP, BitLocker, VeraCrypt, SafeBoot, BestCrypt, Check Point, Sophos y Symantec.
- Resultado final: No se detectaron volúmenes cifrados compatibles con EDD.

### Justificación de la decisión

Al no detectar volúmenes cifrados mediante las comprobaciones de EDD, no fue necesario realizar ninguna actuación adicional para preservar el acceso a volúmenes desbloqueados.

### Incidencias

Ninguna. La herramienta completó las comprobaciones correctamente.

### Evidencias

- [Captura del resultado obtenido](capturas/cifrado.png)

---

## 2.7 Apagado del equipo

**Hora de inicio:** [16:07]
**Hora de finalización:** [16:07]

### Motivo de elección

Una vez finalizadas las adquisiciones en vivo y comprobado el estado del cifrado, se procede al apagado de la máquina virtual para continuar con la adquisición post-mortem.

Se elige un apagado normal para permitir que Windows cierre correctamente sus procesos y evitar un corte brusco de alimentación que pudiera provocar daños o inconsistencias en el sistema de archivos.

### Procedimiento

1. Comprobé que habían finalizado todas las adquisiciones en vivo.
2. Verifiqué que los archivos generados se encontraban en el USB de respuesta.
3. Comprobé que la captura del tráfico de red estaba detenida.
4. Documenté el método de apagado elegido.
5. Ejecuté el comando de apagado desde CMD con permisos de administrador.
6. Comprobé que la máquina virtual se encontraba completamente apagada.

### Método de apagado utilizado

Apagado normal desde Windows mediante el comando shutdown.

### Comando utilizado

shutdown /s /t 0

### Justificación de la decisión

Se eligió el apagado normal porque las adquisiciones en vivo ya habían finalizado y no era necesario mantener el sistema encendido. Este método permite cerrar Windows de forma controlada y continuar posteriormente con la adquisición post-mortem del disco virtual.

### Evidencias

- [Captura del comando](capturas/apagado.png)
- [Captura del estado final de la máquina virtual](capturas/apagado2.png)

---

# 3. Adquisición post-mortem

## 3.1 Adquisición del disco de la máquina virtual: imagen definitiva

Fue necesario realizar una segunda adquisición, este apartado documenta únicamente la imagen definitiva, de ahí el desfase horario con los siguientes pasos.

**Hora de inicio:** [20:35:31]
**Hora de finalización:** [20:38:03]

### Documentación de la máquina

Con la máquina virtual completamente apagada, localicé su carpeta de almacenamiento en VirtualBox y documenté los archivos asociados.

- Máquina virtual: FORENSIC_10, apagada durante la adquisición.
- Sistema invitado: Windows 7 de 64 bits.
- Disco virtual de origen: `FORENSIC_10_disk0.vdi`, en `C:\Users\Raúl\VirtualBox VMs\FORENSIC_10`.
- Tamaño del fichero VDI: 13.358.858.240 bytes.
- Capacidad de origen indicada por FTK Imager: 32.768 MB (67.108.864 sectores).
- Formato de la copia: E01, dividida en cuatro segmentos.

### Herramienta utilizada

- Exterro FTK Imager 8.3.0.27.

### Motivo de elección

FTK Imager permite adquirir el disco virtual apagado en formato E01 y verificar la imagen al terminar. Se trabajó con la máquina virtual apagada para evitar escrituras del sistema invitado durante la adquisición.

### Procedimiento

1. Apagué FORENSIC_10 y seleccioné `FORENSIC_10_disk0.vdi` como origen en FTK Imager.
2. Creé la imagen definitiva con el nombre `DISCO.E01`, en formato E01.
3. Activé la verificación de la imagen tras su creación.
4. Comprobé que la adquisición y la verificación finalizaron correctamente.
5. Conservé la imagen y el informe de FTK en `Adquisición/Imagen2/`.

### Resultado y observaciones

- Imagen definitiva: `Adquisición/Imagen2/DISCO.E01`–`DISCO.E04`.
- Tamaños de los segmentos: E01, 1.572.759.180 bytes; E02, 1.572.786.435 bytes; E03, 1.572.792.018 bytes; E04, 464.183.418 bytes.
- Tamaño total: 5.182.523.097 bytes.
- SHA-256 por segmento:

  - `DISCO.E01`: `314B99B57F70EAC829E4F8530DA5FF54EA52F0D645006261E8F2F01B527639D2`
  - `DISCO.E02`: `482C3CE9ABD7F571526CC8124CD73B764C50C5A2E13760765B0E0F9CCAE48E46`
  - `DISCO.E03`: `FFF494DF6D44FC6078DC4CDFD62C83C62E7C162F206FD9C80DFF798FA7871700`
  - `DISCO.E04`: `9339E4AB11FD5D5D6C8A8B4C02A9FCADB8CE149C2E9D24B85D3B6B33DBFC44D3`
- La verificación de FTK fue correcta: MD5 `2b780c8459f160fd8a94a7516f5ddf44` y SHA-1 `e3a4429a09ea8f22dc703559a891c4207869104a` coinciden con los hashes almacenados en la imagen y en el informe.
- El manifiesto de SHA-256 de los segmentos está también en `hashes/hashes-imagenes-sha256.txt`.
- Se calcularon los SHA-256 del VDI, la configuración `.vbox` y los registros de VirtualBox incluidos en `Adquisición/Imagen 2.1`; los resultados están en [hashes/FORENSIC_10-hashes-Imagen2.1.txt](hashes/FORENSIC_10-hashes-Imagen2.1.txt). La copia coincide con los archivos actuales de la carpeta de la máquina virtual.
### Evidencias

- [Captura del proceso de adquisición de la imagen definitiva y su verificación](capturas/disco.png).
- [Captura de la imagen abierta en FTK Imager mostrando las dos particiones](capturas/disco2.png).

## 3.2 Adquisición del pendrive encontrado

**Hora de inicio:** [17:34:57]
**Hora de finalización:** [18:11:11] (08/11/2026, hora local UTC+01:00).

La verificación de FTK finalizó a las 18:15:35.

### Identificación del dispositivo

- Marca y modelo: Kingston DataTraveler 70.
- Capacidad de origen indicada por FTK Imager: 59.120 MB.
- Número de serie: el informe FTK lo muestra como `0000000005` seguido de caracteres de control; no es legible y no se consigna como identificador fiable.

### Herramienta utilizada

- Exterro FTK Imager 8.3.0.27.
- Utilidad de bloqueo USB por software; su actividad quedó registrada en `hashes/bloqueo-usb.log`.

### Motivo de elección

FTK Imager permitió adquirir el dispositivo físico en formato E01 y verificar la imagen tras su creación. Se activó además la política de bloqueo de escritura por software antes de la adquisición, para reducir el riesgo de modificar el pendrive. Este bloqueo es una medida por software, no un bloqueador físico.

### Procedimiento

1. Activé la política de protección de escritura USB.
2. Conecté y seleccioné el dispositivo Kingston DataTraveler 70.
3. En FTK Imager seleccioné la adquisición de una unidad física y elegí el formato E01.
4. Activé la verificación de la imagen tras su creación.
5. Inicié la adquisición y esperé a que FTK Imager completara la verificación.
6. Conservé los segmentos, el informe FTK y el registro de la política de bloqueo.

### Configuración y comandos utilizados

La adquisición se configuró mediante las interfaces de la utilidad de bloqueo USB y FTK Imager. No se conserva un comando de consola literal para estas operaciones. El registro `hashes/bloqueo-usb.log` consigna la activación de `WriteProtect=1` a las 17:31:04 y su desactivación (`WriteProtect=0`) a las 18:22:02.

### Resultado y observaciones

- Dispositivo adquirido: Kingston DataTraveler 70 USB, unidad física.
- Tamaño de origen: 59.120 MB; 121.077.760 sectores de 512 bytes.
- Bloqueo de escritura: la política global por software estaba activada antes de la adquisición, según el registro.
- Imagen generada: `Adquisición/USB.E01`–`USB.E39`, formato E01.
- Tamaño total de los 39 segmentos: 60.318.665.825 bytes.
- SHA-256: calculado individualmente para cada segmento; los 39 valores están en `hashes/hashes-imagenes-sha256.txt`. No se calculó un único SHA-256 agregado de toda la imagen.
- Hashes del soporte calculados por FTK: MD5 `62e7cb4b67bed558b6189dc32124aaa3` y SHA-1 `eca39eff684c900ee38991f37b73bc2224e57b17`.
- Verificación: correcta; los hashes calculados, almacenados y del informe coinciden.
- Registro de bloqueo: `hashes/bloqueo-usb.log`.
- Incidencias: el número de serie no puede recuperarse de forma fiable del informe FTK.

### Evidencias

- [Fotografía inicial del pendrive](capturas/usb.jpg)
- [Captura de activación del bloqueo de escritura](capturas/bloqueo.png)
- [Captura de comprobación PROTEGIDO](capturas/bloqueo2.png)
- [Captura de selección del dispositivo en FTK Imager](capturas/bloqueo3.png)
- [Captura de adquisición del pendrive](capturas/bloqueo4.png)

---

# 4. Comprobación de la marca del investigador

### Herramientas utilizadas

- FTK Imager 8.3.0.27 para revisar las imágenes E01.
- Herramientas del entorno de análisis para examinar el volcado RAW y los artefactos obtenidos en el triaje.
- Revisión de las imágenes en modo de solo lectura.
- No se utilizaron Linux/CAINE/Kali ni los comandos `strings` y `grep` en estas comprobaciones.

### Motivo de elección

Se comprobó la presencia de la marca en la memoria RAM, los metadatos del triaje y las imágenes definitivas del disco y del pendrive. El objetivo fue corroborar que la marca preparada durante la actividad aparece en las evidencias adquiridas. Las imágenes se examinaron en modo de solo lectura para no modificarlas.

### 4.1 Comprobación en la memoria RAM

#### Procedimiento

1. Busqué la cadena de la marca directamente en el volcado `memoria.raw`, tanto como texto ASCII como UTF-16LE.
2. Confirmé que la cadena aparece en el volcado.

#### Método de comprobación

Examiné el volcado `memoria.raw` y busqué la cadena de la marca en las representaciones ASCII y UTF-16LE. No se conserva en las notas un comando reproducible ni el nombre de una utilidad de búsqueda para esta comprobación.

#### Resultado

- Marca localizada: Sí.
- Marca encontrada: `AFI-RaulToledo-20261108`.
- Observaciones: la cadena aparece en ASCII y UTF-16LE, coherente con la marca escrita y dejada abierta en el Bloc de notas durante la adquisición.


### 4.2 Comprobación en el triaje

La primera comprobación se realizó durante el triaje, en el punto 2.5. La búsqueda inicial con `findstr` solo examinó archivos `.txt`, por lo que no detectó la cadena almacenada en la tabla binaria `$MFT`. Después busqué directamente en `$MFT`.

#### Método de comprobación

La búsqueda inicial con `findstr` se limitó a los archivos TXT del triaje y no encontró la cadena. Después se examinó directamente `$MFT`, donde se localizaron el nombre del archivo y la cadena de la marca. También se comprobó la existencia y el tamaño del archivo original en el escritorio. No se conserva el comando exacto empleado para la búsqueda directa en `$MFT`.

#### Resultado

- Archivo `$MFT` localizado: Sí.
- Acceso directo encontrado: `D:\Salida\Recent\marca-raultoledo.lnk`.
- Archivo original: se comprobó que `marca-raultoledo.txt` existía en el escritorio y tenía 23 bytes.
- Cadena `AFI-RaulToledo-20261108` en `$MFT`: encontrada.
- Nombre `marca-raultoledo.txt` en `$MFT`: encontrado.
- Texto de la marca en los archivos TXT del triaje: la búsqueda inicial no lo encontró porque se limitó a archivos `.txt`.
- Conclusión: la marca y el nombre del archivo aparecen en `$MFT`; el fichero de 23 bytes fue localizado en el escritorio. El triaje no contiene una copia independiente de ese fichero.

### 4.3 Comprobación en la imagen del disco

La imagen examinada es la copia definitiva `Adquisición/Imagen2/DISCO.E01`. El informe de FTK registra el inicio a las 20:35:31, el fin de adquisición a las 20:38:03 y el fin de verificación a las 20:39:48 del 08/11/2026 (hora local UTC+01:00). La imagen se examinó en modo de solo lectura.

#### Procedimiento

1. Abrí los segmentos `DISCO.E01`–`DISCO.E04` como una imagen E01.
2. Examiné las particiones y el sistema de archivos NTFS de la imagen en modo de solo lectura.
3. Accedí a `/Users/Administrador/Desktop`.
4. Localicé `marca-raultoledo.txt` y leí su contenido.

#### Resultado

- Imagen examinada: `Adquisición/Imagen2/DISCO.E01` (segmentos `.E01`–`.E04`).
- Verificación de FTK Imager: correcta; MD5 y SHA-1 calculados, almacenados y del informe aparecen como verificados.
- Archivo localizado: sí, `/Users/Administrador/Desktop/marca-raultoledo.txt`.
- Tamaño: 23 bytes.
- Contenido: `AFI-RaulToledo-20261108`.
- Observaciones: el archivo y su contenido aparecen en la imagen definitiva. La marca coincide con la preparada durante la actividad.
- Cronología: el informe de FTK conserva las marcas de tiempo detalladas de adquisición y verificación.

#### Evidencias

- [Informe de verificación de FTK de la nueva imagen](hashes/DISCO.E01.txt)

### 4.4 Comprobación en la imagen del pendrive

#### Procedimiento

1. Abrí los segmentos `USB.E01`–`USB.E39` como una imagen E01.
2. Examiné la partición y su sistema de archivos en modo de solo lectura.
3. Localicé `/marca-raultoledo.txt` y leí su contenido.

#### Resultado

- Imagen examinada: `Adquisición/USB.E01` (segmentos `.E01`–`.E39`).
- Verificación de FTK Imager: correcta; MD5 y SHA-1 calculados, almacenados y del informe aparecen como verificados.
- Archivo localizado: sí, `/marca-raultoledo.txt`.
- Tamaño: 23 bytes.
- Contenido: `AFI-RaulToledo-20261108`.
- Observaciones: la marca coincide con la encontrada en la imagen del disco `Imagen2/DISCO.E01`.

### Resultado general de la comprobación

| Evidencia           | Marca localizada                                              |
| ------------------- | ------------------------------------------------------------- |
| Memoria RAM         | Sí: cadena encontrada en ASCII y UTF-16LE                    |
| Triaje (`$MFT`)   | Sí: cadena de la marca y nombre del archivo encontrados      |
| Imagen del disco    | Sí: archivo y contenido encontrados en `Imagen2/DISCO.E01` |
| Imagen del pendrive | Sí: archivo y contenido encontrados en `USB.E01`           |

---

# 5. Cadena de custodia

El detalle de las evidencias adquiridas se recoge en el [acta de adquisición](acta-adquisicion.pdf). Registrar en el [registro de cadena de custodia](registro-cadena-custodia.pdf) cada transferencia, incluida la entrega al compañero y la devolución, con fechas, horas, responsables y firmas.

Entrega a Luis Baena Utrera (09/11, 10:00): le paso la carpeta de entrega y le comunico por separado el SHA-256 de `hashes/hashes.sha256`. Generé el manifiesto desde la raíz de la carpeta con todos sus archivos, excepto el propio manifiesto. En la carpeta disponible para esta entrega hay 36 archivos que verificar; los 376 indicados anteriormente no corresponden a los archivos presentes ahora.

    sha256sum -c hashes/hashes.sha256 → 36 OK, 0 errores.
    Firmamos la entrega en el registro de custodia.

Devolución (09/11, 00:00): Luis me devuelve la carpeta. Vuelvo a ejecutar `sha256sum -c hashes/hashes.sha256` → 36 OK, 0 errores. Firmamos la devolución.

Por qué: la entrega con hashes.sha256 permite que quien recibe compruebe por sí mismo que no ha cambiado nada, y pasar el hash del listado por otro canal impide que alguien cambie una evidencia y el listado a la vez. Cada traspaso queda firmado en el registro, sin huecos desde la adquisición hasta la devolución.
