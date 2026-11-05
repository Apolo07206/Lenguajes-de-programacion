---
title: "Introducción a CSS"
area: programacion
tipo: lenguaje
tags: [programacion, css, fundamentos]
created: 2026-10-22
updated: 2026-10-22
related: ["[[MOC-programacion]]", "[[Ruta Css]]", "[[02-Colores y tipografía]]"]
---

# 01 — Introducción a CSS

## Qué es / Para qué sirve

**CSS** (Cascading Style Sheets) da **estilo** al HTML: colores, tipografía, espacios, posición, animaciones. Sin CSS, todo es texto negro sobre fondo blanco.

- **HTML** → estructura → [[01-Introducción a HTML]]
- **CSS** → apariencia
- **JS** → comportamiento → [[01-conceptos basicos de Js]]

## Las 3 formas de aplicar CSS

```html
<!-- 1. Externas (LA CORRECTA) -->
<link rel="stylesheet" href="estilos.css">

<!-- 2. Internas (en el head) -->
<style>
    h1 { color: red; }
</style>

<!-- 3. Inline (mala práctica, solo casos puntuales) -->
<h1 style="color: red;">Hola</h1>
```

En tu `.css` externo:

```css
h1 {
    color: red;
    font-size: 2rem;
}
```

## Anatomía de una regla

```css
selector {
    propiedad: valor;
    propiedad: valor;
}
```

```css
/* selector: a quién afecta */
p {
    /* propiedad: valor; */
    color: #333;
    line-height: 1.6;
}
```

> Termina cada declaración con `;`. Los bloques `{}` encierran las reglas. Los `/* comentarios */` van entre bloques.

## Selectores básicos

| Selector | Selecciona |
| ----------------- | ------------------------------------------ |
| `p` | Todos los `<p>` |
| `.card` | Todo lo con `class="card"` |
| `#main` | El elemento con `id="main"` |
| `h1, h2` | h1 **y** h2 (lista separada por comas) |
| `.card p` | `<p>` dentro de `.card` (descendiente) |
| `.card > p` | `<p>` hijo **directo** de `.card` |
| `a:hover` | estado `:hover` del enlace |
| `p:first-of-type` | el primer `<p>` |

Especifidad (quién gana): `id` (100) > `.class` (10) > `etiqueta` (1). A igual peso, gana el último del archivo.

## Cascada e herencia

- **Cascada**: si dos reglas chocan, gana la más específica; a empate, la última.
- **Herencia**: propiedades como `color`, `font-family` y `line-height` se heredan de padres a hijos. `border`, `margin` y `padding` **no**.

## La caja (introducción)

Todo elemento es una caja: `content → padding → border → margin`. Es el concepto central de [[03-Box model]].

## Cuándo usar / Cuándo NO usar

- **Usa** un único `.css` externo (o uno por módulo).
- **No** uses `!important` como primera opción: rompe la cascada. Mejor sube la especificidad con sentido.
- **No** uses `id` como selector de estilo (reservalo para JS/anclas) → usa clases.

## Ver también

- [[02-Colores y tipografía]]
- [[03-Box model]]
- [[Ruta Css]]
- [[MOC-programacion]]
- [[HOME]]
