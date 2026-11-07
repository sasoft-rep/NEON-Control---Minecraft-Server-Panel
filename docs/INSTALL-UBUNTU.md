# Instalación — Ubuntu Server

**Estado:** guía objetivo para V1.0. El instalador automatizado aún no está publicado.

## 1. Plataforma

Objetivo inicial:
- Ubuntu Server 22.04 LTS o 24.04 LTS.
- Arquitectura x86-64.
- Usuario con permisos sudo para la instalación.
- Conexión saliente a Internet.
- Almacenamiento persistente para el panel, instancias y backups.

BDS tiene sus propios requisitos de sistema y bibliotecas. Antes de producción, verificar los requisitos de la versión de BDS que se vaya a instalar.

## 2. Preparación del host

Actualizar el sistema:

```bash
sudo apt update
sudo apt full-upgrade -y
sudo apt install -y ca-certificates curl wget git tar unzip python3 python3-venv python3-pip build-essential
```

La versión de Node.js LTS y el gestor de paquetes del frontend se fijarán al implementar el frontend, usando una matriz de compatibilidad documentada.

## 3. Usuarios y directorios

La instalación final creará usuarios de sistema separados, por ejemplo:
- neonctl: servicio del panel, sin login interactivo.
- bedrock: propietario de los procesos y archivos BDS, sin login interactivo.

Rutas propuestas:
- Aplicación: /opt/neon-control
- Configuración: /etc/neon-control
- Estado: /var/lib/neon-control
- Instancias: /srv/neon-control/instances
- Backups: /srv/neon-control/backups
- Logs: /var/log/neon-control

No ejecutar la aplicación ni BDS como root.

## 4. Instalación de NEON Control

Cuando se publique el instalador estable, el flujo previsto será:
1. Descargar una versión etiquetada desde GitHub Releases.
2. Verificar checksum y firma si están disponibles.
3. Crear usuarios y directorios.
4. Instalar backend y dependencias bloqueadas.
5. Construir y desplegar frontend.
6. Crear configuración inicial y base SQLite.
7. Instalar unidad systemd con permisos mínimos.
8. Configurar el acceso inicial y mostrar cómo habilitar HTTPS.

No existe todavía un comando de instalación oficial. No usar comandos curl | bash de fuentes no verificadas.

## 5. Red y firewall

Puerto de juego por defecto previsto:
- UDP 19132 para IPv4.
- UDP 19133 puede utilizarse para IPv6 según la configuración del servidor.

El puerto web del panel no debe exponerse directamente a Internet sin TLS, autenticación robusta y firewall. En la primera puesta en marcha debe escucharse en localhost o en una interfaz de administración confiable.

Ejemplo de apertura de juego con UFW (ajustar a la configuración real):

```bash
sudo ufw allow 19132/udp
# Solo si se utiliza el endpoint IPv6 correspondiente:
sudo ufw allow 19133/udp
```

No abrir puertos que no se utilicen. Si se cambia el puerto en server.properties, actualizar también firewall, NAT y documentación.

## 6. Verificación posterior

La futura instalación deberá validar:
- Estado de systemd.
- Salud de la API.
- Acceso a SQLite.
- Permisos de directorios.
- Disponibilidad del ejecutable BDS.
- Puerto UDP y firewall.
- Capacidad de crear, iniciar y detener una instancia de prueba.

## 7. Actualizaciones

Actualizar desde versiones publicadas, no desde la rama de desarrollo. Antes de actualizar:
1. Detener ordenadamente las instancias.
2. Crear backup de configuración, datos y mundos.
3. Verificar espacio libre.
4. Aplicar la actualización.
5. Ejecutar migraciones.
6. Validar salud.
7. Restaurar la versión anterior si falla la validación.

## 8. Desinstalación

El desinstalador deberá distinguir entre archivos de aplicación, configuración, base de datos, instancias y backups. Nunca debe borrar mundos, instancias o backups sin una confirmación explícita y una opción de conservarlos.
