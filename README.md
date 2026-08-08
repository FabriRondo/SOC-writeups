# SOC Writeups

Análisis de incidentes de seguridad realizados como práctica personal, enfocados
en threat hunting, análisis de malware/phishing y mapeo a MITRE ATT&CK.

Cada caso documenta el proceso completo de investigación manual: desde la
muestra cruda hasta la extracción de IOCs y las TTPs correspondientes —
buscando reflejar la metodología de un analista SOC, no solo el resultado.

## Writeups

| Fecha | Caso | Tipo | Técnicas MITRE | Herramientas |
|---|---|---|---|---|
| Feb 2026 | [Easy As 123 — Análisis de tráfico](./ruta-a-la-carpeta) | Malware / NetSupport RAT (PCAP) | T1105, T1219 | Wireshark |
| Ago 2026 | [Phishing IRPF — Adjunto + credential harvesting](./ruta-a-la-carpeta) | Phishing / correo + PDF | T1566.001, T1566.002 | VirusTotal, sandbox dinámico |
| Ago 2026 | [Phishing Microsoft Spoofing — Ingeniería social conversacional](./Phishing-Analysis/phishing-microsoft-spoofing) | Phishing / correo sin adjunto | T1566, T1598.001 | VirusTotal, análisis manual de headers |

*(Completá los IDs MITRE reales de tu caso PCAP y del PDF — puse ejemplos, ajustalos vos)*

## Metodología

Cada writeup sigue una estructura consistente:
1. Contexto de la muestra y fuente
2. Evidencia técnica (headers, tráfico, capturas)
3. Extracción y verificación de IOCs (VirusTotal / AlienVault OTX)
4. Mapeo a MITRE ATT&CK con justificación propia
5. Conclusión y, cuando aplica, cómo se podría detectar/prevenir

## Skills demostradas

- Análisis de headers de correo (SPF/DKIM/DMARC, Received chain)
- Decodificación manual de Base64 / MIME encoded-words
- Extracción de IOCs y verificación en VirusTotal / AlienVault OTX
- Análisis estático y dinámico de artefactos maliciosos
- Mapeo de TTPs a MITRE ATT&CK
- Análisis de tráfico de red con Wireshark
