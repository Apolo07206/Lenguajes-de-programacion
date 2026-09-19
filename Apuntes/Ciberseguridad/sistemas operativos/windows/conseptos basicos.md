## PowerShell

**¿Qué es?** PowerShell es como una "línea de comandos mejorada" de Windows. Mientras que CMD (el símbolo del sistema tradicional) es básico, PowerShell es mucho más poderoso y te permite automatizar tareas y administrar el sistema completo.

**¿Por qué es importante en ciberseguridad?**

- Los atacantes lo usan porque ya está instalado en Windows (no levantan sospechas)
- Puede ejecutar comandos directamente en memoria sin tocar el disco (más difícil de detectar)
- Tiene acceso a casi todo el sistema operativo

**Comandos básicos importantes:**

- `Get-Process`: Ver procesos en ejecución
- `Get-Service`: Listar servicios del sistema
- `Get-EventLog`: Consultar registros de eventos
- `Get-LocalUser`: Ver usuarios del sistema

**Técnicas de ataque comunes:**

- **PowerShell ofuscado**: Los atacantes codifican sus scripts en Base64 o usando caracteres raros para evitar detección
- **Ejecución sin archivos (fileless)**: Correr código malicioso directamente en memoria RAM
- **Bypass de políticas**: Usar `-ExecutionPolicy Bypass` para saltarse restricciones

**Defensas:**

- Activar el logging de PowerShell (registrar todos los comandos ejecutados)
- Restringir la ejecución con políticas de grupo
- Monitorear comandos sospechosos como `Invoke-Expression`, `DownloadString`, etc.

## Servicios de Windows

**¿Qué son?** Los servicios son programas que corren en segundo plano sin que los veas. Por ejemplo: el antivirus, la impresora, las actualizaciones de Windows, etc.

**¿Por qué son críticos en seguridad?**

- Muchos servicios corren con permisos de SYSTEM (el usuario más poderoso)
- Si un atacante controla un servicio, puede tener control total del equipo

**Gestión de servicios:**

- Ver servicios: `services.msc` o `Get-Service` en PowerShell
- Estados: Running (corriendo), Stopped (detenido), Paused (pausado)
- Tipo de inicio: Automático, Manual, Deshabilitado

**Ataques relacionados con servicios:**

1. **Servicios mal configurados**: Si un servicio tiene permisos débiles, un atacante puede modificarlo para ejecutar código malicioso
2. **DLL Hijacking**: Reemplazar las librerías (DLLs) que usa un servicio legítimo
3. **Unquoted Service Paths**: Servicios con rutas sin comillas que Windows puede interpretar mal

**Ejemplo de vulnerabilidad:**

```
Servicio: C:\Program Files\Mi Aplicacion\servicio.exe
Windows podría intentar ejecutar: C:\Program.exe (si existe)
```

**Comandos útiles:**

- `sc query`: Ver estado de servicios
- `sc qc [nombre]`: Ver configuración de un servicio específico
- `icacls [ruta]`: Ver permisos de archivos/carpetas

## Registro de Eventos (Event Logs)

**¿Qué es?** Es como el "diario" de Windows donde se registra todo lo que pasa: inicios de sesión, errores, instalaciones de programas, etc.

**Ubicación:**

