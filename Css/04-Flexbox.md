---
title: "Flexbox"
area: programacion
tipo: lenguaje
tags: [programacion, css]
created: 2026-10-22
updated: 2026-10-22
related: ["[[MOC-programacion]]", "[[Ruta Css]]", "[[03-Box model]]", "[[05-CSS Grid]]"]
---

# 04 — Flexbox

## Qué es / Para qué sirve

**Flexbox** maqueta en **una dimensión** (fila **o** columna) distribuyendo espacio entre elementos de forma automática. Es *la* herramienta para: centrar, alinear barras de nav, poner columnas iguales, gaps limpios.

> Regla mental: **contenedor** decide el eje, **items** se estiran/acomodan.

## Activar flex

```css
.contenedor {
    display: flex;   /* los hijos directos se vuelven flex items */
}
```

### Ejes

- `flex-direction: row` → eje principal **horizontal** (por defecto)
- `flex-direction: column` → eje principal **vertical**
- `flex-wrap: wrap` → permite pasar a la siguiente línea

```css
.contenedor {
    display: flex;
    flex-direction: row;      /* row | column | row-reverse | column-reverse */
    flex-wrap: wrap;          /* wrap | nowrap */
    gap: 16px;                /* espacio entre items (más limpio que margins) */
}
```

(`display: flex` + `flex-wrap: wrap` = shorthand `display: flex-flow`)

## Propiedades del CONTENEDOR

| Propiedad | Eje | Valores típicos |
| ----------------- | ---------- | ------------------------------------ |
| `justify-content` | principal | `flex-start` `center` `flex-end` `space-between` `space-around` `space-evenly` |
| `align-items` | cruzado | `stretch` (defecto) `center` `flex-start` `flex-end` `baseline` |
| `align-content` | cruzado (solo si `wrap`) | `stretch` `center` `space-between` |
| `gap` | ambos | `16px` / `12px 24px` |

```css
header {
    display: flex;
    justify-content: space-between;  /* logo a la izquierda, nav a la derecha */
    align-items: center;             /* centrado vertical */
    padding: 12px 24px;
}
```

## Propiedades de cada ITEM

| Propiedad | Para qué |
| ---------------- | ----------------------------------------------- |
| `flex-grow` | Cuánto **crece** si sobra espacio (proporcional) |
| `flex-shrink` | Cuánto **encoge** si falta espacio |
| `flex-basis` | Tamaño inicial del item |
| `flex` | atajo: `flex: grow shrink basis` |
| `align-self` | Sobrescribe `align-items` para ese item |
| `order` | Reordena items (visual, no el DOM) |

Atajos habituales:

```css
.item { flex: 1; }        /* crece y encoca por igual → columnas iguales */
.item { flex: 2; }        /* doble de ancho que un flex:1 */
.item { flex: none; }     /* tamaño fijo, no crece ni encoge */
```

## Las 3 recetas que más usas

### Centrado perfecto (la clásica)

```css
.padre {
    display: flex;
    justify-content: center;   /* horizontal */
    align-items: center;       /* vertical */
    min-height: 100vh;
}
```

### Barra de nav con logo + enlaces

```css
nav {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 24px;
}
```

### Tarjetas de ancho igual

```css
.grilla {
    display: flex;
    flex-wrap: wrap;
    gap: 16px;
}
.grilla > .tarjeta { flex: 1 1 250px; }  /* crece, encoca, base 250px */
```

## Cuándo usar / Cuándo NO usar

- **Usa Flex** para una dimensión: barras, filas/columnas simples, centrar, listas.
- **Usa Grid** para dos dimensiones (filas **y** columnas a la vez) → [[05-CSS Grid]].
- Prefiere `gap` sobre márgenes negativos.
- Un `flex-direction: column` centrado con `justify-content: center` centra **vertical** (eje principal cambió).

## Ver también

- [[05-CSS Grid]]
- [[03-Box model]]
- [[Ruta Css]]
- [[MOC-programacion]]
- [[HOME]]
