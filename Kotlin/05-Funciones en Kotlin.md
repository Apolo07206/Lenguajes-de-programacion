---
title: "Funciones en Kotlin"
area: programacion
tipo: lenguaje
tags: [programacion, kotlin]
created: 2026-10-22
updated: 2026-10-22
related: ["[[MOC-programacion]]", "[[Ruta Kotlin]]", "[[04-Bucles y rangos en Kotlin]]", "[[07-Clases y data classes en Kotlin]]"]
---

# 05 — Funciones en Kotlin

## Declaración básica

```kotlin
fun saludar(nombre: String): String {
    return "Hola, $nombre"
}

fun main() {
    println(saludar("Ferxxo"))
}
```

### Expresión body (cuando es de una línea)

```kotlin
fun sumar(a: Int, b: Int): Int = a + b

// Kotlin infiere el tipo (devuelve Int)
fun multiplicar(a: Int, b: Int) = a * b
```

### Sin retorno (`Unit`)

```kotlin
fun imprimirSaludo(nombre: String) {   // Unit implícito
    println("Hola $nombre")
}
```

`Unit` es el `void` de Kotlin (pero es un tipo real).

## Argumentos con valor por defecto

```kotlin
fun crearUsuario(nombre: String, rol: String = "user", activo: Boolean = true) {
    println("$nombre · $rol · $activo")
}

crearUsuario("Ana")                          // usa defaults
crearUsuario("Ana", "admin")                 // positional
crearUsuario("Ana", activo = false)          // named argument
crearUsuario(nombre = "Ana", rol = "admin")  // todo named (muy legible)
```

> Los **named arguments** eliminan la torre de parámetros del estilo `crearUsuario("Ana", "admin", true, false, null)`.

## `vararg` (argumentos variables)

```kotlin
fun suma(vararg numeros: Int): Int {
    var total = 0
    for (n in numeros) total += n
    return total
}

println(suma(1, 2, 3))        // 6
println(suma(1, 2, 3, 4, 5)) // 15
```

`vararg` llega como `IntArray`. Para pasar un array: `*arrayOf(1,2)` (operador `*` de spread).

## Parámetros con nombre en la firma

```kotlin
fun imprimir(nombre: String = "", edad: Int = 0) = println("$nombre ($edad)")
```

## Funciones de una sola expresión con `it`

```kotlin
val doble = { x: Int -> x * 2 }     // lambda
println(doble(5))                    // 10
```

## Funciones locales (dentro de otras)

```kotlin
fun calcularTotal(precios: List<Double>): Double {
    fun conIva(p: Double) = p * 1.21
    return precios.sumOf { conIva(it) }
}
```

## `main` con argumentos

```kotlin
fun main(args: Array<String>) {
    args.forEach { println(it) }
}
```

## Funciones miembro vs top-level

```kotlin
// top-level (en el archivo, sin clase)
fun util() = "soy global"

class Servicio {
    // miembro
    fun procesar() = "soy del objeto"
}
```

Kotlin permite funciones en el nivel superior del archivo (Java no).

## Extension preview (firma sin cuerpo)

```kotlin
fun String.mayusculasPrimeras(): String =
    split(" ").joinToString(" ") { it.replaceFirstChar(Char::uppercase) }
```

Se define en [[07-Clases y data classes en Kotlin]] con más detalle.

## Cuándo usar / Cuándo NO usar

- Usa **expresión body** (`=`) cuando es de una línea → menos ruido.
- **Named arguments** cuando hay muchos parámetros o defaults.
- `vararg` para listas cortas y de longitud variable; si la lista es grande, mejor `List<Int>`.
- Funciones pequeñas con un solo uso → lambda local.

## Ver también

- [[07-Clases y data classes en Kotlin]]
- [[06-Colecciones en Kotlin]]
- [[Ruta Kotlin]]
- [[MOC-programacion]]
- [[HOME]]
