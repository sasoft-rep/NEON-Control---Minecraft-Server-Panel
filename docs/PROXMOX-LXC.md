# Despliegue en Proxmox VE — LXC

NEON Control está diseñado para ejecutarse como una aplicación Linux dentro de un contenedor LXC. No requiere instalar un agente en el host Proxmox ni utilizar la API de Proxmox.

## 1. Diseño recomendado

- Proxmox VE administra el ciclo de vida del contenedor.
- El contenedor utiliza Ubuntu Server LTS x86-64.
- NEON Control y BDS se ejecutan dentro del contenedor.
- Los datos persistentes se almacenan en el rootfs del contenedor o en un mount point dedicado.
- El puerto de juego se publica mediante la red del contenedor y las reglas de firewall correspondientes.

## 2. Tipo de contenedor

Preferido: LXC no privilegiado.

Usar contenedor privilegiado únicamente si una necesidad técnica concreta, demostrada y documentada lo exige. La configuración base de NEON Control no debe requerirlo.

No activar nesting ni montar el socket Docker por defecto. No se requiere Docker para la arquitectura prevista.

## 3. Recursos iniciales sugeridos

Son valores de referencia para laboratorio, no mínimos oficiales de Mojang ni una garantía de capacidad:

| Recurso | Laboratorio pequeño |
|---|---:|
| vCPU | 2 |
| RAM | 4 GB |
| Disco | 20 GB |
| Swap | 1–2 GB, según política del host |
| Red | Bridge de Proxmox |
| IP | Estática o reserva DHCP |

Para mundos grandes, muchos jugadores, add-ons o múltiples instancias, aumentar RAM, CPU y almacenamiento según mediciones reales. Mantener margen de recursos para el sistema operativo y el panel.

## 4. Creación del contenedor

Desde la interfaz de Proxmox:
1. Descargar una plantilla Ubuntu Server LTS compatible.
2. Crear un CT nuevo.
3. Elegir contenedor no privilegiado.
4. Asignar CPU, RAM y almacenamiento.
5. Configurar bridge, IP y gateway.
6. Habilitar inicio al arrancar si se desea.
7. Iniciar el contenedor y actualizar Ubuntu.

Los nombres exactos de opciones pueden variar entre versiones de Proxmox VE.

## 5. Red

Bedrock utiliza UDP por defecto. Asegurarse de que:
- El firewall de Proxmox permita el puerto UDP configurado.
- El firewall dentro del contenedor permita el mismo puerto.
- El router/NAT reenvíe el tráfico al IP del CT si el servidor será público.
- El puerto del panel quede restringido a la red administrativa o protegido por HTTPS y autenticación.

Revisar firewall de Proxmox, firewall del CT y router: abrir un solo nivel puede no ser suficiente.

## 6. Almacenamiento

Para instalaciones con mundos importantes, considerar un mount point dedicado para:
- /srv/neon-control/instances
- /srv/neon-control/backups

El mount point debe tener permisos y ownership compatibles con el usuario no privilegiado del contenedor. Al configurar bind mounts, revisar el mapeo UID/GID de LXC. No cambiar ownership recursivamente de datasets existentes sin verificar el impacto.

Los backups almacenados únicamente dentro del mismo CT no protegen contra la pérdida del almacenamiento del CT. Mantener una copia externa al contenedor.

## 7. systemd y procesos

La aplicación y las instancias se administrarán dentro del contenedor mediante servicios/procesos locales. Confirmar que systemd funcione correctamente en la plantilla y que el CT pueda mantener procesos de larga duración.

## 8. Seguridad

- Preferir LXC no privilegiado.
- No montar / del host ni directorios sensibles de Proxmox.
- No entregar acceso al socket de Proxmox.
- No dar acceso al dispositivo /dev/kvm; no es necesario.
- Limitar puertos y origen de administración.
- No ejecutar el panel como root.
- Mantener el host Proxmox y el contenedor actualizados.

## 9. Diagnóstico

Si BDS no inicia:
1. Revisar logs del servicio y salida del ejecutable.
2. Verificar arquitectura x86-64 y dependencias del runtime.
3. Verificar permisos de la carpeta de instancia.
4. Revisar memoria disponible y límites del CT.
5. Revisar que el firewall permita UDP.
6. Comprobar que no exista otra instancia ocupando el mismo puerto.

La compatibilidad debe probarse en versiones concretas de Proxmox VE y Ubuntu antes de declarar una matriz oficial.
