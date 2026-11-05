---
title: "Posicionamiento y z-index"
area: programacion
tipo: lenguaje
tags: [programacion, css]
created: 2026-10-22
updated: 2026-10-22
related: ["[[MOC-programacion]]", "[[Ruta Css]]", "[[03-Box model]]", "[[07-Responsive, variables y transiciones]]"]
---

# 06 — Posicionamiento y z-index

## Qué es / Para qué sirve

Por defecto los elementos están en flujo normal. `position` cambia **cómo** se colocan y habilita desplazarse con `top/right/bottom/left`.

## Los 5 valores de `position`

| Valor | ¿Sale del flujo? | Se mueve con | Se queda al hacer scroll |
| ------------ | ---------------- | ------------ | ----------------------- |
| `static` | No (por defecto) | Ignora offsets | Sí, con la página |
| `relative` | No | Respecto a su sitio original | Sí |
| `absolute` | **Sí** | Respecto al ancestro posicionado más cercano | No (sigue al ancestro) |
| `fixed` | **Sí** | Respecto a la **ventana** | No, queda pegado |
| `sticky` | No (hasta cumplir umbral) | Relativo hasta `top:` alcanzado | Se "pega" al llegar |

```css
.relativo  { position: relative; top: 10px; left: 5px; }
.absoluto  { position: absolute; top: 0; right: 0; }
.fixed     { position: fixed; bottom: 20px; right: 20px; }
.sticky    { position: sticky; top: 0; }
```

## Patrones típicos

### Sticky (barra de nav que se pega arriba)

```css
nav {
    position: sticky;
    top: 0;              /* obligatorio: sin esto no hace nada */
    z-index: 100;
}
```

### Badge/etiqueta sobre una imagen

```css
.tarjeta { position: relative; }
.badge {
    position: absolute;
    top: -8px;
    right: -8px;
}
```

### Botón flotante de ayuda (esquina de la ventana)

```css
.boton-flotante {
    position: fixed;
    bottom: 24px;
    right: 24px;
}
```

> Para `absolute` busque el ancestro posicionado **más cercano**: si ningún padre tiene `position` distinto de `static`, será `<html>` (toda la página).

## z-index — el orden de capas

```css
.modal     { z-index: 1000; }
.overlay   { z-index: 999; }
nav       { z-index: 100; }
```

Reglas:
1. `z-index` solo funciona en elementos **posicionados** (`relative`, `absolute`, `fixed`, `sticky`).
2. Números más altos = más arriba.
3. Por defecto todos valen `0` → gana el orden del HTML.
4. **Cuidado**: `z-index` gigantes (99999) generan guerras de capas. Usa una escala coherente.

### Contexto de apilamiento (por qué "a veces" no funciona)

Un `z-index` se compara **dentro de su contexto de apilamiento**. Un `transform`, `opacity < 1`, `filter` o `position: fixed` crea uno nuevo → lo que había "debajo" ya no se compara con lo de fuera.

```css
.card {
    transform: translateY(10px);   /* crea contexto de apilamiento */
    z-index: 10;                   /* solo comparte con lo de dentro de .card */
}
```

## Cuándo usar / Cuándo NO usar

- `relative` es el punto de partida seguro para anclar hijos con `absolute`.
- `fixed` = flotantes (banners, botones). `sticky` = barras que se pegan.
- Si `z-index` "no te hace caso", casi siempre es por contexto de apilamiento o porque el elemento **no está posicionado**.
- Evita `absolute` para maquetar layouts completos → usa [[05-CSS Grid]] / [[04-Flexbox]].

## Ver también

- [[03-Box model]]
- [[07-Responsive, variables y transiciones]]
- [[Ruta Css]]
- [[MOC-programacion]]
- [[HOME]]
