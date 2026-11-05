---
title: "Resolución de conflictos"
area: programacion
tipo: lenguaje
tags: [programacion, git]
created: 2026-10-22
updated: 2026-10-22
related: ["[[MOC-programacion]]", "[[Ruta Git]]", "[[02-Ramas y merges]]", "[[04-Deshacer cambios]]"]
---

# 05 — Resolución de conflictos

## Qué es / Para qué sirve

Un **conflicto** aparece cuando Git **no puede** unir automáticamente dos cambios: la misma zona fue modificada en dos ramas. No es un error: es Git pidiéndote que decidas.

```bash
git merge feat/login
# CONFLICT (content): Merge conflict in archivo.md
# Automatic merge failed; fix conflicts and then commit the result.
```

## Los marcadores

```html
<<<<<<< HEAD
código de tu rama actual (main)
=======
código de la rama que estás fusionando (feat/login)
>>>>>>> feat/login
```

Tu trabajo: dejar **solo** el código correcto (o combinar ambos), **borrar los marcadores** y guardar.

```html
código final correcto   <!-- sin <<<<<<<, ======= ni >>>>>>> -->
```

## Paso a paso

```bash
# 1. Git te dice dónde están los conflictos
git status                # en rojo: "both modified: archivo.md"

# 2. Abre el archivo y edita quitando los marcadores

# 3. Marca como resuelto
git add archivo.md

# 4. Termina el merge
git commit -m "merge: resuelve conflictos de feat/login"
```

> `Unmerged paths` en `git status` = hay conflictos pendientes. Al hacer `add`, desaparecen de esa lista.

## Si te equivocaste a mitad

```bash
git merge --abort        # vuelve al estado anterior al merge
git reset --merge        # otra vía si el --abort no va
```

## Herramientas que ayudan

```bash
git mergetool             # abre tu tool de merge configurado (VS Code, Beyond Compare...)
git diff --name-only --diff-filter=U   # solo archivos en conflicto
```

En **VS Code**: los conflictos aparecen con botones *Accept Current / Incoming / Both*.

## Estrategias para evitar conflictos

| Hábito | Por qué funciona |
| ------------------------------ | --------------------------------------- |
| `git pull` antes de empezar | Trabajas sobre lo último |
| Ramas cortas | Menos tiempo divergiendo |
| Un archivo = una persona | Evita choques en lo mismo |
| Comunica en quién toca qué | El mejor "merge tool" es hablar |

## Conflictos por eliminación

```bash
# tú la modificaste, otro la borró
deleted by us:    archivo.md    → decides si restaurar o aceptar el borrado
deleted by them:  archivo.md
```

```bash
git rm archivo.md          # aceptas el borrado
git add archivo.md         # o lo conservas modificado
```

## Cuándo usar / Cuándo NO usar

- Lee **ambos** lados antes de borrar marcadores: a veces hay que combinar, no elegir.
- No hagas `add` sin revisar: los marcadores en producción son el clásico fail.
- Antes de un merge grande, `git pull` + rama corta reducen el 80% de conflictos.
- Si te perdiste, `git merge --abort` y repítelo con calma → [[04-Deshacer cambios]].

## Ver también

- [[02-Ramas y merges]]
- [[04-Deshacer cambios]]
- [[Ruta Git]]
- [[MOC-programacion]]
- [[HOME]]
