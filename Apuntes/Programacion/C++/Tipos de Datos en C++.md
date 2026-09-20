---
title: Tipos de Datos en C++
area: programacion
tipo: lenguaje
tags: [programacion, cpp]
created: 2026-09-16
updated: 2026-09-20
related: ["[[MOC-programacion]]", "[[Rutas]]"]
---

En C++, cada variable debe tener un **tipo de dato explícito** definido. El tipo de dato determina cuánto espacio ocupa la variable en la memoria RAM y qué tipo de valores puede almacenar.

## 1. Cheat Sheet Rápido

|**Tipo**|**Espacio típico**|**Rango / Ejemplo de uso**|
|---|---|---|
|**`int`**|4 bytes|Enteros estándar: `-5`, `0`, `42`|
|**`double`**|8 bytes|Decimales de alta precisión (recomendado): `3.14159`|
|**`float`**|4 bytes|Decimales simples (usa menos memoria): `3.14f`|
|**`char`**|1 byte|Un solo carácter (entre comillas simples): `'A'`, `'@'`|
|**`bool`**|1 byte|Booleano: `true` (1) o `false` (0)|
|**`string`**|Dinámico|Texto/Cadenas (requiere `<string>`): `"Hola mundo"`|

## Ver también

- [[MOC-programacion]]
- [[Rutas]]
