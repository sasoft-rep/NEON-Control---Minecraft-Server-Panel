# Política de seguridad

## Versiones

Durante la fase inicial, el proyecto no ofrece garantías de seguridad ni soporte de producción. Al publicarse la primera versión estable se documentarán las versiones soportadas.

## Reportar vulnerabilidades

No publiques detalles explotables de una vulnerabilidad no corregida en un issue público. Contacta a los mantenedores mediante el canal privado de seguridad habilitado en el perfil/organización de GitHub del proyecto. Si no existe un canal privado configurado, solicita primero un medio privado de contacto sin incluir el exploit.

Incluye versión o commit afectado, sistema operativo y entorno (Ubuntu bare metal, VM o Proxmox LXC), pasos para reproducir, impacto observado y logs depurados de secretos.

## Requisitos de seguridad

- El panel y BDS no deben ejecutarse como root.
- No exponer endpoints de ejecución arbitraria de shell.
- Validar y autorizar cada operación.
- Prevenir path traversal y symlink escapes en operaciones de archivos.
- Proteger sesiones, cookies y endpoints WebSocket.
- Aplicar límites a uploads, logs y backups.
- No registrar contraseñas, tokens ni secretos.
- Usar dependencias fijadas y revisar alertas de seguridad.
- Proteger operaciones destructivas con confirmación y backup cuando corresponda.
- Mantener la API ligada a localhost durante el setup inicial.

## Datos sensibles

No subir al repositorio archivos de configuración reales, tokens, contraseñas, claves SSH, certificados privados, datos de jugadores o backups de mundos de usuarios, ni dumps de bases de datos de producción.
