---
title: Introducción a Java
area: programacion
tipo: lenguaje
tags: [programacion, java, fundamentos]
created: 2026-09-20
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Java]]", "[[Pedir e imprimir datos en Java]]"]
---

## Primer Programa y Salida Básica (`System.out.println`)



Uso del método tradicional `System.out.println` con concatenación de texto utilizando el operador `+`.

Java

```
public class PrimerPrograma {
    public static void main(String[] args) {
        // Hola Mundo clásico
        // System.out.println("Hola mundo");

        int edad = 18;
        String nombre = "Ferney Contreras";

        // Imprimir combinando variables con texto usando concatenación (+)
        System.out.println("Nombre: " + nombre);
        System.out.println("Edad: " + edad + " años");
    }
}
```

## 2. Tipos de Datos y Formato sin Concatenar (`System.out.printf`)

Java posee **8 tipos de datos primitivos** más los tipos de referencia como `String`. En lugar de concatenar con `+`, se puede usar `System.out.printf` con especificadores de formato (`%s`, `%d`, `%f`, etc.).

Java

```
public class TiposDeDatos {
    public static void main(String[] args) {
        // --- TIPOS DE DATOS PRIMITIVOS ---

        // Enteros
        byte enteroByte = 127;          // 8 bits  (-128 a 127)
        short enteroShort = 32000;      // 16 bits (-32,768 a 32,767)
        int edad = 20;                  // 32 bits (-2^31 a 2^31-1)
        long enteroLong = 9000000000L;  // 64 bits (requiere el sufijo 'L')

        // Decimales / Coma Flotante
        float precioFloat = 19.99f;     // 32 bits (requiere el sufijo 'f')
        double precioDouble = 19.99;    // 64 bits (precisión por defecto)

        // Booleano
        boolean esActivo = true;        // true o false

        // Carácter
        char letra = 'A';               // 16 bits (carácter Unicode único)

        // --- TIPO REFERENCIA (OBJETO) ---
        String nombre = "Ferney";       // Cadena de texto (S siempre en mayúscula)

        // --- IMPRESIÓN CON FORMATO (SIN CONCATENAR) ---
        System.out.printf("Hola %s, tienes %d años.%n", nombre, edad);

        System.out.printf("Byte: %d | Short: %d | Int: %d | Long: %d%n", 
                enteroByte, enteroShort, edad, enteroLong);

        System.out.printf("Float: %.2f | Double: %.2f | Boolean: %b | Char: %c%n", 
                precioFloat, precioDouble, esActivo, letra);
    }
}
```

### Tabla de Referencia: Especificadores de `System.out.printf`

| **Especificador** | **Tipo de dato que representa**          | **Ejemplo de uso**       |
| ----------------- | ---------------------------------------- | ------------------------ |
| `%s`              | `String` (Texto)                         | `printf("%s", nombre)`   |
| `%d`              | Enteros (`byte`, `short`, `int`, `long`) | `printf("%d", edad)`     |
| `%f`              | Decimales (`float`, `double`)            | `printf("%.2f", precio)` |
| `%b`              | Booleano (`boolean`)                     | `printf("%b", esActivo)` |
| `%c`              | Carácter (`char`)                        | `printf("%c", letra)`    |
| `%n`              | Salto de línea                           | `printf("Texto%n")`      |





# Entrada de Datos en Java (`Scanner`)

Para leer datos que el usuario ingresa por teclado desde la consola, se utiliza la clase `Scanner` del paquete `java.util`.

## 1. Ejemplo Práctico

Java

```java
import java.util.Scanner; // 1. Importar la clase Scanner

public class EntradaDatos {
    public static void main(String[] args) {
        // 2. Crear el objeto Scanner conectado a la entrada estándar (consola)
        Scanner scanner = new Scanner(System.in);

        // Pedir un texto (String)
        System.out.print("Ingresa tu nombre: ");
        String nombre = scanner.nextLine(); // Lee toda la línea completa

        // Pedir un entero (int)
        System.out.print("Ingresa tu edad: ");
        int edad = scanner.nextInt();

        // Pedir un decimal (double)
        System.out.print("Ingresa tu estatura (ej. 1,75 o 1.75): ");
        double estatura = scanner.nextDouble();

        // Mostrar los resultados usando formato sin concatenar
        System.out.println("\n--- Datos Registrados ---");
        System.out.printf("Nombre: %s%n", nombre);
        System.out.printf("Edad: %d años%n", edad);
        System.out.printf("Estatura: %.2f m%n", estatura);

        // 3. Cerrar el scanner al finalizar para liberar recursos
        scanner.close();
    }
}
```

## 2. Métodos Principales de `Scanner`

