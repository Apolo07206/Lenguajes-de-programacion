---
title: Algoritmos de Ordenamientos en Java
area: programacion
tipo: lenguaje
tags: [programacion, java, algoritmos, ordenamiento, sorting]
created: 2026-09-20
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Java]]", "[[Arrays en Java]]", "[[POO en Java]]"]
---

# Ordenamiento en Java

> [!summary] Idea central Ordenar = poner los elementos en un orden (menor a mayor, mayor a menor, A-Z...). Todos los algoritmos hacen lo mismo: **comparar** elementos y **moverlos** de lugar. Cambia la estrategia, y de ahí la velocidad.

Hay dos grupos:

- **Iterativos** (solo ciclos y condicionales): burbuja, selección, inserción, shell.
- **Recursivos** (un método que se llama a sí mismo, "divide y vencerás"): merge y quick.

java

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.Comparator;
```

---

# PARTE 1: Algoritmos hechos a mano

Todos están escritos de **menor a mayor**. Para pasarlos a **mayor a menor**, solo se cambia el signo de la comparación (se indica en cada uno).

## Método auxiliar: intercambiar

Para cambiar dos valores necesitas una variable auxiliar (`temp`). Si haces `a = b` directo, pierdes el valor original de `a`.

java

```java
static void intercambiar(int[] arr, int i, int j) {
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}
```

---

## 1. Burbuja (bubble sort)

**Idea:** compara **vecinos** y los intercambia si están al revés. En cada pasada, el mayor "flota" hasta el final.

`[5, 2, 9, 1]` → pasada 1: `[2, 5, 1, 9]` (el 9 ya quedó en su sitio).

java

```java
static void bubbleSort(int[] arr) {
    for (int i = 0; i < arr.length - 1; i++) {
        boolean huboCambios = false;

        for (int j = 0; j < arr.length - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {          // para mayor a menor: <
                intercambiar(arr, j, j + 1);
                huboCambios = true;
            }
        }

        if (!huboCambios) break;   // si no hubo cambios, ya está ordenado
    }
}
```

- El ciclo de afuera cuenta las pasadas; el de adentro hace cada pasada.
- `length - 1 - i`: los últimos `i` ya están ordenados, no hace falta compararlos.
- **Cuándo se usa:** casi solo para aprender. Es de los más lentos.

---

## 2. Selección (selection sort)

**Idea:** busca el **menor** de todos y lo pone al inicio; luego el menor de los que quedan y lo pone segundo; y así.

java

```java
static void selectionSort(int[] arr) {
    for (int i = 0; i < arr.length - 1; i++) {
        int posMenor = i;

        for (int j = i + 1; j < arr.length; j++) {
            if (arr[j] < arr[posMenor]) {       // para mayor a menor: >
                posMenor = j;
            }
        }

        if (posMenor != i) {
            intercambiar(arr, i, posMenor);
        }
    }
}
```

- Hace pocos intercambios (máximo `n - 1`).
- **Cuándo se usa:** arreglos pequeños, o cuando mover datos es costoso.

---

## 3. Inserción (insertion sort)

**Idea:** como **ordenar cartas en la mano**: tomas la siguiente y la insertas en su lugar entre las ya ordenadas.

java

```java
static void insertionSort(int[] arr) {
    for (int i = 1; i < arr.length; i++) {
        int clave = arr[i];
        int j = i - 1;

        while (j >= 0 && arr[j] > clave) {      // para mayor a menor: <
            arr[j + 1] = arr[j];
            j--;
        }

        arr[j + 1] = clave;
    }
}
```

> [!warning] Ojo con el orden de la condición Siempre `j >= 0` **primero** y `arr[j] > clave` después. Si lo escribes al revés, cuando `j` llega a `-1` Java intenta leer `arr[-1]` y lanza `ArrayIndexOutOfBoundsException`.

- Muy bueno si los datos **ya vienen casi ordenados**.

---

## 4. Shellsort

**Idea:** es una mejora de la inserción. En vez de mover cada elemento de a una posición, compara elementos **separados por un salto (gap)** y lo va reduciendo hasta llegar a 1. Así los datos quedan casi ordenados y el último paso (inserción) es rápido.

java

```java
static void shellSort(int[] arr) {
    for (int salto = arr.length / 2; salto > 0; salto /= 2) {

        for (int i = salto; i < arr.length; i++) {
            int clave = arr[i];
            int j = i;

            while (j >= salto && arr[j - salto] > clave) {   // mayor a menor: <
                arr[j] = arr[j - salto];
                j -= salto;
            }

            arr[j] = clave;
        }
    }
}
```

---

## 5. Mezcla (merge sort) — recursivo

**Idea:** divide el arreglo por la mitad una y otra vez hasta tener pedazos de 1 elemento, y luego los **mezcla** de a dos dejándolos ordenados.

`[38, 27, 43, 3]` → `[38, 27]` y `[43, 3]` → `[38]` `[27]` `[43]` `[3]` → `[27, 38]` y `[3, 43]` → `[3, 27, 38, 43]`

java

```java
static void mergeSort(int[] arr) {
    if (arr.length < 2) return;   // caso base

    int mitad = arr.length / 2;
    int[] izq = Arrays.copyOfRange(arr, 0, mitad);
    int[] der = Arrays.copyOfRange(arr, mitad, arr.length);

    mergeSort(izq);
    mergeSort(der);

    int i = 0, j = 0, k = 0;
    while (i < izq.length && j < der.length) {
        if (izq[i] <= der[j]) {                 // mayor a menor: >=
            arr[k++] = izq[i++];
        } else {
            arr[k++] = der[j++];
        }
    }
    while (i < izq.length) arr[k++] = izq[i++];
    while (j < der.length) arr[k++] = der[j++];
}
```

- **Cuándo se usa:** datos grandes. Siempre rinde bien. Es **estable** (conserva el orden de los iguales).

---

## 6. Rápido (quick sort) — recursivo

**Idea:** elige un **pivote**, deja los menores a su izquierda y los mayores a su derecha, y repite en cada lado.

java

```java
static void quickSort(int[] arr, int bajo, int alto) {
    if (bajo < alto) {
        int posPivote = particion(arr, bajo, alto);
        quickSort(arr, bajo, posPivote - 1);
        quickSort(arr, posPivote + 1, alto);
    }
}

