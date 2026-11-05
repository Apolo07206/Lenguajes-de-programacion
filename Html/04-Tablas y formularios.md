---
title: "Tablas y formularios"
area: programacion
tipo: lenguaje
tags: [programacion, html]
created: 2026-10-22
updated: 2026-10-22
related: ["[[MOC-programacion]]", "[[Ruta Html]]", "[[03-Enlaces e imágenes]]", "[[01-Conceptos de mysql]]"]
---

# 04 — Tablas y formularios

## Tablas `<table>`

```html
<table>
    <thead>
        <tr>
            <th>Nombre</th>
            <th>Edad</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Ana</td>
            <td>25</td>
        </tr>
        <tr>
            <td>Luis</td>
            <td>31</td>
        </tr>
    </tbody>
</table>
```

| Etiqueta | Para qué |
| --------- | -------------------------------------- |
| `<table>` | Contenedor de la tabla |
| `<thead>` / `<tbody>` | Cabecera y cuerpo (semántica) |
| `<tr>` | Table row (fila) |
| `<th>` | Celda de cabecera (negrita + centrada) |
| `<td>` | Celda de datos |
| `<caption>` | Título accesible de la tabla |

### Celdas combinadas

```html
<td colspan="2">Ocupa 2 columnas</td>
<td rowspan="2">Ocupa 2 filas</td>
```

> Para **datos tabulares** usa `<table>`. Para maquetar páginas usa **CSS Grid/Flexbox** → [[04-Flexbox]], [[05-CSS Grid]].

## Formularios `<form>`

```html
<form action="/enviar" method="POST">
    <label for="nombre">Nombre:</label>
    <input type="text" id="nombre" name="nombre" placeholder="Tu nombre" required>

    <label for="email">Email:</label>
    <input type="email" id="email" name="email" required>

    <label for="passwd">Contraseña:</label>
    <input type="password" id="passwd" name="passwd" minlength="8">

    <input type="submit" value="Enviar">
</form>
```

- `action` = a dónde se envía · `method` = `POST` (datos sensibles) o `GET` (van en la URL).
- `name` = **clave** que viaja al servidor. Sin `name`, el campo no se envía.
- `label for` + `id` iguales = al hacer clic en el texto se enfoca el campo (accesibilidad + área de clic grande).

### Tipos de `<input>`

| `type` | Resultado |
| -------------- | -------------------------------------- |
| `text` | Texto libre |
| `email` | Valida formato email |
| `password` | Puntos ocultos |
| `number` | Solo números (+ `min`, `max`) |
| `date` | Selector de fecha |
| `checkbox` | Casilla (varias opciones) |
| `radio` | Radio (una sola opción, mismo `name`) |
| `range` | Deslizador |
| `color` | Selector de color |
| `file` | Subir archivo |

### Otros elementos de formulario

```html
<textarea name="msg" rows="5" cols="40">Texto inicial</textarea>

<select name="pais">
    <option value="es" selected>España</option>
    <option value="mx">México</option>
</select>

<input type="hidden" name="token" value="abc123">
<button type="button">Solo botón</button>
<button type="submit">Enviar</button>
```

### Validación nativa (sin JS)

```html
<input type="email" required>
<input type="number" min="0" max="100" step="5">
<input type="text" minlength="3" maxlength="20" pattern="[A-Za-z]+">
```

## Cuándo usar / Cuándo NO usar

- Tablas solo para **datos tabulares** (filas y columnas de verdad).
- Todo campo visible necesita `<label>`.
- Campos obligatorios → `required`.
- `placeholder` **no** sustituye a `<label>` (desaparece al escribir, mal para accesibilidad).

## Ver también

- [[03-Enlaces e imágenes]]
- [[05-Multimedia e iframe]]
- [[Ruta Html]]
- [[MOC-programacion]]
- [[HOME]]