- Visor de eventos: `eventvwr.msc`
- Archivos físicos: `C:\Windows\System32\winevt\Logs\`

**Registros principales:**

1. **Security**: Auditoría de seguridad (inicios de sesión, accesos a archivos)
2. **System**: Eventos del sistema (servicios, drivers, arranque)
3. **Application**: Aplicaciones instaladas

**Event IDs críticos para ciberseguridad:**

- **4624**: Inicio de sesión exitoso
- **4625**: Intento de inicio de sesión fallido (posible ataque de fuerza bruta)
- **4672**: Inicio de sesión con privilegios especiales
- **4688**: Creación de un nuevo proceso
- **4698-4702**: Tareas programadas (los atacantes las usan para persistencia)
- **7045**: Nuevo servicio instalado

**¿Qué buscan los atacantes?**

- Borrar los logs para ocultar sus rastros: `wevtutil cl Security`
- Desactivar el registro de eventos
- Llenar los logs con basura para dificultar el análisis

**Buenas prácticas:**

- Enviar logs a un servidor central (SIEM)
- Aumentar el tamaño de los archivos de log
- Activar auditoría avanzada para capturar más detalles

## Active Directory (AD)

**¿Qué es?** Active Directory es el "cerebro" de las redes de empresas con Windows. Controla todos los usuarios, computadoras, permisos y políticas de seguridad en una organización.

**Componentes principales:**

- **Domain Controller (DC)**: El servidor que controla todo
- **Domain**: El "reino" de tu red (ejemplo: empresa.local)
- **OU (Organizational Units)**: Carpetas para organizar usuarios y equipos
- **GPO (Group Policy Objects)**: Reglas que se aplican a todos los equipos

**¿Por qué es importante en seguridad?** Quien controla el Active Directory controla toda la red. Es el objetivo principal de los atacantes.

**Ataques comunes contra AD:**

1. **Pass-the-Hash**: Usar el hash de una contraseña sin necesidad de descifrarla
2. **Kerberoasting**: Extraer tickets Kerberos y crackear contraseñas offline
3. **Golden Ticket**: Crear tickets falsos para acceder a todo como administrador
4. **DCSync**: Simular ser un Domain Controller para robar todas las contraseñas
5. **LLMNR/NBT-NS Poisoning**: Interceptar autenticaciones en la red

**Cuentas críticas:**

- **Administrator**: El usuario con más poder
- **Domain Admins**: Grupo con control total del dominio
- **Enterprise Admins**: Control sobre múltiples dominios
- **krbtgt**: Cuenta especial cuya contraseña se usa para firmar tickets Kerberos

**Herramientas de enumeración (reconocimiento):**

- `BloodHound`: Mapea relaciones y encuentra caminos de escalada
- `PowerView`: Scripts de PowerShell para enumerar AD
- `Mimikatz`: Extrae credenciales de la memoria

**Defensas:**

- Implementar privilegios mínimos (Least Privilege)
- Usar cuentas separadas para administración
- Activar auditoría de eventos de AD
- Implementar LAPS (contraseñas locales aleatorias)
- Deshabilitar protocolos antiguos (LLMNR, NBT-NS)

## Elevación de Privilegios

**¿Qué es?** Es cuando un atacante que tiene acceso limitado al sistema encuentra una forma de conseguir permisos de administrador o SYSTEM.

**El proceso típico de un ataque:**

1. **Acceso inicial**: El atacante entra (phishing, vulnerabilidad)
2. **Usuario normal**: Tiene permisos limitados
3. **Elevación de privilegios**: Encuentra una forma de volverse administrador
4. **Control total**: Puede hacer lo que quiera

**Técnicas comunes de escalada:**

**1. Servicios mal configurados**

- Buscar servicios que puedas modificar
- Comando: `accesschk.exe -uwcqv "Usuarios autenticados" *`

**2. Permisos débiles en archivos**

- Si puedes modificar un programa que corre como administrador, reemplázalo

**3. DLL Hijacking**

- Reemplazar DLLs que usa un programa privilegiado

**4. Tareas programadas vulnerables**

- Tareas que corren con permisos altos pero tienen configuración débil

**5. AlwaysInstallElevated**

- Configuración que permite instalar programas MSI como administrador
- Verificar: `reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer`

**6. Tokens de acceso**

- Robar tokens de procesos que corren con más privilegios
- Herramienta: `Incognito`

**7. Exploits del kernel**

- Vulnerabilidades en el núcleo de Windows
- Ejemplo: MS16-032, CVE-2021-1675 (PrintNightmare)

**8. Credenciales almacenadas**

- Buscar contraseñas en archivos, registro, historial de comandos
- Ubicaciones comunes:
    - `C:\unattend.xml`
    - Registro: `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon`

**Comandos de reconocimiento (enumeration):**

powershell

```powershell
# Ver información del sistema
systeminfo

# Ver privilegios actuales
whoami /priv

# Ver grupos del usuario
whoami /groups

# Buscar archivos interesantes
dir /s *pass* == *.config

# Ver servicios con permisos débiles
Get-Service | Where-Object {$_.Status -eq "Running"}

# Ver tareas programadas
schtasks /query /fo LIST /v
```

**Herramientas útiles:**

- **WinPEAS**: Script automatizado que busca vulnerabilidades
- **PowerUp**: Módulo de PowerShell para encontrar vías de escalada
- **AccessChk**: Revisar permisos de servicios/archivos
- **Mimikatz**: Extraer credenciales de memoria

**Defensas contra escalada de privilegios:**

- Mantener el sistema actualizado (parches)
- Principio de mínimo privilegio
- Configurar correctamente permisos de servicios y archivos
- Deshabilitar configuraciones peligrosas (AlwaysInstallElevated)
- Usar Windows Defender Credential Guard
- Implementar AppLocker o políticas de restricción de software

---

**Flujo de un ataque típico:**

1. **Reconocimiento**: PowerShell para enumerar el sistema
2. **Encontrar vulnerabilidad**: Servicio mal configurado o exploit
3. **Elevar privilegios**: Convertirse en administrador
4. **Persistencia**: Crear un servicio o tarea programada
5. **Movimiento lateral**: Usar credenciales para atacar Active Directory
6. **Borrar rastros**: Limpiar registros de eventos

**Consejo final para tus apuntes:** Piensa en Windows como una casa. Los servicios son las puertas y ventanas, el registro de eventos es la cámara de seguridad, PowerShell es una herramienta que puede ser usada para bien o mal, Active Directory es el sistema de llaves maestras, y la elevación de privilegios es cuando alguien encuentra la forma de conseguir la llave maestra sin permiso.