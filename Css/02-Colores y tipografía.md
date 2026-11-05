---
title: "Colores y tipografía"
area: programacion
tipo: lenguaje
tags: [programacion, css]
created: 2026-10-22
updated: 2026-10-22
related: ["[[MOC-programacion]]", "[[Ruta Css]]", "[[01-Introducción a CSS]]", "[[03-Box model]]"]
---

# 02 — Colores y tipografía

## Formatos de color

```css
p { color: red; }                    /* nombre */
p { color: #ff6600; }                /* hex corto/largo */
p { color: #f60; }                   /* hex abreviado */
p { color: rgb(255, 102, 0); }       /* RGB 0-255 */
p { color: rgba(255, 102, 0, 0.5); } /* + alfa (transparencia) */
p { color: hsl(24, 100%, 50%); }     /* tono, saturación, luminosidad */
p { color: hsl(24, 100%, 50%, 0.5); }/* HSL + alfa */
```

> **Truco**: `hsl()` es el más intuitivo para ajustar. Baja la luminosidad (`%`) para oscurecer el mismo tono sin perder coherencia de paleta.

### Propiedades de color útiles

```css
background-color: #1e1e1e;
background: linear-gradient(90deg, #ff6600, #ff9933);
opacity: 0.8;                 /* transparencia del elemento entero */
border: 1px solid currentColor;  /* color del texto actual */
```

## Unidades

| Unidad | Ejemplo | Significado |
| ------ | ------- | ------------------------------- |
| `px` | `font-size: 16px` | Fijo |
| `em` | `font-size: 1.5em` | Relativo al **padre** |
| `rem` | `font-size: 1.5rem` | Relativo a la **raíz** (recomendado) |
| `%` | `width: 50%` | % del contenedor |
| `vw`/`vh` | `width: 100vw` | % del viewport (ancho/alto ventana) |

## Tipografía

```css
body {
    font-family: 'Inter', Arial, sans-serif;  /* pila de fuentes */
    font-size: 16px;          /* tamaño base recomendado: 16px */
    font-weight: 400;         /* 400 normal, 700 bold */
    line-height: 1.6;         /* interlineado (legibilidad) */
    color: #222;
}

h1 {
    font-size: 2.5rem;
    font-weight: 700;
    letter-spacing: -0.02em;  /* espaciado entre letras */
    text-transform: uppercase;/* MAYÚSCULAS automáticas */
}
```

### Alineación y decoración

```css
p {
    text-align: center;       /* left | right | center | justify */
    text-decoration: none;    /* quita subrayado de <a> */
    text-decoration: underline;
    text-transform: capitalize;
}
```

### Fuente personalizada

```css
@font-face {
    font-family: 'MiFuente';
    src: url('fuentes/mifuente.woff2') format('woff2');
}
body { font-family: 'MiFuente', sans-serif; }
```

## Listas y cursiva heredadas

```css
ul { list-style-type: square; }
blockquote { font-style: italic; }
```

## Cuándo usar / Cuándo NO usar

- Define una **base** en `body` y deja que herede → menos código.
- Prefiere `rem` sobre `px` para tipografía (respeta la config del usuario).
- Máximo **2–3 tipografías** por proyecto (Google Fonts es gratis).
- Usa `font-display: swap` en fuentes web para no bloquear el renderizado.

## Ver también

- [[01-Introducción a CSS]]
- [[03-Box model]]
- [[Ruta Css]]
- [[MOC-programacion]]
- [[HOME]]
