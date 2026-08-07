# Phishing Analysis — Microsoft Account Spoofing (Spearphishing via Service)

## Resumen
Breve párrafo: qué es, de dónde salió (phishing_pot), qué intenta el atacante.

## Detalles del correo
| Campo | Valor |
|---|---|
| From (spoofed) | Microsoft account team <no-reply@access-accsecurity.com> |
| Subject | Microsoft account unusual signin activity |
| To | phishing@pot |
| Date | Thu, 27 Jul 2023 07:40:03 +0000 |
| Reply-To | solutionteamrecognizd02@gmail.com |

## Cadena de envío (Received headers)
Screenshot + explicación corta: dominio/IP real del remitente, resultado SPF/DKIM/DMARC.

![received headers](ruta/imagen3.png)

## Indicadores de compromiso (IOC)
| Tipo | Valor | Detección | Rol en el ataque |
|---|---|---|---|
| Dominio spoofed | access-accsecurity.com | — | Se hace pasar por Microsoft |
| Dominio/IP SMTP | nonkfrgr.co.uk / 89.144.9.87 | VT 0/91 | Origen real del envío |
| Email atacante | solutionteamrecognizd02@gmail.com | — | Canal de contacto real |
| URL tracking pixel | thebandalisty.com/track/... | VT 11/92 | Valida apertura del mail |

## Vista del correo (renderizado)
![preview](ruta/imagen5.png)

## Técnica de evasión de spam
Explicación corta del "word salad" en el CSS.

## Mapeo MITRE ATT&CK
| Técnica | ID | Justificación |
|---|---|---|
| Phishing | T1566 | El mail en sí, pidiendo respuesta |
| Gather Victim Identity Information: Spearphishing Service | T1598.001 | Tracking pixel valida cuenta activa |

## Conclusión
Veredicto final: malicioso confirmado. Tipo de campaña: phishing conversacional sin credential harvesting directo.
