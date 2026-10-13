---
title: Pilas en Java
area: programacion
tipo: lenguaje
tags: [programacion, java, estructuras-de-datos, pilas, stack, lifo]
created: 2026-09-20
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Java]]", "[[Estructura de datos en Java]]", "[[Colas en Java]]"]
---

**Pila (Stack):** una colección donde el **último que entra es el primero que sale**. Se llama **LIFO** (_Last In, First Out_). Piensa en una pila de platos: pones uno encima y siempre agarras el de arriba.

Solo puedes tocar **el de arriba**, no cualquier posición como en la lista.

java

```java
import java.util.Stack;

public class Main {
    public static void main(String[] args) {
        Stack<String> pila = new Stack<>();

        pila.push("Ocean");     // mete arriba
        pila.push("Rosé");
        pila.push("Aqua");

        System.out.println(pila.peek());   // Aqua  (mira el de arriba, no lo saca)
        System.out.println(pila.pop());    // Aqua  (lo saca)
        System.out.println(pila.pop());    // Rosé
        System.out.println(pila);          // [Ocean]
        System.out.println(pila.isEmpty());   // false
    }
}
```

**Métodos principales:**

|Método|Qué hace|
|---|---|
|`push(x)`|Mete un elemento arriba|
|`pop()`|Saca y devuelve el de arriba|
|`peek()`|Mira el de arriba **sin sacarlo**|
|`isEmpty()`|Dice si está vacía|
|`size()`|Cuántos elementos hay|

**Dónde se usa en la vida real:**

- El botón de **deshacer** (Ctrl+Z) de cualquier editor.
- El botón **atrás** del navegador.
- Las llamadas a métodos en Java (por eso existe el error `StackOverflowError`).

**Diferencia con la lista:** en la lista accedes a cualquier posición con `get(i)`; en la pila solo al de arriba.

> [!note] Dato para tu trabajo  
> `Stack` es la clase clásica y es la que seguramente trae tu guía, pero hoy en día se recomienda más `ArrayDeque`. Para la actividad usa `Stack`, que es más fácil de explicar, y si quieres menciona `ArrayDeque` como extra.
## Ver también

- [[Ruta Java]]
- [[Estructura de datos en Java]]
- [[Colas en Java]]
- [[MOC-programacion]]
