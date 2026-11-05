---
aliases: ["Arrays en Bash", "Listas en Bash"]
title: Arrays en Bash
area: programacion
tipo: lenguaje
tags: [programacion, bash]
created: 2026-10-25
updated: 2026-10-25
related: ["[[MOC-programacion]]", "[[Ruta Bash]]", "[[02-Variables y tipos de datos en Bash|Variables y tipos de datos en Bash]]", "[[05-Bucles en Bash|Bucles en Bash]]"]
---

# Arrays en Bash

Bash tiene dos tipos de colecciones: **indexados** (listas) y **asociativos** (diccionarios clave→valor).

## 1. Arrays indexados

```bash
sistemas=("Ubuntu" "Debian" "Arch" "CentOS")

echo ${sistemas[0]}
echo ${sistemas[@]}
echo ${#sistemas[@]}

sistemas+=("Fedora")
unset sistemas[2]
echo ${sistemas[@]:1:2}
```

|Expresión|Qué hace|
|---|---|
|`${sistemas[0]}`|primer elemento (el índice empieza en 0)|
|`${sistemas[@]}`|todos los elementos|
|`${#sistemas[@]}`|cantidad de elementos|
|`sistemas+=("Fedora")`|añadir al final|
|`unset sistemas[2]`|eliminar un elemento|
|`${sistemas[@]:1:2}`|desde el índice 1, toma 2|

```bash
for distro in "${sistemas[@]}"; do
    echo "$distro"
done
```

Sin comillas, `"${arr[@]}"` se colapsa en un solo string si un elemento tiene espacios.

No existen arrays multidimensionales. Para matrices se simula con `matriz[fila_columna]`.

## 2. Arrays asociativos

Hay que declararlos con `declare -A`:

```bash
declare -A equipo
equipo[cpu]="Intel Core i5"
equipo[ram]="8GB"
equipo[disco]="SSD 512GB"

echo ${equipo[cpu]}
echo ${!equipo[@]}
echo ${equipo[@]}
```

`${!equipo[@]}` devuelve las claves y `${equipo[@]}` los valores.

```bash
declare -A count=([a]=1 [b]=2)
```

## 3. Uso típico: recorrer resultados

```bash
ips=(192.168.1.1 192.168.1.2 192.168.1.3)

for ip in "${ips[@]}"; do
    ping -c 1 "$ip" &> /dev/null && echo "[UP] $ip"
done
```

## Ver también

- [[MOC-programacion]]
- [[Ruta Bash]]
- [[02-Variables y tipos de datos en Bash|Variables y tipos de datos en Bash]]
- [[05-Bucles en Bash|Bucles en Bash]]
