---
title: "Clases y data classes en Kotlin"
area: programacion
tipo: lenguaje
tags: [programacion, kotlin, poo]
created: 2026-10-22
updated: 2026-10-22
related: ["[[MOC-programacion]]", "[[Ruta Kotlin]]", "[[05-Funciones en Kotlin]]", "[[15-POO en Java]]", "[[26-Clases y objetos en Python]]"]
---

# 07 — Clases y data classes en Kotlin

## Clase básica

```kotlin
class Usuario(val nombre: String, var edad: Int)
```

Esa **una línea** ya tiene: constructor, propiedades, getters/setters y `toString()` decente.

```kotlin
val u = Usuario("Ana", 25)
println(u.nombre)     // getter implícito
u.edad = 26           // setter implícito (var)
println(u)            // Usuario(nombre=Ana, edad=26)
```

### `val` vs `var` en propiedades

- `val` → solo lectura (getter, sin setter).
- `var` → lectura y escritura.

## Cuerpo de la clase

```kotlin
class Calculadora(val prefijo: String) {

    // propiedad inicializada
    val historial = mutableListOf<String>()

    // bloque init (se ejecuta en la construcción)
    init {
        println("Calculadora $prefijo lista")
    }

    // miembro
    fun sumar(a: Int, b: Int): Int {
        val resultado = a + b
        historial.add("$prefijo $a + $b = $resultado")
        return resultado
    }
}
```

## Propiedades con getter/setter personalizados

```kotlin
class Circulo(val radio: Double) {
    val area: Double
        get() = Math.PI * radio * radio

    var doble: Double = radio * 2
        set(valor) {
            field = valor / 2    // 'field' = backing field (evita recursión)
        }
}
```

> Usa `field` dentro de un setter/getter: es la referencia al valor real (sin ella, llamarías al getter en bucle infinito).

## `data class` — la joya (como record de Java)

```kotlin
data class Usuario(val nombre: String, val edad: Int)
```

Gratis obtienes: `equals()`, `hashCode()`, `toString()`, `copy()` y `componentN()` (desestructuración).

```kotlin
val a = Usuario("Ana", 25)
val b = Usuario("Ana", 25)

a == b              // true (compara por contenido)
println(a)          // Usuario(nombre=Ana, edad=25)

val c = a.copy(edad = 30)     // copia cambiando un campo
val (nombre, edad) = a        // desestructuración

// en listas
val iguales = listOf(a, b).distinct()   // deduplica bien por equals
```

> `data class` necesita **mínimo 1 parámetro** en el constructor y debe ser `val/var` (no puede ser sin propiedad).

## Herencia

```kotlin
open class Animal(val nombre: String) {   // 'open' obligatorio para heredar
    open fun sonido() = "..."
}

class Perro(nombre: String) : Animal(nombre) {
    override fun sonido() = "Guau"        // 'override' obligatorio
}

class Gato(nombre: String) : Animal(nombre) {
    override fun sonido() = "Miau"
}
```

> A diferencia de Java, en Kotlin las clases son **final** por defecto. Debes marcar `open` para permitir herencia (diseño deliberado: "herencia opt-in").

## Interfaces

```kotlin
interface Nadador {
    fun nadar()                     // abstracta
    fun saltar() { println("splash") }  // con cuerpo (default)
}

class Pato : Nadador {
    override fun nadar() = println("pataleo")
}
```

## Clase sellada (`sealed`) — para when exhaustivo

```kotlin
sealed class Resultado {
    data class Exito(val datos: String) : Resultado()
    data class Error(val codigo: Int) : Resultado()
    object Vacio : Resultado()
}

fun manejar(r: Resultado) = when (r) {
    is Resultado.Exito -> println("Datos: ${r.datos}")
    is Resultado.Error -> println("Error ${r.codigo}")
    is Resultado.Vacio -> println("Vacío")
}   // sin 'else': Kotlin sabe que cubre todos los casos
```

## `object` (singleton)

```kotlin
object Config {
    const val VERSION = "1.0"
    var intentos = 0
}

println(Config.VERSION)
```

Un único instance global, sin `static`.

## Companion object (miembros "estáticos")

```kotlin
class Punto(val x: Double, val y: Double) {
    companion object {
        val ORIGEN = Punto(0.0, 0.0)
        fun crear(x: Double, y: Double) = Punto(x, y)
    }
}

val o = Punto.ORIGEN
val p = Punto.crear(3.0, 4.0)
```

## Cuándo usar / Cuándo NO usar

- Modelo de datos → **`data class`** (no escribas `equals`/`toString` a mano).
- Para heredar, marca la clase base `open`.
- Conjunto cerrado de variantes → `sealed class` + `when` exhaustivo.
- Singleton → `object`.
- `companion object` para lo que en Java sería `static`.
- Enlaza con la POO de [[15-POO en Java]] y [[26-Clases y objetos en Python]] para comparar.

## Ver también

- [[05-Funciones en Kotlin]]
- [[08-Null safety en Kotlin]]
- [[15-POO en Java]]
- [[Ruta Kotlin]]
- [[MOC-programacion]]
- [[HOME]]
