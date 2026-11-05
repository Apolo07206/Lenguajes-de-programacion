---
title: "Texto y semántica"
area: programacion
tipo: lenguaje
tags: [programacion, html]
created: 2026-10-22
updated: 2026-10-22
related: ["[[MOC-programacion]]", "[[Ruta Html]]", "[[01-Introducción a HTML]]", "[[03-Enlaces e imágenes]]"]
---

# 02 — Texto y semántica

## Títulos y párrafos

```html
<h1>Título principal (solo uno por página, va en el contenido)</h1>
<h2>Subtítulo</h2>
<h3>Sub-subtítulo</h3>
...
<h6>El menos importante</h1>

<p>Un párrafo. Los saltos de línea en el código HTML no pintan: necesitas etiqueta.</p>
<br>  <!-- salto de línea (úsalo poco, mejor párrafos y CSS) -->
<hr>  <!-- línea separadora temática -->
```

> `h1` → `h6` en orden de importancia. No saltes de `h2` a `h5` "porque queda bonito": rompe la accesibilidad y el SEO.

## Texto con énfasis

| Etiqueta | Efecto | Visual |
| ----------------- | ---------------- | ---------- |
| `<strong>` | Importancia fuerte | **negrita** |
| `<em>` | Énfasis / énfasis vocal | *cursiva* |
| `<b>` | negrita sin importancia | **negrita** |
| `<i>` | cursiva sin énfasis | *cursiva* |
| `<mark>` | resaltado | <mark>marcado</mark> |
| `<small>` | letra pequeña | pequeño |
| `<del>` / `<ins>` | borrado / insertado | ~~tachado~~ / subrayado |

Diferencia clave: `strong/em` **significan** algo (lectores de pantalla lo marcan); `b/i` solo son estilo visual.

## Listas

```html
<!-- Ordenada (numerada) -->
<ol>
    <li>Primero</li>
    <li>Segundo</li>
</ol>

<!-- Desordenada (viñetas) -->
<ul>
    <li>Manzana</li>
    <li>Pera</li>
</ul>

<!-- Lista de definiciones -->
<dl>
    <dt>HTML</dt>
    <dd>Lenguaje de marcado</dd>
</dl>
```

Se pueden anidar listas dentro de `<li>`.

## Citas y código

```html
<blockquote cite="https://ejemplo.com">Cita de otra fuente.</blockquote>
<p>Mira el atributo <code>href</code>.</p>
<pre><code>const x = 1;   <!-- preserva espacios y saltos -->
</code></pre>
```

## Elementos semánticos (la clave de esta nota)

En vez de esto (todo `<div>`):

```html
<div class="header">...</div>
<div class="nav">...</div>
<div class="content"><div class="post">...</div></div>
<div class="footer">...</div>
```

Usa esto:

```html
<header>  <!-- cabecera de página o de sección -->
<nav>     <!-- navegación / menús -->
<main>    <!-- contenido principal (UNO por página) -->
<article> <!-- contenido independiente (post, tarjeta) -->
<section> <!-- agrupación temática (suele llevar su h2) -->
<aside>   <!-- contenido lateral (relacionados, sidebar) -->
<footer>  <!-- pie (autoría, copyright, enlaces) -->
```

Ventajas: mejor **SEO**, mejor **accesibilidad** (lectores de pantalla navegan por landmark) y más claro para ti en CSS.

## Entidades de caracteres

```html
&lt;   <       &gt;   >       &amp;   &
&quot;  "       &nbsp; espacio no separable
```

Úsalas cuando el carácter **es contenido** (ej: mostrar `<div>` como texto en un tutorial).

## Cuándo usar / Cuándo NO usar

- Un solo `<h1>` por página.
- `<main>` una sola vez por página.
- No uses `<br>` para separar secciones → usa `<p>`, `<section>` y CSS (`margin`).
- No uses `<b>`/`<i>` si lo que quieres es significado → `<strong>`/`<em>`.

## Ver también

- [[01-Introducción a HTML]]
- [[03-Enlaces e imágenes]]
- [[Ruta Html]]
- [[MOC-programacion]]
- [[HOME]]
