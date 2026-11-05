---
title: Estructuras de control en Java
area: programacion
tipo: lenguaje
tags: [programacion, java, condicionales, switch]
created: 2026-09-20
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Java]]", "[[Bucles en Java]]"]
---

## 1. Toma de decisiones (Condicionales)

### 🔹 `if / else`

Evalúa si una condición es verdadera (`true`) o falsa (`false`). Es ideal para comparaciones complejas o rangos.

```
int edad = 18;

if (edad >= 18) {
    System.out.println("Eres mayor de edad.");
} else if (edad >= 13) {
    System.out.println("Eres un adolescente.");
} else {
    System.out.println("Eres un niño.");
}
```

### 🔹 `switch`

Evalúa una **sola variable** contra varios valores fijos (`case`). Es más limpio que encadenar muchos `if / else` cuando comparas un solo dato (enteros, caracteres, cadenas, enums).

#### Sintaxis tradicional:

```
String dia = "LUNES";

switch (dia) {
    case "LUNES":
        System.out.println("Inicio de semana");
        break; // Evita que siga ejecutando las siguientes opciones
    case "VIERNES":
        System.out.println("Casi fin de semana");
        break;
    default: // Se ejecuta si ningún caso coincide
        System.out.println("Día regular");
        break;
}
```

> [!TIP] Sintaxis moderna (Java 14+) Puedes usar flechas (`->`) para evitar la necesidad de escribir la palabra clave `break`:
> 
> ```
> switch (dia) {
>     case "LUNES" -> System.out.println("Inicio de semana");
>     case "VIERNES" -> System.out.println("Casi fin de semana");
>     default -> System.out.println("Día regular");
> }
> ```

## Ver también

- [[Ruta Java]]
- [[Bucles en Java]]
- [[Operadores en Java]]
- [[MOC-programacion]]