---
title: Polimorfismo y abstracción en Python
area: programacion
tipo: lenguaje
tags: [programacion, python, poo, polimorfismo]
created: 2026-10-12
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Clases y objetos en Python]]", "[[Herencia en Python]]", "[[Encapsulamiento en Python]]", "[[POO en Java]]"]
---

# Polimorfismo y abstracción en Python

**Polimorfismo**: el mismo nombre de método, comportamiento distinto según el objeto. **Abstracción**: definir el QUÉ debe hacer un método sin implementarlo (clases abstractas). Cierre de la saga POO.

## Polimorfismo con ejemplos

```python
class Perro:
    def hablar(self):
        print("Guau 🐕")

class Gato:
    def hablar(self):
        print("Miau 🐈")

class Pato:
    def hablar(self):
        print("Cuac 🦆")

# Polimorfismo: mismo mensaje (hablar), distintas respuestas
animales = [Perro(), Gato(), Pato()]

for animal in animales:
    animal.hablar()     # cada uno responde con SU versión de hablar()
# Guau 🐕 / Miau 🐈 / Cuac 🦆

# Funciona SIN herencia compartida: basta que el método se llame igual
def hacer_hablar(criatura):
    criatura.hablar()   # el objeto decide qué hacer

hacer_hablar(Perro())   # Guau 🐕
hacer_hablar(Pato())    # Cuac 🦆
```

```python
# Polimorfismo con operadores (¡los str y list ya son polimórficos!)
print("hola" + "mundo")        # + concatena strings
print([1, 2] + [3])            # + concatena listas
print(5 + 5)                   # + suma números

# Polimorfismo con la función len() sobre tipos distintos
print(len("hola"))             # 4
print(len([1, 2, 3]))          # 3
print(len({"a": 1, "b": 2}))   # 2
```

## Polimorfismo con herencia (el patrón real)

```python
class Figura:
    def area(self):
        return 0

    def describir(self):
        return f"Soy una figura, mi área es {self.area()}"

class Cuadrado(Figura):
    def __init__(self, lado):
        self.lado = lado

    def area(self):                     # override
        return self.lado ** 2

class Circulo(Figura):
    def __init__(self, radio):
        self.radio = radio

    def area(self):                     # override
        return 3.14159 * self.radio ** 2

figuras = [Cuadrado(4), Circulo(3)]

for f in figuras:
    print(f.describir())     # el MISMO código, áreas distintas
# Soy una figura, mi área es 16
# Soy una figura, mi área es 28.27431
```

## Clases abstractas con `abc`

Una clase abstracta **no se puede instanciar**: sirve de contrato — sus subclases DEBEN implementar ciertos métodos.

```python
from abc import ABC, abstractmethod

# ABC = "Abstract Base Class": hay que heredar de ella
class Forma(ABC):

    @abstractmethod
    def area(self):
        """Cada subclase DEBE implementar area()."""
        pass

    @abstractmethod
    def perimetro(self):
        pass

    # Un método CONCRETO sí puede venir implementado
    def info(self):
        return f"Área: {self.area()}, perímetro: {self.perimetro()}"

class Circulo(Forma):
    def __init__(self, radio):
        self.radio = radio

    def area(self):                     # obligatorio (abstracto en el padre)
        return 3.14159 * self.radio ** 2

    def perimetro(self):                # obligatorio
        return 2 * 3.14159 * self.radio

class Rectangulo(Forma):
    def __init__(self, ancho, alto):
        self.ancho = ancho
        self.alto = alto

    def area(self):
        return self.ancho * self.alto

    def perimetro(self):
        return 2 * (self.ancho + self.alto)

c = Circulo(5)
print(c.info())         # Área: 78.53975, perímetro: 31.4159

# f = Forma()           ← TypeError: Can't instantiate abstract class
#                        (no implementa area/perimetro y además es abstracta)

# El contrato obliga:
class Mal(Forma):
    pass
# Mal()                ← TypeError: Can't instantiate abstract class ...
#                        con el detalle de los métodos faltantes
```

> [!note] ¿Para qué?
> Obligar a las subclases a tener ciertos métodos = errores tempranos y código predecible. Es el mismo contrato que las interfaces de Java (ver [[POO en Java]]).

## `@classmethod`

