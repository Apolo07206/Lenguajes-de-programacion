

## ¿Qué es una variable?

Un **espacio en memoria** donde guardas un valor para usarlo después.

---

## Las 2 formas modernas de declarar variables

||`let`|`const`|
|---|---|---|
|**¿Cambia?**|✅ SÍ|❌ NO|
|**Sintaxis**|`let edad = 25;`|`const PI = 3.14;`|
|**Valor inicial**|Opcional|Obligatorio|
|**Cuándo usar**|El valor va a cambiar|El valor es fijo|

---

## `let` → para valores que CAMBIAN

javascript

```javascript
let edad = 20;
edad = 21; // ✅ Permitido

let contador = 0;
contador = contador + 1; // ✅ Funciona
```

**Usa `let` para:** contadores, acumuladores, variables temporales.

---

## `const` → para valores FIJOS

javascript

```javascript
const nombre = "Juan";
nombre = "Pedro"; // ❌ ERROR

const PI = 3.1416;
const IVA = 0.21;
```

**Usa `const` para:** configuraciones, constantes, valores que no cambian.

---

## ⚠️ Caso especial: objetos y arrays

Con `const` **SÍ puedes modificar** el contenido, pero NO reasignar:

javascript

````javascript
const persona = { nombre: "Ana" };
persona.nombre = "Luis"; // ✅ Permitido (cambiar propiedad)
persona = {}; // ❌ ERROR (reasignar objeto)

const numeros = [1, 2, 3];
numeros.push(4); // ✅ Permitido (agregar elemento)
numeros = []; // ❌ ERROR (reasignar array)
```

---

## 🎯 Regla de oro
```
¿El valor va a cambiar?
├─ NO  → const (por defecto)
└─ SÍ  → let
````

**Siempre empieza con `const`.** Si necesitas cambiar el valor, cámbialo a `let`.

---

## ❌ Evita `var`

`var` es la forma antigua. **No la uses.**

---

## Ejemplo completo

javascript

```javascript
// Valores constantes
const PRECIO_BASE = 100;
const IVA = 0.21;

// Valores que cambian
let descuento = 0;
descuento = 10; // ✅ Cambia según promoción

let total = PRECIO_BASE - descuento;
total = total + (total * IVA);

console.log(total); // 96.9
```