---
title: "Introducción a Git"
area: programacion
tipo: lenguaje
tags: [programacion, git, fundamentos]
created: 2026-10-22
updated: 2026-10-22
related: ["[[MOC-programacion]]", "[[Ruta Git]]", "[[02-Ramas y merges]]"]
---

# 01 — Introducción a Git

## Qué es / Para qué sirve

**Git** es un sistema de **control de versiones**: guarda el historial de tu código para que puedas volver atrás, ramificar sin miedo y colaborar. No es lo mismo que GitHub (GitHub es un hosting donde subes tus repos de Git).

## Configuración inicial (una sola vez)

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"
git config --global init.defaultBranch main   # rama principal por defecto
```

## Los 3 estados de un archivo

```
   Working directory    →    Staging area    →    Repository
   (tu carpeta)            (git add)             (git commit)
      modificado             preparado             guardado
```

1. **Working directory** → estás editando.
2. **Staging (index)** → `git add` → "esto es lo que quiero guardar".
3. **Repository** → `git commit` → instantánea guardada en el historial.

## Comandos base

```bash
git init                  # convierte carpeta en repo (solo la primera vez)
git status                # QUÉ está pasando (úsalos sin parar)
git add archivo.md        # prepara uno
git add .                 # prepara todo
git commit -m "Mensaje"   # guarda la instantánea
git log                   # historial de commits
git log --oneline         # historial compacto
git diff                  # qué cambiaste sin preparar
git diff --staged         # qué cambiaste YA preparado
```

## Flujo completo del día a día

```bash
# 1. editas archivos...
# 2. miras qué hay
git status
# 3. preparas lo que toca
git add .
# 4. guardas con mensaje claro
git commit -m "feat: añade validación de email"
```

## Mensajes de commit convención (Conventional Commits)

```
feat:     nueva funcionalidad
fix:      corrección de bug
docs:     solo documentación
refactor: reestructurar sin cambiar comportamiento
style:    formato, sin cambio lógico
test:     tests
chore:    tareas de mantenimiento
```

Ejemplo: `fix: corrige cálculo de IVA en facturas`

## Qué NO commitear

```gitignore
# .gitignore
node_modules/
.env
*.log
.DS_Store
dist/
```

## Cuándo usar / Cuándo NO usar

- **Usa** Git en **todo** proyecto, aunque sea personal. El día que rompas algo te lo agradecerás.
- **No** hagas commits gigantes que mezclan 10 cosas → un commit = una idea.
- **No** guardes secretos (tokens, `.env`) en el repo.
- Commit **temprano y a menudo** es mejor que un commit perfecto.

## Ver también

- [[02-Ramas y merges]]
- [[04-Deshacer cambios]]
- [[Ruta Git]]
- [[MOC-programacion]]
- [[HOME]]
