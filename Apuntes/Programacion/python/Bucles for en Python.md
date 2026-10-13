---
title: Bucles for en Python
area: programacion
tipo: lenguaje
tags: [programacion, python, bucles, for]
created: 2026-10-12
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Range en Python]]", "[[Listas en Python]]", "[[Bucles while en Python]]"]
---

# Bucles for en Python

El `for` de Python **no es el for de C/Java** (no usa contador `i++`). Es un "for-each": recorre los elementos de una secuencia uno por uno. Cuando necesites números, usás `range()` (ver [[Range en Python]]).

## Recorrer una lista

```python
frutas = ["manzana", "banana", "naranja"]

for fruta in frutas:              # fruta toma el VALOR de cada elemento
    print("Me gusta la", fruta)
# Me gusta la manzana
# Me gusta la banana
# Me gusta la naranja
```

## Recorrer un string

```python
palabra = "Python"

for letra in palabra:
    print(letra)
# P
# y
# t
# h
# o
# n

# Contar vocales
vocales = "aeiouAEIOU"
cantidad = 0

for letra in "hola mundo":
    if letra in vocales:
        cantidad += 1

print("Vocales:", cantidad)       # 4
```

## Recorrer un diccionario

```python
persona = {"nombre": "Ferney", "edad": 20, "ciudad": "Cali"}

# Por defecto recorre las CLAVES
for clave in persona:
    print(clave)
# nombre / edad / ciudad

# Valores directamente
for valor in persona.values():
    print(valor)
# Ferney / 20 / Cali

# Clave y valor juntos (lo más común)
for clave, valor in persona.items():
    print(f"{clave} = {valor}")
# nombre = Ferney
# edad = 20
# ciudad = Cali
```

## "for con índice" (las 3 formas)

```python
lista = ["a", "b", "c"]

# 1) La forma principiante (no recomendada, pero funciona)
for i in range(len(lista)):
    print(i, lista[i])

# 2) enumerate() — la forma PYTHONICA (índice + valor)
for i, valor in enumerate(lista):
    print(i, valor)
# 0 a / 1 b / 2 c

# enumerate con índice de arranque
for i, valor in enumerate(lista, start=1):    # 1-based
    print(f"{i}. {valor}")
# 1. a / 2. b / 3. c
```

## `zip()` — recorrer dos listas en paralelo

```python
nombres = ["Ana", "Luis", "Marta"]
edades  = [25, 30, 22]

# zip une elemento a elemento (se corta en la más corta)
for nombre, edad in zip(nombres, edades):
    print(f"{nombre} tiene {edad} años")
# Ana tiene 25 años
# Luis tiene 30 años
# Marta tiene 22 años

# También sirve para crear diccionarios de una
personas = dict(zip(nombres, edades))
print(personas)      # {'Ana': 25, 'Luis': 30, 'Marta': 22}

# enumerate + zip combinados: índice, nombre y edad
for i, (nombre, edad) in enumerate(zip(nombres, edades), 1):
    print(f"{i}) {nombre}: {edad}")
```

## Recorrer con condición dentro

```python
# Sumar solo los pares
numeros = [3, 8, 1, 6, 9, 4]
suma_pares = 0

for n in numeros:
    if n % 2 == 0:
        suma_pares += n

print("Suma de pares:", suma_pares)      # 18
```

> [!tip] for vs while
> ¿Sabés cuántas vueltas o qué elementos recorrer? → `for`. ¿No sabés hasta cuándo y la condición lo decide? → `while` (ver [[Bucles while en Python]]).

## Ver también

- [[Ruta Python]]
- [[Range en Python]]
- [[Listas en Python]]
- [[Control de bucles en Python]]
- [[MOC-programacion]]

## Problema resuelto

**Enunciado:** Dadas dos listas (nombres y notas), imprimir la tabla: `Ana: 4.3 (Aprobado)` — usando `zip()` y sacando el promedio.

**Solución paso a paso:**
1. `zip()` para emparejar nombre con su lista de notas.
2. `sum(notas) / len(notas)` para el promedio.
3. `if` para el estado.

```python
nombres = ["Ana", "Luis", "Marta"]
notas = [[3.5, 4.0, 4.5], [2.0, 3.0, 2.5], [4.8, 4.9, 5.0]]

for nombre, grupo in zip(nombres, notas):
    prom = sum(grupo) / len(grupo)
    estado = "Aprobado" if prom >= 3.0 else "Reprobado"
    print(f"{nombre}: {prom:.1f} ({estado})")
# Ana: 4.0 (Aprobado)
# Luis: 2.5 (Reprobado)
# Marta: 4.9 (Aprobado)
```

## Problema propio

**Enunciado:** Dada una lista de palabras, imprimir cada una **numerada desde el 1** y, al final, cuántas palabras empiezan con letra "a" (o "A").

**Pista:** `enumerate(lista, start=1)` para la numeración, y `palabra.lower().startswith("a")` para el conteo.
