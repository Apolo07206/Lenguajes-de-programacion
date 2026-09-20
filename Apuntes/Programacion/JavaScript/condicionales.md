---
title: condicionales
area: programacion
tipo: lenguaje
tags: [programacion, javascript]
created: 2026-01-18
updated: 2026-09-20
related: ["[[MOC-programacion]]", "[[Ruta JavaScript]]"]
---

# ejemplos 

if (condición) {
  // código si condición es true
}


if (condición) {
  // true
} else {
  // false
}


if (condición1) {
} else if (condición2) {
} else {
}



# operadores 

| Operador | Significado                  |
| -------- | ---------------------------- |
| `===`    | Igual valor y tipo           |
| `==`     | Igual con conversión         |
| `!==`    | Diferente valor o tipo       |
| `>`      | Mayor que                    |
| `<`      | Menor que                    |
| `>=`     | Mayor o igual                |
| `<=`     | Menor o igual                |
| Operador | Significado                  |
| `&&`     | AND (ambas condiciones true) |
| `\|`     | OR (al menos una true)       |
| `!`      | NOT (invierte el valor)      |


#  Igualdad (`==`) vs Igualdad estricta (`===`)**


### **`==` Igualdad floja**

- Compara valores
    
- Si los tipos no coinciden, intenta convertirlos antes de comparar (type coercion)
    
- Puede producir resultados inesperados
    

**Ejemplos:**

`0 == "0"        // true 0 == false      // true "5" == 5        // true null == undefined  // true`

---

### **`===` Igualdad estricta**

- Compara valor y tipo
    
- No realiza conversiones
    
- Resultado más predecible
    

**Ejemplos:**

`0 === "0"          // false 0 === false        // false "5" === 5          // false null === undefined // false`



## Ver también

- [[MOC-programacion]]
- [[Ruta JavaScript]]
