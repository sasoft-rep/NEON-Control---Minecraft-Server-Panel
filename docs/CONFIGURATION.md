# Configuración base — NEON Control

Este documento define valores y decisiones iniciales. El formato definitivo se implementará junto con el backend.

## 1. Valores iniciales

| Parámetro | Valor inicial |
|---|---|
| Nombre | NEON Control |
| Edición | Minecraft Bedrock |
| Arquitectura | Single-node |
| Instancias | Varias instancias locales previstas |
| API | FastAPI |
| Frontend | React + TypeScript |
| DB | SQLite |
| Puerto Bedrock IPv4 | UDP 19132 |
| Puerto Bedrock IPv6 | UDP 19133, si se habilita |
| Bind de API durante setup | 127.0.0.1 |
| Servicio | systemd |

## 2. Configuración de instancia BDS

Cada instancia debe mantener su propio:
- Identificador y nombre.
- Directorio de instalación.
- Ejecutable BDS.
- Archivo server.properties.
- Puerto y parámetros de red.
- Directorio de mundos.
- Logs.
- Estado y metadatos.
- Política de backups.

No compartir directorios de mundo entre procesos BDS simultáneos.

## 3. server.properties

El panel deberá leer y editar propiedades compatibles con la versión instalada de BDS. Debe preservar propiedades desconocidas para evitar eliminar opciones nuevas al guardar.

Opciones de interés:
- server-name
- gamemode
- difficulty
- allow-cheats
- max-players
- online-mode
- allow-list
- server-port
- server-portv6
- view-distance
- tick-distance
- level-name
- level-seed
- default-player-permission-level

Los nombres y valores válidos dependen de la versión oficial de BDS. El panel debe validar con la documentación/esquema de la versión, no asumir que todas las versiones aceptan el mismo conjunto.

## 4. Puertos y exposición

El puerto UDP 19132 es el valor por defecto previsto para IPv4. El endpoint IPv6 puede usar UDP 19133. Si el administrador cambia estos valores, NEON Control debe mostrar qué reglas de firewall/NAT debe actualizar.

El panel web y la API son servicios de administración: no deben compartir el puerto de juego ni quedar expuestos públicamente por defecto.

## 5. Variables de entorno

Se prevé separar configuración no sensible y secretos. Ningún archivo con contraseñas reales, tokens o claves privadas debe subirse al repositorio. Los ejemplos publicados contendrán solo valores ficticios.

## 6. Backups

El backup debe incluir como mínimo:
- server.properties.
- allowlist y permissions.
- mundos.
- packs y configuraciones adicionales instaladas por el administrador.
- metadatos de la instancia.

La base de datos del panel tendrá su propio mecanismo de backup. Se deben documentar retención, ubicación externa y prueba de restauración.
