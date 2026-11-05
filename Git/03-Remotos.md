---
title: "Remotos"
area: programacion
tipo: lenguaje
tags: [programacion, git, github]
created: 2026-10-22
updated: 2026-10-22
related: ["[[MOC-programacion]]", "[[Ruta Git]]", "[[02-Ramas y merges]]", "[[06-Flujo de trabajo y .gitignore]]"]
---

# 03 — Remotos

## Qué es / Para qué sirve

Un **remoto** es una copia del repo en otro sitio (GitHub, GitLab...). Permite respaldar, compartir y colaborar. El remoto por defecto se llama `origin`.

```bash
git remote -v                        # lista los remotos configurados
git remote add origin URL            # conecta un remoto
git remote remove origin             # lo desconecta
```

## Subir y bajar

```bash
git push origin main                 # sube tu main al remoto
git push -u origin main              # primera vez: -u fija el upstream
git push                             # luego basta con esto

git pull origin main                 # baja y fusiona los cambios
git fetch origin                     # solo descarga (NO fusiona)
git pull                             # fetch + merge automático
```

| Comando | Efecto |
| ------- | ------------------------------------------ |
| `git push` | Sube tus commits al remoto |
| `git pull` | Descarga y **fusiona** en tu rama actual |
| `git fetch` | Descarga sin tocar tu trabajo (más seguro para mirar) |

> Diferencia clave: `pull` = `fetch` + `merge`. Si prefieres revisar antes de fusionar, haz `fetch` y luego `git log HEAD..origin/main`.

## Clonar un proyecto existente

```bash
git clone https://github.com/usuario/repo.git
cd repo
# ya tienes todo el historial y origin configurado
```

## Flujo completo con GitHub

```bash
# primera vez de un repo local
git init
git add .
git commit -m "feat: commit inicial"
git branch -M main
git remote add origin git@github.com:usuario/repo.git
git push -u origin main

# día a día
git pull
# ... trabajar ...
git add .
git commit -m "feat: añade X"
git push
```

## Autenticación

- **HTTPS** → token personal (PAT) o `gh auth login`.
- **SSH** → clave pública (`ssh-keygen -t ed25519`), añades la pública a GitHub.

```bash
gh auth login          # GitHub CLI: login interactivo
gh repo create         # crea repo desde terminal
gh pr create           # crea un Pull Request
```

## Resolución de rechazo de push

```bash
git pull --rebase origin main   # reintenta tus commits encima
git push
```

## Cuándo usar / Cuándo NO usar

- Haz `push` **a menudo**: tu laptop no es un backup.
- `pull` **antes** de empezar a trabajar → [[02-Ramas y merges]].
- Nunca hagas `push --force` en ramas compartidas (borras el trabajo de otros). En tu rama personal, a veces.
- Prefiere `fetch` + revisar cuando quieras control total.

## Ver también

- [[02-Ramas y merges]]
- [[06-Flujo de trabajo y .gitignore]]
- [[Ruta Git]]
- [[MOC-programacion]]
- [[HOME]]
