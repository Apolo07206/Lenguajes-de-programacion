---
title: Algoritmos con listas en Python
area: programacion
tipo: lenguaje
tags: [programacion, python, listas, algoritmos]
created: 2026-10-12
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Listas en Python]]", "[[Ordenamiento de listas en Python]]", "[[Funciones en Python]]"]
---

# Algoritmos con listas en Python

Los clásicos de la carrera: mayor, menor, suma, promedio, contar, buscar e invertir. **Cada uno a mano con bucle (para entender) y con la función built-in (para la vida real).**

## Encontrar el MAYOR

```python
# A MANO (bucle): recorrer y comparar
numeros = [3, 41, 12, 9, 74, 1]

mayor = numeros[0]              # asumimos el primero como mayor
for n in numeros[1:]:           # comparamos con el resto
    if n > mayor:
        mayor = n               # si encontramos uno más grande, lo guardamos

print("Mayor:", mayor)          # 74

# CON BUILT-IN
print("Mayor:", max(numeros))   # 74
```

## Encontrar el MENOR

```python
# A MANO
numeros = [3, 41, 12, 9, 74, 1]

menor = numeros[0]
for n in numeros[1:]:
    if n < menor:
        menor = n

print("Menor:", menor)          # 1

# CON BUILT-IN
print("Menor:", min(numeros))   # 1
```

## Sumar todos los elementos con una función propia

```python
numeros = [3, 41, 12, 9, 74, 1]

# A MANO (acumulador)
def suma_propia(lista):
    total = 0                   # acumulador en 0
    for n in lista:
        total += n              # suma cada elemento
    return total                # devuelve el total

print("Suma:", suma_propia(numeros))    # 140

# CON BUILT-IN
print("Suma:", sum(numeros))            # 140
```

## Promedio

```python
numeros = [3, 41, 12, 9, 74, 1]

# A MANO: suma propia / cantidad
def promedio_propio(lista):
    total = 0
    for n in lista:
        total += n
    return total / len(lista)    # len() = cantidad de elementos

print("Promedio:", promedio_propio(numeros))     # 23.33...

# CON BUILT-IN
print("Promedio:", sum(numeros) / len(numeros))  # 23.33...

print(round(sum(numeros) / len(numeros), 2))     # 23.33 con 2 decimales
```

## Contar elementos (con condición)

```python
numeros = [3, 41, 12, 9, 74, 1]

# A MANO: contar cuántos son pares
def contar_pares(lista):
    c = 0
    for n in lista:
        if n % 2 == 0:
            c += 1
    return c

print("Pares:", contar_pares(numeros))          # 2 (solo 12 y 74)

# CON BUILT-IN (filter + len, o comprehension)
pares = [n for n in numeros if n % 2 == 0]
print("Pares:", len(pares))                     # 2
```

## Buscar un elemento

```python
numeros = [3, 41, 12, 9, 74, 1]
objetivo = 12

# A MANO: buscar y devolver índice (o -1)
def buscar(lista, objetivo):
    for i in range(len(lista)):
        if lista[i] == objetivo:
            return i            # devolvemos el índice en cuanto lo encontramos
    return -1                   # no se encontró

print("Índice:", buscar(numeros, 12))    # 2
print("Índice:", buscar(numeros, 99))    # -1

# CON BUILT-IN
if objetivo in numeros:
    print("Índice:", numeros.index(objetivo))   # 2
```

## Invertir una lista

```python
numeros = [3, 41, 12, 9, 74, 1]

# A MANO: con dos punteros (inicio y fin) intercambiando
def invertir_manual(lista):
    resultado = []
    for i in range(len(lista) - 1, -1, -1):    # del último al primero
        resultado.append(lista[i])
    return resultado

print(invertir_manual(numeros))    # [1, 74, 9, 12, 41, 3]

# OTRA forma manual clásica (intercambio de punteros, in-place)
def swap_in_place(lista):
    i, j = 0, len(lista) - 1
    while i < j:
        lista[i], lista[j] = lista[j], lista[i]
        i += 1
        j -= 1
    return lista

# CON BUILT-IN
invertida = numeros[::-1]          # slicing con paso -1 (la forma más pythonica)
print(invertida)                   # [1, 74, 9, 12, 41, 3]

# Ojo: .reverse() modifica la lista ORIGINAL
numeros.reverse()
print(numeros)                     # [1, 74, 9, 12, 41, 3] (ya quedó dado vuelta)

# list.reverse() no retorna nada (retorna None)
x = numeros.reverse()              # ❌ x = None
```

## Resumen: built-ins del día a día

| Algoritmo | A mano (bucle) | Built-in |
|---|---|---|
| Mayor | comparar y guardar | `max(lista)` |
| Menor | comparar y guardar | `min(lista)` |
| Suma | acumulador `total += n` | `sum(lista)` |
| Promedio | `sum / len` | `sum(lista) / len(lista)` |
| Contar con condición | `if` + contador | `len([n for n in lista if ...])` |
| Buscar | `for` + `return i` | `x in lista` + `.index(x)` |
| Invertir | bucle inverso / swap | `[::-1]` o `.reverse()` |

## Ver también

- [[Ruta Python]]
- [[Listas en Python]]
- [[Ordenamiento de listas en Python]]
- [[Funciones en Python]]
- [[MOC-programacion]]

## Problema resuelto

**Enunciado:** Dada la lista de calificaciones `[3.2, 4.5, 2.1, 4.9, 3.8]`: mostrar el máximo, el mínimo, el promedio, cuántas notas son ≥ 4.0 y la lista invertida.

**Solución paso a paso:**
1. `max()`, `min()`, `sum()/len()` para las estadísticas.
2. Conteo con comprehension filtrando `>= 4.0`.
3. Inversión con `[::-1]`.

```python
notas = [3.2, 4.5, 2.1, 4.9, 3.8]

print("Máximo:", max(notas))
print("Mínimo:", min(notas))
print("Promedio:", round(sum(notas) / len(notas), 2))
print("Notas ≥ 4.0:", len([n for n in notas if n >= 4.0]))
print("Invertida:", notas[::-1])
```

## Problema propio

**Enunciado:** Dada una lista de precios `[2500, 800, 12000, 350, 9900]`: calcular el gasto total, el precio promedio, el más caro y cuántos precios están por debajo de 2000 — TODO con tu propia función `def` (sin `sum/max/min`).

**Pista:** Hacé una función con acumulador para cada caso; para "menor que 2000" sumá 1 en un contador cuando `p < 2000`.
