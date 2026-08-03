# SOC Writeups

Análisis de incidentes de seguridad realizados como práctica personal, 
enfocados en threat hunting, análisis de malware/phishing y mapeo a MITRE ATT&CK.

## Sobre este repositorio

Cada carpeta contiene un análisis independiente: muestra utilizada, metodología, 
IOCs extraídos y TTPs mapeadas a MITRE ATT&CK. El objetivo es demostrar 
capacidad de análisis manual (no solo herramientas automatizadas) y buenas 
prácticas de documentación tipo SOC/CSIRT.

## Writeups

| Fecha | Nombre | Tipo | Herramientas |
|---|---|---|---|
| Feb 2026 | [Easy As 123 - Análisis de tráfico](./ruta-a-la-carpeta) | Análisis de PCAP / NetSupport RAT | Wireshark, IOC enrichment |
| Ago 2026 | [Phishing IRPF - Análisis de correo y PDF](./ruta-a-la-carpeta) | Phishing / análisis de correo y adjunto | VirusTotal, sandbox dinámico |

## Skills demostradas

- Análisis de headers de correo (SPF/DKIM/DMARC)
- Decodificación manual de Base64 / MIME encoded-words
- Extracción de IOCs y verificación en VirusTotal
- Análisis estático y dinámico de artefactos maliciosos
- Mapeo de TTPs a MITRE ATT&CK
- Análisis de tráfico de red con Wireshark