|**Método**|**Tipo de dato que lee**|**Descripción**|
|---|---|---|
|`scanner.nextLine()`|`String`|Lee toda la línea hasta presionar _Enter_.|
|`scanner.next()`|`String`|Lee solo la primera palabra (hasta encontrar un espacio).|
|`scanner.nextInt()`|`int`|Lee un número entero.|
|`scanner.nextDouble()`|`double`|Lee un número decimal.|
|`scanner.nextBoolean()`|`boolean`|Lee `true` o `false`|

## Cuidado con el Bug del Salto de Línea ("Línea Fantasma")

Si usas `nextInt()` o `nextDouble()` y justo después llamas a `nextLine()`, `nextLine()` leerá el carácter de salto de línea (`\n`) que dejó pendiente el Enter anterior y parecerá que se saltó esa entrada.

### 🔴 Problema:

Java

```java
int edad = scanner.nextInt();
String ciudad = scanner.nextLine(); // ¡Se salta sin esperar entrada!
```

### 🟢 Soluciones:

1. **Consumir el salto de línea sobrante:**
    
    Java
    
    ```java
    int edad = scanner.nextInt();
    scanner.nextLine(); // Limpia el búfer consumiendo el '\n'
    String ciudad = scanner.nextLine(); // Ahora sí lee correctamente
    ```
    
2. **Leer todo con `nextLine()` y parsear:**
    
    Java
    
    ```java
    int edad = Integer.parseInt(scanner.nextLine());
    String ciudad = scanner.nextLine();
    ```


# tipos de datos 

| **Tipo**  | **Tamaño**        | **Rango / Ejemplo**                     | **Descripción**                                     |
| --------- | ----------------- | --------------------------------------- | --------------------------------------------------- |
| `byte`    | 1 byte (8 bits)   | `-128` a `127`                          | Números enteros muy pequeños.                       |
| `short`   | 2 bytes (16 bits) | `-32,768` a `32,767`                    | Números enteros pequeños.                           |
| `int`     | 4 bytes (32 bits) | `-2,147,483,648` a `2,147,483,647`      | El entero usado por defecto.                        |
| `long`    | 8 bytes (64 bits) | Grandes cantidades (ej: `10000000000L`) | Enteros de gran tamaño. Requiere sufijo `L`.        |
| `float`   | 4 bytes (32 bits) | 6-7 decimales (ej: `3.14f`)             | Decimales de precisión simple. Requiere sufijo `f`. |
| `double`  | 8 bytes (64 bits) | 15-16 decimales (ej: `3.14159`)         | Decimales de doble precisión (usado por defecto).   |
| `boolean` | 1 bit             | `true` / `false`                        | Valores lógicos (verdadero o falso).                |
| `char`    | 2 bytes (16 bits) | `'A'`, `'9'`, `'\n'`                    | Un solo carácter (usa comillas simples).            |
|           |                   |                                         |                                                     |

**

### Variables y Constantes en Java

Una **variable** es un contenedor en memoria cuyo valor puede cambiar durante la ejecución del programa. Una **constante** es un contenedor cuyo valor se asigna una sola vez y no puede ser modificado posteriormente.

#### 1. Variables

Para declarar una variable en Java se debe especificar el **tipo de dato** seguido del **nombre** (identificador) y, opcionalmente, su **valor inicial**.

Java

```
public class VariablesEjemplo {
    public static void main(String[] args) {
        // Declaración sin inicializar
        int puntaje;
        
        // Inicialización
        puntaje = 100;

        // Declaración e inicialización en una sola línea
        int vidas = 3;
        String jugador = "Ferney";

        // Reasignación de valores (cambio de estado)
        puntaje = 150; 
        vidas = vidas - 1; // Ahora vidas es 2

        System.out.printf("Jugador: %s | Vidas: %d | Puntaje: %d%n", jugador, vidas, puntaje);
    }
}
```

#### 2. Constantes (`final`)

Para definir una constante en Java se utiliza la palabra clave `final`. Si intentas modificar el valor de una variable declarada como `final`, el compilador generará un error de compilación.

- **Convención de nombres:** Por buenas prácticas, los nombres de las constantes se escriben completamente en **MAYÚSCULAS_CON_GUION_BAJO** (Snake Case en mayúsculas).
    

Java

```
public class ConstantesEjemplo {
    public static void main(String[] args) {
        // Declaración de constantes con 'final'
        final double PI = 3.141592653589793;
        final int VALOR_MAXIMO_PUNTOS = 9999;
        final String PAIS_ORIGEN = "Colombia";

        // Intentar cambiar un valor constante genera error de compilación:
        // PI = 3.14; // 🔴 Error: cannot assign a value to final variable PI

        System.out.printf("Valor de PI: %.4f%n", PI);
        System.out.printf("Límite de puntos: %d%n", VALOR_MAXIMO_PUNTOS);
        System.out.printf("País: %s%n", PAIS_ORIGEN);
    }
}
```






## Ver también

- [[Ruta Java]]
- [[Pedir e imprimir datos en Java]]
- [[MOC-programacion]]
- [[Punteros y referencias]]