---
title: TCPdump
area: ciberseguridad
tipo: herramienta
tags: [ciberseguridad, red-team, redes]
created: 2026-01-18
updated: 2026-10-12
related: ["[[MOC-ciberseguridad]]", "[[red-team-killchain]]", "[[Nmap]]", "[[Wireshark]]", "[[Bettercap]]", "[[redes y protocolos]]"]
---

# TCPdump

Analizador de tráfico de red para terminal (sniffer de paquetes). Captura y muestra los paquetes que pasan por una interfaz, con filtros potentes. Es la versión en CLI de [[Wireshark]] — ideal para servidores sin GUI y para piping/automatizar.

## Cuándo usar

- Ver tráfico en vivo en un servidor remoto (SSH)
- Diagnosticar qué habla y por qué puerto
- Guardar capturas (.pcap) para analizar luego en Wireshark
- Validar si un ataque está llegando (ej. tras [[Bettercap]])

## Cuándo NO usar

- Análisis visual profundo de protocolos (usa [[Wireshark]])
- Modificar/inyectar tráfico (usa [[Bettercap]] o [[Hping]])

## Comandos base

```bash
# Escuchar en una interfaz (requiere sudo)
sudo tcpdump -i eth0

# Solo un puerto
sudo tcpdump -i eth0 port 80

# Host y puerto combinados
sudo tcpdump -i eth0 host 192.168.1.10 and port 443

# Guardar a archivo (para abrir en Wireshark)
sudo tcpdump -i eth0 -w captura.pcap

# Leer un archivo .pcap
tcpdump -r captura.pcap

# Limitar paquetes y mostrar datos en ASCII
sudo tcpdump -i eth0 -c 20 -A

# Filtros útiles
sudo tcpdump -i eth0 tcp[tcpflags] & (tcp-syn) != 0   # solo SYN
sudo tcpdump -i eth0 icmp                              # ping
sudo tcpdump -i eth0 dst port 22                        # solo SSH saliente
```

## Filtros rápidos

| Filtro | Qué captura |
|---|---|
| `port 80` | Tráfico web |
| `host IP` | Todo de/hacia un host |
| `net 192.168.1.0/24` | Toda la subred |
| `src/dst` | Solo origen / solo destino |
| `tcp/udp/icmp` | Por protocolo |

## Ver también

- [[MOC-ciberseguridad]]
- [[red-team-killchain]]
- [[Nmap]]
- [[Wireshark]]
- [[Bettercap]]
- [[redes y protocolos]]
