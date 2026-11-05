---
title: "Variables y tipos de datos en Kotlin"
area: programacion
tipo: lenguaje
tags: [programacion, kotlin]
created: 2026-10-22
updated: 2026-10-22
related: ["[[MOC-programacion]]", "[[Ruta Kotlin]]", "[[01-Introducción a Kotlin]]", "[[03-Condicionales en Kotlin]]"]
---

# 02 — Variables y tipos de datos en Kotlin

## Tipos numéricos

| Tipo | Tamaño | Uso |
| -------- | ---------- | -------------------------------- |
| `Int` | 32 bits | Enteros (el más común) |
| `Long` | 64 bits | Enteros grandes (`100L`) |
| `Double` | 64 bits | Decimales (por defecto) |
| `Float` | 32 bits | Decimales precisión corta (`1.5f`) |
| `Short` | 16 bits | Enteros pequeños |
| `Byte` | 8 bits | Mínimo |

```kotlin
val entero: Int = 42
val grande: Long = 10_000_000_000L    // L mayúscula
val decimal = 3.14                    // Double por defecto
val corto = 1.5f                      // Float (f minúscula)
val hexa = 0xFF
val binario = 0b1010

println(entero + decimal)   // Int + Double = Double
```

> Kotlin **no** hace conversión implícita: `Int + Double` → `Double`. Pero `Int + Long` **sí** funciona.

## Cadenas de texto `String`

```kotlin
val nombre = "Ferxxo"
val multi = """Texto
de varias líneas"""          // string literal multilínea

println(nombre.length)         // 6
println(nombre.uppercase())    // FERXXO
println(nombre[0])             // F (por índice)
println(nombre + " " + "Dev")  // concatenación
println("Hola ${nombre}")      // interpolación (preferible)
```

### Métodos útiles de String

```kotlin
"  hola  ".trim()          // quita espacios
"hola".contains("ol")      // true
"hola".replace("l", "r")   // hora
"hola".split("o")          // [h, la]
"123".toIntOrNull()        // Int? (null si no es número)
```

## Booleanos

```kotlin
val activo = true
val mayor = edad >= 18

println(activo && !mayor)   // && AND, || OR, ! NOT
```

## Conversiones (explícitas)

```kotlin
val i: Int = 42
val l: Long = i.toLong()      // entero a entero
val d: Double = i.toDouble()  // a decimal
val s: String = i.toString()  // a texto
val n: Int? = "123".toIntOrNull()  // texto a Int seguro (null si falla)
```

Métodos: `toInt()`, `toLong()`, `toDouble()`, `toString()`, `toBoolean()`. Para textos no numéricos usa `toIntOrNull()` → devuelve `null` en vez de excepción (ver [[08-Null safety en Kotlin]]).

## Carácter `Char`

```kotlin
val letra: Char = 'A'
println(letra.code)      // 65
println('A'.isUpperCase())  // true
```

## Constantes en objetos (compartidas)

```kotlin
const val PI = 3.14159   // solo en top-level o companion object
```

`const` es en tiempo de compilación; `val` normal se evalúa en ejecución.

## Arrays y rangos (avance)

```kotlin
val nums = arrayOf(1, 2, 3)
val rangos = 1..10        // rango inclusivo [1,10]
```

Detalles en [[06-Colecciones en Kotlin]] y [[04-Bucles y rangos en Kotlin]].

## Comparar: `==` vs `===`

```kotlin
val a = "hola"
val b = "hola"
a == b     // igualdad estructural (contenido) → true
a === b    // identidad (misma referencia) → puede ser false
```

En Kotlin `==` compara **contenido** (como `.equals()` de Java). `===` es referencia.

## Cuándo usar / Cuándo NO usar

- Usa `val` por defecto, `var` solo si reasignas.
- Prefiere `Double` para decimales; `Float` solo si importa el tamaño (gráficos, sensores).
- Para texto numérico no confiable usa `toIntOrNull()` + manejo de `null`.
- `10_000_000` con guiones bajos mejora la legibilidad.

## Ver también

- [[01-Introducción a Kotlin]]
- [[03-Condicionales en Kotlin]]
- [[Ruta Kotlin]]
- [[MOC-programacion]]
- [[HOME]]
