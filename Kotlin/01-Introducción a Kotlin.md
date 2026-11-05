---
title: "Introducción a Kotlin"
area: programacion
tipo: lenguaje
tags: [programacion, kotlin, fundamentos]
created: 2026-10-22
updated: 2026-10-22
related: ["[[MOC-programacion]]", "[[Ruta Kotlin]]", "[[01-Introducción a Java]]", "[[02-Variables y tipos de datos en Kotlin]]"]
---

# 01 — Introducción a Kotlin

## Qué es / Para qué sirve

**Kotlin** es un lenguaje moderno que corre sobre la **JVM** (misma plataforma que Java). Es el lenguaje **oficial de Android** y convive perfectamente con Java: puedes usar librerías de Java sin problema.

| | Java | Kotlin |
| --- | --- | --- |
| Verbose | Mucho | **Conciso** (menos código) |
| Null safety | `NullPointerException` fácil | **Seguro por diseño** → [[08-Null safety en Kotlin]] |
| Android | Legado | **Oficial** |
| Corre en JVM | Sí | **Sí** (interopera con Java) |

## Hola mundo

Kotlin moderno usa `main` con **parámetros**:

```kotlin
fun main() {
    println("Hola mundo")
}
```

Con argumentos (antes era `Array<String>`):

```kotlin
fun main(args: Array<String>) {
    println("Primer arg: ${args.getOrElse(0) { "ninguno" }}")
}
```

## Variables: `val` y `var`

```kotlin
val nombre = "Ferxxo"    // INMUTABLE (constante) → usa esto por defecto
var edad = 25            // MUTABLE (se puede reasignar)

edad = 26                // OK
// nombre = "Otro"       // ERROR de compilación
```

> **Regla**: empieza siempre con `val`. Cambia a `var` solo si de verdad necesitas reasignar.

## Tipado estático con inferencia

Kotlin **sabe** el tipo sin que lo escribas, pero es estático:

```kotlin
val saludo: String = "Hola"   // explícito
val saludo = "Hola"           // inferido (String) ← preferible
```

## Estructura de un proyecto Kotlin

- Archivo `.kt` (en IntelliJ: clic derecho → Run).
- No necesitas `class` envolviendo todo (a diferencia de Java) → `main` suelto funciona.

```kotlin
// archivo Main.kt
fun main() {
    val platform = "JVM"
    println("Kotlin corre en $platform")
}
```

### Interpolación de strings (la joya vs Java)

```kotlin
val nombre = "Ferxxo"
println("Hola $nombre")              // sin llaves
println("Tiene ${nombre.length} letras")  // con expresión
println("Ruta: ${'$'}01-Kotlin")      // escapar $
```

## Kotlin vs Java, cara a cara

```java
// Java
public class Usuario {
    private String nombre;
    public Usuario(String nombre) { this.nombre = nombre; }
    public String getNombre() { return nombre; }
}
```

```kotlin
// Kotlin (¡una línea!)
data class Usuario(val nombre: String)
```

(ver detalle en [[07-Clases y data classes en Kotlin]])

## Cuándo usar / Cuándo NO usar

- **Usa** Kotlin para Android, backend JVM (Ktor/Spring), scripting y donde quieras código conciso.
- **Úsalo con Java** cuando necesites una librería Java antigua (interoperan 100%).
- **No** lo fuerces si tu equipo/equipo es 100% Java legacy y no aporta nada.
- Si vienes de Java, empieza por aquí y sigue con [[02-Variables y tipos de datos en Kotlin]].

## Ver también

- [[02-Variables y tipos de datos en Kotlin]]
- [[01-Introducción a Java]] (su hermano mayor)
- [[Ruta Kotlin]]
- [[MOC-programacion]]
- [[HOME]]
