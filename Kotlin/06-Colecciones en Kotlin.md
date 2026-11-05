---
title: "Colecciones en Kotlin"
area: programacion
tipo: lenguaje
tags: [programacion, kotlin]
created: 2026-10-22
updated: 2026-10-22
related: ["[[MOC-programacion]]", "[[Ruta Kotlin]]", "[[04-Bucles y rangos en Kotlin]]", "[[05-Funciones en Kotlin]]"]
---

# 06 — Colecciones en Kotlin

## Las 3 familias

| Colección | Orden | Duplicados | Mutabilidad |
| --------- | ---------- | ---------- | ------------------- |
| `List` | Sí | Sí | `listOf` (fijo) / `mutableListOf` |
| `Set` | No* | **No** | `setOf` / `mutableSetOf` |
| `Map` | Sí* | Claves únicas | `mapOf` / `mutableMapOf` |

\* `LinkedHashSet/LinkedHashMap` preservan el orden de inserción.

## Listas

```kotlin
val frutas = listOf("manzana", "pera", "uva")        // inmutable
val nums = mutableListOf(1, 2, 3)                    // mutable
nums.add(4)
nums.removeAt(0)

println(frutas.size)        // 3
println(frutas[0])          // "manzana"
println(frutas.first())     // "manzana"
println(frutas.last())      // "uva"
println("uva" in frutas)    // true
```

Acceso seguro (sin excepción si está vacía):

```kotlin
val opcional = frutas.getOrNull(10)   // null en vez de crash
val valor = frutas.getOrElse(10) { "por defecto" }
```

## Sets (sin duplicados)

```kotlin
val tags = setOf("python", "kotlin", "python")
println(tags.size)      // 2 (deduplica)

val mutable = mutableSetOf("a")
mutable.add("b")
mutable.add("a")        // ignorado
```

Útil para quitar duplicados de una lista:

```kotlin
val sinDuplicados = listOf(1, 2, 2, 3).toSet().toList()
```

## Mapas (clave → valor)

```kotlin
val edades = mapOf("Ana" to 25, "Luis" to 31)
println(edades["Ana"])          // 25
println(edades["Bob"])          // null (no lanza)

val mutable = mutableMapOf("a" to 1)
mutable["b"] = 2
mutable.put("c", 3)
mutable.remove("a")

for ((clave, valor) in edades) println("$clave = $valor")
```

## Funciones de orden superior (la joya de Kotlin)

```kotlin
val nums = listOf(1, 2, 3, 4, 5, 6)

nums.filter { it % 2 == 0 }        // [2, 4, 6]   (filtro)
nums.map { it * it }               // [1, 4, 9, 16, 25, 36]  (transforma)
nums.forEach { println(it) }       // recorrer
nums.any { it > 5 }                // true (¿alguno cumple?)
nums.all { it > 0 }                // true (¿todos cumplen?)
nums.count { it % 2 == 0 }         // 3
nums.find { it > 3 }               // 4 (primero) o null
nums.firstOrNull { it > 5 }        // 6
nums.sortedDescending()            // [6, 5, 4, 3, 2, 1]
nums.sum()                         // 21
nums.average()                     // 3.5
```

### Encadenar (pipeline legible)

```kotlin
val resultado = listOf(1, 2, 3, 4, 5, 6)
    .filter { it % 2 == 0 }     // pares
    .map { it * 10 }            // x10
    .sortedDescending()         // ordena
// [60, 40, 20]
```

### `reduce` y `fold`

```kotlin
val suma = listOf(1, 2, 3).reduce { acc, n -> acc + n }   // 6
val cuenta = listOf(1, 2, 3).fold("") { acc, n -> "$acc$n" } // "123"
```

### `groupBy` / `associate` / `zip`

```kotlin
val porParidad = listOf(1, 2, 3, 4).groupBy { if (it % 2 == 0) "par" else "impar" }
// {impar=[1, 3], par=[2, 4]}

val mapa = listOf("a", "b").associateWith { it.uppercase() }  // {a=A, b=B}

val pares = listOf("a", "b") zip listOf(1, 2)   // [(a,1), (b,2)]
```

## Colecciones + `it`

En lambdas de un solo parámetro, `it` es el atajo implícito:

```kotlin
listOf(1, 2, 3).map { it * 2 }              // it = cada elemento
mapa.forEach { (clave, valor) -> println("$clave=$valor) }  // desestructurar
```

## Sequences (evaluación perezosa)

```kotlin
val res = (1..1_000_000)
    .asSequence()
    .filter { it % 2 == 0 }
    .map { it * it }
    .take(3)
    .toList()
```

Para listas **enormes** con muchas operaciones encadenadas: `asSequence()` evita crear listas intermedias.

## Cuándo usar / Cuándo NO usar

- Lista ordenada con repetidos → `List`. Sin repetidos → `Set`. Asociar → `Map`.
- `filter/map/forEach` en vez de bucles manuales → menos errores.
- Colecciones grandes + varios pasos → `Sequence`.
- Recuerda: `listOf` es **inmutable** (añade con `mutableListOf` desde el inicio si vas a modificar).

## Ver también

- [[04-Bucles y rangos en Kotlin]]
- [[05-Funciones en Kotlin]]
- [[07-Clases y data classes en Kotlin]]
- [[Ruta Kotlin]]
- [[MOC-programacion]]
- [[HOME]]
