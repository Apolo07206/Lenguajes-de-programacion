---
title: Tuplas en Python
area: programacion
tipo: lenguaje
tags: [programacion, python, tuplas]
created: 2026-10-12
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Listas en Python]]", "[[Funciones en Python]]", "[[Sets en Python]]"]
---

# Tuplas en Python

La tupla (`tuple`) es como una lista pero **inmutable**: una vez creada, no se puede agregar, quitar ni cambiar nada. Se crea con paréntesis `()` o con `tuple()`.

## Crear tuplas

```python
vacia = ()                          # tupla vacía (¡ojo! no es ())
con_coma = 5,                       # ESTO es una tupla: (5,)
tupla = (1, 2, 3)
mixta = (1, "hola", True)
desde_lista = tuple([1, 2, 3])      # desde una lista → (1, 2, 3)

print(type(tupla))                  # <class 'tuple'>

# Un solo elemento NECESITA la coma
raro = (5)                          # esto es un int con paréntesis
real = (5,)                         # ESTO es una tupla
print(type(raro))                   # <class 'int'>
print(type(real))                   # <class 'tuple'>
```

## Inmutabilidad

```python
tupla = (1, 2, 3)

# tupla[0] = 99        ← TypeError: 'tuple' does not support item assignment
# tupla.append(4)      ← AttributeError: 'tuple' object has no attribute 'append'

# Lo que SÍ se puede: leer, concatenar (crea NUEVA tupla) y borrar
print(tupla[1])                     # 2
nueva = tupla + (4, 5)              # (1, 2, 3, 4, 5) — nueva tupla
print(nueva)

# borrar la tupla completa con del
del tupla
```

## Indexado y slicing (igual que listas/strings)

```python
t = ("a", "b", "c", "d")

print(t[0])        # a
print(t[-1])       # d
print(t[1:3])      # ('b', 'c')
print(len(t))      # 4
print("b" in t)    # True
```

## Desempaquetado

```python
# Sacar valores de la tupla en variables (más limpio que índices)
coordenada = (10, 20)
x, y = coordenada
print(x, y)        # 10 20

# Intercambio sin variable temporal (como vimos en variables)
a, b = 1, 2
a, b = b, a        # a=2, b=1

# Desempaquetado con * (resto)
numeros = (1, 2, 3, 4, 5)
primero, *medio, ultimo = numeros
print(primero)     # 1
print(medio)       # [2, 3, 4]  (el resto queda como lista)
print(ultimo)      # 5

# Muy común: funciones que devuelven varias cosas (ver [[Funciones en Python]])
def min_max(lista):
    return min(lista), max(lista)      # Python crea una tupla implícita

lo_min, lo_max = min_max([3, 9, 1])
print(lo_min, lo_max)                  # 1 9
```

## Métodos: count() e index()

```python
t = (1, 2, 3, 2, 4, 2)

print(t.count(2))      # 3   → cuántas veces aparece
print(t.index(3))      # 2   → primer índice (da ValueError si no existe)
print(t.count(99))     # 0
# t.index(99)          ← ValueError: tuple.index(x): x not in tuple
```

## ¿Tupla o lista? (cuándo usar cada una)

| Situación | Usa |
|---|---|
| Vas a agregar/quitar/cambiar elementos | **lista** |
| Los datos NO deben cambiar (coordenadas, puntos, registros fijos) | **tupla** |
| Querés usarla como **clave** de diccionario o elemento de set | **tupla** (la lista no se puede) |
| Una función devuelve varios valores | **tupla** (implícita) |
| Recorrer datos constantes de configuración | **tupla** (más rápida y segura) |

```python
# La inmutabilidad da SEGURIDAD: nadie puede modificar por accidente
SEMANAS = ("lunes", "martes", "miércoles", "jueves", "viernes", "sábado", "domingo")

# Clave de diccionario: la lista no sirve, la tupla sí
coordenadas = {}
# coordenadas[[1, 2]] = "casa"    ← TypeError: unhashable type: 'list'
coordenadas[(1, 2)] = "casa"       # ✅
```

## Tuplas dentro de listas y listas dentro de tuplas

```python
# Lista de tuplas: formato típico de "registros" (se puede ordenar)
estudiantes = [("Ana", 4.3), ("Luis", 2.9), ("Marta", 3.8)]
estudiantes.sort(key=lambda e: e[1])
print(estudiantes)    # [('Luis', 2.9), ('Marta', 3.8), ('Ana', 4.3)]

# Tupla con lista dentro: la tupla es inmutable PERO la lista de adentro sí cambia
mixta = (1, [2, 3])
mixta[1].append(4)
print(mixta)          # (1, [2, 3, 4])
```

## Ver también

- [[Ruta Python]]
- [[Listas en Python]]
- [[Funciones en Python]]
- [[Diccionarios en Python]]
- [[MOC-programacion]]

## Problema resuelto

**Enunciado:** Representar un punto 3D como tupla, calcular su distancia al origen con la fórmula √(x²+y²+z²), usando desempaquetado y `math`.

**Solución paso a paso:**
1. Tupla de3 elementos y desempaquetado `x, y, z = punto`.
2. `math.sqrt()` con la suma de cuadrados.
3. Importar `math` (ver [[Módulos y librerías en Python]]).

```python
import math

punto = (3, 4, 0)
x, y, z = punto

distancia = math.sqrt(x**2 + y**2 + z**2)
print("Distancia al origen:", distancia)    # 5.0
```

## Problema propio

**Enunciado:** Crear una tupla con los días de la semana, imprimir cuántos elementos tiene, el índice del "viernes", cuántas veces aparece "lunes", e intentar (mentalmente) cambiar un valor: ¿qué error daría?

**Pista:** `len(dias)`, `dias.index("viernes")`, `dias.count("lunes")`. El error al asignar es `TypeError`.
