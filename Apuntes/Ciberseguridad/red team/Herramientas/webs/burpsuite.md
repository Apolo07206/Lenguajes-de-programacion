---
title: burpsuite
area: ciberseguridad
tipo: herramienta
tags: [ciberseguridad, red-team, web]
created: 2026-01-18
updated: 2026-10-12
related: ["[[MOC-ciberseguridad]]", "[[red-team-killchain]]", "[[SQLmap]]", "[[Gobuster]]", "[[WFuzz]]", "[[nuclei]]", "[[Conceptos de mysql]]"]
---

# Burp Suite

Plataforma de testing para aplicaciones web. Funciona como **proxy interceptante**: se pone entre el navegador y el servidor, captura cada petición/respuesta y te permite modificar, repetir y automatizar ataques. Es el centro de operaciones del hacking web.

## Cuándo usar

- Explorar y mapear una app web (crawler de contenido)
- Interceptar y modificar requests/responses en vivo
- Reenviar una petición manualmente (login, búsqueda, parámetros)
- Fuerza bruta de parámetros, usuarios, IDs (Intruder)
- Detectar vulnerabilidades automáticas (Scanner, versión Pro)
- Analizar tokens, sesiones y certificados

## Cuándo NO usar

- Recon de red general (usa [[Nmap]])
- Escaneo masivo de vulnerabilidades conocidas (usa [[nuclei]])
- Fuzzing de directorios rápido (usa [[Gobuster]] o [[WFuzz]])

## Componentes clave

| Módulo | Para qué sirve |
|---|---|
| **Proxy → Intercept** | Capturar y modificar tráfico del navegador en vivo |
| **Repeater** | Reenviar una petición editada, tantas veces como quieras |
| **Intruder** | Fuerza bruta / fuzzing de parámetros con payload lists |
| **Scanner** | Escaneo automático de vulns (SQLi, XSS, etc. — versión Pro) |
| **Sequencer** | Analizar calidad de tokens de sesión (aleatoriedad) |
| **Decoder / Comparer** | Codificar/decodificar y comparar respuestas |

## Configuración base

1. Configurar el navegador para usar proxy `127.0.0.1:8080`
2. Importar el certificado de Burp (Proxy → Options → CA Certificate) en el navegador para ver tráfico HTTPS
3. Proxy → Intercept → **Intercept is on** para capturar, o **off** para solo observar

## Flujo típico con SQLmap

1. Intercepta el request vulnerable en Burp
2. Click derecho → **Copy to file** (guarda `request.txt`)
3. Pásalo a SQLmap: `sqlmap -r request.txt --batch --dbs` (ver [[SQLmap]])

## Ver también

- [[MOC-ciberseguridad]]
- [[red-team-killchain]]
- [[SQLmap]]
- [[Gobuster]]
- [[WFuzz]]
- [[nuclei]]
- [[Conceptos de mysql]]
