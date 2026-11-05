---
title: Matrices en Python
area: programacion
tipo: lenguaje
tags: [programacion, python, matrices]
created: 2026-10-12
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Listas en Python]]", "[[Matrices en Java]]", "[[Ordenamiento de matrices en Python]]"]
---

# Matrices en Python

En Python no hay tipo "matriz": es una **lista de listas**. Cada fila es una lista; se accede `[fila][columna]`. Todo lo de listas aplica (ver [[Listas en Python]]).

## Crear matrices

```python
# A mano
matriz = [[1, 2, 3],
          [4, 5, 6],
          [7, 8, 9]]

# Rellenada con ceros (3 filas x 4 columnas)
filas, columnas = 3, 4
ceros = [[0 for _ in range(columnas)] for _ in range(filas)]
print(ceros)    # [[0,0,0,0],[0,0,0,0],[0,0,0,0]]

# O con list comprehension (más corto)
ceros2 = [[0] * columnas for _ in range(filas)]

# ⚠️ MAL: [[0] * columnas] * filas crea filas que SON el mismo objeto
mal = [[0] * 4] * 3
mal[0][1] = 9
print(mal)      # [[0,9,0,0],[0,9,0,0],[0,9,0,0]] ← cambió en TODAS las filas
```

> [!warning] El truco de `* [[0]] * 3`
> `[[0]*4]*3` repite la MISMA referencia 3 veces. Usá siempre la comprehension: `[[0]*4 for _ in range(3)]`.

## Acceder: [fila][columna]

```python
m = [[1, 2, 3],
     [4, 5, 6],
     [7, 8, 9]]

print(m[0])        # [1, 2, 3]   → fila 0 completa
print(m[1][2])     # 6           → fila 1, columna 2
print(m[-1][-1])   # 9           → última fila, última columna
print(len(m))      # 3           → cantidad de FILAS

# Cambiar un valor
m[0][0] = 99
print(m[0])        # [99, 2, 3]
```

## Recorrer con for anidado

```python
m = [[1, 2], [3, 4], [5, 6]]

# Por filas y columnas con range
for i in range(len(m)):                # por cada fila
    for j in range(len(m[i])):         # por cada columna de esa fila
        print(f"[{i}][{j}] = {m[i][j]}")

# Con for-each: primero la fila, después el valor
for i, fila in enumerate(m):
    for j, valor in enumerate(fila):
        print(f"[{i}][{j}] = {valor}")
```

## Sumar filas y columnas

```python
m = [[1, 2, 3],
     [4, 5, 6],
     [7, 8, 9]]

# Suma de CADA FILA
for fila in m:
    print("Fila:", sum(fila))
# Fila: 6 / Fila: 15 / Fila: 24

# Suma de CADA COLUMNA
for j in range(len(m[0])):          # columnas = largo de la primera fila
    total = 0
    for i in range(len(m)):         # recorro todas las filas en esa columna
        total += m[i][j]
    print(f"Columna {j}: {total}")
# Columna 0: 12 / Columna 1: 15 / Columna 2: 18

# Suma TOTAL de la matriz
total = sum(sum(fila) for fila in m)
print("Total:", total)              # 45
```

## Diagonal principal (matriz cuadrada)

```python
# La diagonal son los elementos [0][0], [1][1], [2][2]... (fila == columna)
m = [[1, 2, 3],
     [4, 5, 6],
     [7, 8, 9]]

diagonal = []
for i in range(len(m)):             # matriz cuadrada: filas == columnas
    diagonal.append(m[i][i])

print("Diagonal:", diagonal)        # [1, 5, 9]
print("Suma diagonal:", sum(diagonal))   # 15

# En una línea
diagonal2 = [m[i][i] for i in range(len(m))]
```

## Transpuesta (filas ↔ columnas)

```python
m = [[1, 2, 3],
     [4, 5, 6]]

# zip + list: la forma pythonica
transpuesta = [list(fila) for fila in zip(*m)]
print(transpuesta)    # [[1, 4], [2, 5], [3, 6]]

# Equivalente manual
filas, cols = len(m), len(m[0])
transpuesta2 = [[m[i][j] for i in range(filas)] for j in range(cols)]
print(transpuesta2)   # [[1, 4], [2, 5], [3, 6]]
```

