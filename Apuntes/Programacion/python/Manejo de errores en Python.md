---
title: Manejo de errores en Python
area: programacion
tipo: lenguaje
tags: [programacion, python, errores, exceptions]
created: 2026-10-12
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Funciones en Python]]", "[[Archivos en Python]]", "[[Variables y tipos de datos en Python]]"]
---

# Manejo de errores en Python

Cuando algo sale mal, Python lanza una **excepción** y corta el programa. `try/except` te deja interceptarla, avisar al usuario y seguir. Es la red de seguridad del programa.

## try / except / else / finally

```python
try:
    numero = int(input("Número: "))     # puede fallar
    resultado = 10 / numero             # puede fallar (división por cero)
except ValueError:
    print("Eso no es un número")        # solo corre si hubo ValueError
except ZeroDivisionError:
    print("No se puede dividir por cero")
else:
    print("Todo salió bien:", resultado) # corre SOLO si NO hubo error
finally:
    print("Fin (se ejecuta SIEMPRE)")   # con o sin error
```

Flujo:
1. `try` → intenta ejecutar.
2. Si falla con **esa** excepción → corre su `except` y salta al `finally`.
3. Si no falla → corre el `else`.
4. `finally` → siempre (para cerrar archivos, limpiar, etc.).

## Excepciones que vas a usar todo el tiempo

```python
# ValueError: el TIPO es correcto pero el VALOR no
int("hola")         # ValueError (no es un número convertible)
float("abc")        # ValueError

# TypeError: el TIPO está mal para la operación
10 + "hola"         # TypeError: unsupported operand
int(None)           # TypeError

# ZeroDivisionError
10 / 0              # ZeroDivisionError

# IndexError: índice fuera de rango (listas, strings)
[1, 2, 3][10]       # IndexError

# KeyError: clave inexistente (diccionarios)
{"a": 1}["b"]       # KeyError

# FileNotFoundError: el archivo no existe
open("no_existe.txt")   # FileNotFoundError

# AttributeError: el objeto no tiene ese método/atributo
"x".append("y")     # AttributeError

# RecursionError: se pasó del límite de profundidad (ver [[Recursión en Python]])
```

## Capturar varias excepciones

```python
# Forma 1: varios except
try:
    n = int(input("Número: "))
    print(100 / n)
except ValueError:
    print("Entrada inválida")
except ZeroDivisionError:
    print("División por cero")

# Forma 2: una tupla (mismo manejo para varias)
try:
    n = int(input("Número: "))
    print(100 / n)
except (ValueError, ZeroDivisionError):
    print("Entrada inválida o división por cero")

# Forma 3: capturar TODO (usalo con cuidado, tapa bugs)
try:
    n = int(input("Número: "))
    print(100 / n)
except Exception as e:                  # 'e' guarda el error
    print("Algo falló:", type(e).__name__, "-", e)
```

> [!warning] No tapes todo
> `except Exception` genérico es un paño que esconde errores. Capturá solo lo que esperás y dejá que lo inesperado reviente (así lo ves y lo arreglás).

## `raise` — lanzar errores a propósito

```python
def validar_edad(edad):
    if edad < 0 or edad > 120:
        raise ValueError("Edad fuera de rango (0-120)")   # lanzo YO el error
    return edad

try:
    validar_edad(-5)
except ValueError as e:
    print("Rechazado:", e)      # Rechazado: Edad fuera de rango (0-120)

# Re-lanzar el error después de hacer algo
try:
    datos = int(input("Número: "))
except ValueError:
    print("Log: entrada inválida")
    raise            # el error sigue subiendo (ahora revienta arriba)
```

## Excepciones personalizadas

```python
# Creás tu propio tipo de error heredando de Exception
class ErrorSaldoInsuficiente(Exception):
    """Se lanza cuando el saldo no alcanza."""
    def __init__(self, saldo, monto):
        self.saldo = saldo
        self.monto = monto
        super().__init__(f"Saldo {saldo} insuficiente para retirar {monto}")

def retirar(saldo, monto):
    if monto > saldo:
        raise ErrorSaldoInsuficiente(saldo, monto)
    return saldo - monto

try:
    saldo_final = retirar(100, 500)
except ErrorSaldoInsuficiente as e:
    print("Operación rechazada:", e)
    print("Te faltan:", e.monto - e.saldo)
```

## Validar input del usuario con while + try

El patrón más práctico de todos: pedir hasta que el dato sea válido.

```python
# Pedir un entero válido (el clásico)
while True:
    try:
        edad = int(input("Edad (0-120): "))
        if edad < 0 or edad > 120:
            print("Fuera de rango, intenta otra vez")
            continue
        break                    # dato válido: salimos del while
    except ValueError:
        print("Eso no es un número, intenta otra vez")

print("Edad guardada:", edad)
```

```python
# Pedir opción de un menú hasta que sea correcta (combinando con match)
while True:
    try:
        opcion = int(input("1-Ver 2-Salir: "))
        if opcion not in (1, 2):
            raise ValueError
        break
    except ValueError:
        print("Opción inválida")

print("Elegiste:", opcion)
```

## Ver también

- [[Ruta Python]]
- [[Funciones en Python]]
- [[Archivos en Python]]
- [[Variables y tipos de datos en Python]]
- [[MOC-programacion]]

## Problema resuelto

**Enunciado:** Pedir dos números y operarlos según lo que elija el usuario, manejando: entrada no numérica, división por cero y operación desconocida — sin que el programa se caiga nunca.

**Solución paso a paso:**
1. Pedir cada número con `try/except ValueError`.
2. Dividir con `try/except ZeroDivisionError` (o `if b == 0`).
3. Operación con `match` + `case _` (ver [[Match case en Python]]).

```python
try:
    a = float(input("Número 1: "))
    b = float(input("Número 2: "))
except ValueError:
    print("Entraste algo que no es número 😅")
else:
    op = input("Operación (+ - * /): ")
    match op:
        case "+":
            print("Resultado:", a + b)
        case "-":
            print("Resultado:", a - b)
        case "*":
            print("Resultado:", a * b)
        case "/":
            try:
                print("Resultado:", a / b)
            except ZeroDivisionError:
                print("División por cero imposible")
        case _:
            print("Operación desconocida")
```

## Problema propio

**Enunciado:** Crear una función `leer_entero(mensaje)` que pida al usuario un número entero **hasta** que lo ingrese bien (devolviéndolo), usando while + try. Después usarla 2 veces desde el código principal.

**Pista:** `while True:` → `try: return int(input(mensaje))` → `except ValueError: print("Reintentá")`. El `return` adentro del `try` sale de la función (y del while) solo.
