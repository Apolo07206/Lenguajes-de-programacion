---
title: Matrices en Java
area: programacion
tipo: lenguaje
tags: [programacion, java, matrices, arrays, estructuras-de-datos]
created: 2026-09-20
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Java]]", "[[Arrays en Java]]"]
---

# Matrices en Java


## ¿Qué es una matriz?

Una **matriz** es un arreglo de arreglos (array 2D). Se organiza en **filas** y **columnas** y todos sus elementos son del **mismo tipo**.

```
        col 0  col 1  col 2
fila 0 [  1  ,  2  ,  3  ]
fila 1 [  4  ,  5  ,  6  ]
fila 2 [  7  ,  8  ,  9  ]
```

Se accede con `matriz[fila][columna]`. Los índices empiezan en **0**.

---

## Declaración y creación

```java
// Declarar
int[][] matriz;

// Crear con tamaño (3 filas, 4 columnas), se llena con 0 por defecto
matriz = new int[3][4];

// Todo en una línea
int[][] matriz2 = new int[3][4];

// Crear con valores directos
int[][] matriz3 = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};
```

> [!note] Valores por defecto
> `int` → `0`, `double` → `0.0`, `boolean` → `false`, `String`/objetos → `null`

---

## Acceder y modificar elementos

```java
int[][] m = {
    {1, 2, 3},
    {4, 5, 6}
};

System.out.println(m[1][2]); // 6  (fila 1, columna 2)

m[0][0] = 99;                // modifica el primer elemento
```

---

## Tamaños: `length`

```java
int filas = m.length;        // cantidad de filas
int columnas = m[0].length;  // cantidad de columnas de la fila 0
```

> [!warning] Ojo
> `length` **no lleva paréntesis** en arrays (en `String` sí: `.length()`).

---

## Recorrer una matriz

### Con `for` anidado (el más usado)

```java
for (int i = 0; i < m.length; i++) {          // filas
    for (int j = 0; j < m[i].length; j++) {   // columnas
        System.out.print(m[i][j] + " ");
    }
    System.out.println(); // salto de línea al terminar cada fila
}
```

### Con `for-each`

```java
for (int[] fila : m) {
    for (int valor : fila) {
        System.out.print(valor + " ");
    }
    System.out.println();
}
```

> [!tip] ¿Cuándo usar cuál?
> - `for` normal → cuando necesitas los índices (`i`, `j`) o modificar valores.
> - `for-each` → cuando solo vas a leer.

---

## Llenar una matriz con datos del usuario

```java
import java.util.Scanner;

Scanner sc = new Scanner(System.in);
int[][] m = new int[2][3];

for (int i = 0; i < m.length; i++) {
    for (int j = 0; j < m[i].length; j++) {
        System.out.print("Valor [" + i + "][" + j + "]: ");
        m[i][j] = sc.nextInt();
    }
}
```

---

## Ejemplos típicos

### Suma de todos los elementos

```java
int suma = 0;
for (int i = 0; i < m.length; i++) {
    for (int j = 0; j < m[i].length; j++) {
        suma += m[i][j];
    }
}
```

### Suma de una fila específica

```java
int sumaFila = 0;
for (int j = 0; j < m[0].length; j++) {
    sumaFila += m[0][j];
}
```

### Diagonal principal (matriz cuadrada)

```java
for (int i = 0; i < m.length; i++) {
    System.out.println(m[i][i]);
}
```

### Transpuesta (filas ↔ columnas)

```java
int[][] t = new int[m[0].length][m.length];
for (int i = 0; i < m.length; i++) {
    for (int j = 0; j < m[i].length; j++) {
        t[j][i] = m[i][j];
    }
}
```

---

## Matrices irregulares (jagged arrays)

En Java cada fila es un arreglo independiente, así que pueden tener **tamaños distintos**.

```java
int[][] irregular = new int[3][];
irregular[0] = new int[2];
irregular[1] = new int[4];
irregular[2] = new int[1];
```

Por eso siempre se usa `m[i].length` en el `for` interno y no un número fijo.

---

## Imprimir rápido con `Arrays`

```java
import java.util.Arrays;

System.out.println(Arrays.deepToString(m));
// [[1, 2, 3], [4, 5, 6]]
```

> [!warning]
> `Arrays.toString(m)` **no sirve** para matrices (imprime referencias). Usa `deepToString`.

---

## Errores comunes

- `ArrayIndexOutOfBoundsException` → te pasaste del índice (usar `<=` en vez de `<` en el `for`).
- Confundir `m[fila][columna]` con `m[columna][fila]`.
- Usar `length` con paréntesis.
- Olvidar que los índices empiezan en 0.

---

## Resumen rápido

| Concepto       | Sintaxis                   |
| -------------- | -------------------------- |
| Declarar       | `int[][] m;`               |
| Crear          | `new int[filas][columnas]` |
| Acceder        | `m[i][j]`                  |
| Nº de filas    | `m.length`                 |
| Nº de columnas | `m[i].length`              |
| Recorrer       | `for` dentro de `for`      |
| Imprimir       | `Arrays.deepToString(m)`   |
## Ver también

- [[Ruta Java]]
- [[Arrays en Java]]
- [[MOC-programacion]]
