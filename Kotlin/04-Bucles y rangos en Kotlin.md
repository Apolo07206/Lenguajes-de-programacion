---
title: "Bucles y rangos en Kotlin"
area: programacion
tipo: lenguaje
tags: [programacion, kotlin]
created: 2026-10-22
updated: 2026-10-22
related: ["[[MOC-programacion]]", "[[Ruta Kotlin]]", "[[03-Condicionales en Kotlin]]", "[[06-Colecciones en Kotlin]]"]
---

# 04 — Bucles y rangos en Kotlin

## Rangos (`..`, `until`, `downTo`, `step`)

```kotlin
val r1 = 1..10        // 1,2,...,10   (inclusivo ambos)
val r2 = 1 until 10   // 1,2,...,9    (exclusivo arriba)
val r3 = 10 downTo 1  // 10,9,...,1   (hacia abajo)
val r4 = 0..10 step 2 // 0,2,4,6,8,10 (de 2 en 2)
val r5 = 'a'..'e'     // también con Char
```

| Operador | Significado |
| ------------ | ----------------------- |
| `..` | Inclusivo: `[a, b]` |
| `until` | Exclusivo: `[a, b)` |
| `downTo` | Descendente |
| `step` | Paso personalizado |
| `in` / `!in` | ¿Está en el rango? |

```kotlin
if (x in 1..10) println("dentro")
if (x !in 1..10) println("fuera")
```

## `for` — sobre rangos

```kotlin
for (i in 1..5) {
    println(i)          // 1 2 3 4 5
}

for (i in 5 downTo 1) println(i)   // 5 4 3 2 1
for (i in 0..10 step 3) println(i) // 0 3 6 9
```

## `for` — sobre colecciones

```kotlin
val frutas = listOf("manzana", "pera", "uva")

for (fruta in frutas) println(fruta)

for ((indice, fruta) in frutas.withIndex()) {
    println("$indice: $fruta")
}
```

## `for` — con índice manual (evitar)

```kotlin
val nums = listOf(10, 20, 30)
for (i in nums.indices) println(nums[i])   // funciona pero menos idiomático
```

> Kotlin prefiere recorrer la colección directamente o `withIndex()`.

## `while` y `do-while`

```kotlin
var i = 0
while (i < 5) {
    println(i)
    i++
}

var j = 0
do {
    println(j)      // se ejecuta AL MENOS una vez
    j++
} while (j < 5)
```

## `break` y `continue`

```kotlin
for (i in 1..10) {
    if (i == 5) continue   // salta el 5
    if (i > 8) break       // corta el bucle
    println(i)             // 1 2 3 4 6 7 8
}
```

## `repeat` (elegante para N iteraciones)

```kotlin
repeat(3) { println("Hola $it") }   // it = 0,1,2
```

## Bucles infinitos

```kotlin
while (true) {
    // ... rompe con break cuando toque
    if (condicion) break
}
```

## Iterar mientras (secuencias)

```kotlin
val lista = listOf(1, 2, 3, 4)
lista.takeWhile { it < 3 }.forEach { println(it) }  // 1 2
```

Las funciones de orden superior (`takeWhile`, `filter`, `map`) se detallan en [[06-Colecciones en Kotlin]].

## Cuándo usar / Cuándo NO usar

- Colección conocida → `for (x in lista)`.
- Rango → `for (i in 1..n)` / `until` / `downTo` / `step`.
- Condición variable o al menos una pasada → `while` / `do-while`.
- N fija sin lógica → `repeat(n)`.
- Evita bucles anidados profundos: Kotlin te da `flatMap`, `forEach` anidado o `map`.

## Ver también

- [[06-Colecciones en Kotlin]]
- [[03-Condicionales en Kotlin]]
- [[Ruta Kotlin]]
- [[MOC-programacion]]
- [[HOME]]
