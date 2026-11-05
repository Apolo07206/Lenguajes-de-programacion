---
title: "Flujo de trabajo y .gitignore"
area: programacion
tipo: lenguaje
tags: [programacion, git, flujo]
created: 2026-10-22
updated: 2026-10-22
related: ["[[MOC-programacion]]", "[[Ruta Git]]", "[[03-Remotos]]", "[[01-Introducción a Git]]"]
---

# 06 — Flujo de trabajo y .gitignore

## Estrategias de equipo

| Estrategia | Idea | Cuándo |
| ------------------ | -------------------------------------- | --------------------- |
| **Git Flow** | `main` + `develop` + ramas de feature/release/hotfix | Proyectos con ciclos de release |
| **GitHub Flow** | `main` siempre desplegable + ramas cortas + PR | Web continua, lo más común hoy |
| **Trunk-based** | Todos en `main`, ramas de horas | Equipos ágiles, CI muy fuerte |

### GitHub Flow (el estándar hoy)

```bash
git switch main
git pull
git switch -c feat/nueva-funcion
# ... trabajo ...
git push -u origin feat/nueva-funcion
# abres Pull Request → review → merge
```

## Comandos que complementan el flujo

```bash
git tag v1.0.0                 # marca una versión
git tag -a v1.0.0 -m "release" # tag con mensaje
git cherry-pick abc1234        # copia UN commit a la rama actual
git log main..feat/login       # qué tiene feat que no tiene main
git blame archivo.md           # quién escribió cada línea y cuándo
git shortlog -sn               # quién ha commiteado más
```

### `cherry-pick` (el útil de emergencia)

```bash
git switch main
git cherry-pick abc1234        # trae el fix de otra rama sin fusionar todo
```

### `rebase` (re-escribir historial limpio)

```bash
git switch feat/login
git rebase main        # replanta tus commits ENCIMA de main
```

> **Regla de oro**: rebase en ramas **propias**; nunca en ramas **compartidas** (reescribe historial que ya vieron otros).

## `.gitignore` — qué no se versiona

Crea `.gitignore` en la **raíz** del repo:

```gitignore
# Dependencias
node_modules/
venv/
__pycache__/

# Entorno y secretos
.env
*.key

# Builds y sistema
dist/
build/
*.log
.DS_Store

# IDE
.vscode/
.idea/
```

### Ignorar algo ya trackeado

```bash
git rm -r --cached carpeta/     # deja de trackear pero NO borra del disco
git commit -m "chore: ignora carpeta"
```

### Atajos útiles del patrón

```gitignore
*.log        # cualquier .log
!important.log   # excepto este
build/       # cualquier carpeta build
/temp        # solo /temp en la raíz
```

## Checklist antes de un push importante

- [ ] `git status` limpio
- [ ] Mensajes de commit con sentido
- [ ] Nada de secretos ni `.env`
- [ ] `.gitignore` completo
- [ ] `git pull` para traer lo de los demás
- [ ] Tests pasando

## Cuándo usar / Cuándo NO usar

- Proyecto solo / aprendizaje → GitHub Flow basta y sobra.
- `rebase` para reescribir **tu** rama; `merge` para integrar en **compartidas**.
- `.gitignore` desde el primer commit (subir `node_modules` es el pecado clásico).
- `pull --rebase` deja historial lineal y limpio.

## Ver también

- [[03-Remotos]]
- [[01-Introducción a Git]]
- [[Ruta Git]]
- [[MOC-programacion]]
- [[HOME]]
