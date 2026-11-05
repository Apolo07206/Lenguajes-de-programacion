---
title: "Responsive, variables y transiciones"
area: programacion
tipo: lenguaje
tags: [programacion, css, responsive]
created: 2026-10-22
updated: 2026-10-22
related: ["[[MOC-programacion]]", "[[Ruta Css]]", "[[05-CSS Grid]]", "[[06-Posicionamiento y z-index]]", "[[06-Meta tags y SEO]]"]
---

# 07 — Responsive, variables y transiciones

## Responsive (mobile first)

Que tu web se vea bien en móvil, tablet y escritorio. Requiere el `viewport` en el HTML → [[06-Meta tags y SEO]].

### Media queries

```css
/* Mobile first: estilos base = móvil */
.contenedor { padding: 12px; }

/* Tablet en adelante */
@media (min-width: 768px) {
    .contenedor { padding: 24px; }
}

/* Escritorio */
@media (min-width: 1024px) {
    .contenedor { padding: 48px; max-width: 1200px; margin: 0 auto; }
}
```

> **Mobile first** = escribes lo mínimo para móvil y añades con `min-width`. Invertido (`max-width`) es desktop-first, más difícil de mantener.

### Breakpoints habituales

| Tamaño | Dispositivo | `min-width` |
| ------ | ------------------ | ----------- |
| < 768px | Móvil | (base) |
| ≥ 768px | Tablet | `768px` |
| ≥ 1024px | Laptop | `1024px` |
| ≥ 1440px | Desktop | `1440px` |

Los valores no son ley: mira dónde **rompe tu diseño**.

### Unidades que ayudan al responsive

| Unidad | Relativa a |
| ------ | ---------------------- |
| `rem` | raíz (`<html>`) |
| `em` | elemento padre |
| `vw` / `vh` | ventana (ancho/alto) |
| `%` | contenedor |
| `clamp()` | rango fluido |

```css
h1 { font-size: clamp(1.5rem, 4vw, 3rem); }  /* mínimo, preferido, máximo */
img { max-width: 100%; height: auto; }       /* imágenes que no desbordan */
```

### La grilla responsiva sin media queries

```css
.galeria {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 16px;
}
```
(ver detalle en [[05-CSS Grid]])

## Variables CSS (custom properties)

```css
:root {
    --color-primario: #ff6600;
    --color-texto: #222;
    --espacio: 16px;
    --radio: 12px;
}

.btn {
    background: var(--color-primario);
    padding: var(--espacio);
    border-radius: var(--radio);
}
```

- Deben empezar con `--`.
- Se leen con `var()`.
- `:root` las vuelve globales.
- **Tema oscuro gratis**:

```css
.tema-oscuro {
    --color-fondo: #121212;
    --color-texto: #eee;
}
```

Con JS se cambia la clase y todo el sitio recolorea sin tocar CSS.

## Transiciones

```css
.boton {
    background: var(--color-primario);
    transition: background 0.3s ease, transform 0.2s ease;
}
.boton:hover {
    background: #e65c00;
    transform: scale(1.05);
}
```

| Propiedad | Para qué |
| -------------- | ------------------------------------- |
| `transition` | `propiedad duración easing retraso` |
| `transform` | `translate` `rotate` `scale` (barato, no reflow) |
| `will-change` | Pista al navegador (con moderación) |

## Animaciones con @keyframes

```css
@keyframes aparecer {
    from { opacity: 0; transform: translateY(20px); }
    to   { opacity: 1; transform: translateY(0); }
}

.tarjeta {
    animation: aparecer 0.5s ease-out forwards;
}
```

```css
.rotando {
    animation: girar 2s linear infinite;
}
@keyframes girar { to { transform: rotate(360deg); } }
```

## Cuándo usar / Cuándo NO usar

- Siempre **mobile first** con `min-width`.
- `prefers-reduced-motion` respeta a usuarios sensibles al movimiento:

```css
@media (prefers-reduced-motion: reduce) {
    * { animation: none !important; transition: none !important; }
}
```

- No pares todas las transiciones: las de `transform`/`opacity` son suaves y baratas.
- Usa variables para colores/espaciados → cambiar el look sin tocar cada regla.

## Ver también

- [[05-CSS Grid]]
- [[06-Posicionamiento y z-index]]
- [[Ruta Css]]
- [[MOC-programacion]]
- [[HOME]]
