---
title: Punteros y referencias
area: programacion
tipo: lenguaje
tags: [programacion, cpp]
created: 2026-09-16
updated: 2026-09-20
related: ["[[MOC-programacion]]", "[[Rutas]]"]
---

Markdown

````
# 📌 Cheatsheet: Punteros y Memoria Dinámica en C++

## 1. El Asterisco (`*`): ¿Cuándo SÍ y cuándo NO?

```cpp
int x = 10;
int y = 50;

// --- CUANDO SÍ LLEVA '*' ---

int* ptr;        // DECLARACIÓN: Crea una variable puntero
ptr = &x;

*ptr = 99;       // DESREFERENCIACIÓN: Cambia el valor de x a 99
cout << *ptr;    // IMPRIME VALOR: Muestra 99


// --- CUANDO NO LLEVA '*' ---

ptr = &y;        // CAMBIO DE DIRECCIÓN: Ahora apunta a 'y'
cout << ptr;     // IMPRIME DIRECCIÓN: Muestra 0x7ffe... (GPS)
delete ptr;      // LIBERACIÓN: Se le pasa la dirección sola
````

## 2. Punteros en Funciones

C++

```
#include <iostream>
using namespace std;

// La función recibe la dirección: int* A = &numero;
void duplicar(int* A) {
    *A = *A * 2; // Modifica la variable original en la memoria
}

int main() {
    int numero = 10;

    duplicar(&numero); // Se pasa con '&' (dirección)

    cout << numero; // Imprime 20 (Modificado directamente)
    return 0;
}
```

## 3. Memoria Dinámica (Paso a Paso)

C++

```
#include <iostream>
using namespace std;

int main() {
    // --- 1. Variable Individual ---
    int* pNum = new int;    // Reservar en Heap
    *pNum = 50;             // Asignar valor
    cout << *pNum << endl;  // Usar valor

    delete pNum;            // Liberar memoria
    pNum = nullptr;         // Limpiar puntero


    // --- 2. Arreglo Dinámico ---
    int tamano = 3;
    int* arr = new int[tamano]; // Reservar arreglo

    arr[0] = 10; // Llenar
    arr[1] = 20;
    arr[2] = 30;

    for (int i = 0; i < tamano; i++) {
        cout << arr[i] << " "; // Imprimir
    }

    delete[] arr;  // Liberar arreglo (usar [])
    arr = nullptr; // Limpiar

    return 0;
}
```

## 4. Recorrido de Arreglos con Punteros

C++

```
#include <iostream>
using namespace std;

int main() {
    int datos[] = {10, 20, 30};
    int* ptr = datos; // Apunta al primer elemento (datos[0])

    for (int i = 0; i < 3; i++) {
        cout << *ptr << " "; // Imprime el valor
        ptr++;               // Avanza a la siguiente posición de memoria
    }
    // Salida: 10 20 30

    return 0;
}
```


```
int* ptr = datos;        // 1. Forma corta (La más usada)
int* ptr = &datos[0];    // 2. Forma explícita (Dirección del primer elemento)
int* ptr = &*datos;      // 3. Forma equivalente pero rara
```


# 5 otros ejemplos 



```
#include <iostream>

using namespace std;

  

int mult(int* A){

return *A*=2;

}


int main() {

int a;


cout <<"ingrese un numero : ";

cin>>a;  

cout<<mult(&a);

return 0;

}
```

## Ver también

- [[MOC-programacion]]
- [[Rutas]]
