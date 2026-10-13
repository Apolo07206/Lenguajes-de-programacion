---
title: Ordenamiento de matrices en Python
area: programacion
tipo: lenguaje
tags: [programacion, python, matrices, ordenamiento]
created: 2026-10-12
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Matrices en Python]]", "[[Ordenamiento de listas en Python]]", "[[Matrices en Java]]"]
---

# Ordenamiento de matrices en Python

Ordenar matrices es combinación de lo que ya sabés: `sort()`/`sorted()` (ver [[Ordenamiento de listas en Python]]) aplicado fila por fila, columna por columna, o a todos los elementos. Todo con listas de listas.

## Ordenar cada fila

```python
m = [[5, 1, 3],
     [9, 2, 8],
     [4, 7, 6]]

for fila in m:
    fila.sort()             # sort() es in-place: modifica la fila (que es una lista)

print(m)
# [[1, 3, 5],
#  [2, 8, 9],
#  [4, 6, 7]]

# Con list comprehension (devuelve NUEVA matriz, sin tocar la original)
m2 = [[5, 1, 3], [9, 2, 8], [4, 7, 6]]
ordenada = [sorted(fila) for fila in m2]
print(ordenada)             # [[1, 3, 5], [2, 8, 9], [4, 6, 7]]
```

## Ordenar por columna (con key=lambda)

```python
# Las matrices se guardan por FILAS: ordenar "por columna" = transponer,
# ordenar, y transponer de vuelta.
m = [[5, 1, 3],
     [9, 2, 8],
     [4, 7, 6]]

# Ordenar según la columna 1 (los valores del medio: 1, 2, 7)
ordenada = sorted(m, key=lambda fila: fila[1])
print(ordenada)
# [[5, 1, 3],    ← fila cuyo elemento de columna 1 es el más chico (1)
#  [9, 2, 8],    ← después (2)
#  [4, 7, 6]]    ← al final (7)

# ¿Querés ordenar los VALORES de una columna y que las filas acompañen?
# Es el patrón "ordenar filas por la suma" de abajo: el key apunta a lo que
# querés comparar y la fila entera se mueve junto.
```

## Ordenar todos los elementos de la matriz

```python
m = [[5, 1, 3],
     [9, 2, 8],
     [4, 7, 6]]

# 1) Aplanar → ordenar → reacomodar en la misma forma
plana = [n for fila in m for n in fila]
plana.sort()

filas, cols = len(m), len(m[0])
ordenada = []
for i in range(filas):
    inicio = i * cols
    ordenada.append(plana[inicio : inicio + cols])

print(ordenada)
# [[1, 2, 3],
#  [4, 5, 6],
#  [7, 8, 9]]

# En una línea (misma lógica)
n = len(plana) // filas
rearmada = [plana[i*n : (i+1)*n] for i in range(filas)]
print(rearmada)             # [[1,2,3],[4,5,6],[7,8,9]]
```

## Ordenar filas por su suma

```python
# Cada fila se mueve COMPLETA según la suma de sus elementos
m = [[5, 1, 3],      # suma 9
     [9, 2, 8],      # suma 19
     [4, 7, 6]]      # suma 17

# De menor a mayor suma
por_suma = sorted(m, key=lambda fila: sum(fila))
print(por_suma)      # [[5,1,3], [4,7,6], [9,2,8]]   (9 → 17 → 19)

# De mayor a menor suma
print(sorted(m, key=lambda fila: sum(fila), reverse=True))
# [[9,2,8], [4,7,6], [5,1,3]]
```

## Ordenar según otro criterio (largo, máximo de cada fila...)

```python
# El key puede ser CUALQUIER función sobre la fila
m = [[1, 2, 3],
     [10],
     [5, 5]]

# Por cantidad de elementos (fila más corta primero)
print(sorted(m, key=len))              # [[10], [5, 5], [1, 2, 3]]

# Por el máximo de cada fila
print(sorted(m, key=max))              # [[1, 2, 3], [5, 5], [10]]
```

> [!tip] Regla general
> Pensá la matriz como lista de filas (que son listas). Cualquier operación de listas con `key=` funciona sobre las filas; para operar "por columna", transponé primero (ver [[Matrices en Python]]).

## Resumen

| Qué querés | Cómo |
|---|---|
| Ordenar cada fila | `[sorted(fila) for fila in m]` |
| Ordenar filas por una columna | `sorted(m, key=lambda f: f[col])` |
| Ordenar filas por su suma | `sorted(m, key=sum)` |
| Ordenar todos los elementos | aplanar → sort → rearmar por trozos |
| Ordenar una columna real | transponer → ordenar → transponer |

## Ver también

- [[Ruta Python]]
- [[Matrices en Python]]
- [[Ordenamiento de listas en Python]]
- [[MOC-programacion]]

## Problema resuelto

**Enunciado:** Dada la matriz de notas `[[3.1, 4.0], [2.5, 3.8], [4.5, 4.9]]` (3 estudiantes x 2 pruebas): ordenar las filas por el **promedio** de cada estudiante, de mayor a menor.

**Solución paso a paso:**
1. `sorted()` con `key=lambda fila: sum(fila)/len(fila)` (el promedio).
2. `reverse=True` para mayor a menor.

```python
notas = [[3.1, 4.0],
         [2.5, 3.8],
         [4.5, 4.9]]

ordenado = sorted(notas, key=lambda fila: sum(fila) / len(fila), reverse=True)

for fila in ordenado:
    prom = sum(fila) / len(fila)
    print(f"Promedio {prom:.2f}: {fila}")
# Promedio 4.70: [4.5, 4.9]
# Promedio 3.55: [3.1, 4.0]
# Promedio 3.15: [2.5, 3.8]
```

## Problema propio

**Enunciado:** Dada una matriz cuadrada 3x3: ordenar cada fila de menor a mayor, y luego mostrar cuál fila tiene la suma más alta (sin usar `sorted` para eso último, recorriendo con bucle).

**Pista:** `[sorted(f) for f in m]` para el primer paso; para la suma más alta, compará `sum(fila)` guardando el índice del máximo.
