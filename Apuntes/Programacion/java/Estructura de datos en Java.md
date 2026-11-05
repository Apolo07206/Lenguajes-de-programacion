---
title: Estructura de datos en Java
area: programacion
tipo: lenguaje
tags: [programacion, java, estructuras-de-datos, arraylist, stack, queue, map, generics]
created: 2026-09-20
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Java]]", "[[Arrays en Java]]", "[[Vectores en Java]]", "[[Pilas en Java]]", "[[Colas en Java]]", "[[Map y HashMap en Java]]", "[[Set en Java]]"]
---

> [!summary] Idea central Una estructura de datos es una forma de **guardar y organizar** varios datos. Cada una decide **cómo entran y cómo salen** los datos, y por eso cada una sirve para algo distinto.

|Estructura|Cómo funciona|Clase en Java|
|---|---|---|
|Lista|Por posición, accedes a cualquiera|`ArrayList`|
|Pila|Último en entrar, primero en salir (LIFO)|`Stack`|
|Cola|Primero en entrar, primero en salir (FIFO)|`Queue` + `LinkedList`|
|Mapa|Parejas clave → valor|`Map` + `HashMap`|

---

## 1. Lista (`ArrayList`)

Colección de elementos **en orden**, cada uno con una **posición (índice)** que empieza en 0. Permite repetidos y puedes meter, leer o quitar en **cualquier posición**.

java

```java
import java.util.ArrayList;

public class Main {
    public static void main(String[] args) {
        ArrayList<String> perfumes = new ArrayList<>();

        perfumes.add("Ocean");       // agrega al final
        perfumes.add("Rosé");
        perfumes.add("Aqua");

        System.out.println(perfumes.get(1));             // Rosé
        perfumes.set(0, "Ocean Intenso");                // cambia la posición 0
        perfumes.remove(2);                              // quita la posición 2

        System.out.println(perfumes.size());             // 2
        System.out.println(perfumes.contains("Rosé"));   // true
        System.out.println(perfumes);                    // [Ocean Intenso, Rosé]
    }
}
```

|Método|Qué hace|
|---|---|
|`add(x)`|Agrega al final|
|`get(i)`|Lee la posición `i`|
|`set(i, x)`|Cambia lo que hay en la posición `i`|
|`remove(i)`|Quita la posición `i`|
|`size()`|Cuántos elementos hay|
|`contains(x)`|Dice si existe|

**Clave:** en la lista puedes tocar **cualquier posición**. En pilas y colas solo los extremos.

---

## 2. Pila (`Stack`)

El **último que entra es el primero que sale**: **LIFO** (_Last In, First Out_). Como una pila de platos: pones uno encima y agarras el de arriba. Solo puedes tocar **el de arriba**.

java

```java
import java.util.Stack;

public class Main {
    public static void main(String[] args) {
        Stack<String> pila = new Stack<>();

        pila.push("Ocean");     // mete arriba
        pila.push("Rosé");
        pila.push("Aqua");

        System.out.println(pila.peek());      // Aqua (mira el de arriba, no lo saca)
        System.out.println(pila.pop());       // Aqua (lo saca)
        System.out.println(pila.pop());       // Rosé
        System.out.println(pila);             // [Ocean]
        System.out.println(pila.isEmpty());   // false
    }
}
```

|Método|Qué hace|
|---|---|
|`push(x)`|Mete un elemento arriba|
|`pop()`|Saca y devuelve el de arriba|
|`peek()`|Mira el de arriba **sin sacarlo**|
|`isEmpty()`|Dice si está vacía|
|`size()`|Cuántos elementos hay|

**Dónde se usa:**

- Botón **deshacer** (Ctrl+Z).
- Botón **atrás** del navegador.
- Llamadas a métodos en Java (por eso existe `StackOverflowError`).

> [!note] Dato extra `Stack` es la clase clásica y la más fácil de explicar. Hoy en día se recomienda más `ArrayDeque`, pero para estudiar y para la actividad `Stack` sirve.

---

## 3. Cola (`Queue`)

El **primero que entra es el primero que sale**: **FIFO** (_First In, First Out_). Como la fila de un banco: el que llegó primero se atiende primero.

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

