# Política de licencias — NEON Control

**Estado:** política documental inicial; requiere revisión jurídica antes de una distribución estable.

## 1. Regla general

NEON Control distingue entre:
1. Código propio del panel, interfaz, API, adaptadores y documentación del proyecto.
2. Motores de servidor que el panel administra como procesos externos.
3. Bibliotecas, runtimes, herramientas y otros componentes de terceros.

La licencia de una categoría no se extiende automáticamente a las demás. Consultar siempre el archivo de licencia y avisos del componente concreto.

## 2. Código propio de NEON Control

El material propio expresamente identificado como cubierto por el archivo LICENSE se distribuye bajo NEON Community License 1.0 (NCL-1.0), un modelo source-available con condiciones no comerciales y publicación obligatoria de modificaciones.

NCL-1.0 no es una licencia Open Source aprobada por OSI. No debe describirse el proyecto como OSI-approved Open Source. El texto publicado es un borrador sujeto a revisión legal.

## 3. PaperMC — Java Edition

Paper es un proyecto independiente. NEON Control lo integra como runtime externo:
- No incorpora el código fuente de Paper al repositorio.
- No crea un fork ni modifica su núcleo.
- No relicencia Paper bajo NCL-1.0.
- El JAR se obtiene desde los canales oficiales y se ejecuta como proceso separado mediante una JVM compatible.
- Se deben conservar y respetar los avisos, licencias y términos que acompañen a cada distribución de Paper y sus componentes.

El código de Paper cubierto por GPL-3.0 continúa sujeto a GPL-3.0. Algunas partes pueden tener licencias diferentes indicadas por sus autores. Si en el futuro se copia, modifica, enlaza o combina código de Paper en NEON Control, detener esa integración y realizar una revisión de compatibilidad de licencias antes de publicarla. NCL-1.0 no puede sustituir ni añadir restricciones incompatibles a los derechos que GPL-3.0 concede sobre código GPL.

La integración externa elegida reduce el acoplamiento y evita tratar el código del motor como parte del panel, pero no constituye por sí sola una conclusión legal sobre cualquier posible forma de distribución o combinación.

## 4. Minecraft Bedrock Dedicated Server

BDS y los componentes de Minecraft/Mojang/Microsoft conservan sus propios términos. NEON Control no concede derechos sobre Minecraft ni redistribuye BDS por defecto. El administrador debe obtenerlo desde canales oficiales y aceptar los términos aplicables.

## 5. Java y dependencias

La JVM, paquetes Python, dependencias JavaScript/TypeScript, bibliotecas y herramientas de build conservan sus licencias originales. Antes de empaquetar o redistribuir dependencias, registrar nombre, versión, licencia, avisos y obligaciones de atribución o código fuente.

## 6. Distribuciones futuras

Antes de publicar instaladores o paquetes:
- Generar un inventario/SBOM de componentes.
- Verificar licencia y procedencia de cada artefacto.
- No incluir Paper, BDS ni JVM sin una revisión específica de sus términos de redistribución.
- Incluir avisos y textos de licencia requeridos.
- Separar descargas externas del paquete del panel cuando sea apropiado.
- Someter cualquier cambio que incorpore código GPL a revisión legal y técnica.

## 7. Referencias

- [LICENSE del proyecto](../LICENSE)
- [PaperMC — repositorio oficial](https://github.com/PaperMC/Paper)
- [PaperMC — documentación](https://docs.papermc.io/)
- [GNU GPL v3](https://www.gnu.org/licenses/gpl-3.0.html)

Esta política es informativa y no constituye asesoramiento legal.
