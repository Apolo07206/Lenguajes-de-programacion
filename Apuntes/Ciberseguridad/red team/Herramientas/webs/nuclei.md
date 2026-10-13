---
title: nuclei
area: ciberseguridad
tipo: herramienta
tags: [ciberseguridad, red-team, web]
created: 2026-01-18
updated: 2026-10-12
related: ["[[MOC-ciberseguridad]]", "[[red-team-killchain]]", "[[burpsuite]]", "[[Gobuster]]", "[[Nmap]]"]
---

# nuclei

Escáner de vulnerabilidades basado en **templates** de la comunidad (proyecto projectdiscovery). Un template = una firma YAML que detecta una CVE, una mala configuración o un patrón específico. Es el estándar actual para escaneo masivo y rápido de vulnerabilidades conocidas.

## Cuándo usar

- Escanear muchos hosts contra miles de CVEs/misconfiguraciones conocidas
- Verificar exposición de tecnologías específicas (versión de CMS, paneles)
- Escaneo automatizado en CTFs y pentests

## Cuándo NO usar

- Descubrir rutas ocultas (usa [[Gobuster]])
- Analizar lógica de negocio o flujos de app (usa [[burpsuite]] manual)

## Comandos base

```bash
# Escanear un host con todos los templates
nuclei -u http://target.com

# Solo CVEs críticas/alta
nuclei -u http://target.com -t cves/ -severity critical,high

# Lista de hosts
nuclei -l hosts.txt -severity high

# Tags por tecnología
nuclei -u http://target.com -tags wordpress,exposure

# Guardar resultados
nuclei -u http://target.com -o resultados.txt

# Actualizar templates (¡hacerlo primero!)
nuclei -update-templates
```

## Por qué es potente

- Miles de templates listos (CVEs, misconfigs, paneles, tokens expuestos)
- Velocidad: escanea cientos de hosts en minutos
- Salida estructurada (JSON) para automatizar

## Flujo típico

1. [[Nmap]] descubre hosts y puertos web
2. [[Gobuster]] mapea rutas ocultas
3. **nuclei** escanea todo contra vulnerabilidades conocidas
4. Lo que encuentre se analiza a mano en [[burpsuite]] y se explota (ej. [[SQLmap]])

## Ver también

- [[MOC-ciberseguridad]]
- [[red-team-killchain]]
- [[burpsuite]]
- [[Gobuster]]
- [[Nmap]]
