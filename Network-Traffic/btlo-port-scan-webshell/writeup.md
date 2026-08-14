# Análisis de Escaneo de Puertos y Compromiso Web (BTLO - Port Scan)

## Escenario

El SOC recibió una alerta en el SIEM por "escaneo de puertos de local a local": una IP privada interna comenzó a escanear otro sistema interno. Se investigó el archivo PCAP proporcionado para determinar si la actividad era maliciosa.

## Herramientas utilizadas

- Wireshark

## Resumen de hallazgos

| Dato | Valor |
|---|---|
| IP atacante | `10.251.96.4` |
| IP víctima | `10.251.96.5` |
| Rango de puertos escaneado | 1–1024 |
| Tipo de escaneo | TCP SYN scan |
| Herramientas de reconocimiento adicionales | gobuster 3.0.1, sqlmap 1.4.7 |
| Endpoint de subida de archivos | `/upload.php` |
| Web shell subido | `dbfunctions.php` |
| Parámetro de ejecución de comandos | `cmd` |
| Primer comando ejecutado | `id` |
| Tipo de conexión obtenida | Reverse shell |
| Puerto de la reverse shell | `4422` |

## Análisis paso a paso

### 1. Identificación del escaneo de puertos

Usando **Statistics → Endpoints** y luego **Statistics → Conversations (TCP)** en Wireshark, se identificó que la IP `10.251.96.4` establecía cientos de conversaciones TCP cortas contra la IP `10.251.96.5`, cada una a un puerto de destino distinto, en una ventana de tiempo muy breve.

![Endpoints overview](images/01-endpoints-overview.png)
![Conversations TCP](images/02-conversations-tcp-syn-pattern.png)

### 2. Tipo de escaneo

Filtrando por `ip.src==10.251.96.4`, se observó que cada conexión consta de solo 2 paquetes: un `SYN` enviado por el atacante, seguido de un `SYN-ACK` o `RST` de la víctima, sin que el atacante complete el handshake de tres vías (no se envía el `ACK` final). Este patrón corresponde a un **TCP SYN scan** (half-open scan), una técnica de reconocimiento sigilosa.

![SYN/RST flags pattern](images/04-syn-rst-flags-filtered.png)

### 3. Herramientas de reconocimiento adicionales

Filtrando por tráfico HTTP (`http.request`), se identificaron dos herramientas de reconocimiento adicionales a través del header `User-Agent`:

- **gobuster/3.0.1**: utilizada para enumerar rutas y archivos del servidor web mediante peticiones GET.
- **sqlmap/1.4.7**: utilizada para probar inyecciones SQL mediante peticiones POST.

![gobuster User-Agent](images/05-gobuster-useragent.png)
![sqlmap User-Agent](images/06-sqlmap-useragent.png)

### 4. Subida del web shell

Se identificó una petición `POST /upload.php` con `Content-Type: multipart/form-data`, cuyo `Referer` apunta a `editprofile.php`. Dentro del cuerpo de la petición, el campo `Content-Disposition` revela el archivo subido: `filename="dbfunctions.php"`.

![Upload POST request](images/07-webshell-upload-post.png)
![Filename detail](images/08-webshell-filename-detail.png)

### 5. Ejecución de comandos vía web shell

Una vez subido, el atacante interactuó con el web shell mediante peticiones GET a `/uploads/dbfunctions.php`, pasando comandos del sistema operativo a través del parámetro `cmd` (ej: `?cmd=id`, `?cmd=whoami`).

![Comandos ejecutados vía web shell](images/09-webshell-commands-executed.png)

### 6. Obtención de reverse shell

El atacante ejecutó un comando en Python que abre un socket y se conecta de vuelta hacia su propia IP (`10.251.96.4`) en el puerto `4422`, obteniendo así una **reverse shell** interactiva en lugar de continuar ejecutando comandos uno por uno vía HTTP. Esto se confirmó cruzando el comando con tráfico TCP real entre ambas IPs en dicho puerto.

![Reverse shell confirmada en Conversations](images/10-reverse-shell-port-confirmed.png)

## Conclusión

La actividad detectada es **maliciosa**. Se identificó una cadena de ataque completa: reconocimiento (escaneo de puertos + enumeración web), explotación (subida de web shell vía funcionalidad de subida de archivos mal validada) y post-explotación (obtención de reverse shell interactiva).

### Mapeo MITRE ATT&CK (referencial)

- **T1046** – Network Service Discovery (escaneo de puertos)
- **T1595** – Active Scanning (gobuster, sqlmap)
- **T1190** – Exploit Public-Facing Application (subida del web shell)
- **T1505.003** – Server Software Component: Web Shell
- **T1059** – Command and Scripting Interpreter (ejecución de comandos vía `cmd`)
- **T1071.001** – Application Layer Protocol: Web Protocols (canal inicial vía HTTP antes de la reverse shell)

---
*Fuente: BTLO - Port Scan challenge*
