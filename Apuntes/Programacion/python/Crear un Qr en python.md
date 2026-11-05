---
title: Crear un Qr en python
area: programacion
tipo: nota
tags: [programacion, python, qr, utilidad]
created: 2026-10-12
updated: 2026-10-12
related: ["[[Ruta Python]]", "[[MOC-programacion]]", "[[Flash en python (bases)]]"]
---

# Crear un QR en Python

## Qué es / Para qué sirve

Genera códigos QR a partir de un texto o URL usando la librería `qrcode`. Útil para scripting, reportes y CTFs (payloads por QR, inventarios, tarjetas).

## Instalación

```bash
pip install qrcode[pil]
```

## Ejemplo base

```python
import qrcode

# Tu enlace
url = input("ingrese el link:   ").strip()

# Generar y guardar el QR
img = qrcode.make(url)
img.save("codigo_qr.png")
```

## Con más control (tamaño, bordes, colores)

```python
import qrcode

qr = qrcode.QRCode(
    version=1,                      # 1-40 (None = auto)
    error_correction=qrcode.constants.ERROR_CORRECT_H,  # L/M/Q/H
    box_size=10,                    # px por módulo
    border=4,                       # módulos de margen
)
qr.add_data("https://ejemplo.com")
qr.make(fit=True)

img = qr.make_image(fill_color="black", back_color="white")
img.save("codigo_qr.png")
```

## Ver también

- [[Ruta Python]]
- [[MOC-programacion]]
- [[Flash en python (bases)]]
