---
title: Range en Python
area: programacion
tipo: lenguaje
tags: [programacion, python, bucles, range]
created: 2026-10-12
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Bucles for en Python]]", "[[Control de bucles en Python]]"]
---

# Range en Python

`range()` genera una **secuencia de números** para usar casi siempre con `for`. No es una lista: es un rango perezoso (genera los números sobre la marcha).

## Las3 formas (los 3 números)

```python
# 1) range(stop) → de 0 hasta stop-1
for i in range(5):
    print(i, end=" ")
# 0 1 2 3 4        ← NO llega al 5

# 2) range(start, stop) → de start hasta stop-1
for i in range(2, 6):
    print(i, end=" ")
# 2 3 4 5          ← NO llega al 6

# 3) range(start, stop, step) → con incremento 'step'
for i in range(0, 10, 2):
    print(i, end=" ")
# 0 2 4 6 8        ← de 2 en 2, no llega al 10
```

| Forma | Significado | Ejemplo | Sale |
|---|---|---|---|
| `range(stop)` | empieza en 0, de a 1, hasta stop-1 | `range(4)` | 0 1 2 3 |
| `range(start, stop)` | desde start, de a 1, hasta stop-1 | `range(1, 4)` | 1 2 3 |
| `range(start, stop, step)` | desde start, de a step, hasta stop-1 | `range(0, 6, 2)` | 0 2 4 |

> [!warning] El final es EXCLUSIVO
> El número `stop` **nunca se imprime**. `range(1, 5)` llega hasta el 4. Es la confusión #1.

## Por qué `range(1, 0, 2)` no imprime nada

```python
for i in range(1, 0, 2):
    print(i)
# (no imprime NADA)

# ¿Por qué? start=1, stop=0, step=+2
# Python avanza: 1 → 3 → 5 → ... pero eso ya PASÓ el stop=0.
# La condición es: "imprimir mientras vayamos HACIA stop".
# Como start (1) ya está del otro lado de stop (0) con paso POSITIVO,
# la secuencia está vacía. Ni siquiera entra al bloque.

print(list(range(1, 0, 2)))    # [] → rango vacío, confirmado
```

## Contar hacia atrás con paso negativo

```python
# ¡ATENCIÓN! Si vas hacia atrás, start debe ser MAYOR que stop
for i in range(10, 0, -1):
    print(i, end=" ")
# 10 9 8 7 6 5 4 3 2 1      ← llega al 1, no al 0 (stop excluido)

# Cuenta regresiva de un desafío
for i in range(3, 0, -1):
    print(f"...{i}")
print("¡Despegue! 🚀")
# ...3 / ...2 / ...1 / ¡Despegue!

# Con step negativo y rango vacío (al revés de antes)
print(list(range(0, 10, -1)))   # [] → empezar en 0 hacia el 10 con -1 no tiene sentido
```

## Trucos y usos típicos

```python
# range produce números, no strings
for i in range(3):
    print("Vuelta", i)

# range con len() para recorrer con índice cuando lo necesités
frutas = ["manzana", "pera", "uva"]
for i in range(len(frutas)):
    print(i, frutas[i])

# Convertir a lista (para ver el rango o guardarlo)
print(list(range(5)))         # [0, 1, 2, 3, 4]
print(list(range(2, 7)))      # [2, 3, 4, 5, 6]
print(list(range(0, 10, 3)))  # [0, 3, 6, 9]
print(list(range(5, 0, -1)))  # [5, 4, 3, 2, 1]

# Sumar del 1 al 100
print(sum(range(1, 101)))     # 5050

# Generar una tabla de multiplicar
n = 7
for i in range(1, 11):
    print(f"{n} x {i} = {n * i}")
```

> [!tip] ¿range o lista?
> No conviertas a `list` si solo vas a iterar: `for i in range(...)` es directo y no gasta memoria. `list(range(...))` solo cuando necesites la lista como tal.

## Ver también

- [[Ruta Python]]
- [[Bucles for en Python]]
- [[Control de bucles en Python]]
- [[MOC-programacion]]

## Problema resuelto

**Enunciado:** Imprimir con `range` la cuenta regresiva de un cohete: `10 9 8 ... 1 ¡Despegue!` y después los primeros 5 números pares (0 2 4 6 8).

**Solución paso a paso:**
1. Cuenta regresiva: `range(10, 0, -1)` (de 10 al 1, paso -1).
2. Pares: `range(0, 10, 2)` (del 0 al 8, de 2 en 2).

```python
for i in range(10, 0, -1):
    print(i, end=" ")
print("¡Despegue!")

print("Pares:", end=" ")
for i in range(0, 10, 2):
    print(i, end=" ")
print()
```

## Problema propio

**Enunciado:** Imprimir con `for` + `range` la tabla de multiplicar del número que pida el usuario, del 1 al 10, en formato `5 x 3 = 15`.

**Pista:** `range(1, 11)` y una f-string: `f"{n} x {i} = {n * i}"`.
