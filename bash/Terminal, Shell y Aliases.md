---
title: Terminal, Shell y Aliases
area: programacion
tipo: fundamento
tags: [programacion, bash, terminal, aliases]
created: 2026-10-12
updated: 2026-11-01
related: ["[[MOC-programacion]]", "[[Ruta Bash]]", "[[Comandos de Linux]]", "[[01-Introducción a Bash]]"]
---

# Terminal, Shell y Aliases

## Conceptos clave

- **Terminal**: la ventana donde escribes. Es solo la interfaz.
- **Shell**: el programa que vive dentro y ejecuta lo que escribes (bash, zsh, fish).
- **Prompt**: la línea donde esperas. El símbolo final importa:
  - `$` = usuario normal
  - `#` = root (cuidado, puede dañar el sistema)

> [!warning] Ojo con esto
> Ubuntu usa **bash** por defecto, pero **Kali usa zsh**. La guía dice `.bashrc`, pero en Kali el archivo es `.zshrc`.

## ¿Qué shell estoy usando?

```bash
echo $SHELL    # shell por defecto de tu usuario
echo $0        # shell que está corriendo ahora mismo
cat /etc/shells   # shells instaladas
```

## Archivo de configuración según shell

| Shell | Archivo |
|-------|---------|
| bash  | `~/.bashrc` |
| zsh   | `~/.zshrc` |
| fish  | `~/.config/fish/config.fish` |

## Shell por defecto según distro

| Distro | Shell |
|--------|-------|
| Ubuntu, Debian, Mint, Fedora, Arch | bash |
| Kali (desde 2020.4) | zsh |
| macOS (desde Catalina) | zsh |

Cambiar de shell: `chsh -s /bin/bash` (o `/bin/zsh`) y reiniciar sesión.

## Navegación

```bash
pwd          # dónde estoy
ls           # qué hay aquí
ls -a        # incluye archivos ocultos
cd carpeta   # ruta relativa
cd /etc      # ruta absoluta (empieza con /)
cd ..        # subir un nivel
cd -         # volver al directorio anterior
cd ~         # ir a mi home
```

Atajos:
- `Tab` autocompleta (dos veces muestra opciones)
- `Ctrl+C` cancela
- `Ctrl+L` limpia pantalla
- `Ctrl+R` busca en el historial

## Manual e historial

```bash
man ls                  # manual (buscar con /texto, salir con q)
ls --help               # ayuda rápida
history                 # comandos anteriores
history | grep cd       # filtrar el historial
```

## Alias

Un alias es un apodo para un comando.

```bash
alias lab='cd ~/laboratorio'
```

Para que **no se pierda al cerrar la terminal**, se guarda en el archivo de config de tu shell y se recarga:

```bash
# En bash (Ubuntu):
echo "alias lab='cd ~/laboratorio'" >> ~/.bashrc
source ~/.bashrc

# En zsh (Kali):
echo "alias lab='cd ~/laboratorio'" >> ~/.zshrc
source ~/.zshrc
```

Ver tus alias: `alias`  |  Buscar uno: `alias | grep lab`

### Problema resuelto (el que me pasó a mí)

Creé `alias update="bash ~/actualizar.sh"` en `.bashrc` y al escribir `update` me dio `command not found`.

- **Causa**: estaba en zsh (Kali), y zsh no lee `.bashrc`.
- **Error extra**: al hacer `source ~/.bashrc` desde zsh salieron errores de `shopt` y `complete`, porque son comandos de bash.
- **Solución**: poner el alias en `~/.zshrc` y recargar con `source ~/.zshrc`.

### Alias para ambas shells

```bash
# Poner los alias en ~/.bash_aliases
# En ~/.zshrc agregar:
[ -f ~/.bash_aliases ] && source ~/.bash_aliases
```

## Tuberías y redirecciones

| Símbolo | Qué hace |
|---------|----------|
| `\|` | La salida de un comando entra al siguiente |
| `>` | Guarda la salida en un archivo (**sobrescribe**) |
| `>>` | Guarda la salida **al final** del archivo (añade) |
| `&&` | Ejecuta el segundo solo si el primero salió bien |

```bash
ls /etc | wc -l                              # cuántos elementos hay en /etc
ls /etc | head -5 > ~/laboratorio/a2_lista.txt   # guardar las 5 primeras líneas
cat ~/laboratorio/a2_lista.txt               # ver el archivo
history | tail -20 > ~/laboratorio/a2_comandos.txt
```

## Práctica

```bash
mkdir -p ~/laboratorio        # -p evita error si ya existe
cd ~/laboratorio && pwd
```

## Ejercicios del módulo (para resolver)

- [ ] Ir a `/var/log` y volver con `cd -` sin escribir la ruta
- [ ] Guardar `history | tail -20` en `~/laboratorio/a2_comandos.txt`
- [ ] Crear `tres.txt` con las 3 primeras líneas de `ls /usr/bin` (`|`, `head -3` y `>`)
- [ ] Explicar por escrito la diferencia entre `>` y `>>` (probar ambos sobre el mismo archivo)
- [ ] Usar `man ls` para ver cómo mostrar lo oculto y anotar la bandera

## Mini proyecto: bitácora

Archivo `~/laboratorio/a2_bitacora.md` con 10 comandos propios: qué hace cada uno + salida esperada + los alias que agregué.

## Autoevaluación

- [ ] ¿Llego a `/var/log` y vuelvo con `cd -`?
- [ ] ¿Busco algo en `man` y salgo?
- [ ] ¿Encadeno dos comandos con `|` y guardo el resultado en un archivo?
- [ ] ¿Sé qué hace cada alias sin mirar el archivo de config?

---

## Ver también

- [[MOC-programacion]]
- [[Ruta Bash]]
- [[Comandos de Linux]]
- [[01-Introducción a Bash]]
- [[Comodines (Wildcards) en Linux]]
