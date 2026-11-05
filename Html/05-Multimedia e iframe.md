---
title: "Multimedia e iframe"
area: programacion
tipo: lenguaje
tags: [programacion, html]
created: 2026-10-22
updated: 2026-10-22
related: ["[[MOC-programacion]]", "[[Ruta Html]]", "[[04-Tablas y formularios]]", "[[06-Meta tags y SEO]]"]
---

# 05 — Multimedia e iframe

## Vídeo `<video>`

```html
<video src="clase.mp4" controls width="640" poster="portada.jpg"></video>
```

Con múltiples formatos (compatibilidad):

```html
<video controls width="640" poster="portada.jpg">
    <source src="clase.webm" type="video/webm">
    <source src="clase.mp4" type="video/mp4">
    Tu navegador no soporta vídeo HTML5.
</video>
```

| Atributo | Para qué |
| ------------ | ------------------------------------------------- |
| `controls` | Muestra los botones (sin él no se ve nada interactivo) |
| `poster` | Imagen antes de dar play |
| `muted` | Silenciado (necesario para autoplay) |
| `loop` | Repetir |
| `autoplay` | Autoplay (solo funciona con `muted` en la mayoría de navegadores) |
| `preload="none"` | No precargar (ahorra datos) |

## Audio `<audio>`

```html
<audio controls>
    <source src="podcast.mp3" type="audio/mpeg">
    <source src="podcast.ogg" type="audio/ogg">
</audio>
```

Mismos atributos de control que `video` (sin `poster` ni dimensiones).

## Iframe — embeber contenido externo

```html
<iframe src="https://www.youtube.com/embed/VIDEO_ID"
        title="Vídeo de YouTube"
        width="560" height="315"
        loading="lazy"
        allowfullscreen>
</iframe>
```

> **Siempre** `title` en el iframe (accesibilidad) y `loading="lazy"` (rendimiento).

### Iframe seguro (buenas prácticas)

```html
<iframe src="https://ejemplo.com"
        sandbox="allow-scripts allow-same-origin"
        referrerpolicy="no-referrer">
</iframe>
```

- `sandbox` → restringe lo que puede hacer la página embebida (barrera de seguridad).
- `referrerpolicy="no-referrer"` → no filtras de dónde viene el visitante.

### Ejemplos reales de embed

```html
<!-- Google Maps -->
<iframe src="https://www.google.com/maps?q=Madrid&output=embed"></iframe>

<!-- Mapa de tu web -->
<iframe src="mapa.html"></iframe>
```

## `<embed>` y `<object>` (legacy)

```html
<object data="doc.pdf" type="application/pdf" width="600" height="800"></object>
<embed src="doc.pdf" type="application/pdf">
```

Hoy casi todo lo resuelve `<iframe>` o `<video>/<audio>`.

## SVG embebido

```html
<!-- Directo en el HTML -->
<svg width="100" height="100">
    <circle cx="50" cy="50" r="40" fill="tomato"/>
</svg>

<!-- Como imagen -->
<img src="logo.svg" alt="Logo vectorial">
```

## Cuándo usar / Cuándo NO usar

- Vídeos/hosting externo → `iframe`; tus propios archivos → `<video>/<audio>`.
- Nunca `autoplay` con sonido → molesta y los navegadores lo bloquean.
- Iframes de terceros = punto de seguridad: usa `sandbox`.
- Recuerda comprimir: un `<video>` gigante mata el rendimiento (ver [[06-Meta tags y SEO]]).

## Ver también

- [[04-Tablas y formularios]]
- [[06-Meta tags y SEO]]
- [[Ruta Html]]
- [[MOC-programacion]]
- [[HOME]]
