---
title: Recursión en Python
area: programacion
tipo: lenguaje
tags: [programacion, python, funciones, recursion]
created: 2026-10-12
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Funciones en Python]]", "[[Algoritmos de Ordenamientos en Java]]"]
---

# Recursión en Python

La recursión es cuando una función **se llama a sí misma**. Para que no se llame para siempre, necesitás sí o sí un **caso base** (la condición de parada) y un caso recursivo que se acerque al caso base.

## La idea en 2 frases

1. **Caso base:** la condición donde la función devuelve algo directo (sin seguir llamándose).
2. **Caso recursivo:** la función se llama a sí misma con un problema MÁS CHICO.

```python
# Sin caso base = cuelgue (RecursionError: máximo de profundidad alcanzado)
def loca():
    return loca()           # ❌ se llama para siempre
```

## Ejemplo 1: cuenta regresiva

```python
def cuenta_atras(n):
    if n == 0:              # CASO BASE: acá se detiene
        print("¡Despegue!")
        return
    print(n)
    cuenta_atras(n - 1)     # CASO RECURSIVO: n va achicándose hasta llegar a 0

cuenta_atras(5)
# 5 4 3 2 1 ¡Despegue!
```

## Ejemplo 2: factorial

```python
# factorial(n) = n * (n-1) * ... * 1    →  5! = 120
# Caso base: 0! = 1
# Recursivo: n! = n * (n-1)!

def factorial(n):
    if n == 0 or n == 1:    # CASO BASE
        return 1
    return n * factorial(n - 1)     # CASO RECURSIVO

print(factorial(5))     # 120  (5*4*3*2*1)
print(factorial(0))     # 1
```

### Cómo se despliega (el "qué hace por dentro")

```
factorial(5)
= 5 * factorial(4)
= 5 * (4 * factorial(3))
= 5 * (4 * (3 * factorial(2)))
= 5 * (4 * (3 * (2 * factorial(1))))
= 5 * (4 * (3 * (2 * 1)))     ← el caso base devuelve 1
= 5 * (4 * (3 * 2))
= 5 * (4 * 6)
= 5 * 24
= 120
```

## Ejemplo 3: fibonacci

```python
# fib(0)=0, fib(1)=1, fib(n)=fib(n-1)+fib(n-2)
# 0, 1, 1, 2, 3, 5, 8, 13, 21...

def fibonacci(n):
    if n == 0:              # CASO BASE 1
        return 0
    if n == 1:              # CASO BASE 2
        return 1
    return fibonacci(n - 1) + fibonacci(n - 2)

print([fibonacci(i) for i in range(10)])
# [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

> [!warning] Fibonacci es EXPONENCIAL
> `fibonacci(40)` tarda una eternidad porque recalcula lo mismo muchas veces. Para enteros grandes se usa memoización o un bucle. La recursión no siempre es la mejor herramienta.

## Ejemplo 4: suma de una lista

```python
def suma_lista(lista):
    if len(lista) == 0:     # CASO BASE: lista vacía suma 0
        return 0
    return lista[0] + suma_lista(lista[1:])   # primero + resto

print(suma_lista([3, 1, 4, 1, 5]))    # 14
```

## Ejemplo 5: contar elementos / invertir string

```python
# Contar sin len()
def contar(lista):
    if not lista:
        return 0
    return 1 + contar(lista[1:])

print(contar(["a", "b", "c"]))    # 3

# Invertir un string
def invertir(s):
    if len(s) <= 1:
        return s
    return invertir(s[1:]) + s[0]

print(invertir("python"))         # nohtyp
```

## Recursión vs bucle

```python
# Esto es igual de válido con un for
def factorial_for(n):
    resultado = 1
    for i in range(2, n + 1):
        resultado *= i
    return resultado

print(factorial_for(5))     # 120
```

| | Recursión | Bucle |
|---|---|---|
| Legibilidad | Más elegante para problemas "naturales" (árboles, factorial) | Más directo para secuencias simples |
| Riesgo | `RecursionError` si falta el caso base | Cuelgue si falta la condición de salida |
| Rendimiento | overhead de llamadas (Python: límite ~1000) | más rápido en general |

> [!note] Límite de Python
> Python tiene `sys.setrecursionlimit()` (por defecto 1000), pero NO lo subas: los problemas de profundidad se resuelven con bucles o reestructurando.

## Ver también

- [[Ruta Python]]
- [[Funciones en Python]]
- [[Algoritmos de Ordenamientos en Java]]
- [[MOC-programacion]]

## Problema resuelto

**Enunciado:** Calcular la potencia `base^exp` recursivamente (sin `**`), con caso base `exp == 0 → 1`.

**Solución paso a paso:**
1. Caso base: cualquier número elevado a 0 es 1.
2. Caso recursivo: `base * potencia(base, exp - 1)`.

```python
def potencia(base, exp):
    if exp == 0:            # CASO BASE
        return 1
    return base * potencia(base, exp - 1)    # CASO RECURSIVO

print(potencia(2, 10))      # 1024
print(potencia(5, 0))       # 1
# Despliegue: potencia(2,4) → 2*pot(2,3) → 2*2*pot(2,2) → ... → 2*2*2*2*1 = 16
```

## Problema propio

**Enunciado:** Escribir una función recursiva `contar_vocales(texto)` que cuente cuántas vocales hay en un string (ayudarse con `texto[0]` y `texto[1:]`).

**Pista:** Caso base: string vacío → 0. Si `texto[0]` es vocal → `1 + contar_vocales(texto[1:])`, si no → `contar_vocales(texto[1:])`. Verificá vocal con `texto[0].lower() in "aeiou"`.
