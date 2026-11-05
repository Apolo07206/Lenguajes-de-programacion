---
title: "CSS Grid"
area: programacion
tipo: lenguaje
tags: [programacion, css]
created: 2026-10-22
updated: 2026-10-22
related: ["[[MOC-programacion]]", "[[Ruta Css]]", "[[04-Flexbox]]", "[[07-Responsive, variables y transiciones]]"]
---

# 05 — CSS Grid

## Qué es / Para qué sirve

**Grid** maqueta en **dos dimensiones** (filas **y** columnas simultáneamente). Ideal para: layouts de página (sidebar + contenido), galerías, dashboards.

> Regla mental: **Flex** = una dimensión (barra). **Grid** = dos dimensiones (plano).

## Activar grid

```css
.contenedor {
    display: grid;
    grid-template-columns: 200px 1fr 200px;   /* 3 columnas */
    grid-template-rows: 80px 1fr;             /* 2 filas */
    gap: 16px;
}
```

## Unidades de grid

| Unidad | Significado |
| ------ | ----------------------------------------------- |
| `px` / `rem` | Ancho fijo |
| `fr` | **Fracción** del espacio libre (proporcional) |
| `%` | % del contenedor |
| `auto` | Tamaño según el contenido |
| `minmax(min, max)` | Límites para track flexible |

```css
grid-template-columns: 1fr 2fr;        /* 1 parte / 2 partes */
grid-template-columns: repeat(3, 1fr); /* 3 columnas iguales */
```

## Las dos formas de colocar items

### 1. Por número de pista (explícito)

```css
.item-a {
    grid-column: 1 / 3;   /* desde la línea 1 hasta la 3 (ocupa 2 columnas) */
    grid-row: 1 / 2;
}
.item-b {
    grid-column: 3 / -1;  /* -1 = última línea */
}
```

### 2. Por nombre de área (recomendado para layouts)

```css
.layout {
    display: grid;
    grid-template-areas:
        "header header header"
        "sidebar main   aside"
        "footer footer footer";
    grid-template-columns: 200px 1fr 200px;
    gap: 12px;
}
.header { grid-area: header; }
.sidebar { grid-area: sidebar; }
.main { grid-area: main; }
.aside { grid-area: aside; }
.footer { grid-area: footer; }
```

El layout se lee **en el propio CSS**. Para "vaciar" una celda: `"."`.

## Grid automático responsivo (la joya)

```css
.galeria {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 16px;
}
```

- `auto-fit` → cuántas columnas quepan
- `minmax(250px, 1fr)` → nunca menos de 250px, estirándose

**Responsive puro, sin media queries** → ver también [[07-Responsive, variables y transiciones]].

## Alineación

```css
.contenedor {
    justify-items: center;   /* items dentro de su celda (eje X) */
    align-items: center;     /* items dentro de su celda (eje Y) */
    justify-content: center; /* la grilla dentro del contenedor */
    align-content: center;
}
.item { justify-self: end; } /* sobrescribe por item */
```

Centrar una caja en grid (la más rápida del vault):

```css
.padre {
    display: grid;
    place-items: center;     /* = justify-items + align-items */
    min-height: 100vh;
}
```

## Layout de página completo

```css
body {
    display: grid;
    grid-template-areas:
        "header"
        "main"
        "footer";
    grid-template-rows: auto 1fr auto;
    min-height: 100vh;
}
```

`auto` = alto del contenido · `1fr` = el resto (empuja el footer al fondo).

## Cuándo usar / Cuándo NO usar

- **Grid** → dos dimensiones (páginas, galerías, tableros).
- **Flex** → una dimensión (navs, listas, centrar) → [[04-Flexbox]].
- No forces Grid donde Flex basta; y viceversa. Se pueden **combinar** (grid para el layout, flex dentro de cada celda).
- `place-items: center` es el atajo de centrado más rápido que existe.

## Ver también

- [[04-Flexbox]]
- [[07-Responsive, variables y transiciones]]
- [[Ruta Css]]
- [[MOC-programacion]]
- [[HOME]]
