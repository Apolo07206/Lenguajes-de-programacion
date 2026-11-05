---
aliases: ["Introducción a Bash"]
title: Introducción a Bash
area: programacion
tipo: lenguaje
tags: [programacion, bash]
created: 2026-10-25
updated: 2026-10-25
related: ["[[MOC-programacion]]", "[[Ruta Bash]]", "[[02-Variables y tipos de datos en Bash|Variables y tipos de datos en Bash]]", "[[Concepto de binarios y scripts]]"]
---

# Introducción a Bash

Bash (**B**ourne **A**gain **Sh**ell) es el intérprete de comandos por defecto en la mayoría de distribuciones Linux. Se usa para interactuar con el sistema, automatizar tareas y, en ciberseguridad, para reconnaissance y post-explotación (living off the land).

## 1. Estructura básica de un script

Todo script comienza con un **shebang** que indica qué intérprete debe usarlo:

```bash
#!/bin/bash

echo "¡Hola, mundo desde Bash!"
```

- `#!` solo tiene sentido en la primera línea.
- `#` fuera de esa primera línea es un comentario.

## 2. Permisos y ejecución

```bash
chmod +x mi_script.sh
./mi_script.sh
bash mi_script.sh
```

`./` es obligatorio para ejecutar en el directorio actual. La tercera línea funciona sin dar permiso `+x`.

## 3. Binarios vs scripts

```bash
file /bin/ls
head -1 /usr/bin/apt
```

`file` responde ELF para el primero y el segundo devuelve `#!/bin/sh`, es decir un script.

| |Binario|Script|
|---|---|---|
|Naturaleza|código máquina|texto plano|
|Previo|compilación|ninguno|
|Ejecución|directa por la CPU|necesita intérprete|
|Velocidad|muy alta|más lenta|
|Ejemplos|`/bin/ls`, `/bin/grep`|`/bin/bash`, `.py`, `.sh`|

Teoría completa en [[Concepto de binarios y scripts]].

## 4. Laboratorio inicial

Antes de programar, conoce el entorno:

```bash
mkdir -p ~/laboratorio
lscpu | head -12
free -h
lsblk
ip a
```

Los comandos están detallados en [[08-Comandos básicos para ciberseguridad en Bash|Comandos básicos para ciberseguridad]] y el hardware en [[Arquitectura Básica de Hardware]].

## 5. Primeros comandos

```bash
echo "texto"
pwd
clear
history
man ls
```

`Ctrl`+`C` corta el comando que se esté ejecutando.

## Ver también

- [[MOC-programacion]]
- [[Ruta Bash]]
- [[02-Variables y tipos de datos en Bash|Variables y tipos de datos en Bash]]
- [[Concepto de binarios y scripts]]
