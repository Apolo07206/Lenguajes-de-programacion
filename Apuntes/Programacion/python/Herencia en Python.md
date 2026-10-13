---
title: Herencia en Python
area: programacion
tipo: lenguaje
tags: [programacion, python, poo, herencia]
created: 2026-10-12
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Clases y objetos en Python]]", "[[Polimorfismo y abstracción en Python]]", "[[POO en Java]]"]
---

# Herencia en Python

La herencia hace que una **clase hija** herede atributos y métodos de una **clase padre**, y pueda agregar los suyos o modificarlos. Se escribe `class Hija(Padre):`.

## Herencia básica

```python
class Animal:
    def __init__(self, nombre, sonido):
        self.nombre = nombre
        self.sonido = sonido

    def hablar(self):
        print(f"{self.nombre} hace {self.sonido}")

    def describir(self):
        print(f"Soy {self.nombre}, un animal")

# Perro HEREDA de Animal (recibe todo lo de Animal)
class Perro(Animal):
    def traer_pelota(self):         # método NUEVO solo de Perro
        print(f"{self.nombre} trae la pelota 🎾")

class Gato(Animal):
    def ronronear(self):            # método NUEVO solo de Gato
        print(f"{self.nombre} ronronea... 😺")

perro = Perro("Rex", "guau")
gato = Gato("Mishi", "miau")

perro.hablar()              # Rex hace guau     ← heredado
perro.describir()           # Soy Rex, un animal ← heredado
perro.traer_pelota()        # Rex trae la pelota 🎾 ← propio

gato.ronronear()            # Mishi ronronea... 😺
# gato.traer_pelota()       ← AttributeError: eso es de Perro
```

## `super()` — llamar al padre

```python
class Animal:
    def __init__(self, nombre):
        self.nombre = nombre
        print("Animal.__init__ se ejecutó")

class Perro(Animal):
    def __init__(self, nombre, raza):
        super().__init__(nombre)      # PRIMERO: inicializar la parte del padre
        self.raza = raza              # después: lo propio de la hija
        print("Perro.__init__ se ejecutó")

    def ladrar(self):
        print(f"{self.nombre}: ¡Guau!")

p = Perro("Rex", "Labrador")
# Animal.__init__ se ejecutó
# Perro.__init__ se ejecutó
print(p.nombre, p.raza)    # Rex Labrador  ← atributos de padre + hija
p.ladrar()                 # Rex: ¡Guau!

# super() también sirve para REUTILIZAR un método del padre
class Animal:
    def __init__(self, nombre):
        self.nombre = nombre

    def presentarse(self):
        return f"Soy {self.nombre}"

class Perro(Animal):
    def presentarse(self):
        base = super().presentarse()          # reutilizo la frase del padre
        return base + " y soy un perro 🐕"

print(Perro("Rex").presentarse())   # Soy Rex y soy un perro 🐕
```

## Sobrescritura de métodos (override)

```python
class Animal:
    def hablar(self):
        print("...")

class Pato(Animal):
    def hablar(self):                # SOBRESCRIBO: misma firma, otro comportamiento
        print("Cuac cuac 🦆")

class Vaca(Animal):
    def hablar(self):
        print("Muu 🐄")

# Sin override explícito, el método del padre se hereda tal cual.
# Con override, gana el de la hija.
for a in [Pato("Donald"), Vca("Lola") if False else Vaca("Lola")]:
    a.hablar()          # Cuac cuac 🦆 / Muu 🐄
```

## Herencia múltiple

```python
# Python SÍ permite heredar de varias clases (Java no, usa interfaces)
class Volador:
    def volar(self):
        print("Volando... 🕊️")

class Nadador:
    def nadar(self):
        print("Nadando... 🐬")

# Pato hereda de las DOS
class Pato(Volador, Nadador):
    def hablar(self):
        print("Cuac 🦆")

pato = Pato()
pato.volar()            # Volando... 🕊️
pato.nadar()            # Nadando... 🐬
pato.hablar()           # Cuac 🦆
```

```python
# Orden MRO (Method Resolution Order): Python resuelve de izquierda a derecha
class A:
    def saludar(self):
        print("Soy A")

class B(A):
    def saludar(self):
        print("Soy B")

class C(A):
    def saludar(self):
        print("Soy C")

class D(B, C):          # hereda de B y C (que ambos vienen de A)
    pass

D().saludar()           # Soy B  (gana B: primero en la lista de padres)
print(D.__mro__)        # el orden real de búsqueda: D → B → C → A → object
print(D.mro())
```

> [!warning] Herencia múltiple = cuidado
> Dos padres con métodos del mismo nombre = el orden (MRO) decide. Conocelo con `Clase.mro()` y mantené las jerarquías simples.

## isinstance y issubclass

```python
class Animal:
    pass

class Perro(Animal):
    pass

p = Perro()

print(isinstance(p, Perro))         # True  → es de Perro
print(isinstance(p, Animal))        # True  → TAMBIÉN es de Animal (herencia)
print(isinstance(p, object))        # True  → todo hereda de object

print(issubclass(Perro, Animal))    # True  → Perro es subclase de Animal
print(issubclass(Animal, Perro))    # False

# Muy usado para validar antes de llamar métodos específicos
if isinstance(p, Perro):
    p.wacha()       # solo llamo el método si el objeto es de ese tipo
```

> [!note] Comparación con Java
> Java: `class Perro extends Animal`, y la herencia múltiple de clases está prohibida. Python: `class Perro(Animal)` y la múltiple funciona con MRO (ver [[POO en Java]]).

## Ver también

- [[Ruta Python]]
- [[Clases y objetos en Python]]
- [[Encapsulamiento en Python]]
- [[Polimorfismo y abstracción en Python]]
- [[MOC-programacion]]

## Problema resuelto

**Enunciado:** Crear `Vehiculo` (marca, velocidad) con `describir()`, y la hija `Auto` que agrega `puertas` y sobreescribe `describir()` usando `super()`.

**Solución paso a paso:**
1. Padre con `__init__` y `describir()`.
2. Hija: `super().__init__(...)` primero, luego sus atributos.
3. Override que reutiliza la frase del padre con `super().describir()`.

```python
class Vehiculo:
    def __init__(self, marca, velocidad):
        self.marca = marca
        self.velocidad = velocidad

    def describir(self):
        return f"{self.marca} a {self.velocidad} km/h"

class Auto(Vehiculo):
    def __init__(self, marca, velocidad, puertas):
        super().__init__(marca, velocidad)     # parte del padre
        self.puertas = puertas                 # parte propia

    def describir(self):
        return super().describir() + f", {self.puertas} puertas"

a = Auto("Mazda", 180, 4)
print(a.describir())    # Mazda a 180 km/h, 4 puertas
```

## Problema propio

**Enunciado:** Crear la clase base `Figura` con `nombre` y un método `area()` que devuelva 0, y las hijas `Cuadrado` (lado) y `Circulo` (radio) que sobreescriban `area()` con sus fórmulas.

**Pista:** En cada hija: `super().__init__("Cuadrado")` en `__init__`, y `def area(self): return self.lado ** 2`. (Esto prepara el terreno para [[Polimorfismo y abstracción en Python]].)
