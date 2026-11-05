---
title: Diccionarios en Python
area: programacion
tipo: lenguaje
tags: [programacion, python, diccionarios]
created: 2026-10-12
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Listas en Python]]", "[[Map y HashMap en Java]]", "[[Sets en Python]]"]
---

# Diccionarios en Python

El diccionario (`dict`) guarda pares **clave: valor**. Accedés por clave (no por posición), es mutable y el orden de inserción se respeta (Python 3.7+). Es el equivalente al HashMap de Java (ver [[Map y HashMap en Java]]).

## Crear diccionarios

```python
vacio = {}                                  # dict vacío
persona = {"nombre": "Ferney", "edad": 20, "ciudad": "Cali"}
desde_dict = dict(nombre="Ana", edad=25)    # forma corta con kwargs
desde_listas = dict([("a", 1), ("b", 2)])   # desde lista de pares

print(persona)          # {'nombre': 'Ferney', 'edad': 20, 'ciudad': 'Cali'}
print(type(persona))    # <class 'dict'>

# Claves únicas: si repetís, gana la última
d = {"a": 1, "a": 2}
print(d)                # {'a': 2}
```

## Acceder, agregar y modificar

```python
persona = {"nombre": "Ferney", "edad": 20}

# ACCEDER por clave
print(persona["nombre"])        # Ferney
# print(persona["email"])       ← KeyError si no existe (el peligro de []

# ACCEDER seguro con get() (si no existe devuelve None o el default)
print(persona.get("nombre"))    # Ferney
print(persona.get("email"))     # None  (sin error)
print(persona.get("email", "no registrado"))   # "no registrado"

# AGREGAR clave nueva (asignación = upsert)
persona["email"] = "ferney@mail.com"

# MODIFICAR existente
persona["edad"] = 21

print(persona)   # {'nombre': 'Ferney', 'edad': 21, 'email': 'ferney@mail.com'}
```

## Eliminar

```python
persona = {"nombre": "Ferney", "edad": 21, "email": "f@mail.com"}

del persona["edad"]             # borra la clave (KeyError si no existe)
# print(persona["edad"])        ← KeyError

valor = persona.pop("nombre")   # borra y RETORNA el valor → 'Ferney'
print(valor)

persona.pop("clave_fantasma", None)   # pop con default: no da error

persona.clear()                 # vacía todo → {}

# Claves especiales
d = {"a": 1, "b": 2}
del d["a"]                      # borrar con del
```

## keys(), values(), items()

```python
persona = {"nombre": "Ferney", "edad": 20, "ciudad": "Cali"}

print(persona.keys())     # dict_keys(['nombre', 'edad', 'ciudad'])
print(persona.values())   # dict_values(['Ferney', 20, 'Cali'])
print(persona.items())    # dict_items([('nombre', 'Ferney'), ('edad', 20), ('ciudad', 'Cali')])

# Convertir a listas cuando lo necesités
print(list(persona.keys()))      # ['nombre', 'edad', 'ciudad']
print(list(persona.values()))    # ['Ferney', 20, 'Cali']

# Contar claves/valores
print(len(persona))              # 3
print("nombre" in persona)       # True → 'in' revisa CLAVES
print("Ferney" in persona)       # False (no es clave)
print("Ferney" in persona.values())  # True si querés buscar valores
```

## Recorrer diccionarios

```python
estudiante = {"nombre": "Ana", "notas": [3.5, 4.0], "activo": True}

# 1) Por clave (la forma default del for)
for clave in estudiante:
    print(clave)

# 2) Clave y valor (lo más común) con items()
for clave, valor in estudiante.items():
    print(f"{clave} → {valor}")

# 3) Solo valores con values()
for valor in estudiante.values():
    print(valor)

# 4) Con índice manual si lo necesitás
claves = list(estudiante.keys())
for i in range(len(claves)):
    print(i, claves[i], estudiante[claves[i]])
```

## Diccionarios anidados

```python
# Un diccionario dentro de otro (base de datos mini)
escuela = {
    "ana": {"edad": 20, "notas": [4.0, 3.5]},
    "luis": {"edad": 22, "notas": [2.8, 3.0]},
}

# Acceder por doble clave: [clave][clave]
print(escuela["ana"]["edad"])           # 20
print(escuela["luis"]["notas"][0])      # 2.8

# Recorrer el anidado
for nombre, datos in escuela.items():
    prom = sum(datos["notas"]) / len(datos["notas"])
    print(f"{nombre} ({datos['edad']}): promedio {prom:.2f}")

# Lista de diccionarios (el formato "registros", muy usado con JSON y BDs)
jugadores = [
    {"nombre": "Ana", "puntos": 15},
    {"nombre": "Luis", "puntos": 9},
]
for j in jugadores:
    print(f"{j['nombre']}: {j['puntos']} pts")
```

## Dict comprehension

```python
# {clave: valor for variable in secuencia if condición}
cuadrados = {n: n ** 2 for n in range(1, 6)}
print(cuadrados)    # {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}

# De dos listas
nombres = ["ana", "luis"]
edades = [25, 30]
parejas = {n: e for n, e in zip(nombres, edades)}
print(parejas)      # {'ana': 25, 'luis': 30}

# Filtrando
mayores = {k: v for k, v in cuadrados.items() if v > 10}
print(mayores)      # {4: 16, 5: 25}

# Invertir claves y valores
inverso = {v: k for k, v in cuadrados.items()}
print(inverso)      # {1: 1, 4: 2, 9: 3, 16: 4, 25: 5}
```

## Ver también

- [[Ruta Python]]
- [[Listas en Python]]
- [[Sets en Python]]
- [[Map y HashMap en Java]]
- [[MOC-programacion]]

## Problema resuelto

**Enunciado:** Crear el diccionario de precios de una tienda, agregar un producto nuevo, subir el precio de otro, eliminar uno, y mostrar los productos con precio mayor a 1000.

**Solución paso a paso:**
1. Crear el dict base.
2. `clave nueva = valor` para agregar; `clave existente = valor` para modificar.
3. `del` para eliminar.
4. Recorrer con `items()` filtrando por precio.

```python
precios = {"manzana": 800, "pera": 900, "uva": 1500}

precios["kiwi"] = 1200        # agregar
precios["uva"] = 1800         # modificar
del precios["pera"]           # eliminar

print("Catálogo completo:", precios)

print("Caros (>$1000):")
for producto, precio in precios.items():
    if precio > 1000:
        print(f"  {producto}: ${precio}")
```

## Problema propio

**Enunciado:** Dado `invitados = {"ana": "confirmado", "luis": "pendiente", "marta": "confirmado", "pedro": "ausente"}`: contar cuántos confirmados hay (sin contar a mano) y crear un diccionario nuevo solo con los confirmados (dict comprehension).

**Pista:** `list(invitados.values()).count("confirmado")` para el conteo; `{k: v for k, v in invitados.items() if v == "confirmado"}` para el filtrado.
