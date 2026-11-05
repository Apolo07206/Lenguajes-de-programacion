---
title: Módulos y librerías en Python
area: programacion
tipo: lenguaje
tags: [programacion, python, modulos]
created: 2026-10-12
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Archivos en Python]]", "[[Funciones en Python]]", "[[Sockets]]"]
---

# Módulos y librerías en Python

Un módulo es un archivo `.py` con código reutilizable (funciones, clases, variables). La librería estándar de Python trae montones ya instalados (`math`, `random`, `datetime`, `os`, `csv`, `json`...), y con `pip` instalás de terceros.

## import — las formas

```python
# 1) Importar el módulo entero (usando prefijo: forma más segura)
import math
print(math.sqrt(16))        # 4.0
print(math.pi)              # 3.14159...

# 2) Importar solo lo que necesitás
from math import sqrt, pi
print(sqrt(16))             # ya sin prefijo
print(pi)

# 3) Importar todo (evitar: llena el namespace y genera conflictos)
from math import *
print(sqrt(16))

# 4) Renombrar con as (muy común: pandas as pd, numpy as np)
import math as m
print(m.sqrt(25))           # 5.0

from datetime import datetime as dt
print(dt.now())
```

## math — matemáticas

```python
import math

print(math.pi)              # 3.141592653589793
print(math.e)               # 2.718281828459045
print(math.sqrt(144))       # 12.0        raíz cuadrada
print(math.pow(2, 10))      # 1024.0      potencia (como **)
print(math.ceil(4.2))       # 5           redondea hacia ARRIBA
print(math.floor(4.8))      # 4           redondea hacia ABAJO
print(math.round(4.567, 2)) # 4.57        (o el built-in round())
print(math.factorial(5))    # 120         (ver [[Recursión en Python]])
print(math.gcd(12, 8))      # 4           máximo común divisor
print(math.log(100, 10))    # 2.0         logaritmo base 10
```

## random — azar

```python
import random

print(random.random())              # 0.0 a 1.0 (decimal)
print(random.randint(1, 6))         # 1 a 6 entero (ambos inclusive)
print(random.uniform(1.5, 3.5))     # decimal entre 1.5 y 3.5
print(random.choice(["a", "b", "c"]))    # elige UNO de la lista
print(random.sample([1, 2, 3, 4], 2))    # elige 2 SIN repetir
print(random.choices(["a", "b"], k=3))   # elige 3 PUEDE repetir

numeros = [1, 2, 3, 4, 5]
random.shuffle(numeros)             # baraja in-place
print(numeros)

random.seed(42)                     # fija el azar (para resultados reproducibles)
```

## datetime — fechas y horas

```python
from datetime import datetime, timedelta

ahora = datetime.now()
print(ahora)                        # 2026-10-12 11:30:45.123456
print(ahora.strftime("%d/%m/%Y"))   # 12/10/2026
print(ahora.strftime("%H:%M:%S"))
print(ahora.year, ahora.month, ahora.day)

# Parsear string a fecha
fecha = datetime.strptime("25/12/2026", "%d/%m/%Y")
print(fecha.year)                   # 2026

# Operaciones con timedelta
en_3_dias = ahora + timedelta(days=3)
print(en_3_dias.strftime("%d/%m/%Y"))

# Diferencia entre fechas
evento = datetime(2026, 12, 31)
dias_faltan = (evento - ahora).days
print("Faltan", dias_faltan, "días")
```

## os — sistema operativo

```python
import os

print(os.getcwd())                          # carpeta actual
os.chdir("/tmp")                            # cambiar de carpeta
print(os.listdir("."))                      # archivos de la carpeta
print(os.path.exists("archivo.txt"))        # ¿existe?
print(os.path.isfile("archivo.txt"))        # ¿es archivo?
print(os.path.isdir("Assets"))              # ¿es carpeta?
print(os.path.join("Assets", "foto.png"))   # ruta correcta por SO

os.rename("viejo.txt", "nuevo.txt")         # renombrar
os.remove("nuevo.txt")                      # borrar archivo
os.makedirs("salida/datos", exist_ok=True)  # crear carpetas (anidadas)
```

## Tu propio módulo

```python
# archivo: utilidades.py  (misma carpeta que tu script)
def saludar(nombre):
    return f"Hola {nombre}"

PI = 3.14159
```

```python
# archivo: main.py
import utilidades                     # python busca en la misma carpeta

print(utilidades.saludar("Ferney"))
print(utilidades.PI)

# o directo:
from utilidades import saludar
print(saludar("Ana"))

# python main.py   ← así lo ejecutás
```

> [!note] Import sin comillas
> `import utilidades` va SIN `.py`. Python busca en la carpeta del script, en la librería estándar y en los paquetes instalados.

## pip — instalar paquetes de terceros

```bash
pip install requests                # instalar
pip install requests==2.31.0        # versión exacta
pip list                            # qué tengo instalado
pip uninstall requests              # desinstalar
pip freeze > requirements.txt       # exportar dependencias
pip install -r requirements.txt     # instalar desde el archivo

# En Linux/Mac a veces hace falta:
pip3 install requests
python3 -m pip install requests     # forma "segura" universal
```

```python
# Ejemplo real: requests (HTTP)
import requests

respuesta = requests.get("https://api.github.com")
print(respuesta.status_code)         # 200 si todo bien
print(respuesta.json())              # respuesta como dict (ver [[Archivos en Python]])
```

## Entornos virtuales (venv)

Un venv aísla las dependencias por proyecto: cada proyecto con su versión de cada paquete, sin romper el sistema.

```bash
# 1) Crear el entorno (crea la carpeta .venv/)
python -m venv .venv

# 2) Activar
# Linux / Mac:
source .venv/bin/activate
# Windows (PowerShell):
.venv\Scripts\Activate.ps1

# 3) Instalar dentro del entorno
pip install requests

# 4) Desactivar (vuelve al Python del sistema)
deactivate

# 5) Exportar/importar dependencias
pip freeze > requirements.txt
pip install -r requirements.txt
```

> [!warning] Buenas prácticas
> - Un venv por proyecto.
> - Agregá `.venv/` a tu `.gitignore`.
> - Nunca instales paquetes random en el Python del sistema (especialmente en Kali).

## Ver también

- [[Ruta Python]]
- [[Archivos en Python]]
- [[Funciones en Python]]
- [[Sockets]]
- [[MOC-programacion]]

## Problema resuelto

**Enunciado:** Crear una lotería: generar 6 números aleatorios distintos entre 1 y 49, ordenarlos, y mostrar la fecha de hoy con formato `12/10/2026`.

**Solución paso a paso:**
1. `random.sample(range(1, 50), 6)` → 6 distintos sin repetir.
2. `sorted()` para ordenarlos.
3. `datetime.now().strftime("%d/%m/%Y")` para la fecha.

```python
import random
from datetime import datetime

numeros = sorted(random.sample(range(1, 50), 6))
fecha = datetime.now().strftime("%d/%m/%Y")

print(f"Lotería del {fecha}: {numeros}")
```

## Problema propio

**Enunciado:** Crear un módulo propio `matematicas.py` con una función `es_primo(n)` (con bucle, ver [[Control de bucles en Python]]), y un `main.py` que lo importe para mostrar los primos del 1 al 30.

**Pista:** En `matematicas.py` usá el patrón `for i in range(2, n)` + `else` del bucle (el `else` se ejecuta si no hubo `break`); en `main.py`: `from matematicas import es_primo`.
