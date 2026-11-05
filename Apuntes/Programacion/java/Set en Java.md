---
title: Set en Java
area: programacion
tipo: lenguaje
tags: [programacion, java, estructuras-de-datos, set, hashset, treeset, colecciones]
created: 2026-09-20
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Java]]", "[[Estructura de datos en Java]]", "[[Map y HashMap en Java]]"]
---


## ¿Qué es un Set?

Un **Set** (conjunto) es una colección que **no permite elementos duplicados**. Es parte de Java Collections Framework (`java.util`).

- No hay duplicados: si agregas algo que ya existe, simplemente no se agrega.
- **No tiene índices**: no existe `set.get(0)`.
- Modela el concepto matemático de conjunto.

```
Set<String> → { "Ana", "Luis", "Pedro" }
```

> [!note] Set es una interfaz
> No se puede hacer `new Set<>()`. Se usa una de sus implementaciones: `HashSet`, `LinkedHashSet` o `TreeSet`.

---

## Las 3 implementaciones

| Implementación | Orden | Null | Velocidad | Cuándo usarla |
|---|---|---|---|---|
| `HashSet` | Ninguno (impredecible) | Permite 1 `null` | La más rápida, O(1) | Por defecto, cuando no importa el orden |
| `LinkedHashSet` | Orden de inserción | Permite 1 `null` | Rápida, un poco más lenta que HashSet | Quieres quitar duplicados **manteniendo el orden** |
| `TreeSet` | Ordenado (natural o con Comparator) | No permite `null` | O(log n) | Quieres los elementos **siempre ordenados** |

---

## Crear un Set

```java
import java.util.*;

Set<String> hash = new HashSet<>();
Set<String> linked = new LinkedHashSet<>();
Set<String> tree = new TreeSet<>();
```

> [!tip] Buena práctica
> Declara la variable con la interfaz (`Set<String>`) y crea con la implementación (`new HashSet<>()`). Así cambias de implementación fácil.

### Crear con valores iniciales

```java
// Desde otra colección (ej: una lista)
Set<Integer> numeros = new HashSet<>(Arrays.asList(1, 2, 3, 3, 2));
System.out.println(numeros); // [1, 2, 3]  ← los duplicados se van

// Inmutable (Java 9+)
Set<String> colores = Set.of("rojo", "verde", "azul");
```

> [!warning] `Set.of()` es inmutable
> No puedes agregar ni borrar. Si lo intentas lanza `UnsupportedOperationException`. Además lanza error si le pasas elementos duplicados.

---

## Agregar datos: `add()`

```java
Set<String> nombres = new HashSet<>();

nombres.add("Ana");    // true  (se agregó)
nombres.add("Luis");   // true
nombres.add("Ana");    // false (ya existía, NO se agrega)

System.out.println(nombres); // [Ana, Luis] (orden no garantizado)
```

`add()` devuelve un `boolean`:
- `true` → se agregó
- `false` → ya existía

Sirve para **detectar duplicados**:

```java
if (!nombres.add("Ana")) {
    System.out.println("Ese nombre ya está registrado");
}
```

### Agregar varios: `addAll()`

```java
Set<String> otros = Set.of("Pedro", "Maria");
nombres.addAll(otros);
```

---

## Borrar datos

### `remove()` → borra un elemento

```java
nombres.remove("Luis");   // true si existía, false si no
```

### `removeIf()` → borra según una condición

```java
Set<Integer> nums = new HashSet<>(Set.of(1, 2, 3, 4, 5, 6));
nums.removeIf(n -> n % 2 == 0);  // borra los pares
System.out.println(nums);        // [1, 3, 5]
```

### `removeAll()` → borra todos los que estén en otra colección

```java
nums.removeAll(Set.of(1, 3));
```

### `retainAll()` → deja SOLO los que estén en la otra colección

```java
nums.retainAll(Set.of(5, 7));   // queda solo el 5
```

### `clear()` → vacía el Set

```java
nums.clear();
```

---

## Actualizar datos

> [!important] Los Sets no tienen `set()` ni `update()`
> Como no hay índices, no puedes "modificar la posición X". Para actualizar un elemento: **borras el viejo y agregas el nuevo**.

```java
Set<String> ciudades = new HashSet<>(Set.of("Bogota", "Cali", "Medellin"));

// Actualizar "Cali" por "Barranquilla"
ciudades.remove("Cali");
ciudades.add("Barranquilla");
```

