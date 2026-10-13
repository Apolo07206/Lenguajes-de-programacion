---
title: Operadores en Java
area: programacion
tipo: lenguaje
tags: [programacion, java, operadores, aritmeticos, logicos]
created: 2026-09-20
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Java]]", "[[Funciones integradas en Java]]"]
---

Los operadores son símbolos especiales que le dicen al compilador de Java que realice operaciones matemáticas, lógicas o de manipulación de datos sobre una o más variables (operandos).

---

## 1. Operadores Aritméticos
Se utilizan para realizar operaciones matemáticas básicas.

| Operador | Nombre | Descripción | Ejemplo (`int a = 10, b = 3`) |
| :---: | :--- | :--- | :--- |
| `+` | Suma | Suma dos valores | `a + b` $\rightarrow$ `13` |
| `-` | Resta | Resta el segundo valor del primero | `a - b` $\rightarrow$ `7` |
| `*` | Multiplicación | Multiplica dos valores | `a * b` $\rightarrow$ `30` |
| `/` | División | Divide el primer valor por el segundo (entera si ambos son int) | `a / b` $\rightarrow$ `3` |
| `%` | Módulo | Devuelve el residuo de la división | `a % b` $\rightarrow$ `1` |

> [!NOTE]
> El operador `+` también sirve para **concatenar cadenas de texto**:
> `String saludo = "Hola " + "Mundo";`

---

## 2. Operadores de Asignación
Asignan valores a las variables. Los operadores compuestos realizan una operación matemática y la asignación en un solo paso.

| Operador | Ejemplo | Equivalente a |
| :---: | :--- | :--- |
| `=` | `x = 5` | `x = 5` |
| `+=` | `x += 3` | `x = x + 3` |
| `-=` | `x -= 3` | `x = x - 3` |
| `*=` | `x *= 3` | `x = x * 3` |
| `/=` | `x /= 3` | `x = x / 3` |
| `%=` | `x %= 3` | `x = x % 3` |

---

## 3. Operadores Incrementales y Decrementales
Aumentan o disminuyen el valor de una variable en 1.

| Operador | Tipo | Descripción | Ejemplo (`int x = 5`) |
| :---: | :--- | :--- | :--- |
| `++` | Post-incremento | Usa el valor original y luego incrementa | `y = x++;` $\rightarrow$ `y = 5`, `x = 6` |
| `++` | Pre-incremento | Incrementa primero y luego usa el valor | `y = ++x;` $\rightarrow$ `y = 6`, `x = 6` |
| `--` | Post-decremento | Usa el valor original y luego decrementa | `y = x--;` $\rightarrow$ `y = 5`, `x = 4` |
| `--` | Pre-decremento | Decrementa primero y luego usa el valor | `y = --x;` $\rightarrow$ `y = 4`, `x = 4` |

---

## 4. Operadores Relacionales (Comparación)
Comparan dos valores y siempre devuelven un valor booleano (`true` o `false`).

| Operador | Nombre | Ejemplo (`int a = 10, b = 5`) | Resultado |
| :---: | :--- | :--- | :---: |
| `==` | Igual a | `a == b` | `false` |
| `!=` | Diferente de | `a != b` | `true` |
| `>` | Mayor que | `a > b` | `true` |
| `<` | Menor que | `a < b` | `false` |
| `>=` | Mayor o igual que | `a >= 10` | `true` |
| `<=` | Menor o igual que | `b <= 5` | `true` |

---

## 5. Operadores Lógicos
Se utilizan para combinar expresiones booleanas.

| Operador | Nombre | Descripción | Ejemplo |
| :---: | :--- | :--- | :--- |
| `&&` | AND (Y lógico) | Devuelve `true` si **ambas** expresiones son verdaderas. | `(5 > 3 && 8 > 2)` $\rightarrow$ `true` |
| `\|\|` | OR (O lógico) | Devuelve `true` si **al menos una** expresión es verdadera. | `(5 > 3 \|\| 2 > 8)` $\rightarrow$ `true` |
| `!` | NOT (Negación) | Invierte el valor booleano (de `true` a `false` o viceversa). | `!(5 > 3)` $\rightarrow$ `false` |

> [!TIP]
> Los operadores `&&` y `||` son de **evaluación de cortocircuito**: si el primer operando determina el resultado final, el segundo no se evalúa.

---

## 6. Operador Ternario
Es una forma reducida de escribir un condicional `if / else`. Devuelve un valor dependiendo de la condición.

**Sintaxis:**
`variable = (condicion) ? valor_si_true : valor_si_false;`

```java
int edad = 20;
String resultado = (edad >= 18) ? "Mayor de edad" : "Menor de edad";
System.out.println(resultado); // Imprime: Mayor de edad
````

## 7. Operadores Bit a Bit (Bitwise)

Operan a nivel de bits individuales sobre enteros (`byte`, `short`, `int`, `long`).

|**Operador**|**Nombre**|**Descripción**|
|---|---|---|
|`&`|AND a nivel de bits|Compara bit a bit; resulta `1` si ambos bits son `1`.|
|`\|`|OR a nivel de bits|Compara bit a bit; resulta `1` si al menos un bit es `1`.|
|`^`|XOR a nivel de bits|Compara bit a bit; resulta `1` si los bits son diferentes.|
|`~`|NOT / Complemento|Invierte todos los bits (`0` pasa a `1`, `1` pasa a `0`).|
|`<<`|Desplazamiento izquierda|Desplaza los bits a la izquierda, rellenando con ceros.|
|`>>`|Desplazamiento derecha|Desplaza los bits a la derecha, preservando el signo.|
|`>>>`|Desplazamiento sin signo|Desplaza los bits a la derecha, rellenando con ceros.|
## Ver también

- [[Ruta Java]]
- [[Funciones integradas en Java]]
- [[Estructuras de control en Java]]
- [[MOC-programacion]]