```python
# Recibe la CLASE (cls) como primer parámetro, no un objeto
class Producto:
    def __init__(self, nombre, precio):
        self.nombre = nombre
        self.precio = precio

    @classmethod
    def desde_json(cls, data):
        """Constructor alternativo: crea un Producto desde un dict."""
        return cls(data["nombre"], data["precio"])    # cls(...) = Producto(...)

p = Producto.desde_json({"nombre": "Mouse", "precio": 25000})
print(p.nombre, p.precio)    # Mouse 25000

# Otro uso clásico: contador/atributos de la clase
class Estudiante:
    total = 0

    def __init__(self, nombre):
        self.nombre = nombre
        Estudiante.total += 1

    @classmethod
    def cuantos_hay(cls):
        return cls.total        # cls = Estudiante

Estudiante("Ana")
Estudiante("Luis")
print(Estudiante.cuantos_hay()) # 2
```

## `@staticmethod`

```python
# Sin self ni cls: es una función que vive DENTRO de la clase (por organización)
class Validaciones:
    @staticmethod
    def es_email(texto):
        return "@" in texto and "." in texto.split("@")[-1]

    @staticmethod
    def solo_digitos(texto):
        return texto.isdigit()

print(Validaciones.es_email("a@b.com"))     # True
print(Validaciones.solo_digitos("123"))     # True

# No accede a la instancia ni a la clase: es una utilidad con namespace
```

## ¿Cuándo usar cada uno?

| Decorador | Recibe | Para qué |
|---|---|---|
| método normal | `self` (objeto) | comportamiento del objeto |
| `@classmethod` | `cls` (clase) | constructores alternativos, datos de la clase |
| `@staticmethod` | nada | utilidad relacionada lógicamente, sin estado |
| `@property` | `self` | atributo con lógica (ver [[Encapsulamiento en Python]]) |
| `@abstractmethod` | — | contrato obligatorio para hijas |

```python
# Combinación final típica
class Animal(ABC):
    @abstractmethod
    def hablar(self): ...

class Perro(Animal):
    def hablar(self):
        return "Guau"

    @staticmethod
    def es_domestico():
        return True

    @classmethod
    def especie(cls):
        return "Canino"

print(Perro().hablar())        # Guau     (instancia → polimorfismo)
print(Perro.es_domestico())    # True     (clase → utilidad)
print(Perro.especie())         # Canino   (clase → dato de la clase)
```

## Ver también

- [[Ruta Python]]
- [[Clases y objetos en Python]]
- [[Herencia en Python]]
- [[Encapsulamiento en Python]]
- [[Funciones en Python]]
- [[MOC-programacion]]

## Problema resuelto

**Enunciado:** Crear la clase abstracta `Animal` (con `hablar()` abstracto y un método concreto `presentarse()`), las hijas `Perro` y `Gato`, y recorrer una lista mostrando a todos con `presentarse()` (polimorfismo).

**Solución paso a paso:**
1. `Animal(ABC)` con `@abstractmethod hablar()`.
2. Hijas implementando `hablar()`.
3. `presentarse()` usa `self.hablar()` → cada hija responde distinto.

```python
from abc import ABC, abstractmethod

class Animal(ABC):
    def __init__(self, nombre):
        self.nombre = nombre

    @abstractmethod
    def hablar(self):
        pass

    def presentarse(self):
        return f"Soy {self.nombre} y digo: {self.hablar()}"

class Perro(Animal):
    def hablar(self):
        return "Guau 🐕"

class Gato(Animal):
    def hablar(self):
        return "Miau 🐈"

mascotas = [Perro("Rex"), Gato("Mishi")]
for m in mascotas:
    print(m.presentarse())
# Soy Rex y digo: Guau 🐕
# Soy Mishi y digo: Miau 🐈
```

## Problema propio

**Enunciado:** Crear la clase abstracta `Trabajador` con `tarea()` abstracto y `cobrar()` concreto (devuelve `f"{self.nombre} cobró $X"`), más3 subclases reales (Cocinero, Programador, Médico) con su propia `tarea()`. Recorrer y mostrar quién hace qué.

**Pista:** `class Cocinero(Trabajador):` con `def tarea(self): return "cocinar 🍳"`; en el bucle: `print(trabajador.tarea(), "-", trabajador.cobrar())`.