|Método|Qué hace|
|---|---|
|`offer(x)`|Mete un elemento al final|
|`poll()`|Saca y devuelve el primero (si está vacía, devuelve `null`)|
|`peek()`|Mira el primero **sin sacarlo**|
|`isEmpty()`|Dice si está vacía|
|`size()`|Cuántos elementos hay|

> [!note] Por qué `Queue` y `LinkedList` `Queue` es una **interfaz** (el contrato) y no se puede instanciar sola. Se declara como `Queue` y se crea con `LinkedList`, que es quien cumple el contrato.

**Dónde se usa:** fila de impresión, pedidos que llegan (como los de Joyriver), turnos de atención, mensajes por procesar.

---

## 4. Comparación: lista vs pila vs cola

|Lista|Pila|Cola|
|---|---|---|---|
|Orden de salida|Cualquiera (por posición)|**LIFO**|**FIFO**|
|Dónde accedes|Cualquier índice|Solo arriba|Solo el frente (agregas por el final)|
|Repetidos|Sí|Sí|Sí|
|Métodos clave|`add`, `get`|`push`, `pop`|`offer`, `poll`|
|Ejemplo|Catálogo de perfumes|Deshacer (Ctrl+Z)|Fila de pedidos|

**Pila vs cola, en una frase:** la diferencia está en **cómo entran y salen los datos**. En la pila sale el último que entró; en la cola sale el primero que entró.

---

## 5. `Map` y `HashMap`

**`Map`:** guarda datos en parejas **clave → valor**. Buscas por la **clave**, no por la posición. Las claves **no se pueden repetir**; los valores sí.

**`HashMap`:** la clase más usada que implementa `Map`. `Map` es la interfaz (el contrato) y `HashMap` es quien lo cumple, igual que `Queue` y `LinkedList`.

java

```java
import java.util.HashMap;
import java.util.Map;

public class Main {
    public static void main(String[] args) {
        Map<String, Double> precios = new HashMap<>();

        precios.put("Ocean", 85000.0);    // agrega clave → valor
        precios.put("Rosé", 90000.0);
        precios.put("Aqua", 70000.0);

        System.out.println(precios.get("Rosé"));          // 90000.0
        System.out.println(precios.containsKey("Aqua"));  // true
        precios.put("Ocean", 95000.0);                    // misma clave: reemplaza el valor
        precios.remove("Aqua");

        System.out.println(precios.size());               // 2
        System.out.println(precios);                      // {Rosé=90000.0, Ocean=95000.0}
    }
}
```

|Método|Qué hace|
|---|---|
|`put(clave, valor)`|Agrega, o reemplaza si la clave ya existe|
|`get(clave)`|Devuelve el valor (o `null` si no existe)|
|`remove(clave)`|Quita la pareja|
|`containsKey(clave)`|Dice si existe la clave|
|`containsValue(valor)`|Dice si existe el valor|
|`size()`|Cuántas parejas hay|
|`keySet()`|Todas las claves|
|`values()`|Todos los valores|
|`entrySet()`|Todas las parejas (para recorrer)|

**Recorrerlo:**

java

```java
for (Map.Entry<String, Double> par : precios.entrySet()) {
    System.out.println(par.getKey() + " cuesta " + par.getValue());
}
```

**Diferencia con la lista:** en la lista buscas por posición (`get(0)`); en el mapa buscas por clave (`get("Rosé")`), y es muy rápido aunque haya miles de datos.

> [!warning] Cosas a recordar
> 
> - `HashMap` **no garantiza ningún orden**.
> - Si haces `put` con una clave que ya existe, **reemplaza** el valor (no crea otra pareja).
> - `get` de una clave que no existe devuelve `null`.

---

## 6. Generics

**Qué son:** una forma de decirle a Java **qué tipo de dato va a guardar** una clase o estructura, usando `< >`. Es lo que escribes en `ArrayList<String>`, `Stack<String>` o `Map<String, Double>`.

**Para qué sirven:**

1. **Seguridad de tipos:** Java te avisa del error al **compilar**, no cuando el programa ya está corriendo.
2. **Sin conversiones manuales** (casting).
3. **Reutilización:** escribes una clase una vez y sirve para cualquier tipo.

### Sin generics (el problema)

java

```java
ArrayList lista = new ArrayList();   // sin <>
lista.add("Ocean");
lista.add(5);                        // Java lo permite: mezcla tipos

String s = (String) lista.get(1);    // compila, pero EXPLOTA al correr
                                     // ClassCastException
```

