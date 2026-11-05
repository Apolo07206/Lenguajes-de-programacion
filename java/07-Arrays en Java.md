---
aliases: ["Arrays en Java"]
title: Arrays en Java
area: programacion
tipo: lenguaje
tags: [programacion, java, arrays, colecciones]
created: 2026-09-20
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Java]]", "[[09-Vectores en Java|Vectores en Java]]"]
---

> [!info] Definición Un **array** guarda varios valores **del mismo tipo** en una sola variable, con **tamaño fijo**.

## Declarar y crear


```java
int[] numeros; // declaración
numeros = new int[5]; // creación: array de 5 enteros

// o en una línea:
int[] numeros = new int[5];
```

> [!note] Valores por defecto Al crear un array sin valores, Java lo llena automáticamente:
> 
> - `0` → números
> - `false` → boolean
> - `null` → objetos/String

## Inicializar con valores directos


```java
int[] edades = {18, 25, 30, 40, 55};
String[] nombres = {"Ana", "Luis", "Carlos"};
```

## Acceder y modificar

Los índices empiezan en **0**.


```java
System.out.println(edades[0]); // 18
edades[2] = 99; // cambia el valor en la posición 2
```

> [!warning] Cuidado Acceder a un índice inexistente lanza `ArrayIndexOutOfBoundsException`.

## Tamaño del array


```java
System.out.println(edades.length); // 5 → atributo, sin paréntesis
```

## Recorrer un array

**For clásico** (cuando necesitas el índice):


```java
for (int i = 0; i < edades.length; i++) {
    System.out.println(edades[i]);
}
```

**For-each** (cuando solo necesitas el valor):


```java
for (int edad : edades) {
    System.out.println(edad);
}
```

## Arrays multidimensionales (matrices)


```java
int[][] matriz = new int[3][3]; // 3 filas, 3 columnas
matriz[0][1] = 7;

int[][] tablero = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};
```

## Clase `Arrays` (utilidades)


```java
import java.util.Arrays;

Arrays.sort(edades); // ordena
System.out.println(Arrays.toString(edades)); // [18, 25, 30, 40, 55]
int[] copia = Arrays.copyOf(edades, 5);
```

Cuando **ya conoces los elementos desde el principio** (es decir, quieres inicializar el `Vector` con datos fijos sin tener que hacer varios `.add()` línea por línea), existen varias formas de escribirlo directamente con el `new`.


## Usando `List.of(...)` _(Java 9 en adelante — Recomendado)_

Es la forma más limpia y moderna. Le pasas la lista de elementos fijos al constructor del `Vector`:


```
import java.util.List;
import java.util.Vector;

// Crear e inicializar el Vector en una sola línea
Vector<String> frutas = new Vector<>(List.of("Manzana", "Banana", "Naranja"));
```



### sacar el mayor o el menor elemento

```
int [] lista={1,2,3,4,5,6,7};

  

int mayor=Math.max(lista[0] , lista.length);

  

int menor=Math.min(lista[0] , lista.length);

```


### Ordenar de menor a mayor

```
import java.util.Arrays;

int[] lista = {5, 2, 8, 1, 4};

Arrays.sort(lista);

System.out.println(Arrays.toString(lista));
```

Resultado:

```
[1, 2, 4, 5, 8]
```

### Ordenar de mayor a menor

```
import java.util.Arrays;

int[] lista = {5, 2, 8, 1, 4};

Arrays.sort(lista);

for (int i = lista.length - 1; i >= 0; i--) {
    System.out.print(lista[i] + " ");
}
```

Resultado:

```
8 5 4 2 1
```




## Importación y Declaración (ArrayList)

Para usarlo, primero debes importarlo:


```
import java.util.ArrayList;
```

### Sintaxis básica:


```
// Declaración y creación de un ArrayList de Strings
ArrayList<String> nombres = new ArrayList<>();

// También se puede usar la interfaz List (buena práctica)
List<String> listaNombres = new ArrayList<>();
```

## 2. Métodos Principales (CRUD y Utilidades)

| **Método**                | **Descripción**                                   | **Ejemplo**                                 |
| ------------------------- | ------------------------------------------------- | ------------------------------------------- |
| `add(elemento)`           | Agrega un elemento al final de la lista.          | `nombres.add("Carlos");`                    |
| `add(índice, elemento)`   | Inserta un elemento en una posición específica.   | `nombres.add(0, "Ana");`                    |
| `get(índice)`             | Devuelve el elemento en la posición indicada.     | `String n = nombres.get(1);`                |
| `set(índice, elemento)`   | Modifica/reemplaza el elemento en esa posición.   | `nombres.set(1, "Pedro");`                  |
| `remove(índice o objeto)` | Elimina el elemento por su índice o valor.        | `nombres.remove(0);`                        |
| `size()`                  | Devuelve la cantidad de elementos (tamaño).       | `int total = nombres.size();`               |
| `contains(objeto)`        | Retorna `true` si el elemento existe en la lista. | `boolean existe = nombres.contains("Ana");` |
| `clear()`                 | Vacía toda la lista por completo.                 | `nombres.clear();`                          |
| `isEmpty()`               | Devuelve `true` si la lista está vacía.           | `boolean vacia = nombres.isEmpty();`        |
|                           |                                                   |                                             |

## ¿Cómo combinar `List.of()` con un `ArrayList`? (¡Lo que te interesa!)

Como `List.of()` te da una lista rápida pero inmutable, el truco pro para cuando **sí necesitas modificar los elementos después** es pasar esa lista directamente dentro del constructor del `ArrayList`.

Esto crea un `ArrayList` mutable ya relleno con esos datos:


```
import java.util.ArrayList;
import java.util.List;

public class EjemploListOf {
    public static void main(String[] args) {
        
        // 1. Creamos un ArrayList mutable usando List.of() para inicializarlo rápido
        ArrayList<String> dias = new ArrayList<>(List.of("Lunes", "Martes", "Miércoles"));

        // 2. ¡Ahora sí podemos modificarlo sin problema porque es un ArrayList!
        dias.add("Jueves"); // Agrega un elemento al final
        dias.set(0, "Lunes (Modificado)"); // Modifica el primer elemento
        dias.remove("Martes"); // Elimina un elemento

        System.out.println(dias); 
        // Salida: [Lunes (Modificado), Miércoles, Jueves]
    }
}
```


## Ver también

- [[Ruta Java]]
- [[09-Vectores en Java|Vectores en Java]]
- [[MOC-programacion]]