> [!warning] Cuidado con objetos mutables
> Si guardas objetos y cambias un atributo que participa en `hashCode()` **mientras están dentro del Set**, el Set se corrompe (ya no lo encuentra). Lo correcto: sacar el objeto, modificarlo y volverlo a meter.

---

## Consultar

```java
Set<String> s = new HashSet<>(Set.of("a", "b", "c"));

s.contains("a");      // true  → ¿existe el elemento?
s.size();             // 3     → cantidad de elementos
s.isEmpty();          // false → ¿está vacío?
s.containsAll(Set.of("a", "b")); // true → ¿contiene todos?
```

`contains()` en `HashSet` es **O(1)**, muchísimo más rápido que en una `List` (O(n)). Esa es una de las razones principales para usar Sets.

---

## Recorrer un Set

### `for-each` (el más común)

```java
for (String nombre : nombres) {
    System.out.println(nombre);
}
```

### `forEach` con lambda

```java
nombres.forEach(n -> System.out.println(n));
```

### Con `Iterator` (permite borrar mientras recorres)

```java
Iterator<String> it = nombres.iterator();
while (it.hasNext()) {
    String n = it.next();
    if (n.startsWith("A")) {
        it.remove();   // forma segura de borrar durante el recorrido
    }
}
```

> [!warning] Error clásico
> Si haces `nombres.remove(...)` **dentro** de un `for-each`, lanza `ConcurrentModificationException`. Usa `Iterator` o `removeIf()`.

---

## Operaciones de conjuntos (matemáticas)

Muy útiles y muy preguntadas.

```java
Set<Integer> A = new HashSet<>(Set.of(1, 2, 3, 4));
Set<Integer> B = new HashSet<>(Set.of(3, 4, 5, 6));
```

### Unión (A ∪ B)

```java
Set<Integer> union = new HashSet<>(A);
union.addAll(B);            // [1, 2, 3, 4, 5, 6]
```

### Intersección (A ∩ B)

```java
Set<Integer> interseccion = new HashSet<>(A);
interseccion.retainAll(B);  // [3, 4]
```

### Diferencia (A − B)

```java
Set<Integer> diferencia = new HashSet<>(A);
diferencia.removeAll(B);    // [1, 2]
```

### Subconjunto

```java
Set.of(3, 4).containsAll(Set.of(3)); // true
A.containsAll(Set.of(1, 2));         // true → {1,2} es subconjunto de A
```

> [!note]
> Se hace una **copia** (`new HashSet<>(A)`) para no destruir el conjunto original.

---

## TreeSet: métodos extra (ordenado)

```java
TreeSet<Integer> t = new TreeSet<>(Set.of(10, 20, 30, 40, 50));

t.first();        // 10  → el menor
t.last();         // 50  → el mayor
t.floor(25);      // 20  → el mayor que sea <= 25
t.ceiling(25);    // 30  → el menor que sea >= 25
t.lower(30);      // 20  → estrictamente menor
t.higher(30);     // 40  → estrictamente mayor
t.headSet(30);    // [10, 20]      → menores que 30
t.tailSet(30);    // [30, 40, 50]  → mayores o iguales a 30
t.pollFirst();    // saca y devuelve el 10
t.descendingSet();// orden inverso
```

### TreeSet con orden personalizado

```java
Set<String> porLongitud = new TreeSet<>(Comparator.comparing(String::length));
```

> [!warning]
> En un `TreeSet` la "igualdad" la define el `compareTo`/`Comparator`. Con el ejemplo de arriba, dos Strings de igual longitud se consideran **duplicados** y uno se pierde.

---

## Sets con objetos propios (`equals` y `hashCode`)

Para que un `HashSet` detecte duplicados de **tus clases**, debes sobreescribir `equals()` y `hashCode()`. Si no, cada objeto se considera distinto aunque tengan los mismos datos.

```java
import java.util.Objects;

class Estudiante {
    String documento;
    String nombre;

    Estudiante(String documento, String nombre) {
        this.documento = documento;
        this.nombre = nombre;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Estudiante)) return false;
        Estudiante e = (Estudiante) o;
        return documento.equals(e.documento);
    }

    @Override
    public int hashCode() {
        return Objects.hash(documento);
    }
}

Set<Estudiante> set = new HashSet<>();
set.add(new Estudiante("123", "Ana"));
set.add(new Estudiante("123", "Ana Maria")); // mismo documento → NO se agrega
System.out.println(set.size()); // 1
```

