---
title: Lambda, map, filter y reduce en Python
area: programacion
tipo: lenguaje
tags: [programacion, python, funciones, functional]
created: 2026-10-12
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Funciones en Python]]", "[[Listas en Python]]"]
---

# Lambda, map, filter y reduce en Python

Programación funcional a lo básico: funciones anónimas (`lambda`) y las tres herramientas que las usan: `map` (transformar), `filter` (filtrar) y `reduce` (acumular). Nivel introductorio, sin marear.

## `lambda` — funciones sin nombre

```python
# def normal
def doble(x):
    return x * 2

# SU EQUIVALENTE lambda (una sola expresión, sin return)
doble = lambda x: x * 2

print(doble(5))         # 10

# Con varios parámetros
sumar = lambda a, b: a + b
print(sumar(3, 4))      # 7

# Útiles COMO argumento de otra función (acá es donde brillan)
personas = [("Ana", 25), ("Luis", 20), ("Marta", 30)]

personas.sort(key=lambda p: p[1])        # ordenar por edad
print(personas)     # [('Luis', 20), ('Ana', 25), ('Marta', 30)]

# Ojo: lambda solo admite UNA expresión
# lambda x: x + 1 if x > 0 else 0    ← esto sí (el ternario cuenta como expresión)
```

> [!warning] Cuándo NO usar lambda
> Si la lógica necesita varios `if`, loops o un nombre propio → usá `def` normal (ver [[Funciones en Python]]). Lambda es para lógica de una línea.

## `map()` — transformar cada elemento

```python
# map(función, secuencia) → aplica la función a CADA elemento
numeros = [1, 2, 3, 4, 5]

dobles = list(map(lambda x: x * 2, numeros))
print(dobles)           # [2, 4, 6, 8, 10]

# Equivalente con for:
dobles2 = []
for n in numeros:
    dobles2.append(n * 2)

# Con varias listas (elemento a elemento)
a = [1, 2, 3]
b = [10, 20, 30]
suma = list(map(lambda x, y: x + y, a, b))
print(suma)             # [11, 22, 33]

# Muy usado para convertir tipos
textos = ["1", "2", "3"]
enteros = list(map(int, textos))       # int como función directa
print(enteros)          # [1, 2, 3]
```

## `filter()` — filtrar elementos

```python
# filter(función, secuencia) → se queda con los que devuelven True
numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

pares = list(filter(lambda x: x % 2 == 0, numeros))
print(pares)            # [2, 4, 6, 8, 10]

mayores = list(filter(lambda x: x > 7, numeros))
print(mayores)          # [8, 9, 10]

# Equivalente con comprehension (más pythonico)
pares2 = [x for x in numeros if x % 2 == 0]

# Filtrar strings
palabras = ["sol", "mariposa", "python", "ai"]
largas = list(filter(lambda p: len(p) > 4, palabras))
print(largas)           # ['mariposa', 'python']

# Filtrar None (limpiar listas sucias)
valores = [1, None, 5, None, 9]
limpios = list(filter(None, valores))     # None y 0 son falsy
print(limpios)          # [1, 5, 9]
```

## `reduce()` — acumular en un solo valor

```python
# reduce NO es built-in: viene de functools
from functools import reduce

numeros = [1, 2, 3, 4, 5]

# reduce(func, secuencia) → va juntando: f(f(f(1,2),3),4),5
total = reduce(lambda a, b: a + b, numeros)
print(total)            # 15  (1+2+3+4+5)

# Ejemplos típicos
producto = reduce(lambda a, b: a * b, [1, 2, 3, 4])
print(producto)         # 24

mayor = reduce(lambda a, b: a if a > b else b, [3, 9, 2, 7])
print(mayor)            # 9

# Concatenar strings
frase = reduce(lambda a, b: a + " " + b, ["Python", "es", "genial"])
print(frase)            # Python es genial
```

## Las3 juntas (pipeline)

```python
from functools import reduce

numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# 1) filtrar pares → 2) doblarlos → 3) sumar todo
resultado = reduce(
    lambda a, b: a + b,
    map(lambda x: x * 2,
        filter(lambda x: x % 2 == 0, numeros))
)
print(resultado)        # 110  (2+4+6+8+10 = 30 doblados = 60... revisá: pares 2,4,6,8,10 → dobles 4,8,12,16,20 → suma 60)

# La versión con comprehension se lee mejor:
resultado2 = sum([x * 2 for x in numeros if x % 2 == 0])
print(resultado2)       # 60
```

## map/filter vs comprehension

| Querés | comprehension | equivalencia |
|---|---|---|
| transformar | `[x*2 for x in lista]` | `list(map(f, lista))` |
| filtrar | `[x for x in lista if cond]` | `list(filter(f, lista))` |
| filtrar + transformar | `[x*2 for x in lista if cond]` | `list(map(f2, filter(f1, lista)))` |

> [!tip] Preferí comprehension
> En Python la comunidad prefiere las list comprehensions: se leen más claro. `map/filter/reduce` conviene conocerlos (muchos proyectos los usan), pero no son obligatorios — excepto `reduce`, que a veces es la forma más clara de acumular.

## Ver también

- [[Ruta Python]]
- [[Funciones en Python]]
- [[Listas en Python]]
- [[MOC-programacion]]

## Problema resuelto

**Enunciado:** Dada la lista de precios `[2500, 800, 12000, 350, 9900]`: con `filter` obtener los mayores a 1000, con `map` aplicarles 10% de descuento, y con `reduce` sumar el total final.

**Solución paso a paso:**
1. `filter(lambda p: p > 1000, precios)` → caros.
2. `map(lambda p: p * 0.9, ...)` → con descuento.
3. `reduce(lambda a, b: a + b, ...)` → total.

```python
from functools import reduce

precios = [2500, 800, 12000, 350, 9900]

caros = filter(lambda p: p > 1000, precios)
con_descuento = map(lambda p: p * 0.9, caros)
total = reduce(lambda a, b: a + b, con_descuento)

print("Total con descuento:", round(total, 2))
# 2500+12000+9900 = 24400 → 90% = 21960.0
```

## Problema propio

**Enunciado:** Dada `palabras = ["sol", "mariposa", "python", "red", "algoritmo"]`: usar `filter` para quedarte con las de más de 4 letras, `map` para pasarlas a mayúsculas, y `reduce` para concatenarlas todas en un solo string separado por guiones.

**Pista:** `filter(lambda p: len(p) > 4, palabras)` → `map(str.upper, ...)` → `reduce(lambda a, b: a + "-" + b, ...)`.
