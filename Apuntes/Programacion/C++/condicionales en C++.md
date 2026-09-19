
# Condicionales en C++: `if`, `else` y `switch`

Las estructuras condicionales permiten que tu programa **tome decisiones** y ejecute distinto código según si una condición es verdadera (`true`) o falsa (`false`).

## 1. Cheat Sheet Comparativo

|**Estructura**|**¿Cuándo usarla?**|**Ejemplo de uso**|
|---|---|---|
|**`if` / `else if` / `else`**|Evaluaciones complejas, **rangos** (`> 18`), o **múltiples condiciones** (`&&`, `\|`).|Evaluar notas, edades o temperaturas.|
|**`switch`**|Comparar una sola variable **entera o caracter (`char`)** contra **valores exactos**.|Menús de opciones, días de la semana.|
|**Operador Ternario**|Condicionales ultra cortos en **una sola línea** de código.|Asignaciones rápidas de variables.|

## 2. Código de Ejemplo Completo

C++

```
#include <iostream>
using namespace std;

int main() {
    // -------------------------------------------------------------
    // 1. IF, ELSE IF, ELSE (Rangos y condiciones complejas)
    // -------------------------------------------------------------
    int edad = 16;

    if (edad >= 18) {
        cout << "Eres mayor de edad." << endl;
    } else if (edad >= 13) {
        cout << "Eres un adolescente." << endl; // Se ejecuta este
    } else {
        cout << "Eres un nino." << endl;
    }

    // -------------------------------------------------------------
    // 2. SWITCH (Valores exactos de un int o char)
    // -------------------------------------------------------------
    char opcion = 'B';

    switch (opcion) {
        case 'A':
            cout << "Elegiste la opcion A." << endl;
            break; // Obligatorio para no ejecutar los siguientes casos
        case 'B':
            cout << "Elegiste la opcion B." << endl; // Se ejecuta este
            break;
        case 'C':
            cout << "Elegiste la opcion C." << endl;
            break;
        default:
            cout << "Opcion no valida." << endl; // Equivale al 'else'
            break;
    }

    // -------------------------------------------------------------
    // 3. OPERADOR TERNARIO (Sintaxis corta: condicion ? true : false)
    // -------------------------------------------------------------
    int puntos = 80;
    // Si puntos >= 50 asigna "Aprobado", si no asigna "Reprobado"
    string estado = (puntos >= 50) ? "Aprobado" : "Reprobado";

    cout << "Estado final: " << estado << endl;

    return 0;
}
```

## 💡 Operadores Lógicos (Para combinar condiciones)

- **`&&` (AND / Y):** Verdadero si **ambas** condiciones se cumplen.
    
    C++
    
    ```
    if (edad >= 18 && tieneLicencia == true) { ... }
    ```
    
- **`||` (OR / O):** Verdadero si al menos **una** condición se cumple.
    
    C++
    
    ```
    if (esFinDeSemana || esFestivo) { ... }
    ```
    
- **`!` (NOT / NO):** Invierte el valor de verdad.
    
    C++
    
    ```
    if (!estaLloviendo) { ... } // Si NO esta lloviendo
    ```