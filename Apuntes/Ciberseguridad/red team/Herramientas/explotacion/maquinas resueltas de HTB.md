---
title: maquinas resueltas de HTB
area: ciberseguridad
tipo: herramienta
tags: [ciberseguridad, red-team, explotacion]
created: 2026-02-18
updated: 2026-09-20
related: ["[[MOC-ciberseguridad]]", "[[red-team-killchain]]", "[[Nmap]]", "[[Metasploit]]"]
---

# 🕺 Dancing (SMB)

Servicio expuesto:

`445 → SMB`

## 🧠 Qué había mal

SMB permitía acceso anónimo.

---

## 🔎 Paso 1 – Enumerar shares

`smbclient -L //<IP>/ -N`

`-N` → sin password.

Si ves algo como:

`ADMIN$ C$ WorkShares`

Eso ya es pista.

---

## 📂 Paso 2 – Entrar a un share

`smbclient //<IP>/WorkShares -N`

Dentro:

`ls get archivo.txt`

Ahí encontrabas la flag.

---

## 🎯 Lección real de Dancing

No era exploit.  
Era mala configuración.

SMB abierto + acceso anónimo = data exposure.

Patrón aprendido:

> Siempre probar null session en SMB.

---

# 🔴 Redeemer (Redis)

Servicio expuesto:

`6379 → Redis`

---

## 🔎 Paso 1 – Conectarse

`redis-cli -h <IP>`

Si entra sin pedir password… eso ya es crítico.

---

## 📋 Paso 2 – Ver claves

Primero:

`KEYS *`

Eso listaba claves guardadas.

Luego:

`GET flag`

Y ahí estaba la flag.

---

## 🎯 Lección real de Redeemer

Redis expuesto sin autenticación.

Patrón aprendido:

> Si Redis permite conexión sin auth → probablemente puedes leer datos.

En máquinas más avanzadas eso evoluciona a RCE.

---

# 🧬 Sequel (MySQL)

Servicio expuesto:

`3306 → MariaDB/MySQL`

---

## 🔎 Paso 1 – Conectar

Daba error SSL.

Solución:

`mysql -h <IP> -u root --skip-ssl`

---

## 📋 Paso 2 – Enumerar bases

`SHOW DATABASES; USE htb; SHOW TABLES;`

---

## 📂 Paso 3 – Leer datos

`SELECT * FROM config; SELECT * FROM users;`

Ahí estaba la flag.

---

## 🎯 Lección real de Sequel

No era brute force.  
No era exploit.  
Era:

- Servicio expuesto
    
- Autenticación permisiva
    
- Mala configuración SSL
    

Patrón aprendido:

> Si MySQL expuesto permite login root sin password → enumera bases.

## Ver también

- [[MOC-ciberseguridad]]
- [[red-team-killchain]]
- [[Nmap]]
- [[Metasploit]]
