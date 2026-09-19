**Recon-ng** es un framework modular de **OSINT** (Open Source Intelligence) que automatiza la recolección de información pública sobre objetivos (dominios, personas, empresas). Tiene una **CLI interactiva**, base de datos integrada y más de 80 módulos para distintos propósitos.

Se asemeja por estilo a Metasploit (con comandos como `show`, `modules`, `options`, etc.), pero enfocado en **inteligencia de fuentes abiertas**.

https://youtu.be/ASP_5zfav_A?si=Qa8CL3cJU1AHLOtd

---

## 🧠 ¿Para qué sirve?

- Encontrar **subdominios**
    
- Extraer **correos y contactos**
    
- Integrar con APIs (Shodan, LinkedIn, GitHub, Hunter, etc.)
    
- Generar reportes (HTML, CSV, JSON)
    
- Organizar investigaciones por **workspaces**
    

Recon-ng guarda todo en una base de datos SQLite, así puedes continuar proyectos sin perder info.

---

## 🚀 Instalación rápida

En Kali / Debian suele venir preinstalado. Si no:

`sudo apt update sudo apt install recon-ng`

O desde el repo:

`git clone https://github.com/lanmaster53/recon-ng.git cd recon-ng pip3 install -r REQUIREMENTS python3 recon-ng`

---

## 😎 Flujo básico de uso

### 🗂 Iniciar y crear workspace

`recon-ng workspaces create mi_proyecto`

Esto te ayuda a separar datos por objetivo.

---

### 🔐 Añadir claves API

Muchos módulos requieren API keys para funcionar (p. ej., Shodan, LinkedIn, Hunter).

`keys add shodan_api TU_API_KEY keys add github_api TU_API_KEY`

Ver todas las claves:

`show keys`

---

### 📦 Listar módulos

`marketplace search`

Instalar uno:

`marketplace install recon/domains-hosts/hackertarget`

---

### 🔄 Cargar y usar un módulo

`modules load recon/domains-hosts/hackertarget show options options set SOURCE target.com run`

Esto ejecuta el módulo sobre la **fuente** (dominio, nombre, correo, etc.) que quieras investigar.

---

### 🗃 Ver resultados

Después de ejecutar módulos, puedes ver lo recolectado:

`show hosts show contacts show domains`

---

### 📑 Exportar o reportar

Puedes generar reportes automáticamente si quieres compartir o analizar con otras herramientas.

---

## 🧩 Módulos frecuentes y qué hacen

|Tipo|Ejemplo|Uso principal|
|---|---|---|
|DNS|`recon/domains-hosts/baidu_site`|Encontrar subdominios|
|Contactos|`recon/domains-contacts/whois_pocs`|Extraer correos de WHOIS|
|Búsqueda|`recon/contacts-hosts/google_site`|Buscar en motores|
|Integración API|`recon/hosts-hosts/shodan_hostname`|Buscar en Shodan|

---

## 🧠 Buenas prácticas OSINT

- Empieza siempre por un **workspace** distinto por objetivo
    
- Añade claves API para potenciar resultados
    
- Usa `options show` antes de ejecutar módulos
    
- Organiza salidas en reportes para futuras consultas
    
- Respeta límites de API y licencias de uso
    

---

## ⚠️ Limitaciones

- Sin API keys muchos módulos no devolverán mucho
    
- OSINT es **pasivo** (no entra en sistemas)
    
- Depende de fuentes públicas disponibles
    

---