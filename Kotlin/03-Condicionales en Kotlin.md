---
title: "Condicionales en Kotlin"
area: programacion
tipo: lenguaje
tags: [programacion, kotlin]
created: 2026-10-22
updated: 2026-10-22
related: ["[[MOC-programacion]]", "[[Ruta Kotlin]]", "[[02-Variables y tipos de datos en Kotlin]]", "[[04-Bucles y rangos en Kotlin]]"]
---

# 03 — Condicionales en Kotlin

## `if / else` — como expresión

En Kotlin `if` **devuelve un valor** (no es solo sentencia):

```kotlin
val edad = 20

if (edad >= 18) {
    println("Mayor de edad")
} else {
    println("Menor")
}
```

Como **expresión** (equivalente al ternario de Java/JS, pero más legible):

```kotlin
val estado = if (edad >= 18) "mayor" else "menor"

val maximo = if (a > b) a else b     // como el Math.max
```

> Sin `else`, el `if` **no** puede usarse como expresión (Kotlin exige que todo rama tenga valor).

### `else if` encadenado

```kotlin
val nota = 7.5
val calificacion = when {
    nota >= 9 -> "Sobresaliente"
    nota >= 7 -> "Notable"
    nota >= 5 -> "Aprobado"
    else -> "Suspenso"
}
```

## `when` — el switch de Kotlin (potente)

```kotlin
val dia = 3
when (dia) {
    1 -> println("Lunes")
    2 -> println("Martes")
    3 -> println("Miércoles")
    else -> println("Otro día")
}
```

### `when` con valores agrupados

```kotlin
when (dia) {
    1, 2, 3, 4, 5 -> println("Entre semana")
    6, 0 -> println("Fin de semana")
    else -> println("Inválido")
}
```

### `when` con rangos e in

```kotlin
when (puntuacion) {
    in 90..100 -> println("Excelente")
    in 60..89 -> println("Aprobado")
    !in 0..59 -> println("Fuera de rango")
    else -> println("?")
}
```

### `when` con tipos e inteligencia de tipos

```kotlin
when (valor) {
    is String -> println("Texto de ${valor.length} letras")
    is Int -> println("Entero doble: ${valor * 2}")
    is List<*> -> println("Lista de ${valor.size} elementos")
    else -> println("Otro tipo")
}
```

(`is` deja de ser `Any?` dentro de la rama → puedes usar `.length` sin cast.)

### `when` sin argumento (como `if/else if` puro)

```kotlin
val tienePermiso = when {
    usuario.rol == "admin" -> true
    usuario.rol == "editor" && edad >= 18 -> true
    else -> false
}
```

### `when` como expresión con variable

```kotlin
val mensaje = when (codigo) {
    200 -> "OK"
    404 -> "No encontrado"
    500 -> "Error del servidor"
    else -> "Desconocido"
}
```

## Operador elvis `?:` (con null safety)

```kotlin
val nombre: String? = null
val texto = nombre ?: "anónimo"   // si es null, "anónimo"
```

(ver completo en [[08-Null safety en Kotlin]])

## Cuándo usar / Cuándo NO usar

- `if` con `else` → puede ser expresión; sin `else` solo sentencia.
- Prefiere `when` sobre `if/else if` encadenado: más limpio y con rangos/tipos.
- `when` como expresión **exige** `else` (salvo que sea exhaustivo por tipos sellados).
- `when` con `is` ahorra casts manuales.

## Ver también

- [[04-Bucles y rangos en Kotlin]]
- [[02-Variables y tipos de datos en Kotlin]]
- [[Ruta Kotlin]]
- [[MOC-programacion]]
- [[HOME]]
