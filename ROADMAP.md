# Hoja de ruta

## Fase 0 — Foundation
- [x] Crear repositorio público.
- [x] Definir identidad y alcance.
- [x] Definir arquitectura single-node.
- [x] Documentar Ubuntu Server y Proxmox LXC.
- [x] Publicar borrador de licencia de comunidad.
- [x] Definir alcance Java + Bedrock.
- [x] Elegir Paper como primer motor Java mediante integración externa, sin fork.
- [x] Documentar separación de licencias del panel y runtimes.
- [ ] Revisar licencia jurídicamente.
- [ ] Definir matriz Minecraft / Paper / Java / BDS.
- [ ] Definir contrato y capacidades de Engine Adapter.

## Fase 1 — Core V1.0
- [ ] Backend FastAPI y frontend responsive.
- [ ] Autenticación, roles, SQLite y migraciones.
- [ ] Instalador Ubuntu y servicio systemd.
- [ ] Instance Manager y contrato de adaptadores.
- [ ] Registro/validación de BDS.
- [ ] Registro/validación de Paper y JVM.
- [ ] Crear y administrar instancias locales Java y Bedrock.
- [ ] Start / stop / restart, estado, logs y consola en vivo.
- [ ] Configuración específica por motor.
- [ ] Validación de puertos y protocolos TCP/UDP.

## Fase 2 — Administración del juego
- [ ] Jugadores online según motor.
- [ ] Allowlist / whitelist y permisos / operadores.
- [ ] Mundos y dimensiones.
- [ ] File manager limitado a la instancia.
- [ ] Backups y restauración.
- [ ] Gestión de plugins Paper con controles de seguridad (no requisito para la integración inicial).

## Fase 3 — Operación y estabilidad
- [ ] Monitoreo CPU/RAM/disco por instancia.
- [ ] Actualización de BDS y Paper con backup previo.
- [ ] Detección de compatibilidad Java.
- [ ] Auditoría y recuperación.
- [ ] Pruebas Ubuntu 22.04/24.04 y Proxmox LXC no privilegiado.
- [ ] Primer release estable.

## Fuera de alcance V1.0
- Clúster y nodos remotos.
- Administración/orquestación de Proxmox.
- Fork o modificación del núcleo de Paper.
- Redistribución de binarios de terceros sin verificar sus términos.
- Marketplace comercial de plugins o extensiones.
