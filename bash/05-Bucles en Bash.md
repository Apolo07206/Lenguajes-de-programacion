---
aliases: ["Bucles en Bash"]
title: Bucles en Bash
area: programacion
tipo: lenguaje
tags: [programacion, bash]
created: 2026-10-25
updated: 2026-10-25
related: ["[[MOC-programacion]]", "[[Ruta Bash]]", "[[04-Condicionales en Bash|Condicionales en Bash]]", "[[06-Funciones en Bash|Funciones en Bash]]"]
---

# Bucles en Bash

Tres estructuras: `for` (recorrer), `while` (mientras se cumpla) y `until` (hasta que se cumpla).

## 1. `for`

```bash
for item in Docker Git Python Bash; do
    echo "$item"
done

for i in {1..5}; do
    echo "$i"
done

for i in {1..10..2}; do
    echo "$i"
done
```

```bash
for archivo in "$@"; do
    echo "Procesando: $archivo"
done

for ip in $(cat ips.txt); do
    ping -c 1 "$ip" &> /dev/null && echo "[UP] $ip"
done

for f in /var/log/*.log; do
    [[ -f "$f" ]] || continue
    echo "$f"
done
```

## 2. `while`

```bash
contador=1

while [[ $contador -le 3 ]]; do
    echo "Contador: $contador"
    ((contador++))
done
```

```bash
while IFS= read -r linea; do
    echo "$linea"
done < usuarios.txt
```

```bash
while ping -c 1 -W 1 192.168.1.1 &> /dev/null; do
    echo "$(date +%T) host accesible"
    sleep 5
done
```

## 3. `until`

```bash
intento=1

until [[ $intento -gt 3 ]]; do
    echo "Intento $intento"
    ((intento++))
done
```

## 4. Control de bucles

```bash
for i in {1..10}; do
    [[ $i -eq 5 ]] && continue
    [[ $i -gt 8 ]] && break
    echo "$i"
done
```

|Palabra|Efecto|
|---|---|
|`continue`|salta a la siguiente iteración|
|`break`|sale del bucle completo|

En bucles anidados `break` solo rompe el nivel más externo.

## 5. Bucle infinito

```bash
while true; do
    date
    sleep 1
done
```

## Ver también

- [[MOC-programacion]]
- [[Ruta Bash]]
- [[04-Condicionales en Bash|Condicionales en Bash]]
- [[06-Funciones en Bash|Funciones en Bash]]
