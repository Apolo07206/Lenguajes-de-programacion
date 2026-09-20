---
title: red team killchain
area: ciberseguridad
tipo: hub
tags: [ciberseguridad, red-team, killchain]
created: 2026-09-20
updated: 2026-09-20
related: ["[[MOC-ciberseguridad]]", "[[Nmap]]", "[[Metasploit]]"]
---

# Red Team Killchain (hub de flujo)

Orden recomendado de uso. Cada fase enlaza a tus notas reales.

## 1. Recon pasivo (OSINT)

[[Recon-ng]] → [[The harvester]] → [[sherlock]] → [[Holehe]]
Base: [[redes y protocolos]]

## 2. Recon activo / Escaneo

[[Nmap]] (clásico: `nmap -sV -sC -p- -T4 -oA scan <IP>`) → [[Hping]] → [[Wireshark]] / [[TCPdump]] para validar tráfico.

## 3. Enumeración

Web: [[Gobuster]] / [[WFuzz]] → [[burpsuite]] → [[nuclei]]
Vulns: [[Searchsploit]] → [[SQLmap]] (DB: [[Conceptos de mysql]])
Creds: [[Hydra]] / [[ataque de fuerza bruta a red wifi]] + [[aircrack-ng]]

## 4. Explotación

[[Metasploit]] → [[MSFvenom]] (payloads) → [[Bettercap]] (MITM/LAN) → [[apuntes normales]] (netcat/revshells)

## 5. Post-explotación / Persistencia / Privesc

Linux: [[Permisos, Servicios y Logs en Linux]] (SUID, sudo, cron, LinPEAS) → [[Persistencia por ssh linux]]
Windows: [[conceptos basicos]] + [[conceptos]]
Forense/limpieza: [[Exiftool]], [[John The Ripper]]
Práctica: [[maquinas resueltas de HTB]]

## Ver también

- [[MOC-ciberseguridad]]
- [[MOC-programacion]]
- [[HOME]]
