# Arquitectura técnica — NEON Control

**Documento:** arquitectura base V1.0  
**Estado:** propuesta técnica inicial  
**Modelo:** single-node / local host

## 1. Alcance

Una instalación de NEON Control administra instancias de Minecraft Bedrock Dedicated Server que se ejecutan en el mismo Ubuntu Server o contenedor LXC. La V1.0 no administra nodos remotos, no forma clúster y no depende de servicios cloud.

Se prevé permitir múltiples instancias locales, cada una con su propio directorio, proceso, configuración, puerto y mundo. La cantidad efectiva dependerá de los recursos del host.

## 2. Componentes

### Frontend
- React + TypeScript.
- Interfaz responsive para escritorio, tablet y móvil.
- Dashboard, consola, instancias, jugadores, mundos, backups y configuración.
- No debe almacenar secretos persistentes en el navegador.

### Backend
- Python + FastAPI.
- API REST autenticada.
- WebSocket para consola, logs y métricas en vivo.
- Validación de entrada y autorización por operación.
- Acceso a SQLite mediante una capa de persistencia.

### Process Manager
- Controla procesos BDS locales.
- Registra PID, estado, hora de inicio, código de salida y logs.
- Envía comandos de consola por un canal controlado.
- Evita construir comandos de shell concatenando texto del usuario.
- Debe recuperar estado tras reinicios del panel sin lanzar procesos duplicados.

### Servicio privilegiado
El backend web no debe ejecutarse como root. Las operaciones que requieran permisos adicionales se realizarán mediante un helper mínimo, con interfaz y comandos permitidos explícitamente. No se utilizará sudo genérico ni una shell administrativa expuesta por HTTP.

### BDS Runtime
- Binarios oficiales de Bedrock Dedicated Server para Linux x86-64.
- Una carpeta de instalación por instancia.
- Datos, mundos y configuración separados por instancia.
- Ejecución bajo un usuario Linux sin privilegios.

### Database
SQLite para la primera versión. Guardará usuarios, roles, instancias registradas, preferencias, tareas de backup, eventos de auditoría y metadatos. Los mundos y archivos grandes no se guardan dentro de SQLite.

## 3. Flujo de control

1. El usuario se autentica en la interfaz.
2. El frontend llama a la API.
3. La API verifica sesión, rol, CSRF cuando corresponda y parámetros.
4. El servicio de gestión valida la acción y el estado de la instancia.
5. El Process Manager ejecuta la operación mediante el mecanismo local definido.
6. El resultado y los eventos se registran.
7. El estado actualizado se transmite por WebSocket.

## 4. Directorios propuestos

| Ruta | Propósito |
|---|---|
| /opt/neon-control | Aplicación y entorno virtual |
| /etc/neon-control | Configuración del panel |
| /var/lib/neon-control | Base de datos y estado |
| /var/log/neon-control | Logs del panel |
| /srv/neon-control/instances | Instancias BDS |
| /srv/neon-control/backups | Copias de seguridad |

Las rutas deberán poder configurarse. Cada instancia tendrá subdirectorio propio. No guardar contraseñas ni claves dentro de los directorios públicos del frontend.

## 5. Modelo de instancia

Campos mínimos:
- ID interno y nombre visible.
- Ruta de instalación.
- Versión BDS.
- Ejecutable y argumentos permitidos.
- Puerto IPv4/UDP y configuración IPv6.
- Estado deseado y estado observado.
- Memoria y límites opcionales.
- Fecha de creación y última ejecución.

## 6. Operaciones previstas

- Install / validate runtime.
- Start / stop / restart.
- Status / health.
- Console stream / send command.
- Read and validate server.properties.
- Player list, allowlist and permissions management.
- Backup / restore.
- Update runtime with pre-update backup.

Las acciones destructivas deben requerir confirmación y, cuando corresponda, un backup previo.

## 7. Contenedores Proxmox

La aplicación no necesita administrar Proxmox ni acceder a su API. Se ejecuta dentro del sistema invitado LXC como una aplicación Linux normal. El administrador crea y configura el contenedor desde Proxmox.

Requisitos del contenedor:
- Ubuntu Server LTS x86-64.
- systemd operativo.
- Acceso saliente a Internet para obtener dependencias y BDS.
- Almacenamiento persistente.
- Recursos de CPU/RAM/disco asignados desde Proxmox.
- Reglas de firewall que permitan el tráfico de juego requerido.
- No depender de Docker ni de nesting para el diseño base.

Se recomienda un LXC no privilegiado cuando el entorno lo permita. No habilitar privileged, nesting ni acceso al socket de Docker como solución por defecto.

## 8. Límites de seguridad

- BDS nunca se ejecuta como root.
- El frontend no recibe acceso al filesystem completo.
- No hay endpoint de shell arbitraria.
- Rutas normalizadas y protegidas contra path traversal.
- Límites de tamaño para uploads y backups.
- Sesiones con expiración y cierre de sesión.
- Auditoría de acciones administrativas.
- Secretos fuera del repositorio y de los logs.
- API ligada a localhost por defecto durante la instalación; acceso remoto solo después de configurar HTTPS o un proxy seguro.

## 9. Decisiones pendientes para implementación

- Mecanismo final de comunicación con stdin/stdout del proceso BDS.
- Helper privilegiado y política exacta de permisos.
- Límites por instancia y cuotas de recursos.
- Política de retención de backups.
- Compatibilidad exacta con versiones de Ubuntu y dependencias nativas de BDS.
- Método de actualización y rollback.
