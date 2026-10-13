---
title: John The Ripper
area: ciberseguridad
tipo: herramienta
tags: [ciberseguridad, red-team, post-explotacion, credenciales]
created: 2026-01-17
updated: 2026-10-12
related: ["[[MOC-ciberseguridad]]", "[[red-team-killchain]]", "[[Hydra]]", "[[ataque de fuerza bruta a red wifi]]", "[[aircrack-ng]]", "[[Permisos, Servicios y Logs en Linux]]"]
---

# John The Ripper

Crackeador de contraseñas offline. Toma **hashes** (los que extrajiste con [[Permisos, Servicios y Logs en Linux]] o de un volcado de BD) y prueba wordlists o ataques combinatorios hasta recuperar la contraseña en claro. Es el complemento offline de [[Hydra]] (que ataca servicios en vivo).

## Cuándo usar

- Crackear hashes `/etc/shadow`, `id_rsa` protegidos, hashes de BD
- Probar fortaleza de contraseñas propias (auditoría)
- Romper hashes extraídos en post-explotación

## Cuándo NO usar

- Atacar un servicio remoto en vivo (usa [[Hydra]])
- WiFi: crackear handshake WPA (usa [[aircrack-ng]])

## Comandos base

```bash
# Forma más común: John detecta el tipo de hash
john hash.txt

# Con wordlist explícita
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt

# Modo "single crack" (usa nombres de usuario como contraseñas)
john --single hash.txt

# Fuerza bruta incremental (lento, exhaustivo)
john --incremental hash.txt

# Ver contraseñas ya crackeadas
john --show hash.txt

# Detener y retomar sesión
john --restore
```

## Preparar hashes comunes

```bash
# Shadow de Linux (junto a /etc/passwd)
unshadow passwd.txt shadow.txt > hashes.txt
john hashes.txt

# Hash MySQL (formato mysql-fast)
john --format=mysql-fast hash.txt

# Clave privada SSH protegida
ssh2john id_rsa > id_rsa.hash
john id_rsa.hash

# Zip / RAR / PDF protegidos
zip2john archivo.zip > zip.hash
john zip.hash
```

## Diferencia clave vs Hydra

| | John The Ripper | Hydra |
|---|---|---|
| Objetivo | Hashes (offline) | Servicios en vivo (online) |
| Velocidad | Muy rápido (sin red) | Limitado por el servicio |
| Cuándo | Ya tienes el hash | Solo tienes el login del servicio |

## Ver también

- [[MOC-ciberseguridad]]
- [[red-team-killchain]]
- [[Hydra]]
- [[ataque de fuerza bruta a red wifi]]
- [[aircrack-ng]]
- [[Permisos, Servicios y Logs en Linux]]
