---
title: Funciones integradas en Java
area: programacion
tipo: lenguaje
tags: [programacion, java, strings, metodos]
created: 2026-09-20
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Java]]", "[[Operadores en Java]]"]
---

## 2. Métodos de Cadenas (Strings)

|**Método**|**Descripción**|**Ejemplo**|
|---|---|---|
|`equals()`|Compara si dos cadenas son exactamente iguales.|`"hola".equals("hola")`|
|`length()`|Devuelve la longitud (número de caracteres) de la cadena.|`"hola".length()` $\rightarrow$ `4`|
|`charAt()`|Devuelve el carácter en un índice específico (empieza en 0).|`"hola".charAt(0)` $\rightarrow$ `'h'`|
|`substring()`|Extrae una parte de la cadena entre dos índices.|`"hola".substring(0, 2)` $\rightarrow$ `"ho"`|
|`toUpperCase()`|Convierte todo el texto a mayúsculas.|`"hola".toUpperCase()` $\rightarrow$ `"HOLA"`|
|`toLowerCase()`|Convierte todo el texto a minúsculas.|`"HOLA".toLowerCase()` $\rightarrow$ `"hola"`|
|`contains()`|Verifica si la cadena contiene una secuencia de texto.|`"hola".contains("ol")` $\rightarrow$ `true`|
|`trim()`|Elimina los espacios en blanco al inicio y al final.|`" hola ".trim()` $\rightarrow$ `"hola"`|
|`replace()`|Reemplaza un carácter o texto por otro.|`"hola".replace("o", "a")` $\rightarrow$ `"hala"`|
|`format()`|Devuelve una cadena formateada según especificadores.|`String.format("Hola %s", "Juan")`|

## Ejemplo Completo de Código

Java

```
public class Ejemplo {
    public static void main(String[] args) {
        String texto = "  Hola Mundo  ";
        
        // Limpieza y transformación
        String limpio = texto.trim();
        String mayus = limpio.toUpperCase();
        
        // Métodos de consulta
        int longitud = limpio.length();
        boolean tieneHola = limpio.contains("Hola");
        char primeraLetra = limpio.charAt(0);
        
        // Salida formateada
        System.out.println(String.format("Texto: %s | Longitud: %d", mayus, longitud));
    }
}
```


## Operadores Unitarios  / 

Son operadores que actúan sobre un solo operando.

### Pre-incremento

````

- **Positivo unario (`+`)**: Intenta convertir el valor en número sin cambiar su signo.
- **Negación numérica (`-`)**: Cambia el signo del valor numérico.
- **Incremento (`++`)**: Aumenta el valor en 1.
- **Decremento (`--`)**: Disminuye el valor en 1.
- **Negación lógica (`!`)**: Invierte el valor booleano (`true` pasa a `false`).

```java
int x = 5;
x++; // Incremento (x pasa a ser 6)
boolean flag = !true; // Negación lógica (flag es false)

````

estos cambian su orden de ejecucion dependiendo la posicion del operador matematico 
cuando el operado va antes y despues la vaible 

ejemplo :

```
var numero = 2 

System.out.println(  ++numero )

salida  3
```

primero se ejecuta el incremento o el operador y despues se muestra en pantalla , pero si el operador matematico va despues primero se ejecuta motrar el numero y despues se incrementa. 

### Post-incremento

ejemplo:

```
var numero = 6 

System.out.printl(  numero++  )

salida 6

System.out.println(numero)

salida 7

```

## Ver también

- [[Ruta Java]]
- [[Operadores en Java]]
- [[MOC-programacion]]