### Con generics (la solución)

java

```java
ArrayList<String> lista = new ArrayList<>();
lista.add("Ocean");
// lista.add(5);                     // ERROR al compilar: no es String

String s = lista.get(0);             // sin casting
```

### Crear tu propia clase genérica

`T` es un "tipo comodín" que se define al usar la clase.

java

```java
class Caja<T> {
    private T contenido;

    Caja(T contenido) {
        this.contenido = contenido;
    }

    public T getContenido() {
        return contenido;
    }
}

public class Main {
    public static void main(String[] args) {
        Caja<String> c1 = new Caja<>("Ocean");
        Caja<Integer> c2 = new Caja<>(5);

        System.out.println(c1.getContenido());   // Ocean
        System.out.println(c2.getContenido());   // 5
    }
}
```

La misma clase `Caja` sirve para `String`, `Integer` o lo que sea.

> [!warning] Los generics no aceptan tipos primitivos `ArrayList<int>` da error. Se usan las clases envoltorio: `Integer`, `Double`, `Boolean`, `Character`.

**Nombres comunes de letras:** `T` (type), `E` (element), `K` (key), `V` (value). Por eso `Map<K, V>`.

---

## 7. Resumen rápido

|Quiero...|Uso|
|---|---|
|Guardar en orden y acceder por posición|`ArrayList`|
|Deshacer / ir hacia atrás|`Stack`|
|Atender en orden de llegada|`Queue` con `LinkedList`|
|Buscar rápido por una clave|`HashMap`|
|Que Java verifique el tipo de dato|Generics (`<String>`, `<Integer>`...)|

---

## 8. Ejercicios (escribirlos sin copiar)

### Ejercicio 1: Pila

- [ ]  Crea un `Stack<String>` llamado `historial`.
- [ ]  Con `push`, mete `"inicio"`, `"perfumes"`, `"carrito"`.
- [ ]  Imprime la de arriba con `peek()`.
- [ ]  Saca una con `pop()` e imprime cuál salió (como el botón "atrás").
- [ ]  Imprime la pila completa.

```
carrito
carrito
[inicio, perfumes]
```

### Ejercicio 2: Cola

- [ ]  Crea un `Queue<String>` llamado `pedidos` con `LinkedList`.
- [ ]  Con `offer`, mete `"Pedido 1"`, `"Pedido 2"`, `"Pedido 3"`.
- [ ]  Imprime el primero con `peek()`.
- [ ]  Atiende uno con `poll()` e imprime cuál salió.
- [ ]  Imprime la cola y su tamaño.

```
Pedido 1
Pedido 1
[Pedido 2, Pedido 3]
2
```

### Ejercicio 3: HashMap

- [ ]  Crea un `Map<String, Integer>` llamado `stock`.
- [ ]  Agrega `"Ocean"` con 10 y `"Rosé"` con 5.
- [ ]  Imprime el stock de `"Rosé"` con `get`.
- [ ]  Cambia el stock de `"Ocean"` a 8 con `put`.
- [ ]  Recorre el mapa con `entrySet()` e imprime cada pareja.

```
5
Ocean tiene 8
Rosé tiene 5
```

(El orden de un `HashMap` puede variar.)

### Ejercicio 4: Las tres juntas

Un mini programa de pedidos:

- [ ]  `Map<String, Integer> stock` con `"Ocean"` = 1 y `"Rosé"` = 1.
- [ ]  `Queue<String> pedidos` con `"Ocean"`, `"Rosé"`, `"Ocean"`.
- [ ]  `Stack<String> atendidos` vacía.
- [ ]  Mientras la cola no esté vacía: saca un pedido con `poll()`.
    - Si el stock de ese perfume es mayor que 0 → réstale 1, métela en `atendidos` e imprime `Atendido: ...`.
    - Si no → imprime `Sin stock: ...`.
- [ ]  Al final imprime `atendidos`.

```
Atendido: Ocean
Atendido: Rosé
Sin stock: Ocean
[Ocean, Rosé]
```
## Ver también

- [[Ruta Java]]
- [[Arrays en Java]]
- [[Vectores en Java]]
- [[Pilas en Java]]
- [[Colas en Java]]
- [[Map y HashMap en Java]]
- [[Set en Java]]
- [[MOC-programacion]]
