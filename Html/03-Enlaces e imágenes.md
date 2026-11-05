---
title: "Enlaces e imágenes"
area: programacion
tipo: lenguaje
tags: [programacion, html]
created: 2026-10-22
updated: 2026-10-22
related: ["[[MOC-programacion]]", "[[Ruta Html]]", "[[02-Texto y semántica]]", "[[04-Tablas y formularios]]"]
---

# 03 — Enlaces e imágenes

## Enlaces `<a>` (anchor)

```html
<a href="https://ejemplo.com">Ir a ejemplo</a>
```

### Rutas en `href`

| Tipo | Ejemplo | Cuándo |
| ------------------- | ---------------------------------------- | -------------------------- |
| Absoluta | `https://otro-sitio.com` | Fuera de tu sitio |
| Relativa (archivo) | `paginas/sobre.html` | Dentro de tu proyecto |
| Subir nivel | `../index.html` | `..` = carpeta padre |
| Ancla interna | `#seccion-2` | Mismo documento |
| Email | `mailto:correo@x.com` | Abrir cliente de correo |
| Teléfono | `tel:+34600000000` | Llamar desde móvil |

### Anclas (enlazar dentro de la misma página)

```html
<h2 id="precios">Precios</h2>
<a href="#precios">Saltar a precios</a>
```

### Atributos útiles de `<a>`

```html
<a href="https://x.com" target="_blank" rel="noopener noreferrer">Nueva pestaña</a>
<a href="documento.pdf" download>Descargar PDF</a>
```

> `target="_blank"` **siempre** con `rel="noopener"` → evita que la página abierta manipule tu `window` (riesgo de seguridad real).

## Imágenes `<img>`

```html
<img src="foto.jpg" alt="Descripción de la foto" width="600" height="400">
```

| Atributo | Para qué |
| ------- | ----------------------------------------------- |
| `src` | Ruta de la imagen (misma lógica de rutas que `href`) |
| `alt` | **Obligatorio**: texto si la imagen no carga + accesibilidad |
| `width`/`height` | Reserva el espacio → evita saltos de layout (CLS) |
| `loading="lazy"` | Carga la imagen cuando se ve (rendimiento) |

### Imagen enlazada (clickeable)

```html
<a href="https://ejemplo.com">
    <img src="logo.png" alt="Logo de ejemplo">
</a>
```

### Imagen decorativa

```html
<img src="linea.png" alt="">   <!-- alt vacío = puramente decorativa -->
```

## Figure +figcaption (imagen con pie)

```html
<figure>
    <img src="topologia.png" alt="Topología de red">
    <figcaption>Figura 1 — Topología en estrella</figcaption>
</figure>
```

## Picture (imágenes responsivas)

```html
<picture>
    <source srcset="foto-movil.webp" media="(max-width: 600px)">
    <source srcset="foto-grande.webp" media="(min-width: 1200px)">
    <img src="foto.jpg" alt="Foto adaptable">
</picture>
```

## Cuándo usar / Cuándo NO usar

- **Nunca** dejes `alt` vacío en imágenes con contenido → pierdes SEO y accesibilidad.
- No subas imágenes gigantes: comprime antes (WebP suele ser la mejor opción).
- Rutas rotas = el error clásico: verifica que la ruta relativa sea **desde el archivo HTML**, no desde la raíz del proyecto.

## Ver también

- [[02-Texto y semántica]]
- [[04-Tablas y formularios]]
- [[Ruta Html]]
- [[MOC-programacion]]
- [[HOME]]
