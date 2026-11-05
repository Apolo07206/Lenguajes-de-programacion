---
title: Ruta Bash
area: programacion
tipo: ruta
tags: [programacion, bash]
created: 2026-10-25
updated: 2026-10-25
related: ["[[MOC-programacion]]", "[[Rutas]]", "[[MOC-ciberseguridad]]"]
---

# Ruta Bash

> Roadmap vivo. Marca con [x] lo dominado.

## Fundamentos

- [ ] [[01-Introducción a Bash]] — shebang, permisos `chmod +x`, ejecución, binarios vs scripts

## Datos

- [ ] [[02-Variables y tipos de datos en Bash]] — variables, especiales (`$1 $# $@ $?`), aritmética, `read`
- [ ] [[03-Arrays en Bash]] — listas indexados, `declare -A`, recorrer colecciones

## Control de flujo

- [ ] [[04-Condicionales en Bash]] — `if/elif/else`, operadores, `case`
- [ ] [[05-Bucles en Bash]] — `for`, `while`, `until`, `break`/`continue`

## Estructura

- [ ] [[06-Funciones en Bash]] — parámetros, `local`, `return` vs `echo`
- [ ] [[07-POO simulada en Bash]] — clases y objetos con `declare -A` + `local -n`

## Práctica / ciber

- [ ] [[08-Comandos básicos para ciberseguridad en Bash]] — `find`, `grep`, `lscpu`, `free -h`, `lsblk`, `ps`, `kill`, `systemctl`, `ss`, `nc`
- [ ] [[Comandos de Linux]] — referencia completa de todos los comandos (checklist por área)

## Complementos (sin número, se leen cuando hagan falta)

- [ ] [[Terminal, Shell y Aliases]] — terminal vs shell, prompt, `.bashrc`/`.zshrc`, alias
- [ ] [[Comodines (Wildcards) en Linux]] — `*`, `?`, `[abc]`, `{a,b}`
- [ ] [[Gestión de Procesos y Control en Linux]] — `ps`, `top`, `kill`, señales, `bg`/`fg`
- [ ] [[Permisos y propietarios]] — `chmod`, `chown`, octales, SUID/SGID

## Conexiones

- [ ] Binarios vs scripts → [[Concepto de binarios y scripts]]
- [ ] Hardware → [[Arquitectura Básica de Hardware]] · [[Linux - Hardware y File System]]
- [ ] Servicios, permisos y logs → [[Permisos, Servicios y Logs en Linux]]
- [ ] Aplicar en la killchain → [[red-team-killchain]]
- [ ] Proyecto: script de recon automático

## Ver también

- [[MOC-programacion]]
- [[Rutas]]
- [[HOME]]
