---
title: Gestión de Procesos y Control en Linux
area: programacion
tipo: fundamento
tags: [programacion, bash, linux, procesos]
created: 2026-10-12
updated: 2026-11-01
related: ["[[MOC-programacion]]", "[[Ruta Bash]]", "[[Comandos de Linux]]", "[[Permisos, Servicios y Logs en Linux]]"]
---

# Gestión de Procesos y Control en Linux

## 1. ¿Qué es un proceso?

Un **proceso** es cualquier programa o tarea que se está ejecutando en el sistema operativo en un momento dado (una terminal abierta, un script en Python, un servidor local, etc.). Cada proceso recibe un número de identificación único llamado **PID (Process ID)** para que el kernel y el usuario puedan controlarlo.

## 2. Monitoreo de Procesos: Ver qué está corriendo

### `ps aux` — La foto instantánea

Muestra una lista fija con todos los procesos activos en ese preciso instante.

- **Columnas clave:**
    
    - `USER`: Quién lanzó el proceso.
        
    - `PID`: El número de identificación único.
        
    - `%CPU` / `%MEM`: Consumo de procesador y memoria RAM.
        
    - `COMMAND`: El comando o programa exacto que se ejecutó.
        
- _Truco:_ Se suele combinar con `grep` para buscar algo concreto:
    
    
    ```
    ps aux | grep python
    ```
    

### `top` — Monitoreo en vivo

Muestra la misma lista de procesos pero actualizándose en tiempo real (similar al Administrador de tareas de Windows).

- Presiona **`q`** para salir de la pantalla.
    

### `htop` — El monitor interactivo avanzado

La evolución moderna de `top` con interfaz visual en color.

- Permite usar el mouse, ordenar por columnas y ver barras de consumo exactas de RAM y CPU.
    
- Para cerrar un proceso: Lo buscas con las flechas y presionas **`F9`**.
    
- Para salir: Presiona **`F10`** o **`q`**.
    
- _(Instalación si no viene por defecto: `sudo apt install htop`)_.
    

## 3. Terminación de Procesos: `kill` y `pkill`

Cuando un programa se congela o ya no lo necesitas, puedes enviarle una señal para cerrarlo.

### `kill <PID>` (Cierre ordenado — `SIGTERM`)

Envía la señal estándar pidiendo al programa que finalice de forma limpia.

- El programa guarda archivos temporales y cierra conexiones antes de apagarse.
    
    
    ```
    kill 4859
    ```
    

### `kill -9 <PID>` (Cierre forzado — `SIGKILL`)

Mata al proceso de inmediato a la fuerza, sin pedirle permiso.

- **Úsalo solo como último recurso** si el programa está totalmente bloqueado, ya que podrías perder datos no guardados.
    
    
    ```
    kill -9 4859
    ```
    

### `pkill <nombre>` (Matar por nombre)

Te ahora el paso de buscar el PID numérico con `ps aux`.

- Cierra todos los procesos que coincidan con el nombre exacto que escribas:
    
    
    ```
    pkill python
    ```
    

## 4. Regla de Oro de Permisos

- Solo puedes manipular (matar o gestionar) **los procesos que pertenecen a tu propio usuario**.
    
- Para cerrar o modificar procesos del sistema o de otros usuarios, necesitas privilegios de superusuario anteponiendo `sudo`:
    
    
    ```
    sudo kill <PID>
    ```

---

## Ver también

- [[MOC-programacion]]
- [[Ruta Bash]]
- [[Comandos de Linux]]
- [[Permisos, Servicios y Logs en Linux]]
- [[08-Comandos básicos para ciberseguridad en Bash]]
