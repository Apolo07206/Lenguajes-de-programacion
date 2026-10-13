---
title: conseptos basicos de Js
area: programacion
tipo: lenguaje
tags: [programacion, javascript]
created: 2026-01-17
updated: 2026-09-20
related: ["[[MOC-programacion]]", "[[Ruta JavaScript]]"]
---

## 1️⃣ Tres formas de aplicar JavaScript en HTML

### **A. JavaScript interno (dentro del HTML)**

Se escribe directamente entre las etiquetas `<script>` y `</script>`.

**Ejemplo:**

html

```html
<script>
  alert("Hola desde dentro del HTML");
</script>
```

**Cuándo usarlo:** Para scripts muy cortos o pruebas rápidas.

---

### **B. JavaScript en línea (atributos de eventos)**

Se ejecuta directamente desde un atributo HTML como `onclick`, `onload`, etc.

**Ejemplo:**

html

```html
<button onclick="alert('Funciona')">Click aquí</button>
```

**⚠️ No recomendado:** Mezcla HTML con lógica, dificulta el mantenimiento.

---

### **C. JavaScript externo (archivo `.js` separado)** ✅ **RECOMENDADO**

Se vincula un archivo externo usando el atributo `src` en la etiqueta `<script>`.

**Ejemplo:**

html

```html
<script src="app.js"></script>
```

**Ventajas:**

- Separa estructura (HTML) de lógica (JavaScript)
- Código más limpio y reutilizable
- Facilita el mantenimiento

---

## 2️⃣ ¿Para qué sirve `alert()`?

`alert()` es una función nativa del navegador que muestra una **ventana emergente** con un mensaje.

### **Características:**

- Bloquea la ejecución del código hasta que el usuario cierre la ventana
- Muestra solo texto (no permite formateo HTML)

### **Usos comunes:**

- ✅ Comprobar si JavaScript está funcionando
- ✅ Mostrar valores de variables mientras pruebas
- ✅ Depuración rápida en aprendizaje

**Ejemplo:**

javascript

```javascript
alert("¡Hola! JavaScript está funcionando");
```

---

### **Alternativa moderna: `console.log()`** 🔥

Hoy se prefiere usar `console.log()` para depuración porque:

- No interrumpe la experiencia del usuario
- Muestra información en la consola del navegador (F12)
- Permite ver objetos, arrays y estructuras complejas

**Ejemplo:**

javascript

````
console.log("Funcionando sin molestar al usuario");
```

---

##  Cómo usar JavaScript externo con `src=""`

### **Estructura básica del proyecto:**
```
project/
 ├── index.html
 └── main.js
````

---

### **index.html**

html

```html
<!DOCTYPE html>
<html>
<head>
  <title>Ejemplo</title>
</head>
<body>
  <h1>Hola mundo</h1>
  <script src="main.js"></script>
</body>
</html>
```

---

### **main.js**

javascript

```javascript
alert("Hola desde archivo externo");
```

---

## 📌 Detalles importantes

### **1. Tipos de rutas en `src`:**

|Tipo|Ejemplo|Descripción|
|---|---|---|
|**Relativa**|`src="main.js"`|Archivo en la misma carpeta|
|**Relativa con carpeta**|`src="./scripts/app.js"`|Archivo en subcarpeta|
|**Absoluta (URL)**|`src="https://cdn.com/lib.js"`|Archivo en servidor externo|

---

### **2. ⚠️ Regla importante:**

Si usas `src=""` en un `<script>`, **NO** puedes escribir código JavaScript dentro de ese mismo tag.

**❌ Incorrecto:**

html

```html
<script src="main.js">
  alert("Esto NO funciona");
</script>
```

**✅ Correcto:**

html

```html
<script src="main.js"></script>
<script>
  alert("Esto sí funciona");
</script>
```

---

### **3. Ubicación recomendada del `<script>`:**

**Antes del cierre de `</body>`:**

html

```html
<body>
  <h1>Contenido</h1>
  <script src="main.js"></script>
</body>
```

## Ver también

- [[MOC-programacion]]
- [[Ruta JavaScript]]
