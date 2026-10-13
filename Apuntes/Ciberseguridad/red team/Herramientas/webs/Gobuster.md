---
title: Gobuster
area: ciberseguridad
tipo: herramienta
tags: [ciberseguridad, red-team, web]
created: 2026-01-17
updated: 2026-10-12
related: ["[[MOC-ciberseguridad]]", "[[red-team-killchain]]", "[[burpsuite]]", "[[WFuzz]]", "[[nuclei]]", "[[Nmap]]"]
---

# Gobuster

Fuzzer rápido escrito en Go. Su uso principal es **descubrir directorios, archivos y subdominios ocultos** en un servidor web (dir busting / dns brute force). Es más rápido que dirb/dirbuster y muy usado en CTFs y pentests.

## Cuándo usar

- Encontrar paneles admin, backups, rutas no enlazadas (`/admin`, `/backup`, `/config`)
- Descubrir subdominios (modo dns)
- Virtual host discovery (modo vhost)

## Cuándo NO usar

- Fuzzing de parámetros complejos (usa [[WFuzz]] o Intruder de [[burpsuite]])
- Escaneo de vulnerabilidades (usa [[nuclei]])

## Comandos base

```bash
# Descubrir directorios (lo más común)
gobuster dir -u http://target.com -w /usr/share/wordlists/dirb/common.txt

# Con extensiones (.php, .html, .bak)
gobuster dir -u http://target.com -w wordlist.txt -x php,html,bak

# Ignorar códigos de estado (ej. 404)
gobuster dir -u http://target.com -w wordlist.txt --status-codes-blacklist 404

# Subdominios (modo dns)
gobuster dns -d target.com -w subdomains.txt

# Virtual hosts
gobuster vhost -u http://target.com -w wordlist.txt

# Más threads (más rápido, más ruidoso)
gobuster dir -u http://target.com -w wordlist.txt -t 50
```

## Lectura de resultados

```
/index.html (Status: 200)
/admin (Status: 401)      ← existe pero pide auth
/backup (Status: 403)     ← existe pero prohibido
/secret (Status: 200)     ← objetivo encontrado
```

Status 200/401/403 con rutas "raras" = superficie de ataque nueva. Verifica siempre los 401/403: a veces hay datos detrás.

## Tips

- Combínalo con el mapa de [[Nmap]] (puertos 80/443/8080 descubiertos)
- Tras encontrar rutas, analízalas en [[burpsuite]] y prueba SQLi con [[SQLmap]]
- Wordlists útiles: `seclists/Discovery/Web-Content/raft-medium-directories.txt`

## Ver también

- [[MOC-ciberseguridad]]
- [[red-team-killchain]]
- [[burpsuite]]
- [[WFuzz]]
- [[nuclei]]
- [[Nmap]]
