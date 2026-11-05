---
aliases: ["POO simulada en Bash"]
title: POO simulada en Bash
area: programacion
tipo: lenguaje
tags: [programacion, bash]
created: 2026-10-25
updated: 2026-10-25
related: ["[[MOC-programacion]]", "[[Ruta Bash]]", "[[06-Funciones en Bash|Funciones en Bash]]", "[[03-Arrays en Bash|Arrays en Bash]]"]
---

# POO simulada en Bash

Bash no tiene `class` ni herencia. Se simula combinando **arrays asociativos** (el objeto) y **funciones** (los métodos).

## 1. Equivalencias

|POO|En Bash|
|---|---|
|Clase|Prefijo de nombre en las funciones (`servidor_ping`)|
|Objeto|Array asociativo (`declare -A obj`)|
|Atributo|Clave del array (`obj[ip]`)|
|Método|Función que recibe el objeto como argumento|
|Constructor|Función que devuelve el objeto|
|Encapsulamiento|Prefijo de nombre + `local`|

## 2. Ejemplo: la clase `Servidor`

```bash
#!/bin/bash

crear_servidor() {
    echo "ip=$1"
    echo "estado=$2"
}

servidor_ping() {
    local -n _srv="$1"
    ping -c 1 "${_srv[ip]}" &> /dev/null \
        && echo "[OK] Servidor activo" \
        || echo "[FAIL] Sin respuesta"
}

servidor_estado() {
    local -n _srv="$1"
    echo "Estado actual: ${_srv[estado]}"
}

declare -A mi_servidor
while IFS='=' read -r k v; do
    [[ -n "$k" ]] && mi_servidor[$k]="$v"
done < <(crear_servidor "192.168.1.100" "Activo")

servidor_estado mi_servidor
servidor_ping   mi_servidor
```

`local -n` (nameref, Bash ≥ 4.3) hace que la función trabaje sobre el objeto original y no sobre una copia.

## 3. Versión sin nameref

```bash
declare -A disco
disco[total]="512GB"
disco[libre]="120GB"

disco_info() {
    echo "SSD → ${disco[total]} totales, ${disco[libre]} libres"
}

disco_info
```

Aquí la función lee la variable global directamente: más simple, pero acoplada a un objeto concreto.

## 4. "Herencia" por composición

Sin herencia real, se copian los atributos:

```bash
crear_servidor_web() {
    echo "ip=$1"
    echo "estado=Activo"
    echo "tipo=web"
}
```

## 5. Límites

- No hay privacidad: cualquier parte del script toca cualquier clave.
- Bash 3.x (macOS por defecto) no soporta `declare -A` completo ni `local -n`.
- Si necesitas POO de verdad: [[15-POO en Java|POO en Java]] o [[26-Clases y objetos en Python|Clases y objetos en Python]].

## Ver también

- [[MOC-programacion]]
- [[Ruta Bash]]
- [[06-Funciones en Bash|Funciones en Bash]]
- [[03-Arrays en Bash|Arrays en Bash]]
