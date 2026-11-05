---
title: "Deshacer cambios"
area: programacion
tipo: lenguaje
tags: [programacion, git]
created: 2026-10-22
updated: 2026-10-22
related: ["[[MOC-programacion]]", "[[Ruta Git]]", "[[01-Introducción a Git]]", "[[05-Resolución de conflictos]]"]
---

# 04 — Deshacer cambios

## Qué es / Para qué sirve

La parte que da miedo y que en realidad **te salva**. Git casi nunca borra nada de verdad: casi siempre puedes volver.

## 1. Deshacer cambios sin commit (`restore`)

```bash
git restore archivo.md              # descarta cambios sin preparar
git restore --staged archivo.md     # quita del staging (sigue modificado)
git restore .                       # descarta TODO lo sin preparar
```

| Situación | Comando |
| ------------------------------------- | ----------------------------- |
| Modifiqué y quiero volver al último commit | `git restore archivo` |
| Lo añadí con `add` y me arrepentí | `git restore --staged archivo` |

## 2. Deshacer el último commit (`reset`)

```bash
git reset --soft HEAD~1   # deshace el commit, deja todo en staging
git reset --mixed HEAD~1  # deshace el commit, deja cambios sin preparar
git reset --hard HEAD~1   # deshace el commit y BORRA los cambios (cuidado)
```

```
--soft   → vuelve el commit, el código queda "listo para rehacer"
--mixed  → igual que --soft pero des-preparado (por defecto)
--hard   → lo borra todo. Solo si estás seguro.
```

`HEAD~1` = el commit anterior. `HEAD~3` = tres atrás.

## 3. Deshacer un commit ya publicado (`revert`)

```bash
git revert abc1234       # crea un commit NUEVO que anula el anterior
```

Seguro en ramas compartidas porque **no reescribe el historial**. Es la opción correcta cuando ya hiciste `push`.

## 4. Guardar trabajo temporal (`stash`)

```bash
git stash                       # guardas cambios a un lado (limpia el working dir)
git stash pop                   # los recuperas y se borran del stash
git stash apply                # los recuperas PERO se quedan en stash
git stash list                  # lista de stashes guardados
git stash drop                  # borra el último stash
```

Caso real: estás a medias con una feature y hay que arreglar un bug urgente en otra rama.

```bash
git stash
git switch main
# ... arreglo hotfix, commit, push ...
git switch feat/login
git stash pop   # sigues donde estabas
```

## 5. Corregir el mensaje del último commit

```bash
git commit --amend -m "Mensaje corregido"
```

## 6. Recuperar algo "perdido"

```bash
git reflog                      # historial de TODO lo que hizo HEAD
git reset --hard abc1234        # vuelves al punto que quieras
```

El reflog es tu red de seguridad: si "perdiste" un commit, está ahí (unos 90 días).

## Cuándo usar / Cuándo NO usar

- **Antes de `--hard`**: `git status` y `git diff`. Sin excepción.
- Commit publicado → `revert`, nunca `reset --force push`.
- `reset` reescribe historial; `revert` añade. En local da igual, en remoto importa.
- Cuando dudes, `git reflog` te rescata → [[05-Resolución de conflictos]].

## Ver también

- [[01-Introducción a Git]]
- [[05-Resolución de conflictos]]
- [[Ruta Git]]
- [[MOC-programacion]]
- [[HOME]]
