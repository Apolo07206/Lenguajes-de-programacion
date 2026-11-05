---
title: "Ramas y merges"
area: programacion
tipo: lenguaje
tags: [programacion, git]
created: 2026-10-22
updated: 2026-10-22
related: ["[[MOC-programacion]]", "[[Ruta Git]]", "[[01-Introducción a Git]]", "[[05-Resolución de conflictos]]"]
---

# 02 — Ramas y merges

## Qué es / Para qué sirve

Una **rama** (branch) es una línea de desarrollo paralelo. Te permite trabajar en una feature sin romper `main`, probar ideas y colaborar sin pisar a nadie.

```bash
git branch                  # lista ramas (* = la actual)
git switch -c feat/login    # crea rama y te mete en ella
git switch main             # vuelves a main
git branch -d feat/login    # borra la rama ya fusionada
git branch -m nuevo-nombre  # renombra la rama actual
```

> `git checkout -c` es el atajo antiguo. Hoy se prefiere `git switch` (más claro) y `git restore` en vez de `checkout` para archivos.

## Ciclo de vida de una rama

```bash
git switch -c feat/login    # 1. crear
# ... editar, add, commit ...
git switch main             # 2. volver a main
git merge feat/login        # 3. fusionar en main
git branch -d feat/login    # 4. limpiar
```

## Merge: dos formas

```bash
git merge feat/login        # fusiona feat/login en la rama actual
```

| Tipo | Cuándo | Historial |
| ------------ | ---------------------------------------- | ------------------- |
| **Fast-forward** | main no avanzó desde que creaste la rama | Lineal, sin commit extra |
| **Merge commit** | ambas ramas avanzaron | Crea un commit de unión |

```bash
git merge --no-ff feat/login   # Fuerza el commit de unión (historial explícito)
git merge --abort              # Cancela el merge en conflicto
```

## Flujo recomendado (Git Flow ligero)

```bash
git switch main
git pull                        # siempre actualiza antes de ramificar
git switch -c feat/carrito      # rama por feature/bugfix/hotfix
# ... trabajo y commits ...
git switch main
git pull
git merge feat/carrito
git push
```

## Ver el grafo del historial

```bash
git log --oneline --graph --all
```

## Buenas prácticas de nombres de rama

```
feat/nombre-descriptivo    nueva funcionalidad
fix/nombre-descriptivo     corrección
hotfix/urgente             arreglo en producción
docs/solo-documentacion
```

## Cuándo usar / Cuándo NO usar

- Una rama = un objetivo. Nada de "rama-de-todo-un-poquito".
- Fusiona pronto: ramas de semanas = conflictos de semanas.
- `pull` antes de `merge` reduce la mitad de los conflictos → [[05-Resolución de conflictos]].
- No borres ramas sin fusionar sin mirar `git branch --no-merged`.

## Ver también

- [[01-Introducción a Git]]
- [[03-Remotos]]
- [[05-Resolución de conflictos]]
- [[Ruta Git]]
- [[MOC-programacion]]
- [[HOME]]
