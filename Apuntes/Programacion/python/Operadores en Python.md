---
title: Operadores en Python
area: programacion
tipo: lenguaje
tags: [programacion, python, operadores]
created: 2026-10-12
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Variables y tipos de datos en Python]]", "[[Condicionales en Python]]"]
---

# Operadores en Python

## Aritméticos

```python
a, b = 7, 3

print(a + b)    # 10  suma
print(a - b)    # 4   resta
print(a * b)    # 21  multiplicación
print(a / b)    # 2.3333...  división SIEMPRE da float
print(a // b)   # 2   división entera (descarta el decimal)
print(a % b)    # 1   módulo → el RESTO de la división
print(a ** b)   # 343 potencia (7³)
```

| Operador | Nombre | Ejemplo | Resultado |
|---|---|---|---|
| `+` | suma | `7 + 3` | `10` |
| `-` | resta | `7 - 3` | `4` |
| `*` | multiplicación | `7 * 3` | `21` |
| `/` | división | `7 / 3` | `2.333` |
| `//` | división entera | `7 // 3` | `2` |
| `%` | módulo (resto) | `7 % 3` | `1` |
| `**` | potencia | `7 ** 3` | `343` |

```python
# Usos prácticos del módulo (%)
print(10 % 2)    # 0 → es PAR (si % 2 da 0, es par)
print(7 % 2)     # 1 → es IMPAR
print(123 % 10)  # 3 → última cifra de un número
```

## De comparación

```python
a, b = 5, 10

print(a == b)   # False  igual a
print(a != b)   # True   distinto de
print(a > b)    # False  mayor que
print(a < b)    # True   menor que
print(a >= 5)   # True   mayor o igual
print(a <= 5)   # True   menor o igual

# Truco Python: comparaciones encadenadas (no existe en casi ningún lenguaje)
x = 7
print(1 < x < 10)   # True  ≡ (1 < x) and (x < 10)
```

> [!warning] `=` vs `==`
> `=` asigna (`x = 5`), `==` compara (`x == 5`). Mezclarlos en un `if` es el bug #1 de principiantes.

## Lógicos

```python
es_adulto = True
tiene_cedula = False

print(es_adulto and tiene_cedula)   # False  → AMBOS deben ser True
print(es_adulto or tiene_cedula)    # True   → basta UNO True
print(not es_adulto)                # False  → invierte

# Cortocircuito: en "A and B", si A es False no evalúa B
# En "A or B", si A es True no evalúa B
```

## De asignación (atajos)

```python
x = 10
x += 5    # x = x + 5  → 15
x -= 3    # x = x - 3  → 12
x *= 2    # x = x * 2  → 24
x /= 4    # x = x / 4  → 6.0 (acá queda float)
x //= 2   # x = x // 2 → 3.0... en realidad 3.0, cuidado con /
x **= 2   # x = x ** 2 → 9.0
x %= 4    # x = x % 4  → resto
```

## De identidad: `is` / `is not`

```python
# "is" compara IDENTIDAD (¿es el mismo objeto en memoria?), no valor
a = [1, 2, 3]
b = [1, 2, 3]
c = a

print(a == b)     # True  → mismo CONTENIDO
print(a is b)     # False → objetos DISTINTOS en memoria
print(a is c)     # True  → c apunta al MISMO objeto

# Caso especial: None SIEMPRE se compara con is
x = None
if x is None:        # forma canónica (no uses == None)
    print("no hay nada")
```

## De pertenencia: `in` / `not in`

```python
texto = "hola mundo"
lista = [1, 2, 3]
dic = {"nombre": "Ferney"}

print("mundo" in texto)        # True  → substring dentro del string
print(2 in lista)              # True  → elemento dentro de la lista
print(9 in lista)              # False
print("nombre" in dic)         # True  → en diccionarios busca CLAVES
print(9 not in lista)          # True
```

## Ver también

- [[Ruta Python]]
- [[Variables y tipos de datos en Python]]
- [[Condicionales en Python]]
- [[MOC-programacion]]

## Problema resuelto

**Enunciado:** Dado un número entero, decir si es PAR o IMPAR, y si es menor o mayor a 100.

**Solución paso a paso:**
1. Par/impar se resuelve con `num % 2 == 0`.
2. Comparar con 100 con `>`.
3. Combinamos con `and` o lo hacemos con dos `if`.

```python
num = int(input("Número: "))

if num % 2 == 0 and num > 100:
    print("Par y mayor a 100")
elif num % 2 == 0:
    print("Par")
elif num > 100:
    print("Impar pero mayor a 100")
else:
    print("Impar y menor o igual a 100")
```

## Problema propio

**Enunciado:** Dados 3 números, mostrar el mayor de los tres usando operadores de comparación y lógicos (sin usar `max()`, que lo vemos en [[Algoritmos con listas en Python]]).

**Pista:** `if a >= b and a >= c:` cubre el primer caso; el resto va en `elif`.
