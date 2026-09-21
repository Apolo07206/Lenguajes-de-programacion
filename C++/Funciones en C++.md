---
title: Funciones en C++
area: programacion
tipo: lenguaje
tags: [programacion, cpp]
created: 2026-09-19
updated: 2026-09-20
related: ["[[MOC-programacion]]", "[[Rutas]]"]
---

## 1. Guía Rápida (Tipos de Parámetros)

| **Tipo**               | **Sintaxis**              | **¿Modifica el original?** | **Uso principal**                                            |
| ---------------------- | ------------------------- | -------------------------- | ------------------------------------------------------------ |
| **Por Valor**          | `void f(int x)`           | ❌ No (crea copia)          | Datos simples (`int`, `bool`, `char`).                       |
| **Por Referencia**     | `void f(int &x)`          | 2 Sí (directo)             | Cuando necesitas modificar la variable original.             |
| **Por Ref. Constante** | `void f(const string &s)` | ❌ No (solo lectura)        | Texto (`string`) o datos grandes, para evitar copias lentas. |

## 2. Código de Ejemplo Completo

C++

```
#include <iostream>
#include <string>
using namespace std;

// -------------------------------------------------------------
// 1. VOID (Sin retorno) + POR VALOR (Copia)
// La variable 'intentos' original NO cambiará fuera de aquí.
// -------------------------------------------------------------
void intentarLogin(int intentos) {
    intentos = intentos - 1; 
    cout << "[Copia] Intentos restantes dentro de la funcion: " << intentos << endl;
}

// -------------------------------------------------------------
// 2. RETORNO DE VALOR
// Realiza un cálculo y devuelve el resultado con 'return'.
// -------------------------------------------------------------
int calcularDoble(int numero) {
    return numero * 2;
}

// -------------------------------------------------------------
// 3. PASO POR REFERENCIA (&)
// Modifica la variable ORIGINAL pasada desde main.
// -------------------------------------------------------------
void curarJugador(int &vidaActual, int puntosCuracion) {
    vidaActual = vidaActual + puntosCuracion; // Afecta directo a la variable original
}

// -------------------------------------------------------------
// 4. PASO POR REFERENCIA CONSTANTE (const &)
// Rápido y eficiente (no copia el string) pero PROHÍBE modificarlo.
// -------------------------------------------------------------
void mostrarPerfil(const string &nombreUsuario) {
    // nombreUsuario = "Otro"; // <--- Da error si intentas cambiarlo
    cout << "Usuario activo: " << nombreUsuario << endl;
}

// -------------------------------------------------------------
// MAIN DE PRUEBAS
// -------------------------------------------------------------
int main() {
    // Ejemplo 1: Por valor
    int misIntentos = 3;
    intentarLogin(misIntentos);
    cout << "Intentos en main (Sigue igual): " << misIntentos << "\n\n";

    // Ejemplo 2: Retornar valor
    int resultado = calcularDoble(5);
    cout << "El doble de 5 es: " << resultado << "\n\n";

    // Ejemplo 3: Por referencia
    int vida = 50;
    cout << "Vida inicial: " << vida << endl;
    curarJugador(vida, 30); // Pasamos 'vida' directamente
    cout << "Vida despues de curar (Cambio!): " << vida << "\n\n";

    // Ejemplo 4: Referencia constante
    string jugador = "Alex123";
    mostrarPerfil(jugador);

    return 0;
}
```

## 💡 Regla de Oro para decidir rápido

1. **¿Devuelve algo?**
    
    - Sí $\rightarrow$ Usa el tipo (`int`, `double`, `string`).
        
    - No $\rightarrow$ Usa `void`





## Ver también

- [[MOC-programacion]]
- [[Rutas]]
