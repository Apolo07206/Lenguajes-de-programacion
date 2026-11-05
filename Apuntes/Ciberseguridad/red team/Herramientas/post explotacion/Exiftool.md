---
title: Exiftool
area: ciberseguridad
tipo: herramienta
tags: [ciberseguridad, red-team, post-explotacion, forense]
created: 2026-01-17
updated: 2026-10-12
related: ["[[MOC-ciberseguridad]]", "[[red-team-killchain]]", "[[John The Ripper]]", "[[Permisos, Servicios y Logs en Linux]]"]
---

# Exiftool

Herramienta de línea de comandos para **leer, escribir y editar metadatos** de archivos (imágenes, PDFs, documentos, videos). En seguridad sirve para extraer información oculta: GPS, software con el que se creó el archivo, nombre del autor, y a veces credenciales o rutas internas.

## Cuándo usar

- OSINT: extraer datos de fotos (cámara, fecha, coordenadas GPS)
- Forense: identificar qué programa creó un documento
- Buscar metadatos filtrados (usuario, rutas de red, comentarios)
- Limpiar metadatos antes de publicar un archivo (opurging)

## Cuándo NO usar

- Analizar tráfico de red (usa [[TCPdump]] o [[Wireshark]])
- Crackear contraseñas (usa [[John The Ripper]] o [[Hydra]])

## Comandos base

```bash
# Ver TODOS los metadatos de un archivo
exiftool foto.jpg

# Metadatos en formato agrupado (más legible)
exiftool -a -u -g1 foto.jpg

# Solo coordenadas GPS
exiftool -gps:all foto.jpg

# Buscar un dato concreto
exiftool -Artist -Creator documento.pdf

# Extraer miniatura embebida
exiftool -thumbnailimage foto.jpg -o thumb.jpg

# Borrar metadatos (limpiar antes de compartir)
exiftool -all= foto.jpg

# Ver metadatos de todo un directorio
exiftool -ext jpg -r carpeta/
```

## Qué se puede filtrar (y por qué importa)

| Metadato | Riesgo |
|---|---|
| `GPSPosition` | Ubicación exacta de donde se tomó la foto |
| `Software` | Programa y versión (huella del sistema) |
| `Creator/Author` | Nombre del usuario que creó el archivo |
| `Comments` | Notas ocultas, a veces datos internos |
| `DocumentID` | Identificadores únicos de edición |

## Flujo OSINT típico

1. Consigues un archivo (web, leak, email)
2. `exiftool -a -u -g1 archivo` → lees todo
3. Si hay GPS → geolocalizas; si hay nombres → buscas en redes (ver [[sherlock]], [[Holehe]])
4. Documento corporativo → buscas rutas de red y software interno

## Ver también

- [[MOC-ciberseguridad]]
- [[red-team-killchain]]
- [[John The Ripper]]
- [[Permisos, Servicios y Logs en Linux]]
