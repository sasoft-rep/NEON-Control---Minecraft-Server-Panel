# Configuración base — NEON Control

Este documento define valores y decisiones iniciales. El formato definitivo se implementará junto con el backend.

## 1. Valores iniciales

| Parámetro | Valor |
|---|---|
| Nombre | NEON Control |
| Ediciones | Minecraft Java + Bedrock |
| Arquitectura | Single-node |
| Instancias | Varias instancias locales |
| Backend | FastAPI |
| Frontend | React + TypeScript |
| DB | SQLite |
| Motor Bedrock | Bedrock Dedicated Server |
| Motor Java | PaperMC, integración externa |
| Puerto Bedrock | UDP 19132 |
| Puerto Java | TCP 25565 |
| Bind API durante setup | 127.0.0.1 |
| Servicio | systemd |

## 2. Modelo de instancia

Registrar ID, nombre, edición (java/bedrock), motor (paper/bedrock_dedicated_server), versión, rutas de runtime y datos, ejecutable/JAR, dependencias (Java si corresponde), puerto/protocolo, configuración, logs, mundos, backups, estado y capacidades del adaptador.

No compartir directorios de mundo entre procesos simultáneos.

## 3. Configuración Bedrock

Cada BDS mantiene su server.properties, allowlist, permisos y mundos propios. Opciones de interés: server-name, gamemode, difficulty, allow-cheats, max-players, online-mode, allow-list, server-port, server-portv6, view-distance, tick-distance, level-name, level-seed y default-player-permission-level.

Los valores dependen de la versión. Preservar propiedades desconocidas y validar según el runtime instalado.

## 4. Configuración Java / Paper

Cada instancia Paper mantiene sus propios server.properties, eula.txt, config, plugins y datos, logs y mundos/dimensiones.

Opciones comunes de server.properties: server-port (TCP 25565 por defecto), server-ip, max-players, online-mode, motd, level-name, difficulty, gamemode, view-distance, simulation-distance y white-list.

La disponibilidad y los valores dependen de la versión Minecraft/Paper. Preservar opciones desconocidas y evitar sobrescribir configuraciones de plugins.

Paper requiere una versión Java compatible con el build instalado. La detección/selección de JVM debe formar parte de la validación del runtime.

Los plugins ejecutan código de terceros. Su instalación requiere controles de origen, permisos, backup y advertencias; no se considera requisito del primer soporte básico de Paper.

## 5. Puertos y exposición

| Motor | Puerto predeterminado | Protocolo |
|---|---:|---|
| Bedrock Dedicated Server | 19132 | UDP |
| Paper / Java | 25565 | TCP |

Si se modifican, mostrar las reglas de firewall/NAT necesarias. El panel y la API no deben quedar expuestos públicamente por defecto.

## 6. Secretos

Separar configuración no sensible y secretos. No subir contraseñas, tokens ni claves privadas. Los ejemplos usan valores ficticios.

## 7. Backups

Incluir, según motor: configuración, propiedades, allowlist/whitelist, permisos/operadores, mundos/dimensiones, plugins/add-ons y metadatos. La base SQLite del panel tiene backup propio. Documentar retención, ubicación externa y pruebas de restauración. Para consistencia, detener o guardar correctamente el estado del servidor antes de copiar mundos.
