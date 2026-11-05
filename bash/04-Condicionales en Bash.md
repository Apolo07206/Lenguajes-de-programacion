---
aliases: ["Condicionales en Bash"]
title: Condicionales en Bash
area: programacion
tipo: lenguaje
tags: [programacion, bash]
created: 2026-10-25
updated: 2026-10-25
related: ["[[MOC-programacion]]", "[[Ruta Bash]]", "[[02-Variables y tipos de datos en Bash|Variables y tipos de datos en Bash]]", "[[05-Bucles en Bash|Bucles en Bash]]"]
---

# Condicionales en Bash

Se evalúan dentro de corchetes dobles `[[ ... ]]`, que además de comparar admiten `&&`, `||` y `!`.

## 1. `if`, `elif`, `else`

```bash
edad=18

if [[ $edad -ge 18 ]]; then
    echo "Eres mayor de edad."
elif [[ $edad -eq 17 ]]; then
    echo "Casi eres mayor de edad."
else
    echo "Eres menor de edad."
fi
```

Entre la condición y `then` siempre hace falta `;` o un salto de línea.

## 2. Operadores

### Numéricos

|Operador|Significado|
|---|---|
|`-eq`|igual|
|`-ne`|distinto|
|`-gt`|mayor que|
|`-ge`|mayor o igual|
|`-lt`|menor que|
|`-le`|menor o igual|

### Cadenas

|Operador|Significado|
|---|---|
|`=`|igual|
|`!=`|distinto|
|`-z`|vacía|
|`-n`|no vacía|

```bash
if [[ -z "$password" ]]; then
    echo "La contraseña no puede estar vacía"
fi
```

Comillar las variables es obligatorio: si viene vacía y no la comillas, Bash la borra antes de evaluar.

### Archivos y directorios

|Test|Comprueba|
|---|---|
|`-e ruta`|existe|
|`-d ruta`|es directorio|
|`-f ruta`|es archivo regular|
|`-r / -w / -x`|legible / escribible / ejecutable|
|`-s ruta`|existe y no está vacío|
|`-L ruta`|es enlace simbólico|

### Lógicos

```bash
[[ $a -gt 5 && $b -lt 10 ]]
[[ $a -eq 1 || $b -eq 2 ]]
[[ ! -f "$ruta" ]]
```

## 3. `case`

```bash
case $accion in
    "start" | "run")
        echo "Iniciando servicio..."
        ;;
    "stop")
        echo "Deteniendo servicio..."
        ;;
    *)
        echo "Comando no reconocido."
        ;;
esac
```

- `;;` cierra cada rama.
- `*` es el `default`.
- Admite globbing: `*.txt)` coincide con cualquier `.txt`.

```bash
case $r in
    [sSyY]) echo "Reiniciando..." ;;
    *)      echo "Cancelado" ;;
esac
```

## 4. Cortocircuitos

```bash
[[ -d "$dir" ]] || mkdir -p "$dir"
[[ -f "$f" ]] && cat "$f"
comando && echo OK || echo FAIL
```

## Ver también

- [[MOC-programacion]]
- [[Ruta Bash]]
- [[02-Variables y tipos de datos en Bash|Variables y tipos de datos en Bash]]
- [[05-Bucles en Bash|Bucles en Bash]]
