---
title: Map y HashMap en Java
area: programacion
tipo: lenguaje
tags: [programacion, java, estructuras-de-datos, map, hashmap, colecciones]
created: 2026-09-20
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Java]]", "[[Estructura de datos en Java]]", "[[Set en Java]]"]
---

**`Map`:** guarda datos en parejas **clave → valor**. Buscas por la **clave**, no por la posición. Las claves **no se pueden repetir**; los valores sí.

**`HashMap`:** es la clase más usada que implementa `Map`. `Map` es la interfaz (el contrato) y `HashMap` es quien lo cumple, igual que `Queue` y `LinkedList`.

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

**Métodos principales:**

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
## Ver también

- [[Ruta Java]]
- [[Estructura de datos en Java]]
- [[Set en Java]]
- [[MOC-programacion]]
