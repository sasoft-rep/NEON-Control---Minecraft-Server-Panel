# Cómo contribuir

Gracias por tu interés en NEON Control.

## Antes de contribuir

- Lee el README, LICENSE y la política de licencias.
- Las contribuciones al código propio deben cumplir NCL-1.0 y sus restricciones de publicación de modificaciones y uso no comercial.
- No incluyas binarios de Minecraft, BDS, Paper, JVM ni material de terceros sin verificar previamente sus términos y permisos.
- No copies código de Paper u otro componente GPL al código del panel sin una revisión de compatibilidad de licencias.
- No incluyas credenciales, datos privados o backups reales.

## Flujo recomendado

1. Abre un issue describiendo el problema o propuesta.
2. Para cambios grandes, espera a que se acuerde el enfoque.
3. Crea un fork público.
4. Trabaja en una rama específica.
5. Añade pruebas y documentación.
6. Abre un Pull Request con resumen, motivación, pruebas y riesgos.

## Calidad

- Mantener responsabilidades separadas entre frontend, API, Instance Manager y adaptadores de motores.
- No añadir endpoints de shell arbitraria.
- Validar entradas y permisos en backend.
- No asumir paridad funcional entre Java y Bedrock; declarar capacidades por adaptador.
- Documentar cambios de configuración y migraciones.
- Añadir pruebas para errores y casos límite.
- Mantener compatibilidad con los entornos Linux objetivo.

## Estilo

- Python: PEP 8 y type hints.
- TypeScript: componentes tipados y linting.
- Commits: mensajes claros, preferentemente en inglés.
- Documentación: español e inglés a medida que el proyecto madure.

## Pull Requests

Un PR debe indicar qué cambia, por qué cambia, cómo se probó, compatibilidad Ubuntu/Proxmox, cambios de seguridad o permisos, cambios de base de datos/configuración y cualquier impacto en licencias o dependencias.