static int particion(int[] arr, int bajo, int alto) {
    int pivote = arr[alto];
    int i = bajo - 1;

    for (int j = bajo; j < alto; j++) {
        if (arr[j] < pivote) {                  // mayor a menor: >
            i++;
            intercambiar(arr, i, j);
        }
    }

    intercambiar(arr, i + 1, alto);
    return i + 1;
}

// uso:
// quickSort(numeros, 0, numeros.length - 1);
```

- **Cuándo se usa:** es de los más rápidos en la práctica para datos generales.

---

## Comparación

|Algoritmo|Tipo|Velocidad (promedio)|Para qué sirve|
|---|---|---|---|
|Burbuja|Iterativo|O(n²) lento|Aprender|
|Selección|Iterativo|O(n²) lento|Pocos datos|
|Inserción|Iterativo|O(n²) lento|Pocos datos o casi ordenados|
|Shell|Iterativo|Mejor que O(n²)|Datos medianos|
|Merge|Recursivo|O(n log n) rápido|Datos grandes, estable|
|Quick|Recursivo|O(n log n) rápido|Uso general|

**Tiempos reales (de la guía de la universidad)**, ordenando 2.097.152 números:

|Algoritmo|Tiempo|
|---|---|
|Burbuja|~11.978 s|
|Selección|~10.711 s|
|Inserción|~7.371 s|
|Merge|~1,3 s|
|Shell|~0,85 s|
|Quick|~0,6 s|

Con pocos datos casi no se nota la diferencia; con muchos, es abismal.

---

# PARTE 2: Lo que ya trae Java

En la vida real casi nunca escribes los algoritmos de arriba: usas lo que trae Java.

---

## ARRAY NORMAL

### Menor a mayor

java

```java
int[] nums = {5, 2, 9, 1};
Arrays.sort(nums);
System.out.println(Arrays.toString(nums));   // [1, 2, 5, 9]
```

### Mayor a menor con `Integer[]`

`Collections.reverseOrder()` **no funciona con `int[]`**, hay que usar `Integer[]`.

java

```java
Integer[] nums = {5, 2, 9, 1};
Arrays.sort(nums, Collections.reverseOrder());
System.out.println(Arrays.toString(nums));   // [9, 5, 2, 1]
```

### Mayor a menor con `int[]`

Se ordena de menor a mayor y luego se **invierte** con un ciclo:

java

```java
int[] nums = {5, 2, 9, 1};
Arrays.sort(nums);   // [1, 2, 5, 9]

for (int i = 0; i < nums.length / 2; i++) {
    int temp = nums[i];
    nums[i] = nums[nums.length - 1 - i];
    nums[nums.length - 1 - i] = temp;
}

System.out.println(Arrays.toString(nums));   // [9, 5, 2, 1]
```

### Solo un tramo

El final es exclusivo.

java

```java
int[] nums = {9, 8, 5, 2, 1};
Arrays.sort(nums, 1, 4);   // ordena posiciones 1, 2 y 3
System.out.println(Arrays.toString(nums));   // [9, 2, 5, 8, 1]
```

### Textos

java

```java
String[] nombres = {"Luna", "ana", "Max", "Beto"};

Arrays.sort(nombres);
// [Beto, Luna, Max, ana]  ← las mayúsculas van primero

Arrays.sort(nombres, String.CASE_INSENSITIVE_ORDER);
// [ana, Beto, Luna, Max]  ← ignorando mayúsculas

