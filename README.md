# NEON Control

Panel de administración web para Minecraft Bedrock Dedicated Server en Linux.

> Estado: documentación y definición de la base técnica. La V1.0 está en planificación/desarrollo; las funciones descritas como objetivo no deben interpretarse como ya implementadas.

## Objetivo

NEON Control es un proyecto independiente para administrar una o varias instancias locales de Minecraft Bedrock Dedicated Server desde una interfaz web. Su arquitectura inicial es single-node: panel y servidores de juego se ejecutan en el mismo sistema Linux o contenedor.

## Plataformas objetivo

- Ubuntu Server 22.04 LTS y 24.04 LTS, x86-64.
- Contenedores LXC Ubuntu sobre Proxmox VE.
- Máquinas virtuales Linux sobre Proxmox.
- Otras distribuciones no comprometidas para V1.0.

## Stack propuesto

- Backend: Python 3 + FastAPI.
- Frontend: React + TypeScript.
- API REST y WebSocket.
- SQLite inicialmente.
- systemd para servicios.
- Binarios oficiales de Minecraft Bedrock Dedicated Server para Linux x86-64.

El binario BDS no se redistribuirá en este repositorio. Se obtendrá desde los canales oficiales y el administrador deberá aceptar los términos aplicables.

## Objetivos V1.0

- Instalación automatizada en Ubuntu.
- Ejecución en Proxmox LXC Ubuntu.
- Crear y administrar varias instancias locales.
- Iniciar, detener y reiniciar instancias.
- Consola y logs en vivo.
- Configuración de server.properties.
- Administración de jugadores, allowlist y permisos.
- Gestión básica de mundos.
- Backups y restauración.
- Monitoreo de CPU, RAM y disco.
- Autenticación, roles y auditoría.

## Documentación

- [Arquitectura](docs/ARCHITECTURE.md)
- [Instalación Ubuntu](docs/INSTALL-UBUNTU.md)
- [Proxmox LXC](docs/PROXMOX-LXC.md)
- [Configuración](docs/CONFIGURATION.md)
- [Seguridad](SECURITY.md)
- [Hoja de ruta](ROADMAP.md)
- [Contribuciones](CONTRIBUTING.md)
- [Licencia](LICENSE)

## Principios

1. Standalone: no requiere cuentas ni servicios externos.
2. Local-first: administra procesos BDS del mismo host.
3. Least privilege: ni el panel ni BDS se ejecutan como root.
4. No arbitrary shell: la API no ofrece terminal de sistema irrestricta.
5. Backups antes de operaciones destructivas y actualizaciones.
6. Configuración, logs y documentación accesibles al administrador.

## Instalación

El instalador automatizado todavía no está disponible. Consultá la documentación y no ejecutes scripts de instalación de versiones preliminares en producción sin revisar su contenido.

## Licencia

NEON Control se publica bajo NEON Community License 1.0 (NCL-1.0), una licencia de fuente disponible con restricciones comerciales y obligación de publicar modificaciones. No es una licencia Open Source aprobada por OSI. Leé LICENSE antes de usar, modificar o redistribuir el proyecto.

## Estado del proyecto

- Versión objetivo: 1.0.0
- Fase: Foundation / documentación y arquitectura
- Edición: Minecraft Bedrock
- Arquitectura: single-node
