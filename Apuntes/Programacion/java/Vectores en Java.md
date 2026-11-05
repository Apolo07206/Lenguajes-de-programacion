---
title: Vectores en Java
area: programacion
tipo: lenguaje
tags: [programacion, java, colecciones, vector]
created: 2026-09-20
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Java]]", "[[Arrays en Java]]"]
---

# CRUD en Java usando `Vector`

En Java, las 4 operaciones fundamentales del **CRUD** (_Create, Read, Update, Delete_) se traducen en los siguientes métodos cuando trabajamos con un `Vector`:

## 1. Create (Crear / Agregar)

Usa el método `.add()` para insertar nuevos elementos.

```
Vector<String> tareas = new Vector<>();

// Agregar elementos al final
tareas.add("Estudiar Java");
tareas.add("Hacer ejercicio");

// Insertar en una posición específica: .add(índice, elemento)
tareas.add(1, "Llamar al médico"); // Lo inserta en el índice 1 y desplaza los demás
```

## 2. Read (Leer / Consultar)

Usa `.get()` pasando el índice (comienza en 0) o recorre la lista con un bucle.

```
// Obtener por índice
String primeraTarea = tareas.get(0); // Devuelve "Estudiar Java"

// Verificar existencia
boolean existe = tareas.contains("Hacer ejercicio"); // Devuelve true/false

// Recorrer todos los elementos
for (int i = 0; i < tareas.size(); i++) {
    System.out.println("Índice " + i + ": " + tareas.get(i));
}
```

## 3. Update (Actualizar / Modificar)

Usa `.set(índice, nuevoValor)` para reemplazar un elemento existente.

```
// Reemplazar el elemento del índice 1
tareas.set(1, "Ir al médico a las 4 PM");
```

## 4. Delete (Eliminar / Borrar)

Usa `.remove()` por **índice** o por **objeto/valor**.

```
// Opción A: Eliminar por índice
tareas.remove(0); // Borra el primer elemento

// Opción B: Eliminar por valor exacto
tareas.remove("Hacer ejercicio");

// Opción C: Vaciar todo el Vector
tareas.clear();
```

## 📊 Resumen Rápido

|   |   |   |
|---|---|---|
|**Operación**|**Método**|**Ejemplo**|
|**C**reate|`.add()`|`vector.add("Dato");`|
|**R**ead|`.get()`|`String d = vector.get(0);`|
|**U**pdate|`.set()`|`vector.set(0, "Nuevo Dato");`|
|**D**elete|`.remove()`|`vector.remove(0);`|


// Lista inmutable

```
List<String> frutas = List.of("Manzana", "Banana", "Naranja");

```

```
// Lista modificable (ArrayList)
List<String> frutasModificable = new ArrayList<>(List.of("Manzana", "Banana", "Naranja"));

List<Integer> numeros = new ArrayList<>(List.of(10, 20, 30));
```



Creas la lista vacía y le vas agregando valores con `add()` cuando los tengas.

java

```java
import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<String> nombres = new ArrayList<>(); // lista vacía

        nombres.add("Ana");    // ahora tiene 1
        nombres.add("Luis");   // ahora tiene 2

        System.out.println(nombres); // [Ana, Luis]
    }
}
```

Lo importante es lo que va entre `< >`: el tipo de dato que guarda la lista.

- `List<String>` → textos
- `List<Integer>` → números enteros
- `List<Double>` → números con decimales

Si los valores los va a escribir el usuario por teclado:

java

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        List<String> nombres = new ArrayList<>();

        for (int i = 0; i < 3; i++) {
            System.out.print("Escribe un nombre: ");
            nombres.add(sc.nextLine());
        }

        System.out.println(nombres);
    }
}
```

Otras cosas útiles:

java

```java
nombres.remove("Ana");       // quita por valor
nombres.remove(0);           // quita por posición
nombres.size();              // cuántos hay
nombres.get(0);              // saca el de la posición 0
nombres.isEmpty();           // true si está vacía
```

Y si lo que necesitas es un **vector (arreglo) vacío** sin saber los valores, tienes que decir de qué tamaño será:

java

```java
int[] vector = new int[5]; // 5 espacios, todos en 0
vector[0] = 10;            // luego llenas cada posición
vector[1] = 20;
```

##  Ejemplo

```java

package ejercicios_java;
import java.util.ArrayList;
import java.util.Comparator;
import java.util.List;


public class hola {
public static void main(String[] args) {

  
List<Integer> numeros = new ArrayList<>(List.of(9,1,2,3,4,5,6));
  

// System.out.println(numeros.get(3));


// numeros.set(0,5);

  

//numeros.remove(0);

  

numeros.add(7);

numeros.add(8);

  

numeros.sort(Comparator.reverseOrder());

  

  
  

for (int i : numeros){

System.out.println(i);

}

  
  

System.out.println(" ");

  
  

numeros.sort(Comparator.naturalOrder());

  
  

for (int i : numeros){

System.out.println(i);

}

  
  

/*

get = leer

set actualizar

remove=borrar

add = agregar

*/

}

}

```





## Ver también

- [[Ruta Java]]
- [[Arrays en Java]]
- [[MOC-programacion]]