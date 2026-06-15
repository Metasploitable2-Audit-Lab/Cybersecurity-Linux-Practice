# 📉 Laboratorio Técnico Práctico: Monitoreo, Permisos y Respuesta a Incidentes en Linux (Módulos 1 al 4)

Este documento contiene la documentación de los laboratorios prácticos desarrollados para simular el rol de un Analista de Ciberseguridad en un Centro de Operaciones de Seguridad (SOC). Incluye comandos de auditoría local, filtros forenses y mitigación de amenazas perimetrales.

---

## 🗺️ Módulo 1: Orientación y Navegación del Sistema

### 🕵️‍♂️ Enfoque del SOC
Reconocimiento inicial de la infraestructura tras una alerta de intrusión. Identificación de la ubicación actual y auditoría visual de archivos ocultos que puedan contener artefactos maliciosos.

* **`pwd`** (Print Working Directory): Muestra la ruta absoluta del directorio donde está parado el analista.
* **`ls -la`**: Lista todos los archivos del directorio actual, incluyendo los archivos ocultos (los que comienzan con `.`) con sus metadatos de usuario, grupo, tamaño y permisos.

---

## 📁 Módulo 2: Seguridad de Archivos y Filtros Forenses

### 🕵️‍♂️ Enfoque del SOC
Blindaje inmediato de archivos confidenciales o scripts comprometidos encontrados en el sistema, asegurando que solo el administrador o propietario legítimo pueda acceder a ellos. Uso de cañerías para filtrar datos masivos.

* **`touch <archivo>`**: Creación rápida de archivos de texto o evidencias vacías.
* **`chmod 700 <archivo>`**: Configuración de permisos duros. Otorga control total (`rwx`) al propietario y restringe absolutamente todo acceso (`---`) a grupos y otros usuarios del sistema.
* **`|` (Cañería / Pipe)**: Redirige la salida de un comando para que sirva de entrada en el siguiente.
* **`grep <término>`**: Filtro forense de cadenas de texto específicas.

---

## 💾 Módulo 3: Monitoreo Local y Control de Procesos

### 🕵️‍♂️ Enfoque del SOC
Identificación de anomalías en el rendimiento del sistema operativo causadas por scripts maliciosos (ej. mineros de criptomonedas o bucles infinitos) y ejecución de protocolos de contención forzada.

* **`top`**: Monitor dinámico en tiempo real que muestra el consumo global de CPU, memoria RAM y ID de Procesos (PID).
* **`df -h`**: Muestra el almacenamiento disponible en los discos en formato de lectura humana para evitar puntos ciegos por discos llenos.
* **`ps aux`**: Captura estática de todos los procesos activos en la memoria RAM del sistema.
* **`kill -9 <PID>`**: Envía una señal de terminación forzada e inapelable administrada por el kernel para matar un proceso de forma inmediata.

> ⚠️ **Lección Forense Aprendida:** El directorio virtual `/proc` representa la memoria viva administrada por el Kernel. Linux protege este sistema de archivos impidiendo de forma estricta la modificación de sus permisos (`chmod: Operación no permitida`), por lo que la mitigación se realiza directamente finalizando el PID ejecutor.

---

## 📡 Módulo 4: Redes y Conectividad Perimetral

### 🕵️‍♂️ Enfoque del SOC
Auditoría de conexiones entrantes y salientes, detección de sockets sospechosos levantados en segundo plano por atacantes y mapeo de direccionamiento IP interno.

* **`ip a`**: Identifica las tarjetas de interfaz de red activas en la máquina y sus respectivas direcciones IP privadas.
* **`ss -tulnp`**: Muestra un listado detallado de todos los puertos locales en estado de escucha (`LISTEN`), especificando protocolos (TCP/UDP) y revelando el PID y proceso dueño de dicha conexión.

---

## 📊 Bitácora de Casos Prácticos Resueltos en la Terminal

### Caso 1: Detección y Contención de Bucle Malicioso persistente
* **Comando de Activación en segundo plano:** `bash -c "while true; do sleep 5; done" &`
* **Comando de Detección:** `ss -tulnp` y `ps aux | grep bash`
* **PID Identificado:** `3307`
* **Comando de Mitigación:** `sudo kill -9 3307`
* **Resultado:** Sistema verificado y libre de hilos persistentes de Bash.

### Caso 2: Simulación de Minero Oculto (`crypto_miner`)
* **Comando de Activación en segundo plano:** `sh -c "while true; do sleep 2; done" &`
* **Comando de Detección:** `ps aux | grep sh`
* **PID Identificado:** `2104`
* **Comando de Mitigación:** `sudo kill -9 2104`
* **Resultado:** Proceso fulminado exitosamente. Telemetría de procesos limpia.

---
⚡ *Portfolio de Ciberseguridad desarrollado en entornos de simulación Blue Teaming.*
