---
aliases: ["Funciones en Bash"]
title: Funciones en Bash
area: programacion
tipo: lenguaje
tags: [programacion, bash]
created: 2026-10-25
updated: 2026-10-25
related: ["[[MOC-programacion]]", "[[Ruta Bash]]", "[[05-Bucles en Bash|Bucles en Bash]]", "[[07-POO simulada en Bash|POO simulada en Bash]]"]
---

# Funciones en Bash

Las funciones encapsulan bloques reutilizables. Los parámetros se reciben posicionalmente, igual que en un script.

## 1. Declaración

```bash
saludar() {
    echo "Hola, $1"
}

function saludar {
    echo "Hola, $1"
}

saludar "Ferney"
```

## 2. Parámetros

```bash
calcular_area() {
    local base="$1"
    local altura="$2"
    echo $((base * altura))
}

resultado=$(calcular_area 5 10)
```

```bash
contar() {
    echo "Recibí $# argumentos: $@"
}

contar a b c
```

## 3. `return` vs `echo`

|Qué|Para qué|
|---|---|
|`echo`|devolver un **valor**, se captura con `$(...)`|
|`return N`|devolver un **estado**: `0` = éxito, `1..255` = error|

```bash
existe_usuario() {
    id "$1" &> /dev/null
    return $?
}

if existe_usuario "root"; then
    echo "El usuario existe"
fi
```

## 4. Ámbito

Por defecto las variables son **globales**. Con `local` se limitan a la función:

```bash
variable="global"

mi_funcion() {
    local variable="solo aquí"
}

mi_funcion
echo "$variable"
```

Regla práctica: siempre `local` en las variables internas.

## 5. Definición

Bash lee el script completo antes de ejecutarlo, pero una función solo puede llamar a otra que esté **definida arriba**:

```bash
procesar() {
    validar "$1"
    guardar "$1"
}

validar() { :; }
guardar() { :; }
```

## 6. Ejemplo: menú de reconocimiento

```bash
#!/bin/bash

scan_rapido()   { nmap -sn "$1"; }
scan_puertos()  { nmap -p- "$1"; }
scan_version()  { nmap -sV -sC "$1"; }

case "$1" in
    rapido)   scan_rapido "$2"   ;;
    puertos)  scan_puertos "$2"  ;;
    version)  scan_version "$2"  ;;
    *)        echo "Uso: $0 {rapido|puertos|version} <ip>" ;;
esac
```

## Ver también

- [[MOC-programacion]]
- [[Ruta Bash]]
- [[05-Bucles en Bash|Bucles en Bash]]
- [[07-POO simulada en Bash|POO simulada en Bash]]
