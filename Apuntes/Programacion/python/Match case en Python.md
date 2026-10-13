---
title: Match case en Python
area: programacion
tipo: lenguaje
tags: [programacion, python, match-case]
created: 2026-10-12
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Condicionales en Python]]", "[[Bucles while en Python]]"]
---

# Match case en Python

Desde **Python 3.10** existe `match / case`: el "switch" de Python, pero más potente (no solo compara valores, también desestructura listas y diccionarios). Sintaxis con dos puntos e indentación, igual que `if`.

## Sintaxis básica

```python
dia = 3

match dia:
    case 1:
        print("Lunes")
    case 2:
        print("Martes")
    case 3:
        print("Miércoles")
    case _:                      # _ = "cualquier otra cosa" (el default)
        print("Otro día")
```

> [!warning] Requisito
> `match` necesita **Python 3.10+**. Verificá con `python --version`. Si tu versión es anterior, usá `if/elif` (ver [[Condicionales en Python]]) o un diccionario.

## `case _` — el default

```python
opcion = input("Opción (s/n): ")

match opcion:
    case "s":
        print("Sí")
    case "n":
        print("No")
    case _:
        print("Opción inválida")     # se ejecuta si no matchea NINGUN case
```

## Múltiples valores con `|`

```python
# | = "o" → un solo case acepta varios valores literales
codigo = 500

match codigo:
    case 200 | 201:
        print("OK / Creado")
    case 400 | 401 | 403:
        print("Error de cliente")
    case 500 | 502 | 503:
        print("Error de servidor")
    case _:
        print("Código desconocido")
```

## Capturar el valor en una variable

```python
# case sin literal captura CUALQUIER valor (como un default con nombre)
comando = input("> ")

match comando:
    case "salir":
        print("Chau")
    case otro:                     # variable 'otro' guarda el valor capturado
        print(f"Comando desconocido: {otro}")
```

> [!note] `_` vs nombre
> `case _` es el comodín convencional del default; `case nombre` crea una variable que guarda el valor. Si no te importa el valor, usá `_`.

## Ejemplo tipo menú

```python
def menu():
    print("--- MENÚ ---")
    print("1. Ver saldo")
    print("2. Depositar")
    print("3. Extraer")
    print("4. Salir")

while True:
    menu()
    opcion = input("Elige una opción: ")

    match opcion:
        case "1":
            print("Tu saldo es: $1500.00")
        case "2":
            monto = float(input("Monto a depositar: "))
            print(f"Depositaste ${monto:.2f}")
        case "3":
            monto = float(input("Monto a extraer: "))
            print(f"Extraíste ${monto:.2f}")
        case "4":
            print("Hasta luego 👋")
            break
        case _:
            print("Opción inválida, intenta de nuevo")
```

## Extras: condiciones y patrones (nivel avanzado)

```python
# Un case con "if" extra (guard)
punto = (2, 5)

match punto:
    case (0, 0):
        print("Origen")
    case (x, 0) if x > 0:        # captura x y exige x > 0
        print(f"Sobre el eje X en {x}")
    case (x, y):
        print(f"Punto ({x}, {y})")

# Esto también funciona con listas (desestructuración):
persona = ["Ferney", 20]
match persona:
    case [nombre, edad]:
        print(f"{nombre} tiene {edad} años")
```

## Ver también

- [[Ruta Python]]
- [[Condicionales en Python]]
- [[Bucles while en Python]]
- [[MOC-programacion]]

## Problema resuelto

**Enunciado:** Simular una calculadora que pida dos números y una operación (`+ - * /`) y muestre el resultado con `match`, manejando la división por cero.

**Solución paso a paso:**
1. Leemos los dos números (float) y la operación (str).
2. `match` sobre la operación; en cada case calculamos.
3. La división la protegemos con un `if` dentro del case (try/except lo vemos en [[Manejo de errores en Python]]).

```python
a = float(input("Número 1: "))
op = input("Operación (+ - * /): ")
b = float(input("Número 2: "))

match op:
    case "+":
        print("Resultado:", a + b)
    case "-":
        print("Resultado:", a - b)
    case "*":
        print("Resultado:", a * b)
    case "/":
        if b == 0:
            print("No se puede dividir por cero")
        else:
            print("Resultado:", a / b)
    case _:
        print("Operación desconocida")
```

## Problema propio

**Enunciado:** Crear un menú con `match` que tenga al menos 4 opciones y una opción "info" que acepte **tres** formas de escribirse (`i`, `info`, `informacion`) en el mismo case.

**Pista:** `case "i" | "info" | "informacion":` — y no te olvides del `case _` al final.
