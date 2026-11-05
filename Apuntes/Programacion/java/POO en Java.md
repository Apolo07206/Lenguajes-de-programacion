---
title: POO en Java
area: programacion
tipo: lenguaje
tags: [programacion, java, poo, clases, objetos, herencia, polimorfismo, encapsulamiento]
created: 2026-09-20
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Java]]", "[[Arrays en Java]]", "[[Estructura de datos en Java]]", "[[Algoritmos de Ordenamientos en Java]]"]
---

> [!summary] Idea central En vez de tener variables y funciones sueltas, agrupas **datos** y **comportamiento** en un molde (la **clase**), y de ese molde sacas **objetos**.

**Orden del tema:**

1. Clases y objetos ✅
2. Métodos ✅
3. Constructores y `this` ✅
4. Encapsulamiento (`private`, getters, setters) ✅
5. Herencia (`extends`, `super`) ✅
6. Polimorfismo (`@Override`) ✅
7. Clases abstractas e interfaces ← pendiente

---

## 1. Clase y objeto

- **Clase** = el molde / plano. Define qué datos y qué métodos tendrá.
- **Objeto** = lo que sacas del molde con `new`.

java

```java
class Perro {
    String nombre;
    String color;
}

public class Main {
    public static void main(String[] args) {
        Perro perro1 = new Perro();   // crea el objeto
        perro1.nombre = "Max";        // el punto accede a sus datos
        perro1.color = "Café";

        System.out.println(perro1.nombre);
    }
}
```

- `Perro perro1` → declara una variable de tipo `Perro`
- `new Perro()` → crea el objeto de verdad
- Cada objeto tiene **sus propios datos**.

> [!warning] Errores comunes
> 
> - `class` va en **minúscula** (`Class` no existe).
> - Si metes la clase dentro de otra, tiene que ser `static class` para usarla desde el `main`. Lo más limpio: ponerla **afuera**.

---

## 2. Métodos

Un método es una función que vive dentro de la clase. Es lo que hace que el objeto "haga cosas".

java

```java
class Perro {
    String nombre;
    String color;

    void ladrar() {
        System.out.println(nombre + " dice: ¡Guau!");
    }

    void presentarse() {
        System.out.println("Soy " + nombre + " y soy de color " + color);
    }
}
```

java

```java
Perro perro1 = new Perro();
perro1.nombre = "Max";
perro1.color = "Café";
perro1.ladrar();        // Max dice: ¡Guau!
perro1.presentarse();   // Soy Max y soy de color Café
```

- `void` → hace algo pero **no devuelve** nada.
- Dentro del método usas los atributos directo (`nombre`, `color`).
- Se llaman con el punto: `perro1.ladrar()`.

---

## 3. Constructor y `this`

El constructor es un método especial que se ejecuta al hacer `new`. Sirve para dar los datos iniciales en una sola línea.

java

```java
class Perro {
    String nombre;
    String color;
    int edad;

    // constructor
    Perro(String nombre, String color, int edad) {
        this.nombre = nombre;
        this.color = color;
        this.edad = edad;
    }

    void presentarse() {
        System.out.println("Soy " + nombre + ", de color " + color + " y tengo " + edad + " años");
    }
}

public class Main {
    public static void main(String[] args) {
        Perro perro1 = new Perro("Max", "Café", 3);
        Perro perro2 = new Perro("Luna", "Blanco", 5);

        perro1.presentarse();
        perro2.presentarse();
    }
}
```

> [!note] Reglas del constructor
> 
> - Se llama **igual que la clase**.
> - **No lleva `void`** ni tipo de retorno.
> - Si defines uno con parámetros, ya es obligatorio pasarlos: `new Perro()` vacío da error.

**`this`** = "este objeto". En `this.nombre = nombre;` → `this.nombre` es el **atributo** y `nombre` (derecha) es el **parámetro** que llega. `this` los diferencia.

> [!tip] Desde Python `this` es como `self`, pero no lo pasas como parámetro.

---

## 4. Encapsulamiento

**Problema:** sin protección, cualquiera puede hacer `perro1.edad = -50` y el objeto queda con datos absurdos.

