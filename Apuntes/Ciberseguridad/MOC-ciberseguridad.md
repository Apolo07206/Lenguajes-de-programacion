---
title: MOC ciberseguridad
area: ciberseguridad
tipo: moc
tags: [moc, ciberseguridad]
created: 2026-09-20
updated: 2026-10-12
related: ["[[HOME]]", "[[red-team-killchain]]"]
---

# MOC — Ciberseguridad

> Mapa de todo lo de seguridad. Si creas una nota nueva aquí, enlázala aquí.

## Fundamentos

- [[redes y protocolos]] — modelo OSI, TCP/IP, DNS, SMB, ((base de todo))
- [[topologias de red]] — ![[Assets/topologia-red.png]]
- [[conceptos]] — hub de sistemas operativos (kernel, procesos, memoria, hardening)
- [[conceptos basicos]] (Windows) — UAC, AD, SMB, registro, Event Logs
- [[Permisos, Servicios y Logs en Linux]] — permisos/SUID/sudo, systemd, logs, cron, privesc checklist

## Killchain / Flujo de ataque

- [[red-team-killchain]] — índice del flujo recon → explotacion → post

## Recon / OSINT

- [[Recon-ng]], [[The harvester]], [[sherlock]], [[Holehe]]
- Conexión: OSINT → [[Nmap]] → [[Searchsploit]] → [[Metasploit]]/[[SQLmap]]

## Explotación

- [[Nmap]], [[Hping]], [[Bettercap]], [[Hydra]], [[Metasploit]], [[MSFvenom]], [[Searchsploit]], [[SQLmap]]
- [[ataque de fuerza bruta a red wifi]], [[aircrack-ng]]
- [[Persistencia por ssh linux]]
- [[maquinas resueltas de HTB]], [[otras herramientas]]
- [[apuntes normales]] — cajón por atomizar (netcat, ping, revshells, GTFOBins)

## Web

- [[burpsuite]], [[Gobuster]], [[WFuzz]], [[nuclei]]

## Red / Sniffing

- [[Wireshark]], [[TCPdump]], [[Bettercap]], [[Hping]]

## Post-explotación / Forense

- [[Exiftool]], [[John The Ripper]]

## Blue team (vacío — crear)

- `Ciberseguridad/blue team/Herramientas/` está vacía. Próximo: notas de defensa, logs → SIEM, hardening.

## Ver también

- [[HOME]]
- [[red-team-killchain]]
- [[MOC-programacion]] (Python [[Sockets]] para shells, MySQL [[Conceptos de mysql]] para [[SQLmap]])
- [[MOC-idiomas]]
