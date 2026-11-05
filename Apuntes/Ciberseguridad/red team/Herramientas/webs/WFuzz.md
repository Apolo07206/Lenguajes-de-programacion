---
title: WFuzz
area: ciberseguridad
tipo: herramienta
tags: [ciberseguridad, red-team, web]
created: 2026-01-17
updated: 2026-10-12
related: ["[[MOC-ciberseguridad]]", "[[red-team-killchain]]", "[[burpsuite]]", "[[Gobuster]]", "[[nuclei]]"]
---

# WFuzz

Fuzzer web en Python. Su especialidad es **inyectar palabras en cualquier parte de una URL** (parámetros, rutas, headers, datos POST) y observar cómo responde el servidor. El marcador `FUZZ` marca dónde va cada palabra de la wordlist.

## Cuándo usar

- Fuzzing de parámetros GET/POST y headers
- Descubrir directorios y archivos ocultos
- Fuerza bruta de credenciales en formularios
- Detectar WAF (comparando respuestas)

## Cuándo NO usar

- Dir busting simple y rápido (usa [[Gobuster]], es más veloz)
- Escaneo de CVEs conocidas (usa [[nuclei]])

## Comandos base

```bash
# Fuzzear directorios (filtrando los 404)
wfuzz -w wordlist.txt --hc 404 http://target.com/FUZZ

# Fuzzear un parámetro GET
wfuzz -w wordlist.txt "http://target.com/search?q=FUZZ"

# Fuzzear parámetro POST
wfuzz -w wordlist.txt -d "user=FUZZ&pass=123" http://target.com/login

# Fuzzear headers
wfuzz -w wordlist.txt -H "X-Forwarded-For: FUZZ" http://target.com

# Ocultar respuestas por cantidad de palabras/página
wfuzz -w wordlist.txt --hh 12 http://target.com/FUZZ
```

## Marcadores útiles

| Marcador | Qué reemplaza |
|---|---|
| `FUZZ` | La palabra de la wordlist (principal) |
| `FUZ2Z` | Segunda wordlist (ataques combinatorios) |
| `HOST` | El host (para vhost discovery) |

## Lectura de resultados

```
000000003:   200   124 L   562 W      "admin"     ← ruta/valor interesante
000000004:   403   89  L   230 W      "backup"    ← prohibido pero existe
```

## Ver también

- [[MOC-ciberseguridad]]
- [[red-team-killchain]]
- [[burpsuite]]
- [[Gobuster]]
- [[nuclei]]
