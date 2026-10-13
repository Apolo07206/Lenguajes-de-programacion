---
title: Control de bucles en Python
area: programacion
tipo: lenguaje
tags: [programacion, python, bucles, control]
created: 2026-10-12
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Bucles for en Python]]", "[[Bucles while en Python]]", "[[Range en Python]]"]
---

# Control de bucles en Python

Cuatro herramientas para manejar cuándo cortar, saltear o rellenar bucles: `break`, `continue`, `pass` y el `else` de bucles (sí, los bucles tienen `else`).

## `break` — salir del bucle YA

```python
# Buscar un elemento y cortar cuando lo encuentres
numeros = [4, 8, 15, 16, 23, 42]
objetivo = 15

for n in numeros:
    if n == objetivo:
        print("Encontrado:", n)
        break          # no sigue iterando, no necesita buscar más
    print("Viendo:", n)
# Viendo: 4 / Viendo: 8 / Encontrado: 15

# En while, el break es lo que salva a los bucles infinitos
while True:
    txt = input("Escribe 'fin' para parar: ")
    if txt == "fin":
        break
```

## `continue` — saltar a la siguiente vuelta

```python
# Saltar los impares
for n in range(1, 8):
    if n % 2 != 0:
        continue       # ignora el resto del bloque y va a la próxima vuelta
    print("Par:", n)
# Par: 2 / Par: 4 / Par: 6

# Equivalente sin continue (con else)
for n in range(1, 8):
    if n % 2 == 0:
        print("Par:", n)
```

> [!warning] continue en `while`
> En `while`, si hacés `continue` sin avanzar el contador antes, entrás en loop infinito (ver [[Bucles while en Python]]).

## `pass` — no hacer nada (placeholder)

```python
# pass es un "por ahora no pongo código": solo existe para que no haya error de sintaxis
for letra in "abc":
    if letra == "b":
        pass                     # nada, se ignora
    else:
        print(letra)
# a / c

# Muy usado para definir estructuras que completarás después
def mi_funcion_futura():
    pass          # sin esto: IndentationError

class Vacia:
    pass
```

## `else` en bucles (el que pocos conocen)

```python
# El else de un bucle se ejecuta SI el bucle terminó SIN break
for n in [2, 4, 6]:
    if n % 2 != 0:
        break
else:
    print("Todos eran pares")     # se imprime: no hubo break

for n in [2, 3, 6]:
    if n % 2 != 0:
        break
else:
    print("Todos eran pares")     # NO se imprime: hubo break

# Patrón clásico: ¿es primo?
num = 17
for i in range(2, num):
    if num % i == 0:
        print("No es primo")
        break
else:
    print("Es primo")             # sin break → primo
```

## Bucles anidados (patrones con `*` y tablas)

```python
# 1) Triángulo de asteriscos
n = 5
for fila in range(1, n + 1):
    print("*" * fila)
# *
# **
# ***
# ****
# *****

# 2) Triángulo invertido
for fila in range(n, 0, -1):
    print("*" * fila)

# 3) Cuadrado lleno n x n
for fila in range(n):
    for col in range(n):        # bucle interno: corre COMPLETO por cada fila
        print("* ", end="")
    print()                     # salto de línea al terminar cada fila
# * * * * *
# * * * * *
# * * * * *
# * * * * *
# * * * * *

# 4) Tabla de multiplicar del 1 al 10 completa
for n in range(1, 11):
    for i in range(1, 11):
        print(f"{n}x{i}={n*i:2d}", end="  ")
    print()
```

```python
# break en bucle anidado: SOLO rompe el bucle donde está
for fila in range(3):
    for col in range(3):
        if fila == 1 and col == 1:
            break              # solo rompe el for de columnas
        print(f"({fila},{col})")
# (0,0) (0,1) (0,2) / (1,0) / (2,0) (2,1) (2,2)
# (1,1) y (1,2) se saltan porque el break cortó la fila 1
```

> [!tip] Para romper el bucle EXTERNO
> Usá una bandera: `encontrado = False`, en el `break` del interno seteá `encontrado = True`, y agregá `if encontrado: break` en el externo.

## Ver también

- [[Ruta Python]]
- [[Bucles for en Python]]
- [[Bucles while en Python]]
- [[Range en Python]]
- [[MOC-programacion]]

## Problema resuelto

**Enunciado:** Imprimir la tabla de multiplicar del 1 al 10, pero detener todo en la tabla del 5 (sin imprimir su fila completa) usando bandera + break anidado.

**Solución paso a paso:**
1. Bandera `parar = False`.
2. Bucle externo por tabla; si `parar` está en True, `break`.
3. Bucle interno por filas; antes de imprimir el 5x5 → setear bandera y `break`.

```python
parar = False

for n in range(1, 11):
    if parar:
        break                       # corta el bucle EXTERNO
    for i in range(1, 11):
        if n == 5:
            parar = True
            break                   # corta el bucle INTERNO
        print(f"{n}x{i}={n*i:2d}", end="  ")
    print()
# Imprime tablas 1 a 4 y se corta antes de la 5
```

## Problema propio

**Enunciado:** Con `for` anidado, imprimir el "rombo" de asteriscos de altura 5 (fila1: 1*, fila2: 3*, fila3: 5*, fila4: 3*, fila5: 1*) — con espacios para centrarlo.

**Pista:** Dos fases: `for fila in range(1, 6, 2)` (crece) y `for fila in range(3, 0, -2)` (decrece). Para centrar: `" " * ((n - fila) // 2) + "*" * fila`.
