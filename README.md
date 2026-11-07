# NEON Control

Panel web independiente para administrar servidores Minecraft Java Edition y Bedrock Edition en Linux.

> **Estado:** documentación y definición de la base técnica. La V1.0 está en planificación; las funciones descritas como objetivo no deben interpretarse como ya implementadas.

## Objetivo

NEON Control administra una o varias instancias locales desde una única máquina Linux (arquitectura single-node). Soportará:

- **Bedrock Edition:** Minecraft Bedrock Dedicated Server (BDS), obtenido desde canales oficiales.
- **Java Edition:** PaperMC como primer motor Java, integrado como runtime externo.

La interfaz, API, base de datos, gestión de instancias y adaptadores son componentes propios. Paper no será un fork ni se incorporará su código al repositorio.

## Plataformas objetivo

- Ubuntu Server 22.04 LTS y 24.04 LTS, x86-64.
- Contenedores LXC Ubuntu sobre Proxmox VE.
- Máquinas virtuales Linux sobre Proxmox.

## Stack propuesto

- Backend Python 3 + FastAPI.
- Frontend React + TypeScript.
- API REST + WebSocket.
- SQLite inicialmente.
- systemd.
- Adaptadores separados para BDS y Paper.
- JVM compatible con la versión de Paper seleccionada.

Los binarios de BDS y Paper no se incorporan al repositorio. Se obtienen desde sus respectivas fuentes oficiales y conservan sus propios términos.

## Objetivos de la primera versión

- Instalación automatizada en Ubuntu y ejecución en Proxmox LXC.
- Crear y administrar varias instancias locales Java y Bedrock.
- Selección de edición y motor por instancia.
- Iniciar, detener y reiniciar; consola y logs en vivo.
- Configuración específica de cada motor.
- Administración de jugadores y permisos según las capacidades de cada edición.
- Mundos, backups, restauración y monitoreo.
- Autenticación, roles y auditoría.

## Documentación

- [Arquitectura](docs/ARCHITECTURE.md)
- [Instalación Ubuntu](docs/INSTALL-UBUNTU.md)
- [Proxmox LXC](docs/PROXMOX-LXC.md)
- [Configuración](docs/CONFIGURATION.md)
- [Política de licencias](docs/LICENSING.md)
- [Seguridad](SECURITY.md)
- [Hoja de ruta](ROADMAP.md)
- [Contribuciones](CONTRIBUTING.md)
- [Licencia del panel](LICENSE)

## Principios

1. Standalone y local-first.
2. Motor desacoplado mediante adaptadores.
3. Least privilege: panel y servidores no se ejecutan como root.
4. Sin terminal de sistema irrestricta.
5. Backups antes de operaciones destructivas.
6. Separación de licencias entre el panel y los runtimes de terceros.

## Instalación

El instalador automatizado todavía no está disponible. No ejecutes scripts de instalación preliminares en producción sin revisar su contenido.

## Licencia

El código propio de NEON Control se publica bajo NEON Community License 1.0 (NCL-1.0), una licencia source-available con restricciones comerciales; no está aprobada por OSI. Paper, BDS y las dependencias conservan sus licencias. Consultá [LICENSE](LICENSE) y [LICENSING](docs/LICENSING.md).

## Estado del proyecto

- Versión objetivo: 1.0.0
- Fase: Foundation / documentación y arquitectura
- Ediciones: Java + Bedrock
- Primer motor Java: Paper, integración externa
- Arquitectura: single-node
