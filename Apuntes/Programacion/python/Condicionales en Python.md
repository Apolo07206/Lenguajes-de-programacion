---
title: Condicionales en Python
area: programacion
tipo: lenguaje
tags: [programacion, python, condicionales]
created: 2026-10-12
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Operadores en Python]]", "[[Bucles while en Python]]", "[[Match case en Python]]"]
---

# Condicionales en Python

`if` evalúa una condición: si da `True`, ejecuta el bloque indentado. No se usan paréntesis ni llaves: **la indentación delimita el bloque** y los dos puntos arrancan el bloque.

## if / elif / else

```python
edad = int(input("Tu edad: "))

if edad < 13:
    print("Niño")
elif edad < 18:          # elif = "else if" (tantos como quieras)
    print("Adolescente")
elif edad < 65:
    print("Adulto")
else:
    print("Adulto mayor")
```

```python
# OJO: sin else, si ninguna condición se cumple simplemente no imprime nada
# OJO: el else va solo, SIN condición, y SIEMPRE al final
```

## Condiciones combinadas y anidadas

```python
tiene_ticket = True
es_de_noche = False

# Combinadas con and / or / not
if tiene_ticket and es_de_noche:
    print("Entrás gratis")
elif tiene_ticket or es_de_noche:
    print("Entrás con descuento")
else:
    print("No entrás")

# Anidadas: un if dentro de otro (cada nivel, +4 espacios)
temperatura = 30
llueve = True

if temperatura > 25:
    if llueve:
        print("Caluroso pero mojado")
    else:
        print("Caluroso y soleado")
else:
    print("Hace fresco")
```

## Ternario (if en una línea)

```python
edad = 20

# forma if/else clásica
if edad >= 18:
    estado = "mayor"
else:
    estado = "menor"

# misma cosa en una línea (ternario)
estado = "mayor" if edad >= 18 else "menor"

print(f"Eres {estado} de edad")

# Muy usado para imprimir condicional
print("Par" if 4 % 2 == 0 else "Impar")

# Anidados (lee de adentro hacia afuera, pero evitalos si queda ilegible)
detalle = "A" if edad > 18 else ("B" if edad > 13 else "C")
```

> [!warning] Condiciones que siempre dan True
> `if 5:` o `if "texto":` son válidos (truthy), pero escribir `if 5 == 5:` donde querías `if x == 5` es un bug silencioso. Revisá siempre las variables de la condición.

## Errores comunes

```python
x = 5
# if x = 5:            ← SyntaxError (eso es asignación)
# if x == 5            ← SyntaxError (faltan los dos puntos)
# if (x == 5)          ← en Python los paréntesis son innecesarios (estilo Java/C)
```

## Ver también

- [[Ruta Python]]
- [[Operadores en Python]]
- [[Match case en Python]]
- [[Bucles while en Python]]
- [[MOC-programacion]]

## Problema resuelto

**Enunciado:** Clasificar una nota (0 a 10): ≥7 Aprobado, ≥5 Recuperación, <5 Reprobado, y rechazar notas fuera de rango.

**Solución paso a paso:**
1. Primero validamos el rango con un `if` aparte (guard clause).
2. Después encadenamos `if/elif/else` de mayor a menor.

```python
nota = float(input("Nota (0-10): "))

if nota < 0 or nota > 10:
    print("Nota fuera de rango")
elif nota >= 7:
    print("Aprobado")
elif nota >= 5:
    print("Recuperación")
else:
    print("Reprobado")
```

## Problema propio

**Enunciado:** Pedir un número del 1 al 7 y mostrar el día de la semana con `if/elif`, y luego **lo mismo** con el ternario en una línea (variable `dia = ...`).

**Pista:** Encadená `if n == 1: dia = "Lunes"` / `elif n == 2: ...` — para el ternario: `dia = "Lunes" if n == 1 else ("Martes" if n == 2 else ...)`. (En la práctica, esto es lo que resuelve [[Match case en Python]] de forma mucho más limpia.)
