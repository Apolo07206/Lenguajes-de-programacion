---
title: Variables y tipos de datos en Python
area: programacion
tipo: lenguaje
tags: [programacion, python, variables, tipos]
created: 2026-10-12
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Introducción a Python]]", "[[Operadores en Python]]", "[[Strings en Python]]"]
---

# Variables y tipos de datos en Python

En Python no declarás el tipo: escribís `x = 5` y listo. El tipo lo infiere el lenguaje en tiempo de ejecución (tipado dinámico) y podés cambiarlo cuando quieras.

## Los 5 tipos básicos

```python
edad = 25              # int      → entero
precio = 19.99         # float    → decimal
nombre = "Ferney"      # str      → texto (comillas ' o ")
activo = True          # bool     → True / False (con mayúscula)
nada = None            # NoneType → "sin valor" (el null de Python)
```

## `type()` — saber qué tipo es algo

```python
print(type(25))        # <class 'int'>
print(type(19.99))     # <class 'float'>
print(type("hola"))    # <class 'str'>
print(type(True))      # <class 'bool'>
print(type(None))      # <class 'NoneType'>

# Truco: isinstance() para preguntar "¿esto es un int?"
print(isinstance(25, int))   # True
```

## Nombres válidos de variables

```python
# REGLAS:
# - Minúsculas, sin espacios, sin tildes
# - No empieza con número
# - No es palabra reservada (if, for, class, import...)
# Convención: snake_case (mi_variable, cuenta_total)

mi_variable = 10       # ✅ ok
_cuenta = 5            # ✅ ok (empieza con _ es permitido)
cuenta2 = 7            # ✅ ok
2cuenta = 7            # ❌ SyntaxError: no empieza con número
mi-variable = 7        # ❌ SyntaxError: el - es resta, no parte del nombre
class = 7              # ❌ SyntaxError: es palabra reservada
```

## Asignación múltiple y simultánea

```python
a, b, c = 1, 2, 3          # asigna de una
a = b = c = 0              # mismo valor a varios
x, y = 10, 20              # el clásico intercambio en una línea:
x, y = y, x                # x=20, y=10 (sin variable temporal)
```

## Conversión de tipos (casting)

```python
# str() → a texto
edad = 25
print(str(edad))           # "25"  (str)

# int() → a entero (corta los decimales, NO redondea)
print(int("42"))           # 42    (desde texto)
print(int(19.99))          # 19
print(int(True))           # 1

# float() → a decimal
print(float("3.14"))       # 3.14  (desde texto)
print(float(7))            # 7.0

# bool() → a booleano (falsy: 0, "", [], None, vacío)
print(bool(0))             # False
print(bool("hola"))        # True

# Errores típicos:
# int("hola")   → ValueError: invalid literal for int()
# int("19.99")  → ValueError: primero float("19.99") y luego int()
```

## `input()` — leer del teclado (siempre devuelve str)

```python
nombre = input("Tu nombre: ")
print(type(nombre))        # <class 'str'> aunque escribas números

# Para usarlo como número, convertís SIEMPRE
edad = int(input("Tu edad: "))
estatura = float(input("Tu estatura: "))

# Concatenar en texto: str() obligatorio si mezclás con números
print("Tienes " + str(edad) + " años")
```

> [!tip] Atajo
> En vez de concatenar con `+`, usá f-strings (ver [[Strings en Python]]): `print(f"Tienes {edad} años")`.

## Ver también

- [[Ruta Python]]
- [[Introducción a Python]]
- [[Operadores en Python]]
- [[Strings en Python]]
- [[MOC-programacion]]

## Problema resuelto

**Enunciado:** Pedir al usuario la base y la altura de un rectángulo (como decimales) y mostrar el área y el perímetro.

**Solución paso a paso:**
1. `input()` devuelve texto → convertimos ambos a `float`.
2. Área = base × altura, perímetro = 2 × (base + altura).
3. Mostramos con `print()` usando `round()` para no imprimir 20 decimales.

```python
base = float(input("Base: "))
altura = float(input("Altura: "))

area = base * altura
perimetro = 2 * (base + altura)

print("Área:", round(area, 2))
print("Perímetro:", round(perimetro, 2))
```

## Problema propio

**Enunciado:** Pedir al usuario un precio y un descuento (porcentaje entero, ej: 15) y mostrar el precio final con el descuento aplicado.

**Pista:** `precio_final = precio - (precio * descuento / 100)`. Ojo: `input()` te devuelve str, el descuento va como `int` o `float`.
