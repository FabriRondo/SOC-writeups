# Phishing Analysis — Microsoft Account Spoofing (Spearphishing via Service)
**Fuente de la muestra:** [rf-peixoto/phishing_pot](https://github.com/rf-peixoto/phishing_pot/blob/main/email/sample-1001.eml) — `sample-1001.eml`
**Fecha del análisis:** 07/08/2026
**Tipo de campaña:** 

## Resumen
Análisis de un correo de phishing obtenido de phishing_pot que suplanta a
"Microsoft account team" para alertar sobre un inicio de sesión inusual desde
Rusia. A diferencia de otros casos, este no usa un link falso de login ni
adjunto malicioso: busca que la víctima responda directamente al correo.

## Detalles del correo

| Campo | Valor |
|---|---|
| From (spoofed) | Microsoft account team <no-reply@access-accsecurity.com> |
| Subject | Microsoft account unusual signin activity |
| To | phishing@pot |
| Date | Thu, 27 Jul 2023 07:40:03 +0000 |
| Reply-To | solutionteamrecognizd02@gmail.com |

![headers](assets/headers.png)
*Header principal: se puede ver el remitente falso y el Reply-To que
redirige a una cuenta de Gmail ajena a Microsoft.*

## Cadena de envío (Received headers) y autenticación

![received-chain](assets/received-chain.png)
*El servidor real que envió el correo es `nonkfrgr.co.uk` (89.144.9.87),
sin relación con Microsoft. Los tres mecanismos de autenticación fallaron:
SPF none, DKIM none, DMARC permerror — evidencia de que el dominio
"access-accsecurity.com" fue usado sin autorización.*

## Vista del correo (renderizado)

![email-preview](assets/email-preview.png)
*Así lo ve la víctima. Se resalta la barra de estado inferior del navegador:
al pasar el mouse sobre "Report The User" el link real no va a Microsoft,
va a un `mailto:` hacia la cuenta de Gmail del atacante.*

## Tracking pixel

![tracking-pixel](assets/tracking-pixel.png)
*Imagen de 1x1 invisible (`visibility:hidden`) alojada en un dominio externo.
Se carga automáticamente cuando el cliente de correo renderiza el HTML,
confirmándole al atacante que la dirección está activa y el correo fue abierto.*

## Relleno de texto (Bayesian poisoning)

Dentro del `<style>` del correo se encontró un bloque de cientos de palabras
sueltas sin relación entre sí (nombres, términos random en varios idiomas,
IDs alfanuméricos). Esto es una técnica de evasión de filtros antispam
clásicos: los filtros bayesianos analizan la frecuencia de palabras para
decidir si un mail es spam, y agregar texto "normal" en volumen diluye las
señales que delatarían al correo como malicioso.

## Indicadores de compromiso (IOC)

| Tipo | Valor | Detección | Rol en el ataque |
|---|---|---|---|
| Dominio spoofed | access-accsecurity.com | — | Se hace pasar por Microsoft |
| Dominio/IP SMTP real | nonkfrgr.co.uk / 89.144.9.87 | VirusTotal: 0/91 | Origen real del envío |
| Email del atacante | solutionteamrecognizd02@gmail.com | — | Canal de contacto real (Reply-To) |
| URL tracking pixel | thebandalisty.com/track/... | VirusTotal: 11/92 | Valida que el correo fue abierto |

![virustotal-url](assets/virustotal-url.png)
*Resultado de VirusTotal para la URL del tracking pixel: 11/92 motores la
marcan como maliciosa, categorizada como phishing.*

## Mapeo MITRE ATT&CK

| Técnica | ID | Justificación |
|---|---|---|
| Phishing | T1566 | El correo en sí, ingeniería social vía email pidiendo respuesta directa |
| Gather Victim Identity Information: Spearphishing Service | T1598.001 | El tracking pixel confirma que la cuenta/dirección está activa |

## Conclusión

Correo confirmado como malicioso. A diferencia de un phishing clásico de
robo de credenciales (link falso de login), este caso es de **ingeniería
social conversacional**: busca que la víctima responda al mail para abrir
un canal de contacto directo con el atacante, quien luego podría escalar el
engaño (pedir datos, avanzar hacia un ataque tipo BEC, etc).

## Lo que aprendí

- Diferencia entre phishing con link/adjunto vs. phishing que busca
  respuesta directa (T1566 sin sub-técnica de link/attachment).
- Qué es un tracking pixel y su rol en reconnaissance (T1598.001), separado
  del vector de ataque principal.
- Bayesian poisoning como técnica de evasión de filtros antispam.
- A leer Authentication-Results correctamente: que SPF/DKIM/DMARC fallen no
  significa que el filtro "ignoró" el correo, significa que lo evaluó y
  igual lo dejó pasar.