Arrays.sort(nombres, Collections.reverseOrder());
// Z-A
```

### Objetos propios con `Comparator`

java

```java
Producto[] productos = {
    new Producto("Ocean", 85000),
    new Producto("Rosé", 90000),
    new Producto("Aqua", 70000)
};

// por precio, menor a mayor
Arrays.sort(productos, Comparator.comparingDouble(Producto::getPrecio));

// por precio, MAYOR A MENOR
Arrays.sort(productos, Comparator.comparingDouble(Producto::getPrecio).reversed());

// por nombre, A-Z
Arrays.sort(productos, Comparator.comparing(Producto::getNombre));
```

---

## ARRAYLIST

### Menor a mayor

java

```java
ArrayList<Integer> nums = new ArrayList<>();
nums.add(5);
nums.add(2);
nums.add(9);
nums.add(1);

Collections.sort(nums);
System.out.println(nums);   // [1, 2, 5, 9]
```

Equivalente:

java

```java
nums.sort(null);
```

### Mayor a menor (3 formas)

java

```java
// 1. Collections.sort con reverseOrder
Collections.sort(nums, Collections.reverseOrder());

// 2. el método sort de la lista con Comparator
nums.sort(Comparator.reverseOrder());

// 3. ordenar y luego invertir
Collections.sort(nums);
Collections.reverse(nums);

System.out.println(nums);   // [9, 5, 2, 1]
```

### Textos

java

```java
ArrayList<String> nombres = new ArrayList<>();
nombres.add("Luna");
nombres.add("ana");
nombres.add("Max");

Collections.sort(nombres);                        // mayúsculas primero
nombres.sort(String.CASE_INSENSITIVE_ORDER);      // ignorando mayúsculas
nombres.sort(Comparator.reverseOrder());          // Z-A
```

### Objetos propios con `Comparator`

java

```java
ArrayList<Producto> productos = new ArrayList<>();
productos.add(new Producto("Ocean", 85000));
productos.add(new Producto("Rosé", 90000));
productos.add(new Producto("Aqua", 70000));

// por precio, menor a mayor
productos.sort(Comparator.comparingDouble(Producto::getPrecio));

// por precio, MAYOR A MENOR
productos.sort(Comparator.comparingDouble(Producto::getPrecio).reversed());

// por nombre, A-Z
productos.sort(Comparator.comparing(Producto::getNombre));

// por precio y, si empatan, por nombre
productos.sort(
    Comparator.comparingDouble(Producto::getPrecio)
              .thenComparing(Producto::getNombre)
);
```

### Otros métodos útiles de `Collections`

java

```java
Collections.reverse(lista);        // invierte el orden
Collections.shuffle(lista);        // desordena al azar
Collections.swap(lista, 0, 2);     // intercambia posiciones 0 y 2
Collections.max(lista);            // el mayor
Collections.min(lista);            // el menor
```

---

## Clase `Producto` usada en los ejemplos

java

```java
class Producto {
    private String nombre;
    private double precio;

    Producto(String nombre, double precio) {
        this.nombre = nombre;
        this.precio = precio;
    }

    public String getNombre() {
        return nombre;
    }

    public double getPrecio() {
        return precio;
    }
}
```

> [!note] Sobre `Producto::getPrecio` Es una forma corta de decir "usa el método `getPrecio` de cada producto".

---

## Resumen: Array vs ArrayList

|Array|ArrayList|
|---|---|---|
|Tamaño|`arr.length`|`lista.size()`|
|Leer|`arr[i]`|`lista.get(i)`|
|Cambiar|`arr[i] = x`|`lista.set(i, x)`|
|Menor a mayor|`Arrays.sort(arr)`|`Collections.sort(lista)`|
|Mayor a menor|`Arrays.sort(arr, Collections.reverseOrder())` (con `Integer[]`)|`lista.sort(Comparator.reverseOrder())`|
|Objetos por un campo|`Arrays.sort(arr, Comparator.comparing...)`|`lista.sort(Comparator.comparing...)`|
|Imprimir|`Arrays.toString(arr)`|`System.out.println(lista)`|

---

## Ejercicios

- [ ]  Hacer a mano burbuja con `[5, 2, 9, 1]`
- [ ]  Correr `bubbleSort` y comprobar que imprime `[1, 2, 5, 9]`
- [ ]  Cambiar `bubbleSort` para que ordene de **mayor a menor**
- [ ]  Probar `selectionSort` e `insertionSort` con el mismo arreglo
- [ ]  Ordenar un `Integer[]` de mayor a menor con `Arrays.sort`
- [ ]  Ordenar un `ArrayList<Integer>` de mayor a menor de las 3 formas
- [ ]  Ordenar una lista de `Producto` por precio de mayor a menor
- [ ]  Ordenar una lista de `Usuario` por nombre
## Ver también

- [[Ruta Java]]
- [[Arrays en Java]]
- [[POO en Java]]
- [[MOC-programacion]]