**Solución:** `private` + getters y setters.

java

```java
class Perro {
    private String nombre;
    private String color;
    private int edad;

    Perro(String nombre, String color, int edad) {
        this.nombre = nombre;
        this.color = color;
        this.edad = edad;
    }

    // GETTER: entrega el dato hacia afuera
    public String getNombre() {
        return nombre;
    }

    public int getEdad() {
        return edad;
    }

    // SETTER: cambia el dato, con validación
    public void setEdad(int edad) {
        if (edad >= 0) {
            this.edad = edad;
        } else {
            System.out.println("La edad no puede ser negativa");
        }
    }
}
```

java

```java
Perro perro1 = new Perro("Max", "Café", 3);

// perro1.edad = -50;   // ERROR de compilación: edad es private
perro1.setEdad(-50);    // lo rechaza
perro1.setEdad(4);      // lo acepta
System.out.println(perro1.getEdad());   // 4
```

> [!important] Lo que hace la diferencia El `if` solo sirve porque `private` **obliga** a pasar por el setter. Sin `private`, la gente se salta el `if` y asigna directo.
> 
> - `private` = cierra la puerta.
> - **Setter** = la única puerta, con un guardia (el `if`) que **comprueba antes de guardar**.
> - **Getter** = entrega el dato hacia afuera.

### Modificadores de acceso

|Modificador|Quién puede usarlo|
|---|---|
|`public`|Cualquier clase|
|`private`|Solo la propia clase|

**Regla práctica:** atributos `private`, métodos `public`.

### `static`

- Sin `static` → pertenece al **objeto** (cada perro tiene su nombre).
- Con `static` → pertenece a la **clase**, se usa sin crear objeto.
- `main` es `static` porque Java lo ejecuta antes de que exista cualquier objeto.

Por ahora: `main` con `static`, atributos `private`, métodos `public`.

---

## 5. ¿Cuándo uso `return`?

> [!question] Pregúntate: ¿quién necesita el dato?
> 
> - ¿Lo usa la **misma clase**? → úsalo directo (`nombre`, `edad`), sin return.
> - ¿Lo necesita **otra clase** (el hijo, el `main`)? → hazle un **getter con `return`**.

java

```java
class Animal {
    private String nombre;
    private int edad;

    public String getNombre() {   // devuelve algo → return
        return nombre;
    }

    public void presentarse() {   // no devuelve nada → void
        System.out.println("Soy " + nombre + " y tengo " + edad + " años");
    }
}
```

- `void` → hace algo, no devuelve nada.
- `String` / `int` / etc. → **devuelve** un valor y obligatoriamente termina con `return` de ese tipo.
- `edad` no tiene getter porque nadie fuera de `Animal` lo ha necesitado.

---

## 6. Herencia (`extends` y `super`)

Si varias clases comparten datos, los pones en una clase **padre** y las demás la **heredan**.

java

```java
class Animal {
    private String nombre;
    private int edad;

    Animal(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }

    public String getNombre() {
        return nombre;
    }

    public void presentarse() {
        System.out.println("Soy " + nombre + " y tengo " + edad + " años");
    }
}

class Perro extends Animal {
    private String raza;

    Perro(String nombre, int edad, String raza) {
        super(nombre, edad);   // llama al constructor del padre
        this.raza = raza;
    }

    public void ladrar() {
        System.out.println(getNombre() + " dice: ¡Guau!");
    }
}

public class Main {
    public static void main(String[] args) {
        Perro perro1 = new Perro("Max", 3, "Labrador");
        perro1.presentarse();   // heredado de Animal
        perro1.ladrar();        // propio de Perro
    }
}
```

**Salida:**

```
Soy Max y tengo 3 años
Max dice: ¡Guau!
```

- `extends` → "Perro **es un** Animal", hereda sus métodos.
- `this` → este objeto (la clase donde estás).
- `super` → la clase **padre**.

> [!warning] Reglas
> 
> 1. `super(...)` tiene que ser la **primera línea** del constructor del hijo.
> 2. Si un atributo del padre es `private`, el hijo **no lo toca directo**: usa el getter (`getNombre()`). Aquí se ve para qué sirve el encapsulamiento.