## Mayor y menor de una matriz

```python
m = [[3, 8, 1],
     [4, 9, 2]]

# Aplanar la matriz (lista de listas → lista plana) y usar max/min
plana = []
for fila in m:
    plana.extend(fila)              # o: plana += fila
print("Mayor:", max(plana))         # 9
print("Menor:", min(plana))         # 1

# En una línea
plana2 = [n for fila in m for n in fila]
print(plana2)                       # [3, 8, 1, 4, 9, 2]

# También sirve para la posición del mayor
mayor = max(plana)
for i in range(len(m)):
    for j in range(len(m[i])):
        if m[i][j] == mayor:
            print(f"Mayor {mayor} en [{i}][{j}]")
```

## Suma de dos matrices

```python
# Solo se pueden sumar matrices de las MISMAS dimensiones
a = [[1, 2],
     [3, 4]]
b = [[10, 20],
     [30, 40]]

suma = [[0, 0],
        [0, 0]]

for i in range(len(a)):
    for j in range(len(a[0])):
        suma[i][j] = a[i][j] + b[i][j]

print(suma)    # [[11, 22], [33, 44]]

# Con comprehension
suma2 = [[a[i][j] + b[i][j] for j in range(len(a[0]))] for i in range(len(a))]
print(suma2)   # [[11, 22], [33, 44]]
```

## Multiplicación de matrices

```python
# Regla: (n x m) * (m x p) → resultado (n x p)
# resultado[i][j] = suma de a[i][k] * b[k][j] para toda k
a = [[1, 2],
     [3, 4]]            # 2x2
b = [[5, 6],
     [7, 8]]            # 2x2

filas_a, cols_a = len(a), len(a[0])
filas_b, cols_b = len(b), len(b[0])

# Verificar compatibilidad
if cols_a != filas_b:
    print("No se pueden multiplicar")
else:
    resultado = []
    for i in range(filas_a):                 # cada fila de A
        fila_resultado = []
        for j in range(cols_b):              # cada columna de B
            suma = 0
            for k in range(cols_a):          # producto punto fila×columna
                suma += a[i][k] * b[k][j]
            fila_resultado.append(suma)
        resultado.append(fila_resultado)

    for fila in resultado:
        print(fila)
    # [19, 22]
    # [43, 50]
```

> [!tip] Multiplicar en la vida real
> Para matrices grandes se usa la librería `numpy` (`numpy.dot`). Con los bucles aprendés la lógica; en producción, numpy.

## Ver también

- [[Ruta Python]]
- [[Listas en Python]]
- [[Matrices en Java]]
- [[Ordenamiento de matrices en Python]]
- [[MOC-programacion]]

## Problema resuelto

**Enunciado:** Dada una matriz 3x3, mostrar su transpuesta, la suma de su diagonal principal y el mayor de todos sus elementos.

**Solución paso a paso:**
1. Transpuesta con `zip(*)`.
2. Diagonal con `[m[i][i] for i in range(3)]`.
3. Mayor aplanando la matriz.

```python
m = [[4, 2, 7],
     [1, 9, 3],
     [8, 5, 6]]

transpuesta = [list(f) for f in zip(*m)]
diagonal = [m[i][i] for i in range(len(m))]
plana = [n for fila in m for n in fila]

print("Transpuesta:", transpuesta)
print("Diagonal:", diagonal, "→ suma:", sum(diagonal))
print("Mayor:", max(plana))
```

## Problema propio

**Enunciado:** Crear una matriz 3x3 con los números del 1 al 9, imprimir cada fila, la suma de cada columna, y multiplicar esa matriz por ella misma (mostrar el resultado).

**Pista:** Para el llenado: `[[i*3 + j + 1 for j in range(3)] for i in range(3)]`. Para la multiplicación, seguí el patrón de triple bucle con acumulador.
