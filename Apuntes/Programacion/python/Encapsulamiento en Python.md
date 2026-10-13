---
title: Encapsulamiento en Python
area: programacion
tipo: lenguaje
tags: [programacion, python, poo, encapsulamiento]
created: 2026-10-12
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Python]]", "[[Clases y objetos en Python]]", "[[Herencia en Python]]", "[[POO en Java]]"]
---

# Encapsulamiento en Python

Encapsular es esconder los detalles internos de una clase y exponer solo lo necesario. Python NO tiene `private` real: usa **convenciones de nombres** (`_` y `__`) y el mecanismo formal `@property`.

## Público / protegido / privado

```python
class Cuenta:
    def __init__(self, titular, saldo):
        self.titular = titular        # PÚBLICO: cualquiera lo toca
        self._saldo = saldo           # PROTEGIDO (convención _): "no lo toques"
        self.__pin = "1234"           # PRIVADO (name mangling __): muy escondido

c = Cuenta("Ferney", 100)

print(c.titular)        # Ferney     → público: OK
print(c._saldo)         # 100        → "protegido": Python NO te impide leerlo
# print(c.__pin)        → AttributeError: no existe (así se ve desde fuera)

# El truco de Python: se ACCEDERÍA con el nombre "cambiado"
print(c._Cuenta__pin)   # 1234  → name mangling: NO es seguridad real, solo avisa
```

| Prefijo | Nombre | Intención |
|---|---|---|
| `self.x` | público | cualquiera lo lee/escribe |
| `self._x` | protegido | convención: "uso interno / subclases, no tocar" |
| `self.__x` | privado (mangled) | "es detalle interno"; difícil acceder por fuera |

> [!warning] No es seguridad
> En Python el encapsulamiento es **un acuerdo entre programadores**, no una barrera. `__pin` se puede leer igual con `_Cuenta__pin`. La fuerza real está en exponer métodos/properties y no atajos.

## Getters y setters (forma clásica)

```python
class Temperatura:
    def __init__(self, celsius):
        self.set_celsius(celsius)     # validamos desde el principio

    def get_celsius(self):
        return self._celsius

    def set_celsius(self, valor):
        if valor < -273.15:
            raise ValueError("Por debajo del cero absoluto")   # ver [[Manejo de errores en Python]]
        self._celsius = valor

    def get_fahrenheit(self):
        return self._celsius * 9/5 + 32

t = Temperatura(25)
print(t.get_fahrenheit())      # 77.0
t.set_celsius(100)
# t.set_celsius(-300)          ← ValueError: validado
```

## `@property` — la forma PYTHONICA

```python
class Circulo:
    def __init__(self, radio):
        self._radio = radio            # atributo "protegido" por convención

    @property
    def radio(self):
        """Getter: se lee como atributo, vive como método."""
        return self._radio

    @radio.setter
    def radio(self, valor):
        """Setter: valida antes de guardar."""
        if valor <= 0:
            raise ValueError("El radio debe ser positivo")
        self._radio = valor

    @property
    def area(self):
        """Atributo calculado: se LEE sin paréntesis."""
        return 3.14159 * self._radio ** 2

c = Circulo(5)

print(c.radio)            # 5        ← parece atributo, es el getter
c.radio = 10              # parece asignación, es el setter (con validación)
print(c.area)             # 314.159  ← sin paréntesis: parece atributo
# c.radio = -1            ← ValueError: el setter lo frena
# c.area = 99             ← AttributeError: no hay setter (no se puede asignar)
```

```python
# Equivalencia para que quede claro:
objeto.radio        ==  objeto.get_radio()
objeto.radio = x    ==  objeto.set_radio(x)
objeto.area         ==  objeto.get_area()    (pero sin escribir paréntesis)
```

## Encapsular la lógica interna completa

```python
# Lo ideal: el usuario de la clase NUNCA toca atributos directamente
class Billetera:
    def __init__(self, titular, saldo=0):
        self.titular = titular        # público: no necesita protección
        self.__saldo = saldo          # privado: solo los métodos lo tocan

    @property
    def saldo(self):                  # solo LECTURA (no hay setter)
        return self.__saldo

    def depositar(self, monto):
        if monto <= 0:
            raise ValueError("El depósito debe ser positivo")
        self.__saldo += monto

    def retirar(self, monto):
        if monto > self.__saldo:
            raise ValueError("Fondos insuficientes")
        self.__saldo -= monto

b = Billetera("Ferney", 100)
b.depositar(50)
b.retirar(30)
print(b.saldo)           # 120
# b.saldo = 999999       ← AttributeError: no hay setter (protegido ✅)
```

## Para qué sirve (más allá de la nota)

- **Validar** los datos antes de que entren (setters).
- **Ocultar** cómo funciona por dentro: si mañana cambiás `_saldo` por un dict, ningún código externo se rompe porque nadie tocaba el atributo directamente.
- **Interfaz clara**: los métodos documentan lo que la clase SÍ permite hacer.

> [!note] Comparación con Java
> Java tiene `private` de verdad (el compilador lo bloquea). Python simula con `__` y convence con `@property` (ver [[POO en Java]] para el lado Java).

## Ver también

- [[Ruta Python]]
- [[Clases y objetos en Python]]
- [[Herencia en Python]]
- [[Polimorfismo y abstracción en Python]]
- [[MOC-programacion]]

## Problema resuelto

**Enunciado:** Crear la clase `Banco` con saldo privado, `@property` de solo lectura, y métodos `depositar`/`retirar` que validen montos positivos y fondos suficientes.

**Solución paso a paso:**
1. `__saldo` privado en `__init__`.
2. `@property saldo` sin setter (solo lectura).
3. Métodos con validación (`raise ValueError`).

```python
class Banco:
    def __init__(self, titular, saldo=0):
        self.titular = titular
        self.__saldo = saldo

    @property
    def saldo(self):
        return self.__saldo

    def depositar(self, monto):
        if monto <= 0:
            raise ValueError("Monto inválido")
        self.__saldo += monto

    def retirar(self, monto):
        if monto <= 0 or monto > self.__saldo:
            raise ValueError("Fondos insuficientes")
        self.__saldo -= monto

cuenta = Banco("Ferney", 200)
cuenta.depositar(100)
cuenta.retirar(50)
print(cuenta.saldo)         # 250
# cuenta.saldo = 1000       ← AttributeError: no hay setter
```

## Problema propio

**Enunciado:** Crear la clase `Password` que guarde la contraseña privada, permita cambiarla **solo** si la nueva tiene8+ caracteres, y exponga un `@property es_fuerte` que diga si tiene8+ caracteres.

**Pista:** Setter con `if len(valor) < 8: raise ValueError(...)`; `es_fuerte` es un `@property` que devuelve `len(self.__password) >= 8` (sin setter).
