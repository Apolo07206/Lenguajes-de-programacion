---
title: "Meta tags y SEO"
area: programacion
tipo: lenguaje
tags: [programacion, html, seo]
created: 2026-10-22
updated: 2026-10-22
related: ["[[MOC-programacion]]", "[[Ruta Html]]", "[[05-Multimedia e iframe]]"]
---

# 06 — Meta tags y SEO

## Qué es / Para qué sirve

Los **meta tags** van en el `<head>` y no se ven en la página, pero los leen **motores de búsqueda y redes sociales**. Son la primera impresión de tu web en Google.

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ferxxo Notes — Apuntes de hacking</title>
    <meta name="description" content="Apuntes de programación y ciberseguridad, de fácil a difícil.">
    <link rel="icon" href="favicon.ico">
</head>
```

## Meta esenciales

| Meta | Ejemplo | Efecto |
| ---- | ------- | ------------------------------------- |
| `charset` | `<meta charset="UTF-8">` | Acentos y ñ correctos |
| `viewport` | `width=device-width, initial-scale=1.0` | **Obligatorio** para móvil/responsive |
| `description` | texto < 160 chars | Lo que muestra Google bajo el título |
| `robots` | `noindex, nofollow` | Oculta páginas del índice |

## El `<title>` (lo más importante)

```html
<title>Categoría — Nombre de tu web</title>
```

- Entre **30 y 60 caracteres**.
- Único por página (duplicados = Google los ignora).
- Incluye la palabra clave **al principio**.

## Open Graph (compartir en redes)

```html
<meta property="og:title" content="Mi página">
<meta property="og:description" content="Resumen que se ve al compartir.">
<meta property="og:image" content="https://tuweb.com/preview.png">
<meta property="og:url" content="https://tuweb.com/pagina">
<meta property="og:type" content="website">
```

Sin estas etiquetas, compartir tu enlace en Discord/Twitter/WhatsApp aparece **sin imagen ni texto bonito**.

## Twitter Cards

```html
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="Mi página">
<meta name="twitter:image" content="https://tuweb.com/preview.png">
```

## Favicon y app icons

```html
<link rel="icon" href="/favicon.ico">
<link rel="apple-touch-icon" href="/apple-touch-icon.png">
<link rel="manifest" href="/manifest.json">   <!-- PWA -->
```

## SEO on-page básico (más allá de meta)

```html
<html lang="es">                    <!-- idioma correcto -->
<h1>Una sola palabra clave principal</h1>
<img src="foto.webp" alt="descriptivo con keyword">
<a href="https://externo.com" rel="noopener">externo</a>   <!-- noopener por seguridad -->
```

Reglas de oro:
1. Un solo `<h1>` por página con la keyword principal.
2. `alt` descriptivo en **todas** las imágenes.
3. Enlaces internos con texto descriptivo (nada de "clic aquí").
4. URLs limpias: `/apuntes/python` en vez de `/pagina?id=3&cat=7`.

## Cuándo usar / Cuándo NO usar

- **Usa** la `description` como anuncio de 1 línea, no como relleno.
- **No** repitas keywords (keyword stuffing = penalización).
- **No** copies la misma `description` en todas las páginas.
- `viewport` es obligatorio: sin él, el responsive no funciona → [[07-Responsive, variables y transiciones]].

## Ver también

- [[05-Multimedia e iframe]]
- [[Ruta Html]]
- [[MOC-programacion]]
- [[HOME]]
