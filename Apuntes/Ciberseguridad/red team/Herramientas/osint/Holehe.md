---
title: Holehe
area: ciberseguridad
tipo: herramienta
tags: [ciberseguridad, red-team, osint]
created: 2026-01-18
updated: 2026-09-20
related: ["[[MOC-ciberseguridad]]", "[[red-team-killchain]]", "[[Nmap]]"]
---

Tool de **OSINT** que permite verificar si un correo electrónico está registrado en múltiples servicios populares. No revela contraseñas ni datos internos, solo confirma presencia. Útil en **reconocimiento temprano** y **perfilado inicial** de objetivos.


https://youtube.com/shorts/O-cOb_6Vs1s?si=KRk9o1udLXMe2EhF
---

## ¿Para qué sirve?

- Verificar si un correo está asociado a cuentas online
    
- Identificar posibles perfiles y redes del objetivo
    
- Cruce con otras herramientas de OSINT (p. ej. Sherlock, theHarvester)
    
- Preparar un ataque de ingeniería social o **phishing awareness** (contexto ético)
    
- Reconocer superficie digital sin interactuar directamente con la víctima
    

---

## Instalación

**Kali / Debian:**

`sudo apt install holehe`

**Python / pip:**

`pip3 install holehe`

---

## Uso básico

Comando principal:

`holehe <correo>`

Ejemplo:

`holehe target@mail.com`

---

## ¿Qué servicios verifica?

Depende de la versión, pero típicamente:

- Redes sociales
    
- E-commerce
    
- Plataformas de juegos
    
- Foros y servicios varios
    

No usa fuerza bruta ni spam. Simplemente consulta APIs públicas o formularios de recuperación.

---

## Interpretación de resultados

Generalmente marca:

- **Found**: El correo existe en el servicio
    
- **Not Found**: No existe o no se pudo verificar
    
- **Rate limited / Unknown**: No se confirmó debido a límites o cambios en APIs
    

Esto permite deducir:

- Qué plataformas usa el objetivo
    
- Si hay relación entre cuentas
    
- Si se puede cruzar con nicks detectados por Sherlock
    

---

## Buenas prácticas OSINT

Combinar Holehe con:

- **Sherlock** (usernames)
    
- **theHarvester** (emails y dominios)
    
- **EmailHippo / Hunter.io** (validación)
    
- **Avatar analyzers** para extraer metadata de imágenes asociadas
    

Si buscas perfiles más profundos:

- Usa los hallazgos de Holehe para intentar encontrar usernames, leaks o redes asociadas.
    

---

## Limitaciones

- No revela contraseñas
    
- No accede a cuentas
    
- Puede fallar por cambios en servicios
    
- Puede devolver falsos negativos en plataformas protegidas
    

---

## Uso ético

Ideal para:

- Awareness en empresas
    
- Red Team recon
    
- Preparación de campañas de phishing ético
    
- Auditoría de exposición digital
    
- Investigaciones privadas con autorización

## Ver también

- [[MOC-ciberseguridad]]
- [[red-team-killchain]]
- [[Nmap]]
