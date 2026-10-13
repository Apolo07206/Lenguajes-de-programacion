---
title: Pedir e imprimir datos en Java
area: programacion
tipo: lenguaje
tags: [programacion, java, fundamentos, scanner]
created: 2026-09-20
updated: 2026-09-21
related: ["[[MOC-programacion]]", "[[Ruta Java]]", "[[Introducción a Java]]"]
---

# Fundamentos de Java: Lectura de Datos con Scanner

## 1. Estructura Básica para Leer Entradas

Para leer datos desde la consola necesitamos importar la clase `Scanner` e instanciar un objeto conectado a la entrada del sistema (`System.in`).

```
import java.util.Scanner;

public class Inicio {
    public static void main(String[] args) {
        // Instancia del lector
        Scanner scanner = new Scanner(System.in);

        // Lógica del programa aquí...

        // Siempre cerrar el scanner al finalizar
        scanner.close();
    }
}
```

---

## 2. Tipos de Datos y Métodos de Lectura

| Tipo de Dato | Variable | Método recomendado | Ejemplo |
| :--- | :--- | :--- | :--- |
| **Texto / Frase** | `String` | `scanner.nextLine()` | `String nombre = scanner.nextLine();` |
| **Número Entero** | `int` | `Integer.parseInt(scanner.nextLine())` | `int edad = Integer.parseInt(scanner.nextLine());` |

---

## 3. El Problema del "Salto de Línea Fantasma"

> [!WARNING] ¿Por qué usar `Integer.parseInt(scanner.nextLine())` en vez de `nextInt()`?
> Cuando usas `scanner.nextInt()`, el programa lee únicamente los dígitos (ej. `25`), pero deja la tecla **Enter** guardada en el búfer de memoria.
>
> Si inmediatamente después pides un texto con `scanner.nextLine()`, este consumirá ese **Enter** pendiente y **se saltará la entrada del usuario**.

### Comparativa:

* ❌ **Forma propensa a errores (al mezclar tipos):**
  ```
  int numero = scanner.nextInt(); // Deja el Enter flotando
  String texto = scanner.nextLine(); // Se salta sin esperar entrada
  ```

*  **Forma segura y limpia:**
  ```
  int numero = Integer.parseInt(scanner.nextLine()); // Lee la línea completa y la convierte
  String texto = scanner.nextLine(); // Funciona sin problemas
  ```

---

## 4. Ejemplo Completo: Suma de dos números

```
import java.util.Scanner;

public class Inicio {
    public static void main(String[] args) {
        
        Scanner scanner = new Scanner(System.in);

        int num1;
        int num2;
        int suma;

        System.out.println("Ingrese un número:");
        num1 = Integer.parseInt(scanner.nextLine());

        System.out.println("Ingrese otro número:");
        num2 = Integer.parseInt(scanner.nextLine());

        suma = num1 + num2;

        // Concatenación de texto y variable usando '+'
        System.out.println("La suma de los dos números es: " + suma);

        scanner.close();
    }
}
```

---

##  Reglas Rápidas de Sintaxis
- **Sensibilidad a mayúsculas:** `scanner` (instancia) ≠ `Scanner` (clase).
- **Concatenación:** Para unir texto y variables en `System.out.println()`, se usa el operador `+` (las comas `,` generan error de compilación).

## Ver también

- [[Ruta Java]]
- [[Introducción a Java]]
- [[MOC-programacion]]