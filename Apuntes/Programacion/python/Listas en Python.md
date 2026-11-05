---
title: Listas en Python
area: programacion
tipo: lenguaje
tags: [programacion, python, listas]
created: 2026-10-12
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Algoritmos con listas en Python]]", "[[Tuplas en Python]]", "[[Sets en Python]]", "[[Matrices en Python]]"]
---

# Listas en Python

La lista (`list`) es el contenedor por excelencia: **ordenada**, **permite duplicados** y **mutable** (se puede modificar). Se crea con corchetes `[]` o con `list()`.

## Crear listas

```python
vacia = []                          # lista vacía
numeros = [10, 20, 30]
mixta = [1, "hola", True, 3.14]     # puede mezclar tipos
desde_range = list(range(1, 6))     # [1, 2, 3, 4, 5]
desde_string = list("hola")         # ['h', 'o', 'l', 'a']
repetida = [0] * 5                  # [0, 0, 0, 0, 0]
```

## Indexado y slicing (igual que strings)

```python
lista = [10, 20, 30, 40, 50]

print(lista[0])      # 10
print(lista[-1])     # 50 (último)
print(lista[1:4])    # [20, 30, 40]  (fin exclusivo)
print(lista[:3])     # [10, 20, 30]
print(lista[::2])    # [10, 30, 50]
print(lista[::-1])   # [50, 40, 30, 20, 10]  invertida

lista[0] = 99        # las listas SÍ se pueden modificar por índice
print(lista)         # [99, 20, 30, 40, 50]
```

## Modificar: append / insert / extend / replace

```python
frutas = ["manzana", "pera"]

frutas.append("uva")            # agrega al FINAL → ['manzana', 'pera', 'uva']
frutas.insert(0, "fresa")       # agrega en la POSICIÓN 0 → ['fresa', 'manzana', 'pera', 'uva']
frutas.extend(["kiwi", "mora"]) # agrega VARIOS al final (no una lista dentro de lista)
frutas[1] = "sandía"            # reemplaza por índice

print(frutas)   # ['fresa', 'sandía', 'pera', 'uva', 'kiwi', 'mora']

# Diferencia clave: append mete el objeto tal cual, extend une elementos
a = [1, 2]
a.append([3, 4])      # [1, 2, [3, 4]]  ← metió la lista entera
b = [1, 2]
b.extend([3, 4])      # [1, 2, 3, 4]    ← unió los elementos
```

## Eliminar: remove / pop / del / clear

```python
lista = [10, 20, 30, 40, 30]

lista.remove(30)      # elimina la PRIMERA ocurrencia del VALOR 30 → [10, 20, 40, 30]
valor = lista.pop()   # elimina y RETORNA el último → 30, lista: [10, 20, 40]
valor = lista.pop(0)  # elimina y retorna por ÍNDICE → 10, lista: [20, 40]
del lista[0]          # elimina por índice sin retornar nada → [40]

lista.clear()         # vacía la lista → []

# remove con valor inexistente da ValueError
# lista.remove(999)   ← ValueError: list.remove(x): x not in list
```

## Buscar: in / index / count / len

```python
lista = [3, 8, 1, 8, 5]

print(8 in lista)          # True   → ¿existe?
print(9 not in lista)      # True
print(lista.index(8))      # 1      → primer índice donde está (da error si no existe)
print(lista.count(8))      # 2      → cuántas veces aparece
print(len(lista))          # 5      → cantidad de elementos
print(max(lista))          # 8
print(min(lista))          # 1
print(sum(lista))          # 25
```

## Copiar (¡cuidado con el =)

```python
original = [1, 2, 3]
copia = original            # NO es una copia: ambas apuntan al MISMO objeto
copia.append(4)
print(original)             # [1, 2, 3, 4] ← también cambió

copia2 = original.copy()    # copia REAL (también: original[:] o list(original))
copia2.append(5)
print(original)             # [1, 2, 3, 4] ← esta vez intacta
```

## Listas anidadas

```python
# Una lista dentro de otra (= matriz, ver [[Matrices en Python]])
matriz = [[1, 2, 3],
          [4, 5, 6],
          [7, 8, 9]]

print(matriz[0])        # [1, 2, 3]    → la primera fila
print(matriz[1][2])     # 6            → fila 1, columna 2

for fila in matriz:
    print(fila)
```

## List comprehension (listas en una línea)

```python
# [expresión for variable in secuencia if condición]
cuadrados = [n ** 2 for n in range(1, 6)]
print(cuadrados)        # [1, 4, 9, 16, 25]

pares = [n for n in range(1, 11) if n % 2 == 0]
print(pares)            # [2, 4, 6, 8, 10]

mayusculas = [p.title() for p in ["ana", "luis", "marta"]]
print(mayusculas)       # ['Ana', 'Luis', 'Marta']

# Equivalente "largo" para que se entienda:
cuadrados2 = []
for n in range(1, 6):
    cuadrados2.append(n ** 2)
```

> [!tip] Cuándo usar comprehension
> Si te queda en UNA línea legible, usala. Si queda un lío de ifs anidados, el `for` normal se lee mejor.

## Ver también

- [[Ruta Python]]
- [[Algoritmos con listas en Python]]
- [[Matrices en Python]]
- [[Tuplas en Python]]
- [[Sets en Python]]
- [[MOC-programacion]]

## Problema resuelto

**Enunciado:** Dada la lista `[12, 5, 8, 12, 3]`: agregar el 20 al final, insertar el 7 en la posición 1, eliminar la primera aparición del 12, y mostrar el resultado final y su longitud.

**Solución paso a paso:**
1. `append(20)` → final.
2. `insert(1, 7)` → posición 1.
3. `remove(12)` → solo la primera.
4. `len()` para el total.

```python
lista = [12, 5, 8, 12, 3]

lista.append(20)        # [12, 5, 8, 12, 3, 20]
lista.insert(1, 7)      # [12, 7, 5, 8, 12, 3, 20]
lista.remove(12)        # [7, 5, 8, 12, 3, 20]

print("Lista:", lista)
print("Longitud:", len(lista))
```

## Problema propio

**Enunciado:** Dada `compras = ["leche", "pan", "huevos", "pan"]`: mostrar cuántas veces aparece "pan", el índice de "huevos", y crear una lista nueva **sin duplicados** (con comprehension o con `set`).

**Pista:** `compras.count("pan")`, `compras.index("huevos")`, y `list(set(compras))` — o con comprehension: `[x for x in compras if compras.count(x) == 1]`.
