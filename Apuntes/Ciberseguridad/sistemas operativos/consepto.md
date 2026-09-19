# Sistemas Operativos y Ciberseguridad

## Concepto Fundamental

Un **sistema operativo (SO)** es el software fundamental que gestiona los recursos de hardware de un equipo y proporciona servicios esenciales para las aplicaciones. Actúa como intermediario entre el usuario, las aplicaciones y el hardware. Desde la perspectiva de ciberseguridad, el SO es tanto la primera línea de defensa como un objetivo principal para los atacantes, ya que su compromiso implica control total del sistema.

## Arquitectura de Sistemas Operativos

**Kernel (Núcleo)**: El componente central que gestiona CPU, memoria, dispositivos y procesos. Opera en modo privilegiado con acceso completo al hardware. Las vulnerabilidades en el kernel son críticas porque permiten escalada de privilegios y ejecución de código a nivel del sistema.

**Modo Usuario vs Modo Kernel**: Los SO modernos separan estos dos modos de ejecución. Las aplicaciones operan en modo usuario con privilegios limitados, mientras que el kernel opera con privilegios completos. Esta separación es una medida fundamental de seguridad que previene que aplicaciones maliciosas accedan directamente al hardware.

**System Calls (Llamadas al Sistema)**: Interfaz controlada que permite a las aplicaciones solicitar servicios del kernel. Cada llamada debe ser validada para prevenir abuso y escalada de privilegios.

## Principales Sistemas Operativos

### Windows

**Características de Seguridad**: Windows Defender, BitLocker para cifrado de disco, Windows Firewall, Control de Cuentas de Usuario (UAC), Windows Hello para autenticación biométrica, y sandboxing de aplicaciones.

**Vectores de Ataque Comunes**: Explotación de vulnerabilidades en servicios SMB, RDP mal configurado, ataques a Active Directory, macros maliciosas en documentos Office, escalada de privilegios mediante tokens, y persistencia a través del registro y servicios.

**Hardening**: Deshabilitar servicios innecesarios, implementar políticas de grupo restrictivas, actualizar parches regularmente, usar LAPS para contraseñas de administrador local, habilitar auditoría detallada, y aplicar AppLocker o WDAC para control de aplicaciones.

### Linux

**Características de Seguridad**: Modelo de permisos robusto (usuarios/grupos/otros), SELinux o AppArmor para control de acceso obligatorio, iptables/nftables para firewall, gestión granular de capacidades del kernel, y separación estricta de privilegios.

**Vectores de Ataque Comunes**: Explotación de servicios expuestos (SSH, Apache, nginx), escalada de privilegios mediante binarios SUID mal configurados, inyección de comandos, vulnerabilidades en el kernel, y rootkits que ocultan presencia de malware.

**Hardening**: Mantener sistema actualizado, deshabilitar root login vía SSH, usar autenticación por claves SSH, configurar SELinux en modo enforcing, limitar privilegios sudo, auditar logs con auditd, y minimizar superficie de ataque instalando solo paquetes necesarios.

### macOS

**Características de Seguridad**: Gatekeeper para validar software firmado, XProtect como antimalware, FileVault 2 para cifrado de disco, System Integrity Protection (SIP) que protege archivos del sistema, y sandboxing obligatorio para apps de la App Store.

**Vectores de Ataque Comunes**: Bypass de Gatekeeper mediante aplicaciones no firmadas, explotación de vulnerabilidades en software de terceros, ataques de phishing dirigidos, y malware disfrazado de software legítimo.

**Hardening**: Habilitar FileVault, mantener Gatekeeper activo, instalar solo software de fuentes confiables, usar firewall integrado, y mantener actualizado tanto el SO como las aplicaciones.

### Móviles (Android/iOS)

**Android**: Basado en kernel Linux con capa de abstracción. Utiliza sandboxing a nivel de aplicación, permisos granulares desde Android 6, Google Play Protect, y cifrado de disco completo. Vulnerable a fragmentación (dispositivos sin actualizaciones) y malware en tiendas no oficiales.

**iOS**: Arquitectura cerrada con control estricto de aplicaciones. Implementa cifrado por hardware, validación obligatoria de código, sandbox riguroso, y actualizaciones uniformes. Menos vulnerable que Android pero objetivo de ataques dirigidos mediante jailbreak y spyware como Pegasus.

## Gestión de Procesos y Seguridad

**Aislamiento de Procesos**: Cada proceso opera en su propio espacio de memoria protegido. La violación de este aislamiento mediante exploits de corrupción de memoria es una técnica común de ataque.

**Threads (Hilos)**: Múltiples hilos dentro de un proceso comparten memoria. Las condiciones de carrera (race conditions) pueden crear vulnerabilidades explotables.

**Servicios y Daemons**: Procesos en segundo plano que ejecutan con privilegios elevados. Deben ser minimizados, actualizados y monitoreados, ya que representan superficie de ataque permanente.

## Gestión de Memoria

