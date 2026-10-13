---
title: Arrays en Java
area: programacion
tipo: lenguaje
tags: [programacion, java, arrays, colecciones]
created: 2026-09-20
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Java]]", "[[Vectores en Java]]"]
---

> [!info] Definición Un **array** guarda varios valores **del mismo tipo** en una sola variable, con **tamaño fijo**.

## Declarar y crear

java

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

java

```java
int[] edades = {18, 25, 30, 40, 55};
String[] nombres = {"Ana", "Luis", "Carlos"};
```

## Acceder y modificar

Los índices empiezan en **0**.

java

```java
System.out.println(edades[0]); // 18
edades[2] = 99; // cambia el valor en la posición 2
```

> [!warning] Cuidado Acceder a un índice inexistente lanza `ArrayIndexOutOfBoundsException`.

## Tamaño del array

java

```java
System.out.println(edades.length); // 5 → atributo, sin paréntesis
```

## Recorrer un array

**For clásico** (cuando necesitas el índice):

java

```java
for (int i = 0; i < edades.length; i++) {
    System.out.println(edades[i]);
}
```

**For-each** (cuando solo necesitas el valor):

java

```java
for (int edad : edades) {
    System.out.println(edad);
}
```

## Arrays multidimensionales (matrices)

java

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

java

```java
import java.util.Arrays;

Arrays.sort(edades); // ordena
System.out.println(Arrays.toString(edades)); // [18, 25, 30, 40, 55]
int[] copia = Arrays.copyOf(edades, 5);
```

Cuando **ya conoces los elementos desde el principio** (es decir, quieres inicializar el `Vector` con datos fijos sin tener que hacer varios `.add()` línea por línea), existen varias formas de escribirlo directamente con el `new`.


## Usando `List.of(...)` _(Java 9 en adelante — Recomendado)_

Es la forma más limpia y moderna. Le pasas la lista de elementos fijos al constructor del `Vector`:

Java

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

## Ver también

- [[Ruta Java]]
- [[Vectores en Java]]
- [[MOC-programacion]]