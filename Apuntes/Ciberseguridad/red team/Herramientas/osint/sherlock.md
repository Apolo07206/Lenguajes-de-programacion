Herramienta OSINT que busca un nombre de usuario en decenas de sitios web, redes sociales y plataformas para decirte dónde existe esa cuenta y dónde no.

Sirve para:

- Recolectar perfiles de una persona objetivo
    
- Investigación OSINT
    
- Atribución en CTFs
    
- Doxing (solo en entornos legales)
    
- Perfiles para análisis de amenazas
    

No hackea, no rompe nada. Solo consulta plataformas y reporta coincidencias.


https://youtube.com/shorts/mnTaA9OPTtw?si=gBXX0xW0_Te3-q8B
---

## Instalación útil y realista

En Kali suele venir o puedes instalarlo con:

`git clone https://github.com/sherlock-project/sherlock.git cd sherlock python3 -m pip install -r requirements.txt`

---

## Uso básico

Buscar un usuario:

`python3 sherlock <usuario>`

Ejemplo:

`python3 sherlock ferneysec`

Esto genera una lista con:

- Plataformas donde el usuario **existe**
    
- Plataformas donde **no existe**
    
- En algunos casos, enlaces directos
    

---

## Opciones útiles

Guardar resultados en archivos:

`python3 sherlock <usuario> --output results.txt`

Especificar tiempo de espera para evitar falsos negativos:

`python3 sherlock <usuario> --timeout 5`

Buscar varios usuarios al mismo tiempo:

`python3 sherlock ferney juanpepe darkwolf`

Ignorar errores ruidosos:

`python3 sherlock <usuario> --print-found`

---

## Qué información te da realmente

Si el usuario existe en una plataforma, te da:

- Perfil (URL)
    
- Confirmación
    
- Posibilidad de visitar y mirar información pública
    

Con eso puedes obtener:

- Fotos
    
- Alias ligados
    
- Fechas
    
- Emails
    
- Ciudades
    
- Bio
    
- Amigos/seguidores
    
- Otros identificadores (GitHub, Discord, Instagram)
    

---

## Limitaciones realistas

Sherlock **no** te dice:

- Contraseñas
    
- Datos privados
    
- IP del usuario
    
- Emails ocultos
    
- Información que requiere login
    

También falla si:

- El sitio bloquea scraping
    
- El usuario requiere inicio de sesión
    
- La plataforma usa métodos antiscraping modernos
    

---

## Para qué te sirve en Red Team y OSINT (Ferney)

En tu caso, como estás orientado a Red Team + Blue Team, Sherlock ayuda a:

**En Red Team:**

- Recolectar perfiles de empleados
    
- Encontrar nombres de usuarios reutilizados
    
- Conseguir correos indirectamente
    
- Preparar ataques de ingeniería social mejor dirigidos
    
- Saber dónde alguien sube código (GitHub, GitLab)
    
- Ver leaks en plataformas viejas
    

**En Blue Team/Forense:**

- Atribuir amenazas
    
- Conectar alias en investigaciones
    
- Perfilamiento de sospechosos
    
- Documentar comportamiento online
    

---

## Integraciones interesantes

Puedes combinar Sherlock con:

- **Holehe** para email-to-platform lookup
    
- **Have I Been Pwned** para leaks
    
- **Username Generator** para variaciones
    
- **theHarvester** para dominios y correos
    
- **Maltego** para vínculos visuales
    
- **SpiderFoot** para automatización
    

---

## Ejemplo de flujo OSINT rápido

1. Tienes un alias: `darkwolf23`
    
2. Lo pasas por Sherlock
    
3. Encuentras GitHub y Instagram
    
4. En GitHub hallas email
    
5. Con email buscas leaks
    
6. Con leaks encuentras contraseñas antiguas
    
7. Tienes una ruta completa para pivotear
    

---

## Cosas que muchos no saben

- Sherlock tiene archivo de **sitios** editable
    
- Puedes añadir plataformas nuevas
    
- Puedes correrlo con **Tor** para anonimizar
    
- Puede usarse en **CTFs OSINT**
    
- Funciona en Linux, Mac, Windows
    

Ejecutar con Tor:

`torsocks python3 sherlock <usuario>`

---