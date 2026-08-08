# 🛡️ SOC Writeups

**Documentación técnica de análisis de incidentes de seguridad**, hechos como práctica autodidacta hacia un rol de SOC Analyst L1 → Cyber Threat Intelligence.

No son ejercicios copiados de un curso: cada caso parte de una muestra real (PCAP, .eml, malware sample) y se resuelve con análisis manual — Wireshark, headers crudos, VirusTotal — documentando el proceso completo, no solo el resultado final.

---

## 📂 Casos analizados

### 🎣 Phishing — Microsoft Account Spoofing
**[Ver writeup completo →](./Phishing-Analysis/phishing-microsoft-spoofing)**

Correo que suplanta a Microsoft alertando un login sospechoso desde Rusia. Sin adjunto ni link falso de login: el vector es **conversacional** — busca que la víctima responda el mail para abrir un canal directo con el atacante.

`T1566` `T1598.001` · Herramientas: VirusTotal, análisis manual de headers

---

### 🎣 Phishing — IRPF (PDF + Credential Harvesting)
**[Ver writeup completo →](./ruta-a-la-carpeta)**

Correo con PDF adjunto que redirige a un login falso de Azure para robo de credenciales — vector clásico de attachment + link.

`T1566.001` `T1566.002` · Herramientas: VirusTotal, sandbox dinámico

---

### 🦠 Malware Traffic Analysis — NetSupport RAT
**[Ver writeup completo →](./ruta-a-la-carpeta)**

Análisis de PCAP ("Easy As 123") con RAT comercial abusado como malware. Identificación de host infectado (IP, MAC, hostname, usuario) vía DHCP, Kerberos y SAMR.

`T1219` · Herramientas: Wireshark

---

## 🧠 Metodología

Cada writeup sigue el mismo esqueleto de investigación:

1. **Contexto** — de dónde salió la muestra, qué se sabe antes de tocar nada
2. **Evidencia técnica** — headers, tráfico, capturas, todo documentado
3. **IOCs** — extraídos y verificados contra VirusTotal / AlienVault OTX
4. **MITRE ATT&CK** — técnicas mapeadas con justificación propia, no copiada
5. **Conclusión** — veredicto +, cuando aplica, cómo se detectaría/prevendría a escala

---

## 🛠️ Skills demostradas

`Análisis de headers SMTP (SPF/DKIM/DMARC)` `Wireshark`