> [!note] Advertencia amarilla en VS Code `The value of the field ... is not used` no es un error: guardas el dato pero nunca lo lees. Compila normal. Se quita usándolo (un getter o dentro de un método).

---

## 7. Ejercicio hecho: Usuario y Cliente

java

```java
class Usuario {
    private String numeroTel;
    private String nombre;
    private String correo;

    Usuario(String numeroTel, String nombre, String correo) {
        this.numeroTel = numeroTel;
        this.nombre = nombre;
        this.correo = correo;
    }

    public String getNombre() {
        return nombre;
    }

    public String getCorreo() {
        return correo;
    }

    public String getNumeroTel() {
        return numeroTel;
    }
}

class Cliente extends Usuario {
    private int id;

    Cliente(int id, String numeroTel, String nombre, String correo) {
        super(numeroTel, nombre, correo);
        this.id = id;
    }

    public int getId() {
        return id;
    }
}

public class POO4 {
    public static void main(String[] args) {
        Cliente cliente1 = new Cliente(123456, "30122111", "Eduardo", "Eduardo@gmail.com");

        var nombre = cliente1.getNombre();   // heredado de Usuario
        var correo = cliente1.getCorreo();   // heredado de Usuario

        System.out.println("el usuario : " + nombre + " y su correo : " + correo + " estan registrados");
        System.out.println("Su id es: " + cliente1.getId());
    }
}
```

**Notas:**

- Un teléfono va como `String`, no `int`: un celular colombiano pasa el límite de `int` (~2.147 millones), no se suma ni se resta, y así no pierdes ceros ni el `+57`.
- `var` deja que Java deduzca el tipo (aquí `String`).
- `Cliente` no escribió `getNombre()`, pero funciona porque lo heredó.

---

## 8. Polimorfismo

### 8.1 Sobreescritura (`@Override`)

El padre tiene un método y cada hijo lo **reescribe a su manera**. Mismo nombre, comportamiento distinto.

java

```java
class Usuario {
    // ... atributos y constructor igual que antes

    public void presentarse() {
        System.out.println("Soy el usuario " + getNombre());
    }
}

class Cliente extends Usuario {
    private int id;

    // ... constructor con super(...)

    @Override
    public void presentarse() {
        System.out.println("Soy el cliente " + getNombre() + " con id " + id);
    }
}

class Administrador extends Usuario {
    private String cargo;

    Administrador(String cargo, String numeroTel, String nombre, String correo) {
        super(numeroTel, nombre, correo);
        this.cargo = cargo;
    }

    @Override
    public void presentarse() {
        System.out.println("Soy el administrador " + getNombre() + ", cargo: " + cargo);
    }
}
```

**Salida:**

```
Soy el usuario Ana
Soy el cliente Eduardo con id 123
Soy el administrador Laura, cargo: Gerente
```

> [!note] Reglas de `@Override`
> 
> - Le dice a Java: "este método reemplaza el del padre". Si te equivocas en el nombre (`@Overide`) o en el método, el compilador avisa.
> - Debe tener **el mismo nombre y los mismos parámetros** que el del padre.
> - Al llamar `c1.presentarse()`, Java ejecuta la versión de `Cliente`, no la de `Usuario`.

### 8.2 Ejercicio hecho: Persona, Aleman y Japones

java

```java
class Persona {
    private String nombre;
    private int edad;
    private String nacionalidad;

    Persona(String nombre, int edad, String nacionalidad) {
        this.nombre = nombre;
        this.edad = edad;
        this.nacionalidad = nacionalidad;
    }

    public String getNombre() {
        return nombre;
    }

    public int getEdad() {
        return edad;
    }

    public String getNacionalidad() {
        return nacionalidad;
    }

    public void presentarse() {
        System.out.println("Hola mi nombre es " + nombre + ", tengo " + edad + " años y soy de " + nacionalidad);
    }
}

class Aleman extends Persona {

    Aleman(String nombre, int edad, String nacionalidad) {
        super(nombre, edad, nacionalidad);
    }

    @Override
    public void presentarse() {
        System.out.println("Hola mi nombre es " + getNombre() + " y soy de " + getNacionalidad());
    }
}

class Japones extends Persona {

    Japones(String nombre, int edad, String nacionalidad) {
        super(nombre, edad, nacionalidad);
    }

    @Override
    public void presentarse() {
        System.out.println("Hola mi nombre es " + getNombre() + " y tengo " + getEdad());
    }
}
```

