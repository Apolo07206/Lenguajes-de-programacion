---
title: Introducción a Python
area: programacion
tipo: lenguaje
tags: [programacion, python, fundamentos]
created: 2026-10-12
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Variables y tipos de datos en Python]]", "[[Condicionales en Python]]"]
---

# Introducción a Python

Python es un lenguaje interpretado (no se compila), de tipado dinámico y con una sintaxis minimalista: la indentación (espacios al inicio de la línea) es parte de la sintaxis, no decoración. Se usa para scripting, ciber, web, datos y automatización.

## Instalar

```bash
# Windows / Linux / Mac: desde python.org (marcar "Add to PATH" en Windows)
# Verificar instalación (en Linux suele ser python3)
python --version
# o
python3 --version
```

> [!tip] Para ciber
> Kali Linux y la mayoría de distros traen Python preinstalado. Nunca borres el python del sistema, instala aparte (ver [[Módulos y librerías en Python]]).

## Ejecutar un script

```bash
# Guardas el código en un archivo y lo ejecutas
python mi_script.py

# También existe el REPL: escribes python y pruebas código en vivo
python
>>> print("hola desde el REPL")
hola desde el REPL
>>> exit()   # salís del REPL
```

```python
# mi_script.py — el "hola mundo" mínimo
print("Hola Ferney")
```

## `print()` — lo que más vas a usar

```python
print("Hola mundo")                    # imprime texto
print("Hola", "mundo")                 # varios args: los une con espacio
print("Hola", "mundo", sep=" - ")      # cambias el separador → Hola - mundo
print("sin salto", end="")             # no mete salto de línea al final
print(" y seguimos")                   # queda en la misma línea → "sin salto y seguimos"
print()                                # línea vacía
```

## Comentarios

```python
# Esto es un comentario: Python lo ignora
print("esto sí se ejecuta")  # comentario al final de la línea

"""
Esto es un docstring / comentario multilinea.
Se usa entre tres comillas dobles.
"""
```

## Indentación: aquí Python manda

```python
# CORRECTO: el bloque de if va indentado (4 espacios por convención)
if True:
    print("indentado con 4 espacios")
    print("todo esto está DENTRO del if")

print("esto queda FUERA del if")

# MAL: si metes menos espacios, Python no sabe dónde termina el bloque
# if True:
# print("IndentationError: unindent does not match")  ← error clásico
```

> [!warning] Error clásico
> `IndentationError` = mezclaste espacios con tabulaciones o indentaste mal. Usa siempre **4 espacios** y no mezcles con TABs.

## Resumen rápido del flujo

1. Escribís el `.py`
2. `python archivo.py` lo interpreta línea por línea
3. Indentación define los bloques, `#` comenta, `print()` muestra

## Ver también

- [[Ruta Python]]
- [[Variables y tipos de datos en Python]]
- [[Condicionales en Python]]
- [[MOC-programacion]]

## Problema resuelto

**Enunciado:** Crear un script que pida tu nombre y tu edad, y salude con formato: `Hola Ferney, tienes 20 años.`

**Solución paso a paso:**
1. Pedimos el nombre con `input()` (siempre devuelve texto).
2. Pedimos la edad y la convertimos a número con `int()`.
3. Mostramos con `print()` concatenando.

```python
nombre = input("Ingresa tu nombre: ")
edad = int(input("Ingresa tu edad: "))
print("Hola " + nombre + ", tienes " + str(edad) + " años.")
```

## Problema propio

**Enunciado:** Crear un script que imprima por consola tu ciudad, tu hobby y un emoji, cada uno en una línea distinta, y al final una línea vacía antes de despedirse.

**Pista:** `print()` ya mete salto de línea solo; para la línea vacía llamá a `print()` sin argumentos.
