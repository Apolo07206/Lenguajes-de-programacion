---
title: "Box model"
area: programacion
tipo: lenguaje
tags: [programacion, css]
created: 2026-10-22
updated: 2026-10-22
related: ["[[MOC-programacion]]", "[[Ruta Css]]", "[[02-Colores y tipografía]]", "[[04-Flexbox]]"]
---

# 03 — Box model

## Qué es / Para qué sirve

**Todo** elemento HTML es una caja rectangular formada por 4 capas, de dentro hacia fuera:

```
┌──────── margin ────────┐
│  ┌────── border ─────┐ │
│  │  ┌── padding ──┐  │ │
│  │  │   content   │  │ │
│  │  └─────────────┘  │ │
│  └───────────────────┘ │
└────────────────────────┘
```

Entender esto es **la** diferencia entre "le pongo margen y no pasa nada" y maquetar bien.

## Propiedades

```css
.caja {
    width: 300px;      /* solo el content-box */
    height: 200px;

    padding: 20px;               /* interior */
    padding-top: 10px;           /* por lado */
    padding: 10px 20px;          /* vertical | horizontal */
    padding: 10px 20px 5px 10px; /* top right bottom left */

    border: 2px solid #333;
    border: 2px dashed #999;
    border-radius: 12px;         /* esquinas redondeadas */

    margin: 16px;                /* exterior */
    margin: 0 auto;              /* CENTRA la caja (con width definido) */
}
```

| Atajo | Significado |
| --------- | -------------------------------------- |
| `10px` | los 4 lados |
| `10px 20px` | vertical / horizontal |
| `a b c` | top / horizontal / bottom |
| `a b c d` | top / right / bottom / left |

## box-sizing — el ajuste que lo cambia todo

Por defecto `width` mide **solo el contenido**. Con `padding` y `border`, la caja final crece (300 + 40 + 4 = 344px). Solución estándar:

```css
*, *::before, *::after {
    box-sizing: border-box;   /* width = content + padding + border */
}
```

Con `border-box`, `width: 300px` **significa 300px finales**. Casi todo proyecto lo pone desde el inicio.

## display — cómo se comporta la caja

| Valor | Comportamiento | Ejemplos |
| ------------ | ------------------- | ------------------- |
| `block` | Ocupa todo el ancho, admite width/height | `div`, `p`, `h1` |
| `inline` | Fluye con el texto, **ignora** width/height | `a`, `span`, `strong` |
| `inline-block` | Fluye pero admite width/height | alternativa a `div` |
| `none` | Desaparece del layout | ocultar elemento |
| `flex` | Contenedor flex | ver [[04-Flexbox]] |
| `grid` | Contenedor grid | ver [[05-CSS Grid]] |

```css
nav a {
    display: inline-block;
    padding: 8px 12px;
}
```

## Margin collapsing (colapso de márgenes)

Dos márgenes **adyacentes** verticales no se suman: se queda el mayor.

```css
h1 { margin-bottom: 20px; }
p  { margin-top: 16px; }
/* resultado visible: 20px, NO 36px */
```

Truco: evita márgenes verticales en ambos lados o usa `padding`/`gap` de flex/grid.

## Cuándo usar / Cuándo NO usar

- `box-sizing: border-box` en todos los proyectos (casi siempre).
- Para centrar una caja en horizontal: `width` + `margin: 0 auto`.
- Para centrar en vertical no existe magia con el box model puro → usa [[04-Flexbox]] o [[05-CSS Grid]].
- `width: 100%` **no** funciona en elementos `inline` → pásalos a `inline-block` o `block`.

## Ver también

- [[04-Flexbox]]
- [[02-Colores y tipografía]]
- [[Ruta Css]]
- [[MOC-programacion]]
- [[HOME]]