### 8.3 El cierre: polimorfismo con un solo ciclo

Hasta aquí cada objeto se llamaba por separado. La gracia es guardarlos **todos como el tipo padre** y recorrerlos con un solo `for`:

java

```java
public class POO5 {
    public static void main(String[] args) {

        Persona estadounidense = new Persona("John", 30, "Estados Unidos");
        Aleman aleman = new Aleman("Hans", 25, "Alemania");
        Japones japones = new Japones("Kenji", 22, "Japón");

        Persona[] personas = { estadounidense, aleman, japones };

        for (Persona p : personas) {
            p.presentarse();   // cada uno ejecuta SU versión
        }
    }
}
```

**Salida:**

```
Hola mi nombre es John, tengo 30 años y soy de Estados Unidos
Hola mi nombre es Hans y soy de Alemania
Hola mi nombre es Kenji y tengo 22
```

> [!important] Polimorfismo en una frase Una variable del tipo padre (`Persona`) puede guardar cualquier hijo (`Aleman`, `Japones`), y al llamar un método Java ejecuta la versión del **objeto real**, no la del tipo de la variable. Si mañana agregas `Colombiano extends Persona`, el `for` no cambia.

---

## Convenciones de estilo en Java

- Clases con mayúscula inicial: `Usuario`, `Cliente`.
- Atributos, parámetros y métodos en **camelCase**: `numeroTel`, `getNumeroTel()`.
- Evitar `Numero_tel` o `Nombre` con mayúscula (funciona, pero no es el estándar).
- Cuidar los espacios en los textos de `println` (`" años y soy de "`, no `"años y soy de"`).
- Identar bien el código.

---

## Resumen rápido

|Concepto|Palabra clave|En una frase|
|---|---|---|
|Clase|`class`|El molde|
|Objeto|`new`|Lo que sale del molde|
|Método|`void` / tipo|Lo que el objeto sabe hacer|
|Constructor|mismo nombre de la clase|Da los datos iniciales al hacer `new`|
|`this`|`this`|Este objeto|
|Encapsulamiento|`private` + get/set|Esconder el dato y revisarlo antes de guardar|
|Herencia|`extends`|El hijo reutiliza al padre|
|`super`|`super(...)`|Llamar al constructor del padre|
|Polimorfismo|`@Override`|Mismo método, cada hijo lo hace a su manera|

---

## Ejercicios

**Hechos**

- [x]  `Usuario` + `Cliente extends Usuario` (herencia con `super`)
- [x]  `Persona` + `Aleman` + `Japones` con `@Override`
- [x]  Polimorfismo: recorrer varios hijos como `Persona` con un solo `for`

**Pendientes**

- [ ]  Clase `Producto` con `private double precio`, `setPrecio` que rechace `<= 0` y `getPrecio`
- [ ]  `Gato extends Animal` con método `maullar()`
- [ ]  `Administrador extends Usuario` escrito por mí (con `getCargo()`)
- [ ]  `setCorreo` en `Usuario` que solo acepte correos con `@` (`correo.contains("@")`)

---

## Siguiente en la ruta

- [ ]  Algoritmos de ordenamiento: bubble sort → selection sort → insertion sort → merge/quick sort
- [ ]  Clases abstractas e interfaces
- [ ]  SOLID → patrones de diseño (Singleton, Factory, Observer, Strategy...)
- [ ]  Índices en MySQL (`CREATE INDEX`, `EXPLAIN`) para búsquedas rápidas con muchos datos
- [ ]  SDD (confirmar si es Spec-Driven Development o Software Design Document)
## Ver también

- [[Ruta Java]]
- [[Arrays en Java]]
- [[Estructura de datos en Java]]
- [[Algoritmos de Ordenamientos en Java]]
- [[MOC-programacion]]
