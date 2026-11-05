---
title: "Introducción a HTML"
area: programacion
tipo: lenguaje
tags: [programacion, html, fundamentos]
created: 2026-10-22
updated: 2026-10-22
related: ["[[MOC-programacion]]", "[[Ruta Html]]", "[[02-Texto y semántica]]"]
---

# 01 — Introducción a HTML

## Qué es / Para qué sirve

**HTML** (HyperText Markup Language) es el lenguaje de marcado que estructura el contenido de una web. No es un lenguaje de programación: no hace cálculos ni lógica, solo define **qué hay** en la página (títulos, párrafos, imágenes, enlaces).

- **HTML** → estructura (huesos)
- **CSS** → apariencia (piel) → [[01-Introducción a CSS]]
- **JS** → comportamiento (músculo) → [[01-conceptos basicos de Js]]

## Estructura básica de un documento

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Título de la pestaña</title>
</head>
<body>
    <h1>Hola mundo</h1>
    <p>Mi primera página.</p>
</body>
</html>
```

| Parte | Para qué sirve |
| ------- | ------------------------------------------ |
| `<!DOCTYPE html>` | Le al navegador que es HTML5 (va siempre primero) |
| `<html lang="es">` | Raíz del documento + idioma (importa para SEO y accesibilidad) |
| `<head>` | Info **invisible**: meta, título, CSS, scripts |
| `<body>` | Todo el contenido **visible** de la página |
| `<meta charset="UTF-8">` | Codificación → evita los típicos `Ã±` de acentos |
| `<meta viewport>` | Hace que responsive funcione en móvil |

## Anatomía de una etiqueta (tag)

```html
<p class="parrafo" id="intro" hidden>Texto de ejemplo</p>
```

- **Etiqueta de apertura** `<p>` · **de cierre** `</p>` · **contenido** `Texto de ejemplo`
- **Atributos**: `class` (para CSS/JS), `id` (único en la página), `hidden` (booleano, solo poner el nombre)
- Etiquetas **vacías** (sin cierre): `<img>`, `<br>`, `<hr>`, `<meta>`

## Anidamiento

```html
<body>
    <div>
        <h1>Título</h1>
        <p>Párrafo dentro del div.</p>
    </div>
</body>
```

Regla: se cierra en orden inverso a cómo se abre. `</p></div>` está mal, debe ser `</div></p>`... o sea al revés: `<div>` abrió primero → `<div>` se cierra **al final**.

## Comentarios

```html
<!-- Esto es un comentario, no se ve en la página -->
```

## Cuándo usar / Cuándo NO usar

- **Usa HTML** para toda la estructura y el contenido estático.
- **No uses** `<script>` con JS dentro del `<head>` sin `defer` (bloquea el pintado).
- **No abuses** de `<div>` cuando existe una etiqueta semántica → ver [[02-Texto y semántica]].

## Ver también

- [[02-Texto y semántica]] — siguiente paso
- [[Ruta Html]]
- [[MOC-programacion]]
- [[HOME]]
