---
title: Bucles while en Python
area: programacion
tipo: lenguaje
tags: [programacion, python, bucles, while]
created: 2026-10-12
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Condicionales en Python]]", "[[Bucles for en Python]]", "[[Control de bucles en Python]]"]
---

# Bucles while en Python

`while` repite un bloque **mientras** la condición sea `True`. Lo usás cuando no sabés cuántas vueltas van a ser (a diferencia del `for`, que las veces que vas a dar las sabés de antemano).

## La condición

```python
# Contar del 1 al 5
contador = 1

while contador <= 5:
    print(contador)
    contador += 1      # SIN esta línea, el while NUNCA termina

print("Fin del bucle")
```

Flujo: Python evalúa `contador <= 5` → `True` → ejecuta el bloque → vuelve a evaluar... hasta que la condición da `False` y sale.

## Contadores y acumuladores

```python
# Sumar los números que va pidiendo el usuario hasta que escriba 0
total = 0
num = int(input("Número (0 para salir): "))

while num != 0:
    total += num
    num = int(input("Número (0 para salir): "))

print("La suma total es:", total)
```

```python
# Contar cuántos positivos hay
positivos = 0
n = int(input("Número: "))

while n != -1:                  # -1 como "señal de stop"
    if n > 0:
        positivos += 1
    n = int(input("Número: "))

print("Diste", positivos, "positivos")
```

## Bucles infinitos (y cómo salir)

```python
# Un while con condición siempre True = bucle infinito
while True:                     # se ejecuta para siempre
    respuesta = input("¿Continuar? (s/n): ")
    if respuesta == "n":
        break                   # break rompe el bucle AHORA
    print("Dale, seguimos...")

print("Saliste del bucle")
```

> [!warning] Bucle infinito accidental
> `while contador < 5:` con un `contador` que nunca cambia dentro = cuelgue seguro (Ctrl+C para cortar en la terminal). Revisá que algo del bloque modifique la condición o uses `break`.

## Do-while: no existe en Python (el truco `while True` + `break`)

En C/Java existe el `do-while`: ejecuta el cuerpo **al menos una vez** y recién DESPUÉS chequea la condición. Python no lo tiene, pero se emula con `while True:` + `break` al final del cuerpo:

```c
// Sintaxis en C/Java (para que la reconozcas si la ves)
do {
    pedir_datos();
} while (datos_invalidos);
```

```python
# Equivalente en Python: el cuerpo corre SIEMPRE (como el do),
# y la condición de salida queda al final con break (como el while)
while True:
    pedir_datos()               # 1) el cuerpo primero (¡al menos una vez!)
    if not datos_invalidos:
        break                   # 2) condición de salida al final = el "while cond"
```

```python
# Ejemplo real: pedir un número del 1 al 5 hasta que sea válido
while True:
    texto = input("Número (1-5): ")
    if texto.isdigit() and 1 <= int(texto) <= 5:
        numero = int(texto)
        break                   # dato válido: salgo
    print("Inválido, probá otra vez")   # solo se muestra si NO hicimos break

print("Elegiste:", numero)
```

```python
# Otro do-while clásico: menú que se muestra SIEMPRE antes de preguntar
while True:
    print("--- MENÚ ---")
    print("1. Opción A")
    print("2. Opción B")
    print("3. Salir")
    opcion = input("> ")

    if opcion == "3":
        print("Chau 👋")
        break
    elif opcion == "1":
        print("Hiciste A")
    elif opcion == "2":
        print("Hiciste B")
    else:
        print("Opción inválida")
    # el menú se volvió a mostrar: el ciclo repite aunque la opción sea mala
```

Con `continue` en esta emulación: vuelve arriba y repite el cuerpo (como en un do-while real). Cuidado: si el `continue` queda ANTES de donde se piden/calculan los datos, el ciclo se cuelga — en `while`, todo lo que haga avanzar el estado tiene que ejecutarse antes del `continue` (lo vimos arriba). La versión con validación robusta de input (while + try) está en [[Manejo de errores en Python]].

## `while` con `continue`

```python
# Imprimir solo los pares del 1 al 10, saltando los impares
n = 1

while n <= 10:
    if n % 2 != 0:
        n += 1           # OJO: antes del continue hay que avanzar el contador
        continue         # saltea el resto del bloque y vuelve a evaluar
    print(n)
    n += 1
```

> [!warning] El continue que cuelga
> En `while`, si usás `continue` sin avanzar el contador antes, quedás en un loop infinito que ni el `break` salva. Regla: en `while` + `continue`, **siempre incrementá antes**.

## `while True` + bandera (flag)

```python
# Bandera: una variable booleana que controla el ciclo
encontrado = False
numerito = 3

while not encontrado:
    intento = int(input("Adivina el número (1-5): "))
    if intento == numerito:
        encontrado = True       # bandera en True → el while termina
    else:
        print("Otra vez...")

print("¡Adivinaste!")
```

## Ver también

- [[Ruta Python]]
- [[Bucles for en Python]]
- [[Control de bucles en Python]]
- [[Condicionales en Python]]
- [[MOC-programacion]]

## Problema resuelto

**Enunciado:** Juego de adivinar un número: el programa tiene el número 7 guardado y el usuario intenta hasta acertar. Si se rinde (ingresa 0), mostrar cuál era.

**Solución paso a paso:**
1. `while True` porque no sabemos cuántos intentos hará.
2. Pedimos el intento; si es 0 → `break` (rendición).
3. Si acertó → mensaje y `break`.
4. Si no → seguimos.

```python
secreto = 7

while True:
    intento = int(input("Adivina (0 para rendirse): "))

    if intento == 0:
        print("El número era:", secreto)
        break
    elif intento == secreto:
        print("¡Correcto! 🎉")
        break
    else:
        print("No es, prueba otra vez")
```

## Problema propio

**Enunciado:** Pedir números al usuario y mostrar cuál es el mayor que ingresó, hasta que escriba un número negativo (que no se cuenta).

**Pista:** Inicializá `mayor = None`. Dentro del `while`, si el número es negativo → `break`; si `mayor is None` o el número es mayor que `mayor` → `mayor = numero`. (La versión bonita con `max()` está en [[Algoritmos con listas en Python]].)
