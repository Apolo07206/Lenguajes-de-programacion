---
aliases: ["Variables y tipos de datos en Bash"]
title: Variables y tipos de datos en Bash
area: programacion
tipo: lenguaje
tags: [programacion, bash]
created: 2026-10-25
updated: 2026-10-25
related: ["[[MOC-programacion]]", "[[Ruta Bash]]", "[[01-Introducción a Bash|Introducción a Bash]]", "[[03-Arrays en Bash|Arrays en Bash]]"]
---

# Variables y tipos de datos en Bash

Bash tiene **tipado dinámico débil**: por defecto todo es texto. No se declara el tipo, solo el nombre.

## 1. Declarar variables

```bash
nombre="Ferney"
edad=20
readonly VERSION="1.0.0"
unset nombre
```

- Sin espacios alrededor del `=`. `nombre = "Ferney"` es un error.
- Al escribir no se usa `$`; al leer sí: `echo $nombre`.
- Si viene texto pegado, usa llaves: `echo "${nombre}apellido"`.

```bash
echo "Usuario: $nombre, Edad: $edad"
```

## 2. Variables especiales

|Variable|Contenido|
|---|---|
|`$0`|Nombre del script|
|`$1`, `$2` …|Argumentos posicionales|
|`$#`|Cantidad de argumentos|
|`$@`|Todos los argumentos|
|`$?`|Código de salida del último comando (`0` = éxito)|
|`$$`|PID del script|
|`$PWD`|Directorio actual|

```bash
#!/bin/bash
# ./saludo.sh Ana 25
echo "Hola $1, tienes $2 años"
echo "Recibí $# argumentos"
```

## 3. Aritmética

```bash
num1=10
num2=3

echo $((num1 + num2))    # 13
echo $((num1 / num2))    # 3
echo $((num1 % num2))    # 1
((num1++))
```

La división es entera (se trunca, no redondea). El incremento se hace con `(( ))` sin `$`.

Para decimales no hay soporte nativo: usa `echo "scale=2; 10/3" | bc`.

## 4. Entrada de datos

```bash
read -p "Usuario: " usuario
read -sp "Contraseña: " pass
```

`-p` escribe el prompt y `-s` oculta lo que se escribe, ideal para contraseñas.

## Ver también

- [[MOC-programacion]]
- [[Ruta Bash]]
- [[03-Arrays en Bash|Arrays en Bash]]
- [[04-Condicionales en Bash|Condicionales en Bash]]
