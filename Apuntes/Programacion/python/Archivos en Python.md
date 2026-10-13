---
title: Archivos en Python
area: programacion
tipo: lenguaje
tags: [programacion, python, archivos]
created: 2026-10-12
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Manejo de errores en Python]]", "[[Módulos y librerías en Python]]"]
---

# Archivos en Python

Leer y escribir archivos con `open()`: texto plano (`.txt`), CSV (con el módulo `csv`) y JSON (con `json`). La forma segura es siempre `with open(...)`.

## open() y modos

```python
# open(ruta, modo) → devuelve un objeto archivo
# MODOS:
# "r"  → read (LECTURA). Si el archivo no existe: FileNotFoundError
# "w"  → write (ESCRITURA). Si existe: LO VACÍA (¡cuidado!). Si no: lo crea
# "a"  → append (AGREGAR). Si existe: agrega al final. Si no: lo crea
# "r+" → leer y escribir
# En modo texto agregá encoding: open(..., encoding="utf-8")

archivo = open("saludo.txt", "w", encoding="utf-8")
archivo.write("Hola Ferney\n")
archivo.write("Segunda línea\n")
archivo.close()             # SIEMPRE cerrar (si no usás with)

contenido = open("saludo.txt", "r", encoding="utf-8").read()
print(contenido)
```

> [!warning] `"w"` borra
> `open("datos.txt", "w")` **trunca el archivo** (lo deja vacío) antes de escribir. Para agregar sin borrar, usá `"a"`.

## with open() — la forma correcta

```python
# with cierra el archivo SOLO, aunque el programa dé error en el medio
with open("saludo.txt", "w", encoding="utf-8") as f:
    f.write("Primera línea\n")
    f.write("Segunda línea\n")
# el archivo ya está cerrado acá

with open("saludo.txt", "r", encoding="utf-8") as f:
    contenido = f.read()
    print(contenido)
```

## read / readline / readlines

```python
with open("saludo.txt", "r", encoding="utf-8") as f:
    todo = f.read()             # TODO el contenido como UN string
    print(todo)

with open("saludo.txt", "r", encoding="utf-8") as f:
    linea = f.readline()        # UNA línea (con el \n final)
    print(repr(linea))
    linea2 = f.readline()       # la siguiente (el cursor va avanzando)
    print(repr(linea2))

with open("saludo.txt", "r", encoding="utf-8") as f:
    lineas = f.readlines()      # TODAS las líneas como LISTA de strings
    print(lineas)               # ['Primera línea\n', 'Segunda línea\n']

# Lo más común: recorrer el archivo línea por línea (memoria eficiente)
with open("saludo.txt", "r", encoding="utf-8") as f:
    for linea in f:
        print(linea.strip())    # strip() quita el \n
```

## write / writelines

```python
with open("notas.txt", "w", encoding="utf-8") as f:
    f.write("Ana,4.5\n")        # write() no mete \n: agregalo vos
    f.write("Luis,3.2\n")

with open("notas.txt", "a", encoding="utf-8") as f:
    f.write("Marta,4.9\n")      # agrega al final sin borrar

with open("lista.txt", "w", encoding="utf-8") as f:
    f.writelines(["uno\n", "dos\n", "tres\n"])   # lista de strings

# Copiar archivo línea por línea (patrón típico)
with open("origen.txt") as src, open("copia.txt", "w") as dst:
    for linea in src:
        dst.write(linea)
```

## CSV

```python
import csv

# ESCRIBIR CSV
with open("jugadores.csv", "w", newline="", encoding="utf-8") as f:
    escritor = csv.writer(f)
    escritor.writerow(["nombre", "puntos"])       # fila de encabezados
    escritor.writerow(["Ana", 15])
    escritor.writerow(["Luis", 9])

# También desde diccionarios
with open("jugadores2.csv", "w", newline="", encoding="utf-8") as f:
    escritor = csv.DictWriter(f, fieldnames=["nombre", "puntos"])
    escritor.writeheader()
    escritor.writerow({"nombre": "Marta", "puntos": 22})

# LEER CSV
with open("jugadores.csv", "r", encoding="utf-8") as f:
    lector = csv.reader(f)               # cada fila es una LISTA
    encabezados = next(lector)           # next() = primera fila
    for fila in lector:
        print(f"{fila[0]}: {fila[1]} pts")

# LEER CSV como diccionarios (las columnas tienen nombre)
with open("jugadores2.csv", "r", encoding="utf-8") as f:
    for fila in csv.DictReader(f):
        print(fila["nombre"], fila["puntos"])   # ya no importa el orden
```

## JSON

```python
import json

datos = {"nombre": "Ferney", "edad": 20, "hobbies": ["python", "ciber"]}

# ESCRIBIR JSON
with open("persona.json", "w", encoding="utf-8") as f:
    json.dump(datos, f, indent=4, ensure_ascii=False)   # indent = legibilidad

# también como string: texto = json.dumps(datos, indent=2)

# LEER JSON
with open("persona.json", "r", encoding="utf-8") as f:
    recuperado = json.load(f)            # dict/list de Python
    print(recuperado["nombre"])          # Ferney

# desde string: json.loads(texto_json)
# Ojo: las claves del JSON vuelven como STR ({"edad": 20} no es {edad: 20})
```

> [!tip] `ensure_ascii=False`
> Sin eso, las tildes y ñ se guardan como `\u00f1`. Con `False` se guardan tal cual (legible).

## Manejo de errores al trabajar con archivos

```python
# Combinar con try/except (ver [[Manejo de errores en Python]])
try:
    with open("inexistente.txt", "r", encoding="utf-8") as f:
        print(f.read())
except FileNotFoundError:
    print("El archivo no existe")

# JSON mal formado
try:
    with open("datos.json") as f:
        json.load(f)
except json.JSONDecodeError:
    print("El JSON está roto")
```

## Ver también

- [[Ruta Python]]
- [[Manejo de errores en Python]]
- [[Módulos y librerías en Python]]
- [[Conceptos de mysql]]
- [[MOC-programacion]]

## Problema resuelto

**Enunciado:** Crear un registro de contactos: pedir nombre y teléfono, guardarlos en `contactos.csv` (agregando sin borrar lo anterior), y al final listar todos los contactos del archivo.

**Solución paso a paso:**
1. Abrir en modo `"a"` con `csv.writer` para agregar.
2. Escribir encabezado solo si el archivo está vacío (o la primera vez).
3. Leer con `csv.reader` y mostrar.

```python
import csv
import os

# 1) Agregar contacto
nombre = input("Nombre: ")
tel = input("Teléfono: ")

existe = os.path.exists("contactos.csv")
with open("contactos.csv", "a", newline="", encoding="utf-8") as f:
    escritor = csv.writer(f)
    if not existe:
        escritor.writerow(["nombre", "telefono"])   # encabezado solo la primera vez
    escritor.writerow([nombre, tel])

# 2) Listar contactos
print("--- Contactos ---")
with open("contactos.csv", "r", encoding="utf-8") as f:
    lector = csv.reader(f)
    next(ctor)  # saltar encabezado
    for fila in lector:
        print(f"{fila[0]} → {fila[1]}")
```

## Problema propio

**Enunciado:** Pedir 3 números al usuario, guardarlos en `numeros.json`, y luego leer el archivo mostrando la suma de los números guardados.

**Pista:** Acumulá los números en una lista, `json.dump(lista, f)`, y después `json.load(f)` + `sum()`.
