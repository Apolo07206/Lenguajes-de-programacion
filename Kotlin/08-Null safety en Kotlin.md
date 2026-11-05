---
title: "Null safety en Kotlin"
area: programacion
tipo: lenguaje
tags: [programacion, kotlin, null-safety]
created: 2026-10-22
updated: 2026-10-22
related: ["[[MOC-programacion]]", "[[Ruta Kotlin]]", "[[02-Variables y tipos de datos en Kotlin]]", "[[22-Manejo de errores en Python]]"]
---

# 08 — Null safety en Kotlin

## Qué es / Para qué sirve

La `NullPointerException` es la excepción más famosa de la JVM. Kotlin la **elimina en compilación** separando dos mundos:

- `String` → **nunca** null (el compilador te obliga a inicializarlo).
- `String?` → **puede** ser null (nullable).

```kotlin
var a: String = "hola"
a = null            // ERROR de compilación

var b: String? = null   // OK, declarado nullable
b = "hola"
b = null                // OK
```

## Cómo usar un nullable (obligatorio)

```kotlin
var nombre: String? = "Ferxxo"

// ❌ ERROR: no puedes usar directamente
// println(nombre.length)      // compile error

// ✅ Opción 1: comprobación explícita
if (nombre != null) {
    println(nombre.length)    // smart cast: Kotlin sabe que no es null
}

// ✅ Opción 2: safe call ?. (devuelve null si lo es)
val largo = nombre?.length    // Int? (puede ser null)

// ✅ Opción 3: elvis ?: (valor alternativo)
val texto = nombre ?: "anónimo"

// ✅ Opción 4: non-null assert !! (¡peligroso!)
val riesgo = nombre!!.length  // lanza NPE si es null
```

## El operador elvis `?:` (el más útil)

```kotlin
val config = cargarConfig() ?: ConfigPorDefecto()
val puerto = args.getOrNull(0)?.toInt() ?: 8080
```

Se lee: "si es null, usa esto".

## Safe call encadenado `?.`

```kotlin
val ciudad = usuario?.direccion?.ciudad   // corta la cadena en el primer null
```

Sin esto, cada salto podría reventar.

## `let` — ejecutar solo si no es null

```kotlin
val nombre: String? = obtenerNombre()

nombre?.let {
    println("Bienvenido $it")   // solo se ejecuta si no es null
    guardar(it)
}
```

## `!!` — úsalo con respeto

```kotlin
val largo = nombre!!.length
```

Convierte `String?` a `String` **forzando**. Si era null → NPE en ejecución. Solo válido cuando **estás seguro** (o en pruebas rápidas). Es la única puerta de entrada a NPE.

## Tipos nullable vs colecciones

```kotlin
val lista: List<String?> = listOf("a", null, "b")   // elementos anulables
val listaNula: List<String>? = null                  // lista completa anulable
val listaFija: List<String> = listOf("a")            // ni elementos ni lista null
```

## `lateinit` y `lazy` (inicialización diferida)

```kotlin
// lateinit: se asigna DESPUÉS (solo var, no nullable)
class Servicio {
    lateinit var repositorio: Repositorio
    fun iniciar() { repositorio = Repositorio() }
}

// lazy: se calcula al primer uso (solo val)
val cache: Map<String, String> by lazy { cargarDatos() }
```

`lateinit` → `::repositorio.isInitialized` comprueba si ya se asignó. Si accedes antes → excepción, no null.

## Manejo de excepciones (estilo Kotlin)

```kotlin
val resultado = try {
    "123".toInt()
} catch (e: NumberFormatException) {
    0
}
```

Para conversiones seguras usa `toIntOrNull()` + `?:` en vez de try/catch → [[02-Variables y tipos de datos en Kotlin]].

## Platform types (el punto ciego)

Cuando interopera con **Java** (que no tiene null safety), Kotlin no puede saber si es null:

```kotlin
val dato: String! = javaApi.obtener()   // platform type
```

Tómalos como `String?` y comprueba siempre, o puedes recibir NPE sorpresa.

## Cuándo usar / Cuándo NO usar

- **Deja que el compilador te obligue**: prefiere tipos no-null.
- `?.` + `?:` cubren el 90% de los casos.
- Evita `!!` fuera de pruebas.
- `lateinit` solo cuando **seguro** que inicializas antes de usar.
- Con Java, trata los platform types como nullable.

## Ver también

- [[02-Variables y tipos de datos en Kotlin]]
- [[22-Manejo de errores en Python]] (try/except para comparar)
- [[Ruta Kotlin]]
- [[MOC-programacion]]
- [[HOME]]
