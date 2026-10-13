---
title: Arrays en C++
area: programacion
tipo: lenguaje
tags: [programacion, cpp]
created: 2026-09-16
updated: 2026-09-20
related: ["[[MOC-programacion]]", "[[Rutas]]"]
---

La forma de añadir, actualizar y eliminar elementos en C++ depende de si usas **Arrays Tradicionales** o la alternativa moderna recomendada: **`std::vector`**.

### Con `std::vector` (Recomendado)

`std::vector` administra el tamaño dinámicamente, por lo que es la opción estándar para estas operaciones.

C++

```
#include <iostream>
#include <vector>
#include <algorithm> // Necesario para std::find

using namespace std;

int main() {
    vector<int> numeros = {10, 20, 30};

    // 1. AÑADIR
    numeros.push_back(40);            // Añade 40 al final -> {10, 20, 30, 40}
    numeros.insert(numeros.begin(), 5); // Inserta 5 al inicio -> {5, 10, 20, 30, 40}

    // 2. ACTUALIZAR
    numeros[2] = 25;                  // Cambia el valor en el índice 2 -> {5, 10, 25, 30, 40}

    // 3. ELIMINAR
    numeros.pop_back();               // Elimina el último elemento (40)
    numeros.erase(numeros.begin() + 1); // Elimina el elemento en el índice 1 (10)

    // Mostrar resultado: 5 25 30
    for (int num : numeros) {
        cout << num << " ";
    }

    return 0;
}
```

### Con Arrays Tradicionales

Los arrays estáticos tienen un tamaño fijo definido al crearse, por lo que **no puedes cambiar su espacio en memoria**. Para "añadir" o "eliminar", debes llevar un control manual del número de elementos ocupados.

C++

```
#include <iostream>
using namespace std;

int main() {
    int capacidad = 10;
    int array[10] = {10, 20, 30}; // Tamaño reservado: 10
    int tamano = 3;              // Elementos reales usados: 3

    // 1. AÑADIR (Solo si tamano < capacidad)
    array[tamano] = 40; 
    tamano++; // Ahora hay 4 elementos: {10, 20, 30, 40}

    // 2. ACTUALIZAR
    array[1] = 25; // Cambia 20 por 25 en el índice 1

    // 3. ELIMINAR (Desplazando los elementos hacia la izquierda)
    int indiceAEliminar = 0; // Queremos borrar el 10
    for (int i = indiceAEliminar; i < tamano - 1; i++) {
        array[i] = array[i + 1];
    }
    tamano--; // Se reduce el contador de elementos útiles

    // Mostrar resultado: 25 30 40
    for (int i = 0; i < tamano; i++) {
        cout << array[i] << " ";
    }

    return 0;
}
```

### Resumen de Operaciones

| **Operación**                | **Arrays Tradicionales**                                  | **std::vector**               |
| ---------------------------- | --------------------------------------------------------- | ----------------------------- |
| **Actualizar**               | `array[i] = valor;`                                       | `vec[i] = valor;`             |
| **Añadir al final**          | `array[tamano++] = valor;` _(requiere espacio reservado)_ | `vec.push_back(valor);`       |
| **Eliminar al final**        | `tamano--;`                                               | `vec.pop_back();`             |
| **Eliminar en posición $i$** | Desplazar manualmente con un bucle `for`                  | `vec.erase(vec.begin() + i);` |

## Ver también

- [[MOC-programacion]]
- [[Rutas]]
