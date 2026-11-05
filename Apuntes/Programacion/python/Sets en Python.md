---
title: Sets en Python
area: programacion
tipo: lenguaje
tags: [programacion, python, sets]
created: 2026-10-12
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Listas en Python]]", "[[Set en Java]]", "[[Tuplas en Python]]"]
---

# Sets en Python

El set (conjunto) es una colección **sin orden**, **sin duplicados** y **mutable**. Su superpoder: matemática de conjuntos (unión, intersección, diferencia) y quitar duplicados al instante.

## Crear sets

```python
vacio = set()                       # set vacío (¡OJO! {} es un DICCIONARIO, ver [[Diccionarios en Python]])
nums = {1, 2, 3, 4}
letras = {"a", "b", "c"}
mixto = {1, "hola", True}

# Duplicados se eliminan solos
con_dups = {1, 2, 2, 3, 3, 3}
print(con_dups)                     # {1, 2, 3}

# Desde una lista (la forma de quitar duplicados)
numeros = [1, 2, 2, 3, 3, 3]
sin_dups = set(numeros)
print(sin_dups)                     # {1, 2, 3}
print(list(sin_dups))               # [1, 2, 3] (el orden no está garantizado)

# Desde un string
letras_str = set("hola")
print(letras_str)                   # {'h', 'o', 'l', 'a'}
```

## add / remove / discard / clear

```python
s = {1, 2, 3}

s.add(4)            # agrega un elemento → {1, 2, 3, 4}
s.add(4)            # ya existe: no hace nada (no da error)

s.remove(2)         # elimina; da KeyError si NO existe
# s.remove(99)      ← KeyError

s.discard(99)       # elimina si existe; si no, no pasa NADA (seguro)

s.clear()           # vacía → set()

print(3 in s)       # pertenencia rápida → True
print(len(s))       # cantidad de elementos
```

> [!tip] ¿Set o lista?
> Si solo necesitás leer y el orden importa → lista. Si necesitás pertenencia rápida (`in`), no duplicados o operaciones de conjuntos → set.

## Operaciones de conjuntos

```python
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}

# UNIÓN (todos los elementos de ambos) → |
print(a | b)            # {1, 2, 3, 4, 5, 6}
print(a.union(b))       # lo mismo

# INTERSECCIÓN (comunes a ambos) → &
print(a & b)            # {3, 4}
print(a.intersection(b))

# DIFERENCIA (los de a que NO están en b) → -
print(a - b)            # {1, 2}
print(a.difference(b))

# DIFERENCIA SIMÉTRICA (los que NO son comunes) → ^
print(a ^ b)            # {1, 2, 5, 6}
print(a.symmetric_difference(b))

# SUBCONJUNTO / SUPERCONJUNTO
print({1, 2}.issubset(a))        # True  → {1,2} está contenido en a
print(a.issuperset({1, 2}))      # True  → a contiene a {1,2}
```

| Operador | Método | Qué hace |
|---|---|---|
| `\|` | `union()` | unión: todos |
| `&` | `intersection()` | intersección: los comunes |
| `-` | `difference()` | diferencia: los de a sin b |
| `^` | `symmetric_difference()` | simétrica: los que no son comunes |

```python
# Modifican in-place también: |= &= -= ^=
a = {1, 2}
a |= {3}        # a = {1, 2, 3}  (unión in-place)
a &= {1, 3}     # a = {1, 3}     (intersección in-place)
```

## Quitar duplicados de una lista (patrón estrella)

```python
numeros = [3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5]

# Forma rápida (pierde el orden)
sin_dups = list(set(numeros))
print(sin_dups)              # [1, 2, 3, 4, 5, 6, 9] (orden arbitrario)

# Forma que RESPETA el orden (Python 3.7+)
visto = set()
sin_dups_ordenados = []
for n in numeros:
    if n not in visto:
        visto.add(n)
        sin_dups_ordenados.append(n)
print(sin_dups_ordenados)    # [3, 1, 4, 5, 9, 2, 6] — mismo orden de aparición

# En una línea preservando orden
sin_dups_ordenados2 = list(dict.fromkeys(numeros))
print(sin_dups_ordenados2)   # [3, 1, 4, 1... ] → [3, 1, 4, 5, 9, 2, 6]
```

## Ejemplo práctico: duplicados entre listas

```python
equipo_a = ["ana", "luis", "marta", "pedro"]
equipo_b = ["luis", "sofia", "pedro", "diego"]

print(set(equipo_a) & set(equipo_b))   # {'luis', 'pedro'} → en ambos equipos
print(set(equipo_a) | set(equipo_b))   # todos sin repetir
print(set(equipo_a) - set(equipo_b))   # solo en A
```

## Ver también

- [[Ruta Python]]
- [[Listas en Python]]
- [[Diccionarios en Python]]
- [[Set en Java]]
- [[MOC-programacion]]

## Problema resuelto

**Enunciado:** Dados dos cursos, devolver: quiénes están en los dos cursos, quiénes solo en el primero, y la lista total sin duplicados.

**Solución paso a paso:**
1. Convertir ambas listas a `set`.
2. `&` para los comunes, `-` para los exclusivos del primero, `|` para la unión.

```python
curso_python = ["ana", "luis", "marta", "pedro"]
curso_java = ["luis", "sofia", "pedro", "diego"]

en_ambos = set(curso_python) & set(curso_java)
solo_python = set(curso_python) - set(curso_java)
todos = set(curso_python) | set(curso_java)

print("En ambos:", en_ambos)
print("Solo en Python:", solo_python)
print("Total sin duplicados:", len(todos))
```

## Problema propio

**Enunciado:** Pedir al usuario5 números (uno por uno), guardarlos en una lista, y al final mostrar cuántos distintos ingresó (sin contar repetidos) usando `set`.

**Pista:** Acumulá en una lista con `append` dentro de un `for range(5)`, y después `len(set(lista))`.