**Protecciones Modernas**: ASLR (Address Space Layout Randomization) aleatoriza ubicaciones de memoria para dificultar exploits. DEP/NX (Data Execution Prevention) previene ejecución de código en áreas de datos. Stack canaries detectan desbordamientos de buffer.

**Vulnerabilidades Comunes**: Buffer overflow, use-after-free, double-free, y heap spraying. Estas vulnerabilidades permiten ejecución de código arbitrario y son la base de muchos exploits.

## Sistema de Archivos y Permisos

**Windows (NTFS)**: Utiliza Access Control Lists (ACLs) detalladas que especifican permisos por usuario y grupo. Soporta cifrado a nivel de archivo (EFS), compresión y journaling.

**Linux (ext4, XFS, Btrfs)**: Permisos tradicionales (lectura/escritura/ejecución) más atributos extendidos y ACLs. Los permisos especiales (SUID, SGID, sticky bit) requieren configuración cuidadosa para evitar escalada de privilegios.

**Configuraciones Inseguras**: Archivos world-writable, binarios SUID innecesarios, permisos excesivos en directorios críticos, y ACLs mal configuradas son vectores comunes de ataque.

## Virtualización y Contenedores

**Hypervisores**: Tipo 1 (bare-metal como VMware ESXi, Hyper-V) ejecutan directamente sobre hardware. Tipo 2 (VirtualBox, VMware Workstation) ejecutan sobre un SO host. Las vulnerabilidades de escape de VM permiten al atacante salir de la máquina virtual y comprometer el host.

**Contenedores (Docker, Kubernetes)**: Comparten kernel del host pero aíslan espacios de usuario. Más ligeros que VMs pero con menor aislamiento. Requieren imágenes seguras, escaneo de vulnerabilidades, limitación de capacidades, y no ejecutar como root.

**Seguridad**: Ambas tecnologías requieren parches constantes, configuración apropiada de red y almacenamiento, y monitoreo de actividad anómala entre entornos virtualizados.

## Registro de Eventos y Auditoría

**Windows Event Logs**: Registra eventos de seguridad, sistema y aplicaciones. Fuentes clave incluyen Security.evtx (logons, cambios de privilegios), System.evtx (servicios, drivers) y Application.evtx.

**Linux Syslog**: Centraliza logs en /var/log/. Archivos críticos incluyen auth.log (autenticación), syslog (mensajes del sistema), y kern.log (kernel). Auditd proporciona auditoría detallada de llamadas al sistema.

**Análisis Forense**: Los logs son fundamentales para detección de intrusiones, análisis post-incidente y cumplimiento normativo. Deben protegerse contra manipulación y enviarse a SIEM centralizado.

## Actualizaciones y Gestión de Parches

**Criticidad**: Las vulnerabilidades sin parchear son la causa principal de compromiso de sistemas. Exploits como EternalBlue (SMB) y BlueKeep (RDP) han causado infecciones masivas.

**Estrategia**: Implementar gestión centralizada de parches, priorizar actualizaciones críticas, mantener inventario de sistemas, probar en entorno de desarrollo antes de producción, y establecer ventanas de mantenimiento regulares.

**Zero-Days**: Vulnerabilidades sin parche disponible requieren mitigaciones compensatorias como deshabilitar servicios afectados, implementar reglas de firewall, o usar detección basada en comportamiento.

## Técnicas de Ataque Específicas del SO

**Escalada de Privilegios**: Explotación de vulnerabilidades en el kernel, mal configuración de permisos, abuso de servicios mal programados, o captura de credenciales en memoria.

**Persistencia**: Modificación del registro de Windows, creación de servicios, tareas programadas, scripts de inicio, backdoors en bootloader, o rootkits a nivel de kernel.

**Living off the Land**: Uso de herramientas nativas del SO (PowerShell, WMI, bash) para ejecutar ataques sin introducir malware detectable.

**Ransomware**: Cifrado de archivos del usuario mediante API del sistema. Requiere backups offline, segmentación de red y detección basada en comportamiento anómalo de I/O.

## Mejores Prácticas de Seguridad

Aplica el principio de mínimo privilegio en todas las cuentas. Deshabilita servicios y características innecesarias. Implementa autenticación multifactor para accesos administrativos. Cifra datos en reposo y en tránsito. Mantén inventario actualizado de software instalado. Realiza auditorías de seguridad periódicas. Implementa whitelisting de aplicaciones cuando sea posible. Monitorea logs centralizadamente. Segmenta la red para limitar propagación de compromisos. Establece políticas de contraseñas robustas. Capacita usuarios sobre ingeniería social. Realiza backups regulares y prueba restauraciones. Implementa EDR (Endpoint Detection and Response) para detección avanzada.

El sistema operativo es el fundamento sobre el cual se construye toda la seguridad informática. Un SO comprometido invalida cualquier control de seguridad implementado sobre él, por lo que su protección debe ser prioridad máxima en cualquier estrategia de ciberseguridad.
