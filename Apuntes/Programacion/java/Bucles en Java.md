---
title: Bucles en Java
area: programacion
tipo: lenguaje
tags: [programacion, java, bucles, for, while]
created: 2026-09-20
updated: 2026-10-12
related: ["[[MOC-programacion]]", "[[Ruta Java]]", "[[Estructuras de control en Java]]"]
---

### Bucle `for`

Se usa cuando **sabes exactamente cuántas veces** quieres repetir un bloque de código.

Tiene 3 partes clave:

1. **Inicialización:** `int i = 0` _(dónde empieza)_
    
2. **Condición:** `i < 5` _(mientras sea cierto, continúa)_
    
3. **Actualización:** `i++` _(cómo avanza en cada vuelta)_
    

```
for (int i = 0; i < 5; i++) {
    System.out.println("Número: " + i);
}
// Imprime: 0, 1, 2, 3, 4
```

###  Bucle `while`

Se usa cuando **no sabes cuántas veces** se repetirá la tarea, pero conoces la condición para continuar. Evalúa la condición **antes** de ejecutar el código.

```
int contador = 3;

while (contador > 0) {
    System.out.println("Cuenta regresiva: " + contador);
    contador--; // Crucial para evitar un bucle infinito
}
```

### Bucle `do-while`

Es idéntico a `while`, con la única diferencia de que evalúa la condición **después** de ejecutar el código. Por tanto, **siempre se ejecuta al menos una vez**.

```
int opcion = 0;

do {
    System.out.println("Mostrando menú...");
} while (opcion != 0); // Se imprime una vez aunque opcion sea 0
```


 ## Ejercicio  practico 
 
```
import java.util.Scanner;

public class practica {

public static void main(String[] args) {

int intentos=5;

Scanner scanner = new Scanner(System.in);


String password1 = "Ferney0708";

String user1 = "Ferney";

  

String password2 ;

String user2;

  

do{

System.out.println("Ingrese su usuario :");

user2= scanner.nextLine();

  

System.out.println("Ingrese su contraseña :");

password2=scanner.nextLine();

  

if (user2.equals(user1) && password2.equals(password1)){

System.out.println("usuario y contraseña correctos ");

break;

}

else{

intentos --;

System.out.println("Intento fallido ");

}

  

}while(intentos>0);

  

if (intentos==0){

System.out.println("numero de intentos acabados ");

}

  

scanner.close();

}

}
``` 
## Ver también

- [[Ruta Java]]
- [[Estructuras de control en Java]]
- [[Arrays en Java]]
- [[MOC-programacion]]
