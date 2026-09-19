# Bucles en C++: `for`, `while` y `do-while`

Los bucles se usan para repetir un bloque de código varias veces. La elección depende de **si sabes de antemano cuántas veces vas a repetir el proceso**.

## 1. Cheat Sheet Comparativo

|**Bucle**|**¿Cuándo usarlo?**|**¿Cuándo evalúa la condición?**|**Mínimo de ejecuciones**|
|---|---|---|---|
|**`for`**|Sabes exactamente **cuántas veces** repetirás el bloque (ej. de 1 a 10).|Al inicio|0 veces|
|**`while`**|No sabes cuántas veces, depende de **una condición** (ej. mientras el usuario no ponga '0').|Al inicio|0 veces|
|**`do-while`**|Necesitas que el código se ejecute **al menos una vez** antes de comprobar la condición.|Al final|**1 vez**|

## 2. Código de Ejemplo

C++

```
#include <iostream>
using namespace std;

int main() {
    // -------------------------------------------------------------
    // 1. BUCLE FOR (Para cuando sabes el número exacto de iteraciones)
    // Sintaxis: for (inicio; condicion; incremento)
    // -------------------------------------------------------------
    cout << "--- Bucle FOR ---" << endl;
    for (int i = 1; i <= 5; i++) {
        cout << "Contador: " << i << endl; // Imprime del 1 al 5
    }

    // -------------------------------------------------------------
    // 2. BUCLE WHILE (Se repite MIENTRAS la condición sea verdadera)
    // -------------------------------------------------------------
    cout << "\n--- Bucle WHILE ---" << endl;
    int energia = 3;
    while (energia > 0) {
        cout << "Atacando... Energia restante: " << energia << endl;
        energia--; // Importante: reducir la variable para evitar bucles infinitos
    }

    // -------------------------------------------------------------
    // 3. BUCLE DO-WHILE (Ejecuta PRIMERO, pregunta DESPUÉS)
    // -------------------------------------------------------------
    cout << "\n--- Bucle DO-WHILE ---" << endl;
    int opcion;
    do {
        cout << "1. Jugar\n2. Salir\nElige una opcion: ";
        cin >> opcion;
    } while (opcion != 2); // Repite si NO elige la opción de salir

    return 0;
}
```

## 💡 Control del Bucle: `break` y `continue`

- **`break`**: Rompe y sale del bucle de inmediato.
    
- **`continue`**: Salta la iteración actual y pasa directamente a la siguiente.
    

C++

```
for (int i = 1; i <= 5; i++) {
    if (i == 3) continue; // Salta el 3, no lo imprime
    if (i == 5) break;    // Detiene el bucle por completo
    cout << i << " ";     // Imprime: 1 2 4
}
```