# Despliegue en Proxmox VE — LXC

NEON Control se ejecuta como aplicación Linux dentro de un contenedor LXC. No requiere agente en el host Proxmox ni acceso a su API.

## 1. Diseño recomendado

- Proxmox administra el ciclo de vida del CT.
- Ubuntu Server LTS x86-64 como sistema invitado.
- NEON Control administra instancias Java y Bedrock dentro del CT.
- Datos persistentes en rootfs o mount points dedicados.
- Puertos de juego publicados por la red del contenedor y firewall.

## 2. Tipo de contenedor

Preferido: LXC no privilegiado. No activar nesting ni montar el socket Docker por defecto. La arquitectura no requiere Docker.

## 3. Recursos iniciales

Valores orientativos para laboratorio, no mínimos oficiales ni garantía de capacidad:

| Recurso | Laboratorio pequeño |
|---|---:|
| vCPU | 2 |
| RAM | 4 GB |
| Disco | 20 GB |
| Swap | 1–2 GB según política |
| Red | Bridge Proxmox |
| IP | Estática o reserva DHCP |

El consumo depende de versión, plugins, add-ons, mundo, jugadores y cantidad de instancias. Java/Paper puede requerir memoria adicional y heap adecuado. Medir y dejar margen para Ubuntu y el panel.

## 4. Creación del CT

1. Descargar plantilla Ubuntu LTS.
2. Crear CT no privilegiado.
3. Asignar CPU, RAM y almacenamiento.
4. Configurar bridge, IP y gateway.
5. Habilitar inicio al arrancar si se desea.
6. Iniciar y actualizar Ubuntu.

Los nombres de opciones pueden variar según Proxmox VE.

## 5. Red

| Motor | Puerto predeterminado | Protocolo |
|---|---:|---|
| Bedrock Dedicated Server | 19132 | UDP |
| Paper / Java | 25565 | TCP |

Permitir el tráfico necesario en firewall Proxmox, firewall del CT y router/NAT si el servidor será público. Restringir el panel a la red administrativa o protegerlo con HTTPS y autenticación. No abrir puertos de motores no utilizados.

## 6. Almacenamiento

Considerar mount points dedicados para /srv/neon-control/instances y /srv/neon-control/backups. Revisar mapeo UID/GID de LXC antes de cambiar ownership. No cambiar permisos recursivamente en datasets existentes sin evaluar impacto.

Los backups dentro del mismo CT no protegen contra pérdida del almacenamiento del CT. Mantener una copia externa.

## 7. systemd y runtimes

Confirmar que systemd funciona y admite procesos de larga duración. Paper requiere JVM compatible dentro del CT; BDS requiere sus bibliotecas nativas. No asumir que dependencias instaladas en el host Proxmox están disponibles dentro del contenedor.

## 8. Seguridad

- Preferir LXC no privilegiado.
- No montar directorios sensibles del host.
- No entregar acceso al socket/API de Proxmox.
- No dar acceso a /dev/kvm.
- Limitar puertos y origen de administración.
- No ejecutar panel ni servidores como root.
- Separar permisos y datos por instancia.

## 9. Diagnóstico

Si una instancia no inicia, revisar logs, arquitectura y dependencias BDS o versión Java, integridad/permisos del runtime, memoria/heap/límites CT, firewall/protocolo y conflictos de puertos.

La compatibilidad debe probarse en versiones concretas antes de declarar una matriz oficial.
