---
title: Clases y objetos en Python
area: programacion
tipo: lenguaje
tags: [programacion, python, poo, clases]
created: 2026-10-12
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Encapsulamiento en Python]]", "[[Herencia en Python]]", "[[POO en Java]]"]
---

# Clases y objetos en Python

Una **clase** es el plano; un **objeto** es lo que construís con ese plano. La clase agrupa datos (atributos) y comportamiento (métodos) en un solo lugar. Se define con `class`.

## Primera clase

```python
class Perro:
    # Atributos de CLASE (compartidos por todos los objetos)
    especie = "Canino"

    # __init__ = constructor: se ejecuta al crear cada objeto
    def __init__(self, nombre, edad):
        # Atributos de INSTANCIA: cada objeto tiene los suyos
        self.nombre = nombre     # 'self' = el objeto actual (como 'this' en Java)
        self.edad = edad

    # Método de instancia (recibe self automáticamente)
    def ladrar(self):
        print(f"{self.nombre}: ¡Guau!")

    def presentarse(self):
        print(f"Soy {self.nombre}, tengo {self.edad} años y soy {self.especie}")

# CREAR OBJETOS (instanciar la clase)
mi_perro = Perro("Rex", 5)          # llama a __init__ con los argumentos
otro_perro = Perro("Luna", 2)

mi_perro.ladrar()                   # Rex: ¡Guau!
otro_perro.presentarse()            # Soy Luna, tengo 2 años y soy Canino

print(otro_perro.nombre)            # Luna  → acceso a atributo
print(Perro.especie)                # Canino → atributo de clase
```

## `__init__` y `self` sin misterio

```python
class Cuenta:
    def __init__(self, titular, saldo=0):
        self.titular = titular      # self.ALGO = atributo del objeto
        self.saldo = saldo

    def depositar(self, monto):
        self.saldo += monto         # self.saldo accede al atributo

    def mostrar(self):
        print(f"{self.titular}: ${self.saldo}")

cuenta1 = Cuenta("Ferney", 100)     # saldo con default 0 si no lo pasás
cuenta2 = Cuenta("Ana")             # saldo = 0

cuenta1.depositar(50)
cuenta1.mostrar()                   # Ferney: $150
cuenta2.mostrar()                   # Ana: $0

# ¿Por qué 'self' explícito? Python lo pide a propósito: es claro qué
# pertenece al objeto. Cuando llamás cuenta1.depositar(50), Python convierte
# eso en Cuenta.depositar(cuenta1, 50) — el self es cuenta1.
```

## Métodos que "hablan" del objeto

```python
class Rectangulo:
    def __init__(self, ancho, alto):
        self.ancho = ancho
        self.alto = alto

    def area(self):
        return self.ancho * self.alto

    def perimetro(self):
        return 2 * (self.ancho + self.alto)

    def es_cuadrado(self):
        return self.ancho == self.alto

r1 = Rectangulo(5, 3)
print(r1.area())            # 15
print(r1.perimetro())       # 16
print(r1.es_cuadrado())     # False

# Un objeto puede guardarse en variables, listas, etc.
rectangulos = [Rectangulo(2, 2), Rectangulo(4, 1)]
for r in rectangulos:
    print(f"{r.ancho}x{r.alto} = {r.area()}")
```

## Atributos de clase vs de instancia

```python
class Animal:
    kingdom = "Animalia"                # DE CLASE: uno solo, compartido

    def __init__(self, nombre):
        self.nombre = nombre            # DE INSTANCIA: uno por objeto

    def hablar(self):
        pass

gato = Animal("Michi")
perro = Animal("Rex")

print(gato.kingdom)         # Animalia  (vive en la clase)
print(perro.kingdom)        # Animalia  (mismo para todos)

gato.kingdom = "Domesticus" # ⚠️ esto NO cambia la clase: crea un atributo
print(Animal.kingdom)       # Animalia  (la clase intacta)
print(gato.kingdom)         # Domesticus (solo el gato)

# Contador de objetos: uso clásico de atributo de clase
class Usuario:
    total = 0                       # contador compartido

    def __init__(self, nombre):
        self.nombre = nombre
        Usuario.total += 1          # incrementa el de CLASE

Usuario("Ana")
Usuario("Luis")
print(Usuario.total)                # 2
```

## `__str__` — cómo se ve el objeto

```python
class Persona:
    def __init__(self, nombre, edad):
        self.nombre = nombre
        self.edad = edad

    def __str__(self):
        # lo que se muestra al hacer print(objeto) o str(objeto)
        return f"Persona({self.nombre}, {self.edad} años)"

p = Persona("Ferney", 20)

print(p)                # Persona(Ferney, 20 años)   ← gracias a __str__
print(str(p))           # lo mismo

# Sin __str__ verías algo feo: <__main__.Persona object at 0x7f...>
```

> [!tip] `__str__` vs `__repr__`
> `__str__` es para humanos (print); `__repr__` para desarrolladores/debug. Empezá con `__str__`; si querés más, agregá `__repr__` devolviendo lo mismo.

## Métodos útiles del día a día

```python
class Punto:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def distancia_al_origen(self):
        return (self.x ** 2 + self.y ** 2) ** 0.5

    def mover(self, dx, dy):
        self.x += dx               # modifica el propio objeto
        self.y += dy
        return self                # devuelve self para encadenar

p = Punto(3, 4)
print(p.distancia_al_origen())     # 5.0
p.mover(1, 1).mover(2, 0)          # encadenado
print(p)                           # Punto en (6, 5) si tenés __str__
```

> [!note] Comparación con Java
> En Java todo va dentro de `class X { }` con `this.` explícito. En Python: `self` como primer parámetro manual, `__init__` como constructor y `__str__` como `toString()` (ver [[POO en Java]] para el lado Java).

## Ver también

- [[Ruta Python]]
- [[Encapsulamiento en Python]]
- [[Herencia en Python]]
- [[Polimorfismo y abstracción en Python]]
- [[POO en Java]]
- [[MOC-programacion]]

## Problema resuelto

**Enunciado:** Crear la clase `Cancion` (título, artista, duración en segundos) con un método `mostrar_ficha()` y otro `es_larga()` (mayor a 200 segundos). Crear2 canciones y comparar.

**Solución paso a paso:**
1. `__init__` con los3 atributos.
2. `mostrar_ficha()` con f-string.
3. `es_larga()` devuelve bool.

```python
class Cancion:
    def __init__(self, titulo, artista, duracion):
        self.titulo = titulo
        self.artista = artista
        self.duracion = duracion      # en segundos

    def mostrar_ficha(self):
        minutos = self.duracion // 60
        segundos = self.duracion % 60
        print(f'"{self.titulo}" - {self.artista} ({minutos}:{segundos:02d})')

    def es_larga(self):
        return self.duracion > 200

c1 = Cancion("Bohemian", "Queen", 354)
c2 = Cancion("Song 2", "Blur", 122)

c1.mostrar_ficha()        # "Bohemian" - Queen (5:54)
c2.mostrar_ficha()        # "Song 2" - Blur (2:02)
print(c1.es_larga())      # True
print(c2.es_larga())      # False
```

## Problema propio

**Enunciado:** Crear la clase `Estudiante` (nombre, lista de notas) con: `promedio()`, `aprobo()` (≥3.0) y `__str__` que muestre nombre y estado. Crear2 estudiantes de prueba.

**Pista:** Para el promedio usá `sum(self.notas) / len(self.notas)` (ver [[Algoritmos con listas en Python]]); `aprobo()` puede llamar a `self.promedio()`.
