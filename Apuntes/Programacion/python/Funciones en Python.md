---
title: Funciones en Python
area: programacion
tipo: lenguaje
tags: [programacion, python, funciones]
created: 2026-10-12
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Lambda, map, filter y reduce en Python]]", "[[Funciones en C++]]", "[[Recursión en Python]]"]
---

# Funciones en Python

Una función es un bloque de código con nombre que hacés una vez y usás cuantas veces quieras. Se define con `def` y se ejecuta llamándola con paréntesis.

## def, parámetros y return

```python
# DEFINIR (no se ejecuta hasta que la llames)
def saludar():
    print("Hola Ferney")

# LLAMAR
saludar()               # Hola Ferney

# Con PARÁMETROS
def saludar_a(nombre):
    print(f"Hola {nombre}")

saludar_a("Ana")        # Hola Ana

# Con RETURN (devuelve un valor y termina la función)
def sumar(a, b):
    return a + b

resultado = sumar(3, 5)
print(resultado)        # 8

# Sin return, la función devuelve None
def sin_retorno():
    print("solo imprimo")

x = sin_retorno()       # imprime "solo imprimo"
print(x)                # None
```

```python
# return corta la función EN ESE MOMENTO
def verificar(n):
    if n < 0:
        return "negativo"      # si se cumple, sale acá y no sigue
    return "positivo o cero"

print(verificar(-5))    # negativo
```

## Valores por defecto

```python
def saludar(nombre, saludo="Hola"):       # saludo tiene DEFAULT
    print(f"{saludo}, {nombre}")

saludar("Ana")              # Hola, Ana        (usa el default)
saludar("Ana", "Buenas")    # Buenas, Ana     (usa lo que pasaste)
```

> [!warning] Mutable en el default
> `def f(lista=[])` es un BUG clásico: la lista default se crea UNA vez y persiste entre llamadas. Usá `lista=None` y creá dentro: `if lista is None: lista = []`.

## Argumentos con nombre (keyword args)

```python
def crear_perfil(nombre, edad, ciudad):
    print(f"{nombre}, {edad}, {ciudad}")

# Posicional (por orden)
crear_perfil("Ana", 25, "Cali")

# Con nombre: el orden ya no importa y el código se lee solo
crear_perfil(ciudad="Bogotá", nombre="Luis", edad=30)

# Mezclar: primero posicionales, después con nombre
crear_perfil("Marta", ciudad="Medellín", edad=22)
```

## *args y **kwargs

```python
# *args → argumentos VARIABLES: llegan como TUPLA
def sumar_todo(*args):
    print(type(args))       # <class 'tuple'>
    return sum(args)

print(sumar_todo(1, 2, 3))      # 6
print(sumar_todo(1, 2, 3, 4))   # 10

# **kwargs → argumentos con NOMBRE variables: llegan como DICCIONARIO
def mostrar_info(**kwargs):
    print(type(kwargs))    # <class 'dict'>
    for clave, valor in kwargs.items():
        print(f"  {clave}: {valor}")

mostrar_info(nombre="Ana", edad=25)
# nombre: Ana
# edad: 25

# Los3 juntos (orden obligatorio: args, kwargs, y args con nombre)
def mezcla(*args, **kwargs):
    print("args:", args)
    print("kwargs:", kwargs)

mezcla(1, 2, 3, ciudad="Cali", activo=True)
# args: (1, 2, 3)
# kwargs: {'ciudad': 'Cali', 'activo': True}
```

## Scope: local vs global

```python
global_var = "soy global"

def mi_funcion():
    local_var = "soy local"
    print(global_var)       # desde dentro SÍ se lee la global
    # print(local_var)      # esto funciona dentro
    # pero fuera de la función NO existe

mi_funcion()
# print(local_var)          ← NameError: no existe fuera

# Modificar una global desde dentro necesita 'global'
contador = 0

def incrementar():
    global contador         # aviso: voy a tocar la global
    contador += 1

incrementar()
incrementar()
print(contador)             # 2

# REGLA: si podés, devolvé valores con return en vez de usar globals
```

## Funciones que retornan varios valores

```python
# Python devuelve una TUPLA implícita (sin escribir paréntesis)
def min_max(lista):
    return min(lista), max(lista)

lo_min, lo_max = min_max([3, 9, 1, 5])
print(lo_min, lo_max)       # 1 9

# También podés devolver datos "compuestos"
def estadisticas(lista):
    return {
        "min": min(lista),
        "max": max(lista),
        "prom": sum(lista) / len(lista),
    }

stats = estadisticas([2, 4, 6])
print(stats["prom"])        # 4.0
```

## Docstrings

```python
def calcular_imc(peso, altura):
    """Calcula el índice de masa corporal.

    Args:
        peso (float): en kilogramos
        altura (float): en metros

    Returns:
        float: IMC redondeado a 2 decimales
    """
    return round(peso / altura ** 2, 2)

print(calcular_imc(70, 1.75))   # 22.86
help(calcular_imc)              # te muestra la docstring
```

## Ver también

- [[Ruta Python]]
- [[Lambda, map, filter y reduce en Python]]
- [[Recursión en Python]]
- [[Funciones en C++]]
- [[MOC-programacion]]

## Problema resuelto

**Enunciado:** Crear una función `calcular(n1, n2, operacion="+")` que sume, reste, multiplique o divida según el operador, manejando la división por cero con un mensaje (sin errores).

**Solución paso a paso:**
1. `operacion` con default `"+"`.
2. `if/elif` por operador (o `match`, ver [[Match case en Python]]).
3. División: verificar `n2 == 0` antes de dividir.

```python
def calcular(n1, n2, operacion="+"):
    """Devuelve el resultado de la operación pedida."""
    if operacion == "+":
        return n1 + n2
    elif operacion == "-":
        return n1 - n2
    elif operacion == "*":
        return n1 * n2
    elif operacion == "/":
        if n2 == 0:
            return "Error: división por cero"
        return n1 / n2
    else:
        return "Operación desconocida"

print(calcular(10, 3))              # 13
print(calcular(10, 3, "*"))         # 30
print(calcular(10, 0, "/"))         # Error: división por cero
```

## Problema propio

**Enunciado:** Crear una función `resumen_notas(*notas)` que reciba cantidad variable de notas y devuelva un diccionario con min, max, promedio y si está aprobado (≥3.0). Probarla con 3 y con 5 notas.

**Pista:** `*notas` llega como tupla; usá `min/max/sum/len` y un `return` con dict (ver [[Diccionarios en Python]]).
