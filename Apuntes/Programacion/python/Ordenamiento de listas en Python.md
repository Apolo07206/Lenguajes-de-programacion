---
title: Ordenamiento de listas en Python
area: programacion
tipo: lenguaje
tags: [programacion, python, listas, ordenamiento]
created: 2026-10-12
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Listas en Python]]", "[[Algoritmos con listas en Python]]", "[[Algoritmos de Ordenamientos en Java]]"]
---

# Ordenamiento de listas en Python

Dos formas ordenadas de ordenar: `sort()` (modifica la lista original) y `sorted()` (devuelve una copia ordenada). Y la forma manual: el **bubble sort** (ordenamiento burbuja), para entender qué pasa por dentro.

## `sort()` vs `sorted()`

```python
numeros = [41, 12, 9, 74, 3]

# .sort() → ordena IN-PLACE (modifica la lista y no retorna nada)
numeros.sort()
print(numeros)              # [3, 9, 12, 41, 74]

numeros.sort(reverse=True)  # de MAYOR a menor
print(numeros)              # [74, 41, 12, 9, 3]

# sorted() → devuelve una NUEVA lista ordenada, la original queda igual
original = [41, 12, 9, 74, 3]
copia_ordenada = sorted(original)
print(copia_ordenada)       # [3, 9, 12, 41, 74]
print(original)             # [41, 12, 9, 74, 3]  ← intacta

copia_desc = sorted(original, reverse=True)
print(copia_desc)           # [74, 41, 12, 9, 3]

# OJO: x = lista.sort() deja x en None (retorna None, como reverse())
```

## Ordenar strings

```python
palabras = ["banana", "manzana", "uva", "cereza"]

palabras.sort()                          # alfabéticamente A-Z
print(palabras)                          # ['banana', 'cereza', 'manzana', 'uva']

palabras.sort(reverse=True)              # Z-A
print(palabras)                          # ['uva', 'manzana', 'cereza', 'banana']

# Ignorar mayúsculas al ordenar (sin key, "Zebra" va antes que "apple")
nombres = ["ana", "Zoe", "luis", "Carla"]
nombres.sort(key=str.lower)
print(nombres)                           # ['ana', 'Carla', 'luis', 'Zoe']
```

## `key=` — ordenar por criterio

```python
# key recibe una función y ordena según lo que esa función devuelva
personas = [("Ana", 25), ("Luis", 20), ("Marta", 30)]

# Por nombre (índice 0 de cada tupla)
personas.sort(key=lambda p: p[0])
print(personas)   # [('Ana', 25), ('Luis', 20), ('Marta', 30)]

# Por edad (índice 1)
personas.sort(key=lambda p: p[1])
print(personas)   # [('Luis', 20), ('Ana', 25), ('Marta', 30)]

# De mayor a menor edad
personas.sort(key=lambda p: p[1], reverse=True)
print(personas)   # [('Marta', 30), ('Ana', 25), ('Luis', 20)]

# Ejemplo con strings: ordenar por largo
palabras = ["sol", "mariposa", "red", "python"]
palabras.sort(key=len)
print(palabras)   # ['red', 'sol', 'python', 'mariposa']

# Ordenar diccionarios por valor (muy usado)
stock = {"manzana": 12, "pera": 5, "uva": 30}
ordenado = sorted(stock, key=lambda k: stock[k])
print(ordenado)   # ['pera', 'manzana', 'uva'] (claves ordenadas por su valor)
```

> [!note] `key` no muta el valor
> `key` es solo el criterio de comparación; los datos siguen siendo los originales.

## Bubble sort a mano (paso a paso)

El más simple de los sorts: recorrés la lista comparando **elementos vecinos** y los intercambías si están mal ordenados. Cada pasada "flota" el mayor hacia el final (como una burbuja que sube).

```python
def burbuja(lista):
    n = len(lista)
    for i in range(n - 1):              # cada pasada asegura una posición más
        for j in range(n - 1 - i):      # comparamos hasta donde falta ordenar
            if lista[j] > lista[j + 1]: # si el izquierdo es mayor que el derecho...
                lista[j], lista[j + 1] = lista[j + 1], lista[j]  # ...se intercambian
    return lista

numeros = [41, 12, 9, 74, 3]
print(burbuja(numeros))
# [3, 9, 12, 41, 74]
```

### Paso a paso visual (lista `[41, 12, 9]`)

```
Pasada 1:
  (41,12) → 41 > 12 → swap → [12, 41, 9]
  (41, 9) → 41 >  9 → swap → [12, 9, 41]   ← el 41 "flotó" al final ✅
Pasada 2:
  (12, 9) → 12 >  9 → swap → [9, 12, 41]   ← ordenada, y el 41 no se toca
Pasada 3: (ya está ordenada, no hay swaps)
Resultado: [9, 12, 41]
```

```python
# Versión con bandera: si en una pasada NO hubo ningún swap,
# la lista ya está ordenada y podemos parar antes (optimización clásica)
def burbuja_optimizada(lista):
    n = len(lista)
    for i in range(n - 1):
        hubo_swap = False
        for j in range(n - 1 - i):
            if lista[j] > lista[j + 1]:
                lista[j], lista[j + 1] = lista[j + 1], lista[j]
                hubo_swap = True
        if not hubo_swap:
            break       # ordenada, no hace falta seguir
    return lista
```

> [!warning] Rendimiento
> Bubble sort es O(n²) — lento en listas grandes. En la vida real usá `sort()`/`sorted()` (Timsort, O(n log n)). El burbuja es para entender la lógica y para la carrera (ver [[Algoritmos de Ordenamientos en Java]] para los demás: selection, insertion, merge, quick).

## Ver también

- [[Ruta Python]]
- [[Listas en Python]]
- [[Algoritmos con listas en Python]]
- [[Ordenamiento de matrices en Python]]
- [[MOC-programacion]]

## Problema resuelto

**Enunciado:** Dada la lista de edades `[25, 19, 30, 22, 19]`: mostrarla ordenada de menor a mayor, de mayor a menor (sin tocar la original) y ordenarla por **largo de representación en texto** (usando `key`).

**Solución paso a paso:**
1. `sorted()` para no tocar la original.
2. `sorted(..., reverse=True)` para la versión descendente.
3. `key=str` ordena según el texto ("19" < "22" < "25" < "30").

```python
edades = [25, 19, 30, 22, 19]

print("Menor a mayor:", sorted(edades))
print("Mayor a menor:", sorted(edades, reverse=True))
print("Como texto:   ", sorted(edades, key=str))
print("Original:     ", edades)     # intacta
```

## Problema propio

**Enunciado:** Ordenar `productos = [("leche", 1200), ("pan", 350), ("huevos", 4800)]` primero por precio de menor a mayor, y luego alfabéticamente por nombre — con `sort()` (in-place) usando `key=lambda`.

**Pista:** `p[1]` es el precio y `p[0]` el nombre: `productos.sort(key=lambda p: p[1])`.
