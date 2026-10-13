---
title: Strings en Python
area: programacion
tipo: lenguaje
tags: [programacion, python, strings]
created: 2026-10-12
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Variables y tipos de datos en Python]]", "[[Listas en Python]]", "[[Condicionales en Python]]"]
---

# Strings en Python

Un `str` es una secuencia de caracteres **inmutable**: podés leer y crear nuevos strings, pero no modificar uno existente. Se escriben con `"comillas"` o `'apóstrofes'`, y con `"""triple"""` para multilinea.

## Indexado y slicing

```python
texto = "Python"

print(texto[0])     # P      → índice desde 0
print(texto[2])     # t
print(texto[-1])    # n      → negativos cuentan desde el final
print(texto[-2])    # o

# slicing: [inicio:fin:paso] → fin es EXCLUSIVO
print(texto[0:3])   # Pyt    → desde 0 hasta el 2 (el 3 no entra)
print(texto[:3])    # Pyt    → inicio default 0
print(texto[3:])    # hon    → fin default = hasta el final
print(texto[:])     # Python → copia completa
print(texto[::2])   # Pto    → paso 2 (cada 2do carácter)
print(texto[::-1])  # nohtyP → paso -1 = invertir
```

## `len()`, concatenación e interpolación

```python
nombre = "Ferney"
print(len(nombre))          # 6

# Concatenación
saludo = "Hola " + nombre + "!"    # "Hola Ferney!"
saludo = "Hola " * 3               # "Hola Hola Hola " (repetición)

# F-strings: la forma moderna (Python 3.6+)
edad = 20
print(f"Hola {nombre}, tienes {edad} años")
print(f"El doble de tu edad es {edad * 2}")       # puede llevar expresiones
print(f"Resultado: {edad / 3:.2f}")               # formato: 2 decimales
print(f"{nombre=}")                               # debug: nombre='Ferney'
```

> [!tip] F-strings ganan
> `f"texto {variable}"` es más legible que concatenar con `+` y no tenés que acordarte del `str()`.

## Métodos de strings

```python
texto = "  hola Mundo  "

# --- MAYÚSCULAS / MINÚSCULAS ---
print("hola".upper())        # HOLA
print("HOLA".lower())        # hola
print("hola mundo".title())  # Hola Mundo   → primera de cada palabra
print("hola mundo".capitalize())  # Hola mundo → solo la primera

# --- ESPACIOS ---
print(texto.strip())         # "hola Mundo"  → quita espacios de LOS DOS lados
print(texto.lstrip())        # "hola Mundo  " → solo izquierda
print(texto.rstrip())        # "  hola Mundo" → solo derecha

# --- BUSCAR / REEMPLAZAR ---
print("hola mundo".replace("mundo", "ferney"))   # hola ferney
print("hola mundo".find("mundo"))    # 5    → índice donde empieza
print("hola mundo".find("xyz"))      # -1   → -1 si no existe (NO da error)
print("hola mundo".count("o"))       # 2    → cuántas veces aparece

# --- DIVIDIR / UNIR ---
print("a,b,c".split(","))      # ['a', 'b', 'c']  → lista de strings
print("a b  c".split())        # ['a', 'b', 'c']  → sin args: por espacios
print(" - ".join(["ene", "feb", "mar"]))   # ene - feb - mar

# --- VALIDAR ---
print("hola".startswith("ho"))    # True
print("foto.png".endswith(".png"))# True
print("123".isdigit())            # True  (solo dígitos)
print("abc123".isdigit())         # False
print("abc".isalpha())            # True  (solo letras)
print("abc123".isalpha())         # False
```

| Método | Qué hace | Ejemplo → resultado |
|---|---|---|
| `upper()` | todo mayúscula | `"hola".upper()` → `"HOLA"` |
| `lower()` | todo minúscula | `"HOLA".lower()` → `"hola"` |
| `title()` | mayúscula inicial de cada palabra | `"hola a todos".title()` → `"Hola A Todos"` |
| `capitalize()` | mayúscula inicial nomás | `"hola a todos".capitalize()` → `"Hola a todos"` |
| `strip()` | quita espacios de los extremos | `"  x  ".strip()` → `"x"` |
| `replace(a, b)` | reemplaza | `"aa".replace("a", "o")` → `"oo"` |
| `split(sep)` | divide → lista | `"a,b".split(",")` → `["a","b"]` |
| `join(lista)` | une → string | `"-".join(["a","b"])` → `"a-b"` |
| `find(sub)` | índice del substring (-1 si no está) | `"hola".find("la")` → `1` |
| `count(sub)` | cuántas apariciones | `"hola".count("l")` → `1` |
| `startswith/endswith` | ¿empieza/termina con...? | `"x.py".endswith(".py")` → `True` |
| `isdigit()/isalpha()` | ¿todo dígitos / todo letras? | `"12".isdigit()` → `True` |

```python
# Ojo con la inmutabilidad: los métodos devuelven NUEVO string
s = "hola"
s.upper()        # no hace nada visible
print(s)         # sigue "hola"
s = s.upper()    # así sí se guarda
print(s)         # HOLA
```

## Ver también

- [[Ruta Python]]
- [[Variables y tipos de datos en Python]]
- [[Listas en Python]]
- [[Funciones en Python]]
- [[MOC-programacion]]

## Problema resuelto

**Enunciado:** Recibir un string del usuario y mostrar: cuántos caracteres tiene, todo en mayúsculas, y si termina en vocal o no.

**Solución paso a paso:**
1. `len()` para el largo.
2. `upper()` + `endswith()` para la validación de vocal (probamos "A E I O U").

```python
texto = input("Texto: ")

print("Caracteres:", len(texto))
print("Mayúsculas:", texto.upper())

if texto.upper().endswith(("A", "E", "I", "O", "U")):
    print("Termina en vocal")
else:
    print("No termina en vocal")
```

## Problema propio

**Enunciado:** Pedir una frase y mostrarla con la primera letra de cada palabra en mayúscula, sin usar `title()`, recorriendo carácter por carácter.

**Pista:** Un string se recorre con `for c in texto:`. Acumulá en un string nuevo: si el carácter anterior era un espacio, lo subís con `.upper()`, si no con `.lower()`.