> [!tip]
> Para un `TreeSet` de objetos propios, la clase debe implementar `Comparable` o pasar un `Comparator`.

---

## ¿Para qué se usa? (casos reales)

### 1. Eliminar duplicados de una lista

```java
List<Integer> lista = Arrays.asList(1, 2, 2, 3, 3, 3);
Set<Integer> sinDuplicados = new HashSet<>(lista);
// [1, 2, 3]

// Manteniendo el orden original:
Set<Integer> ordenado = new LinkedHashSet<>(lista);
```

### 2. Verificar si un usuario/correo ya existe

```java
Set<String> correos = new HashSet<>();

if (!correos.add("ferney@mail.com")) {
    System.out.println("Correo ya registrado");
}
```

### 3. Contar elementos únicos

```java
String texto = "hola mundo";
Set<Character> letras = new HashSet<>();
for (char c : texto.toCharArray()) {
    if (c != ' ') letras.add(c);
}
System.out.println("Letras distintas: " + letras.size());
```

### 4. Buscar rápido (lista blanca / negra)

```java
Set<String> ipsBloqueadas = new HashSet<>(Set.of("10.0.0.5", "192.168.1.9"));

if (ipsBloqueadas.contains(ipEntrante)) {
    System.out.println("Acceso denegado");
}
```

> [!tip] Útil para ciberseguridad
> Listas de IPs, hashes, dominios maliciosos (IOCs) o usuarios permitidos se manejan con `HashSet` porque `contains()` es instantáneo aunque haya millones de datos.

### 5. Encontrar elementos comunes entre dos grupos

```java
Set<String> inscritosJava = new HashSet<>(Set.of("Ana", "Luis", "Pedro"));
Set<String> inscritosPython = new HashSet<>(Set.of("Luis", "Maria", "Pedro"));

inscritosJava.retainAll(inscritosPython);
// [Luis, Pedro] → están en ambos cursos
```

### 6. Detectar duplicados en un arreglo

```java
int[] datos = {4, 7, 2, 7, 9};
Set<Integer> vistos = new HashSet<>();

for (int d : datos) {
    if (!vistos.add(d)) {
        System.out.println("Duplicado: " + d);
    }
}
```

---

## Set vs List vs Map

| | List | Set | Map |
|---|---|---|---|
| Duplicados | Sí | **No** | Claves no, valores sí |
| Índices | Sí (`get(i)`) | No | No (se accede por clave) |
| Orden | Orden de inserción | Depende de la implementación | Depende de la implementación |
| Búsqueda (`contains`) | O(n) | **O(1)** en HashSet | O(1) por clave |
| Úsalo cuando... | Importa la posición o hay repetidos | Necesitas elementos únicos | Necesitas pares clave-valor |

---

## Errores comunes

- Intentar `new Set<>()` → es interfaz, usa `new HashSet<>()`.
- Esperar un orden en `HashSet` → no hay garantía.
- Intentar `set.get(0)` → no existe, los Sets no tienen índices.
- Borrar dentro de un `for-each` → `ConcurrentModificationException`.
- Olvidar `equals()` y `hashCode()` en clases propias.
- Meter `null` en un `TreeSet` → `NullPointerException`.
- Modificar un `Set.of(...)` → `UnsupportedOperationException`.

---

## Resumen rápido de métodos

| Acción | Método |
|---|---|
| Agregar | `add(x)`, `addAll(col)` |
| Borrar | `remove(x)`, `removeIf(cond)`, `removeAll(col)`, `retainAll(col)`, `clear()` |
| Actualizar | `remove(viejo)` + `add(nuevo)` |
| ¿Existe? | `contains(x)`, `containsAll(col)` |
| Tamaño | `size()`, `isEmpty()` |
| Recorrer | `for-each`, `forEach()`, `iterator()` |
| Unión | `addAll()` |
| Intersección | `retainAll()` |
| Diferencia | `removeAll()` |
## Ver también

- [[Ruta Java]]
- [[Estructura de datos en Java]]
- [[Map y HashMap en Java]]
- [[MOC-programacion]]
