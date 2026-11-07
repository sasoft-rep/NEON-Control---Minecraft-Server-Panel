# Instalación — Ubuntu Server

**Estado:** guía objetivo para V1.0. El instalador automatizado aún no está publicado.

## 1. Plataforma

- Ubuntu Server 22.04 LTS o 24.04 LTS, x86-64.
- Usuario con sudo para instalación.
- Acceso saliente a Internet.
- Almacenamiento persistente para panel, instancias y backups.

Cada motor tiene requisitos independientes que deberán validarse para la versión seleccionada.

## 2. Preparación del host

Actualizar Ubuntu e instalar herramientas base: ca-certificates, curl, wget, git, tar, unzip, Python 3, venv, pip y build-essential. La versión de Node.js y gestor frontend se fijarán durante la implementación.

### Requisitos según motor

**Bedrock:** BDS oficial para Linux x86-64 y las bibliotecas nativas requeridas por la versión descargada. Verificar los requisitos publicados por Mojang/Microsoft.

**Java / Paper:** JVM compatible con la versión de Paper seleccionada. La versión exacta de Java depende de Minecraft/Paper; no asumir que una única JVM sirve para todas las versiones. El JAR se obtiene desde PaperMC y no se incluye en este repositorio.

El panel deberá detectar y validar los requisitos antes de crear o iniciar una instancia.

## 3. Usuarios y directorios

La instalación final creará usuarios de sistema sin login interactivo, por ejemplo neonctl para el panel y neongame para los procesos de juego. La implementación podrá aislar usuarios por instancia.

- /opt/neon-control — aplicación
- /etc/neon-control — configuración
- /var/lib/neon-control — estado
- /srv/neon-control/instances — instancias
- /srv/neon-control/backups — backups
- /var/log/neon-control — logs

No ejecutar el panel ni los servidores como root.

## 4. Instalación del panel

Cuando exista instalador estable, el flujo será descargar un release, verificar checksum/firma si están disponibles, crear usuarios/directorios, instalar dependencias bloqueadas, desplegar frontend/backend, inicializar SQLite, instalar systemd y configurar acceso seguro.

El instalador aún no existe. No usar comandos curl | bash de fuentes no verificadas.

## 5. Incorporación de motores

- **BDS:** obtener el paquete desde los canales oficiales y aceptar los términos aplicables.
- **Paper:** obtener el JAR desde PaperMC y disponer de una JVM compatible.

Los runtimes se instalarán en directorios separados del código del panel. La instalación deberá mostrar origen, versión y avisos/licencia relevantes. NEON Control no los redistribuye por defecto.

## 6. Red y firewall

| Motor | Puerto predeterminado | Protocolo |
|---|---:|---|
| Bedrock Dedicated Server | 19132 | UDP |
| Paper / Java | 25565 | TCP |

Abrir solo los puertos utilizados. El panel web no debe exponerse sin TLS, autenticación robusta y firewall. Durante la puesta en marcha debe escuchar en localhost o interfaz administrativa confiable.

## 7. Verificación posterior

Validar systemd, API, SQLite, permisos, runtime, dependencias BDS o versión de Java, puertos/firewall y creación/inicio/detención de una instancia de prueba.

## 8. Actualizaciones

Actualizar panel y motores por separado. Antes de actualizar un motor: detener ordenadamente, respaldar configuración/datos/mundos, verificar espacio y compatibilidad, actualizar, validar arranque y restaurar si falla. Una actualización del panel no debe reemplazar runtimes silenciosamente.

## 9. Desinstalación

Distinguir aplicación, configuración, base de datos, runtimes, instancias y backups. Nunca borrar mundos o backups sin confirmación explícita y opción de conservarlos.
