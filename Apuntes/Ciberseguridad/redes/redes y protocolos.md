---
title: redes y protocolos
area: ciberseguridad
tipo: fundamento
tags: [ciberseguridad, redes]
created: 2026-01-17
updated: 2026-10-12
related: ["[[MOC-ciberseguridad]]", "[[red-team-killchain]]", "[[Nmap]]"]
---

## Concepto Fundamental

Las **redes** son infraestructuras que permiten la comunicación entre dispositivos mediante la transferencia de datos. Los **protocolos** son el conjunto de reglas y estándares que gobiernan cómo se transmite, formatea y procesa esa información. En ciberseguridad, comprender ambos es crítico porque cada protocolo y arquitectura de red presenta vectores de ataque específicos y requiere controles de seguridad particulares.

## Tipos de Redes según Alcance

**LAN (Local Area Network)**: Redes locales en espacios limitados como oficinas o edificios. Son el primer perímetro de defensa donde se implementan segmentación de red, VLANs y controles de acceso a nivel de switch.

**WAN (Wide Area Network)**: Conectan redes geográficamente dispersas. Requieren cifrado robusto (IPSec, VPN) ya que los datos transitan por infraestructura potencialmente no confiable.

**WLAN (Wireless LAN)**: Redes inalámbricas vulnerables a ataques como sniffing, evil twin y deautenticación. Requieren WPA3, autenticación 802.1X y monitoreo de puntos de acceso no autorizados.

**DMZ (Demilitarized Zone)**: Zona intermedia entre redes internas y externas donde se ubican servidores públicos, minimizando la exposición de sistemas internos.

## Modelo OSI y Seguridad por Capas

Cada capa del modelo OSI presenta amenazas específicas:

**Capa 1 (Física)**: Acceso físico no autorizado, wiretapping, destrucción de infraestructura.

**Capa 2 (Enlace de Datos)**: ARP spoofing, MAC flooding, VLAN hopping, ataques a switches.

**Capa 3 (Red)**: IP spoofing, routing attacks, ICMP floods, fragmentación maliciosa.

**Capa 4 (Transporte)**: SYN flooding, secuestro de sesión TCP, escaneo de puertos.

**Capa 5-7 (Sesión/Presentación/Aplicación)**: Inyección SQL, XSS, malware, phishing, ataques a APIs.

## Protocolos Críticos en Ciberseguridad

**TCP/IP**: Base de Internet. TCP garantiza entrega confiable pero es vulnerable a secuestro de sesión. UDP es rápido pero sin garantías, usado en ataques DDoS de amplificación.

**HTTP/HTTPS**: HTTPS con TLS 1.3 es obligatorio para proteger datos en tránsito. Configuraciones débiles de TLS o certificados mal gestionados crean vulnerabilidades.

**DNS**: Vulnerable a envenenamiento de caché, tunneling para exfiltración de datos y DDoS. DNSSEC añade autenticación criptográfica.

**SSH**: Protocolo seguro para administración remota. Requiere claves fuertes, deshabilitar autenticación por contraseña y mantener versiones actualizadas.

**SMB**: Usado para compartir archivos en Windows. Explotado en WannaCry y NotPetya. Requiere parcheo constante y segmentación de red.

**SNMP**: Gestión de dispositivos de red. Las versiones 1 y 2 transmiten credenciales en texto plano; SNMPv3 añade cifrado y autenticación.

**LDAP/Kerberos**: Autenticación en Active Directory. Ataques como Kerberoasting explotan configuraciones débiles.

## Arquitecturas de Seguridad de Red

**Segmentación**: Dividir la red en zonas basadas en nivel de confianza y función, limitando el movimiento lateral de atacantes.

**Zero Trust**: "Nunca confiar, siempre verificar". Autenticación continua y verificación de cada acceso, sin confianza implícita basada en ubicación de red.

**Defense in Depth**: Múltiples capas de controles de seguridad, de modo que el fallo de una no comprometa todo el sistema.

## Herramientas y Tecnologías de Protección

**Firewalls**: Filtrado de tráfico basado en reglas. Los NGFW (Next-Generation Firewalls) añaden inspección profunda de paquetes y detección de amenazas.

**IDS/IPS**: Sistemas de detección/prevención de intrusiones que identifican patrones maliciosos en el tráfico de red.

**VPN**: Túneles cifrados para acceso remoto seguro. IPSec para site-to-site, SSL/TLS VPN para usuarios remotos.

**NAC (Network Access Control)**: Controla qué dispositivos pueden conectarse a la red basándose en cumplimiento de políticas de seguridad.

**SIEM**: Agregación y correlación de logs de múltiples fuentes para detectar incidentes de seguridad.

## Vectores de Ataque Comunes

**Man-in-the-Middle (MITM)**: Interceptación de comunicaciones. Mitigado con cifrado fuerte y validación de certificados.

**DDoS**: Saturación de recursos mediante tráfico masivo. Requiere soluciones de mitigación especializadas y ancho de banda suficiente.

**Escaneo y Enumeración**: Reconocimiento de la red para identificar servicios vulnerables. Minimizado ocultando servicios innecesarios y usando firewalls.

**Exfiltración de Datos**: Robo de información sensible. Prevenido con DLP (Data Loss Prevention), monitoreo de tráfico saliente y cifrado.

## Mejores Prácticas

Implementa el principio de mínimo privilegio en todos los niveles de red. Mantén inventarios actualizados de dispositivos y servicios. Realiza análisis de vulnerabilidades periódicos. Cifra todo el tráfico sensible. Segmenta la red según criticidad y función. Monitorea continuamente el tráfico en busca de anomalías. Mantén parches y actualizaciones al día. Documenta arquitectura de red y flujos de datos. Implementa autenticación multifactor para accesos administrativos.

## Ver también

- [[MOC-ciberseguridad]]
- [[red-team-killchain]]
- [[Nmap]]
