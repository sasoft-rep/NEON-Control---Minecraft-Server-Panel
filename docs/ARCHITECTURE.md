# Arquitectura técnica — NEON Control

**Documento:** arquitectura base V1.0  
**Estado:** propuesta técnica inicial  
**Modelo:** single-node / local host  
**Ediciones:** Minecraft Java Edition + Bedrock Edition

## 1. Alcance

Una instalación administra múltiples instancias locales Java y Bedrock en el mismo Ubuntu Server, VM o LXC. V1.0 no administra nodos remotos ni forma clúster.

El panel es independiente de los motores. Cada instancia se asocia a un adaptador que valida, inicia, detiene, consulta y se comunica con un runtime concreto.

Motores iniciales:
- **Bedrock:** Minecraft Bedrock Dedicated Server (BDS) oficial para Linux x86-64.
- **Java:** PaperMC como primer motor Java, ejecutado externamente mediante una JVM compatible.

Paper no es una interfaz gráfica ni parte del panel. NEON Control no hará un fork, no modificará su núcleo ni copiará su código fuente al repositorio. Paper se instala como runtime independiente y se controla como proceso externo.

## 2. Capas y componentes

### Frontend
- React + TypeScript; interfaz responsive.
- Dashboard, instancias, consola, jugadores, mundos, backups y configuración.
- Identificación visible de edición y motor.
- Sin secretos persistentes en el navegador.

### Backend
- Python + FastAPI.
- API REST autenticada y WebSocket para consola, logs y métricas.
- Validación, autorización por operación y persistencia SQLite.
- Orquestación común sin asumir que Java y Bedrock ofrecen funciones idénticas.

### Instance Manager
Mantiene ID, nombre, edición, motor, rutas, estado deseado/observado, puertos, recursos, versión y adaptador de cada instancia.

### Engine Adapter Interface
Contrato común para:
- Identificar motor y edición.
- Validar runtime y dependencias.
- Resolver ejecutable y argumentos permitidos.
- Start / stop / restart / status.
- Acceso controlado a stdin/stdout y logs.
- Detectar versión.
- Leer/validar configuración.
- Declarar capacidades específicas (jugadores, allowlist, permisos, mundos, actualizaciones).

La interfaz mostrará funciones según las capacidades del adaptador; no supondrá paridad entre ediciones.

### Adaptadores iniciales

**BDS Adapter**
- BDS oficial para Linux x86-64.
- Configuración, allowlist, permisos y mundos según versión.
- UDP 19132 como puerto IPv4 predeterminado.

**Paper Adapter**
- JAR obtenido desde PaperMC oficial.
- JVM compatible con la versión de Paper instalada.
- Ejecución Java con argumentos controlados.
- server.properties, EULA, consola y archivos propios.
- TCP 25565 como puerto predeterminado.
- Gestión de plugins queda sujeta a diseño y controles de seguridad específicos.

El JAR y los datos de Paper se almacenan fuera del árbol de código de NEON Control. No se copia el repositorio de Paper ni se parchea su JAR.

### Process Manager
- Ejecuta procesos locales con usuario sin privilegios.
- Registra PID, estado, inicio, código de salida y logs.
- Conecta stdin/stdout de forma controlada.
- No concatena entradas de usuario en comandos shell.
- Evita duplicados y recupera estado después de reinicios.
- Aplica límites de recursos cuando sea posible.

### Servicio privilegiado
El backend no corre como root. Las operaciones que requieran permisos adicionales usan un helper mínimo y explícito. No se expone sudo genérico ni shell administrativa por HTTP.

### Database
SQLite para usuarios, roles, instancias, preferencias, backups, auditoría y metadatos. Los mundos, JAR y archivos grandes quedan fuera de SQLite.

## 3. Flujo de control

1. El usuario se autentica.
2. Selecciona una instancia y solicita una operación.
3. La API valida sesión, rol, CSRF cuando corresponda y parámetros.
4. Instance Manager identifica motor y adaptador.
5. El adaptador valida acción y estado.
6. Process Manager ejecuta con permisos mínimos.
7. Se registran resultado y eventos.
8. El estado se transmite por WebSocket.

## 4. Directorios propuestos

| Ruta | Propósito |
|---|---|
| /opt/neon-control | Aplicación y entorno virtual |
| /etc/neon-control | Configuración del panel |
| /var/lib/neon-control | Base de datos y estado |
| /var/log/neon-control | Logs del panel |
| /srv/neon-control/instances | Instancias y runtimes |
| /srv/neon-control/backups | Copias de seguridad |

Cada instancia separa runtime, datos, configuración y logs cuando corresponda.

## 5. Modelo de instancia

- ID y nombre.
- Edición: java o bedrock.
- Motor: paper o bedrock_dedicated_server.
- Versión de Minecraft/runtime.
- Rutas de runtime y datos.
- Ejecutable/JAR y argumentos permitidos.
- Versión de Java asociada, si corresponde.
- Puerto y protocolo TCP/UDP.
- Estado deseado y observado.
- Memoria, límites y capacidades del adaptador.

## 6. Operaciones previstas

Install/validate runtime; start/stop/restart; status/health; consola; configuración; jugadores/permisos según motor; backup/restore; actualización con backup previo. Las acciones destructivas requieren confirmación.

## 7. Proxmox LXC

NEON Control se ejecuta dentro del invitado como aplicación Linux normal. No requiere API de Proxmox, Docker ni nesting.

Requisitos: Ubuntu LTS x86-64, systemd, acceso saliente a Internet, almacenamiento persistente, recursos asignados y reglas de firewall TCP/UDP. Se recomienda LXC no privilegiado.

## 8. Seguridad

- Panel y juegos nunca como root.
- Sin shell arbitraria.
- Protección contra path traversal y symlink escapes.
- Límites de uploads/backups.
- Sesiones con expiración y auditoría.
- Secretos fuera del repositorio y logs.
- API en localhost durante setup; acceso remoto solo con HTTPS/proxy seguro.
- Verificar origen e integridad de ejecutables externos cuando existan checksums o firmas.

## 9. Licencias y límites

El código propio del panel se rige por LICENSE, salvo componentes expresamente identificados con otra licencia. Paper mantiene sus licencias originales, incluida GPL-3.0 para el código cubierto por ella y otras licencias cuando sus autores así lo indiquen. NEON Control lo integra como runtime externo; no lo relicencia bajo NCL-1.0.

BDS, Minecraft, Java y dependencias mantienen sus propios términos. Consultar docs/LICENSING.md antes de redistribuir binarios o dependencias.

## 10. Decisiones pendientes

Contrato definitivo de adaptadores; matriz Paper/Minecraft/Java/BDS; descarga y verificación de runtimes; comunicación stdin/stdout; helper privilegiado; cuotas; retención de backups; actualizaciones y rollback; matriz de Ubuntu y dependencias nativas.
