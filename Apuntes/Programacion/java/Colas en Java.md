---
title: Colas en Java
area: programacion
tipo: lenguaje
tags: [programacion, java, estructuras-de-datos, colas, queue, fifo]
created: 2026-09-20
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Java]]", "[[Estructura de datos en Java]]", "[[Pilas en Java]]"]
---

**Cola (Queue):** el **primero que entra es el primero que sale**. Se llama **FIFO** (_First In, First Out_). Es como la fila de un banco: el que llegó primero lo atienden primero.

java

```java
import java.util.LinkedList;
import java.util.Queue;

public class Main {
    public static void main(String[] args) {
        Queue<String> cola = new LinkedList<>();

        cola.offer("Pedido 1");    // entra por el final
        cola.offer("Pedido 2");
        cola.offer("Pedido 3");

        System.out.println(cola.peek());   // Pedido 1 (mira el primero, no lo saca)
        System.out.println(cola.poll());   // Pedido 1 (lo saca)
        System.out.println(cola.poll());   // Pedido 2
        System.out.println(cola);          // [Pedido 3]
        System.out.println(cola.size());   // 1
    }
}
```

**Métodos principales:**

|Método|Qué hace|
|---|---|
|`offer(x)`|Mete un elemento al final|
|`poll()`|Saca y devuelve el primero (si está vacía, devuelve `null`)|
|`peek()`|Mira el primero **sin sacarlo**|
|`isEmpty()`|Dice si está vacía|
|`size()`|Cuántos elementos hay|

Se declara como `Queue` (la interfaz) y se crea con `LinkedList`, porque `Queue` por sí sola no se puede instanciar.

**Dónde se usa:** fila de impresión, pedidos que llegan (como los de Joyriver), turnos de atención, mensajes por procesar.

---

**Comparación de las tres (esto es lo que pide el punto 3 de tu actividad):**

|Lista|Pila|Cola|
|---|---|---|---|
|Orden de salida|Cualquiera (por posición)|**LIFO**: el último en entrar sale primero|**FIFO**: el primero en entrar sale primero|
|Dónde accedes|Cualquier índice|Solo arriba|Solo el frente (y agregas por el final)|
|Repetidos|Sí|Sí|Sí|
|Método clave|`get(i)`, `add(x)`|`push`, `pop`|`offer`, `poll`|
|Ejemplo|Catálogo de perfumes|Deshacer (Ctrl+Z)|Fila de pedidos|
## Ver también

- [[Ruta Java]]
- [[Estructura de datos en Java]]
- [[Pilas en Java]]
- [[MOC-programacion]]
