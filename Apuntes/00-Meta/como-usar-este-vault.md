---
title: como usar este vault
tipo: meta
tags: [meta, second-brain]
created: 2026-09-20
updated: 2026-09-20
---

# Cómo usar este vault

## Reglas mínimas

1. Toda nota lleva frontmatter: `title, area, tipo, tags, related`.
2. Toda nota enlaza mínimo a: su MOC + 2 notas relacionadas.
3. Nombres: Title Case legible, sin typos. Evita duplicados entre carpetas.
4. Imágenes van en `Assets/`, nunca en raíz. Referencia como `![[Assets/nombre.png]]`.
5. `custom-frames` no es contenido: si ves un bloque `frame: xxx` vacío, conviértelo en ruta real o bórralo.

## Tipos

- `hub` / `moc`: índice de un área
- `herramienta`: Nmap, Burp, etc. Debe tener: qué es, cuándo usar, comandos base, ver también
- `fundamento`: redes, SO, protocolos
- `lenguaje` / `ruta`: roadmap + notas del lenguaje
- `idioma`: metodo, progreso, vocabulario, errores

## Dónde poner qué

- Ciberseguridad: `Ciberseguridad/redes/`, `sistemas operativos/linux|windows/`, `red team/Herramientas/{explotacion,webs,redes,osint,post explotacion}/`, `blue team/`
- Programación: `Programacion/<lenguaje>/` + `MOC-programacion`
- Idiomas: `Idiomas/Ingles| Aleman/` con mismo template (metodo, progreso, errores, vocabulario, recursos, plan)
- Meta: `00-Meta/`, imágenes: `Assets/`, diario opcional: `98-Daily/`

## Mantenimiento semanal (10 min)

- Graph View → huérfanas → enlázalas a su MOC
- Revisa `maquinas resueltas de HTB.md` y extrae técnica → enlaza a herramienta usada

## Ver también


