# TerraPulse · Cada cambio cuenta

Aplicación en español para explorar observaciones NISAR y escenarios de cambio en humedales, inspirada en el desafío de observación terrestre de NASA–ISRO NISAR. Construida con React, TypeScript, Vite, Leaflet y Recharts.

## Problemática y solución

Los cambios del agua y la vegetación de los humedales pueden ser difíciles de observar y comunicar. TerraPulse propone reunir un mapa, una serie temporal y recomendaciones de revisión en una interfaz que ayude a priorizar investigaciones y conservación.

**Cumplimiento parcial del reto:** la vista «NISAR real» consulta metadatos oficiales GCOV y muestra coberturas reales, enlaces e imágenes de consulta cuando son accesibles. El laboratorio de cambios sigue siendo simulado. No se han procesado los rásteres científicos ni validado una detección de pérdida de humedal. Una reducción de superficie inundada no equivale necesariamente a pérdida del humedal.

## Ejecutar

Requiere una versión de Node compatible con Vite 8 (Node 20.19+ o 22.12+).

```sh
npm ci
npm run dev
```

Abre la dirección local que muestra Vite. Para generar la versión de producción:

```sh
npm run build
npm run preview
```

## Funciones

- Mapa navegable con base satelital Esri y alternativa OpenStreetMap.
- Cuatro regiones reales: Ozama, Pantanal, Sundarbans y Okavango.
- Capas de cambios e inundación con polígonos sintéticos.
- Serie mensual ilustrativa de abril a octubre de 2026; selección manual y reproducción.
- Indicadores y gráfica sincronizados con la región y el mes seleccionados.
- Alertas calculadas para el intervalo elegido, con umbrales configurables del 3, 5, 10 o 15 %.
- Regiones favoritas persistidas en localStorage del navegador.
- Reporte CSV según la vista actual: región seleccionada, regiones con alertas o favoritos. Incluye referencia, observación y diferencias absolutas y porcentuales, rotulado `SIMULADO_NO_NISAR`.
- Explicación de metodología, limitaciones y enlaces a fuentes oficiales.
- Diseño adaptable a escritorio y móvil, controles de teclado y diálogo con cierre por Escape.

Las teselas geográficas y las fuentes de Google necesitan conexión a internet. Las imágenes del mapa base no corresponden necesariamente a la fecha seleccionada. La línea temporal modifica solamente los datos y polígonos de demostración. No hay servidor propio, registro de usuarios ni notificaciones externas. Las consultas nuevas de NISAR se hacen directamente al catálogo público de ASF desde el navegador.

## Organización

- `src/App.tsx`: vistas y controles interactivos.
- `src/data/regions.ts`: ubicaciones y series sintéticas.
- `src/App.css`: diseño responsive.
- `src/index.css`: estilos globales y tipografía.

## Integración científica pendiente

Para cumplir el componente de datos reales del desafío se debe construir y validar una canalización científica:

1. Seleccionar productos NISAR adecuados y registrar sus identificadores, fechas, geometrías, versiones y calidad.
2. Obtenerlos desde los servicios oficiales y realizar el procesamiento fuera del navegador; no incluir credenciales Earthdata en el frontend.
3. Alinear escenas y controlar calidad, geometría, polarización, estacionalidad y condiciones hidrológicas.
4. Aplicar un método validado de clasificación de inundación y detección de cambios. Informar incertidumbre y validación de campo.
5. Publicar teselas o GeoJSON derivados y series de superficies con procedencia verificable para reemplazar el conjunto sintético.

El prototipo no afirma que una variación de retrodispersión pruebe degradación ni atribuye cambios a una causa específica.

## Fuentes oficiales

- [NISAR: ciencia del agua](https://nisar.jpl.nasa.gov/science/water/)
- [NASA: acceso a datos NISAR](https://science.nasa.gov/mission/nisar/data/)
- [NASA: bosques y humedales](https://www.nasa.gov/missions/nisar/nasa-isro-radar-mission-to-provide-dynamic-view-of-forests-wetlands/)

Proyecto independiente; no es un producto oficial de NASA ni de ISRO.

## Exploración y validación interactiva

- Selector de fechas de referencia y observación, compartido por indicadores, alertas y exportación.
- Comparación de contornos sintéticos con referencia discontinua; no es una comparación de imágenes satelitales de dos fechas.
- Control de opacidad, escala métrica, vista mundial, regreso a la región y mapa ampliado (Escape para cerrar).
- Inspección de polígonos con contexto; clic en el mapa añade un punto de coordenadas que puede abrirse.
- Errores de teselas visibles, reintento y alternativa cartográfica.
- Favoritos eliminables desde su propia vista; búsqueda con clasificación consistente.
- Gráfica completa con marcadores para las dos fechas de comparación.

Las geometrías sintéticas escalan proporcionalmente a la serie regional, pero sus áreas no son las superficies reportadas y sus ubicaciones no delimitan humedales reales.

Pruebas de cálculos y consistencia geométrica (Node 22.18+ o Node 24):

```sh
node --experimental-strip-types --test tests/analysis.test.mjs
```

Se prueban comparaciones de fechas, sensibilidad de alertas y proporciones de geometrías. Estas pruebas no sustituyen la revisión visual y de interacción en navegador.

## Presentación ante Space Apps

El [programa de premios de 2025](https://www.spaceappschallenge.org/2025/awards/) reconoce uso válido de ciencia, datos, tecnología e impacto, entre otros aspectos. Este prototipo no implica aprobación ni elegibilidad confirmada para una edición del evento. Antes de presentarlo: integrar productos NISAR verificables, validar resultados con evidencia independiente, documentar incertidumbre y revisar las reglas de la edición y del reto seleccionado.

## Conexión oficial NISAR

La sección inicial **NISAR real** incluye:

- Catálogo local verificable de ocho adquisiciones GCOV: dos por cada una de las cuatro regiones, recuperadas de ASF el 21 de octubre de 2026.
- Metadatos, contornos geográficos oficiales, fechas, trayectoria, cuadro, polarización, versión y colección.
- Selección de pares con trayectoria, cuadro, dirección, polarización principal, ancho de banda, versión y colección coincidentes. Esto reduce incompatibilidades, pero no garantiza comparabilidad científica completa.
- Imágenes de consulta oficiales enlazadas desde ASF, en vistas de referencia, reciente y lado a lado. No se superponen como rásteres georreferenciados ni se utilizan para calcular hectáreas.
- Consulta actual a la API pública de ASF limitada a 24 resultados que intersectan el punto de la región. No es un inventario exhaustivo de todas las adquisiciones ni de toda la extensión del humedal.
- Exportación JSON con identificadores, URL de consulta, fechas y enlaces de origen. Las actualizaciones del navegador se conservan solo durante esa sesión.

**Limitación comprobada en esta red:** la consulta de metadatos funcionó, pero el dominio de imágenes mostró una cadena TLS de Cisco Umbrella que no pudo validarse. No se desactivó la validación TLS ni se descargaron imágenes. La aplicación intenta cargar las imágenes oficiales y muestra un estado de error con enlace de origen si fallan. No se garantiza que puedan verse desde esta red.

Para actualizar el paquete de metadatos:

```sh
node --experimental-strip-types scripts/fetch-nisar.mjs --metadata-only
```

Para descargar también las ocho imágenes públicas de consulta, en una red donde el dominio oficial sea accesible y su certificado válido:

```sh
node --experimental-strip-types scripts/fetch-nisar.mjs
```

El script requiere Node 22.18+ o Node 24 y `curl`. No usa contraseñas y no descarga archivos HDF5. El acceso a los archivos científicos requiere Earthdata Login según la [guía oficial de ASF](https://nisar-docs.asf.alaska.edu/asf-search/). Nunca introducir credenciales en el frontend ni en el repositorio.

### Estado frente al enunciado

| Requisito | Estado |
| --- | --- |
| Aplicación interactiva | Implementado; revisión visual exhaustiva pendiente |
| Uno o más cambios de superficie | Humedales elegidos; cambios todavía simulados |
| Lugares de todo el mundo | Cuatro regiones en América, Asia y África |
| Uso de NISAR | Metadatos oficiales y enlaces de consulta integrados |
| Rastreo de cambios reales mediante radar | Pendiente de procesamiento y validación de los productos científicos |
| Aprobación o clasificación NASA | No evaluada; no se afirma |

El siguiente paso científico es obtener un par GCOV con autenticación Earthdata, verificar calidad y cobertura, procesar mediciones comparables y validar un método de detección de inundación. Las imágenes de consulta por sí solas no cumplen ese paso.

## Bienvenida, identidad y libro interactivo

La aplicación abre ahora en una portada ilustrada con accesos a NISAR real, el libro y el laboratorio. La bienvenida presenta la capacidad actual de exploración sin atribuir mediciones reales al simulador.

- Logo original vectorial: `public/terrapulse-logo.svg`. Combina hoja, pulso y órbita; está integrado en el menú, la bienvenida y el favicon. Puede descargarse desde la portada.
- Ilustración SVG de un humedal, incluida en el código; no requiere cargar una imagen externa.
- Libro de seis capítulos sobre valor, problemática, ciencia, solución, validación y acción.
- Índice, botones de página y flechas del teclado cuando el libro tiene foco.
- Escenarios ilustrados de agua, señal de radar conceptual, recorrido por etapas y una pregunta con retroalimentación.
- Marcador de lectura local opcional y botón para volver al principio.
- Vista de dos páginas en escritorio y páginas apiladas en móvil. Animaciones respetan `prefers-reduced-motion`.

El contenido distingue explícitamente ilustraciones, datos de demostración y observaciones oficiales. Los escenarios del libro no son diagnósticos ambientales.

## Recuperación de vistas previas NISAR

La vista NISAR ya no depende de cargar `catalog.json` por red para iniciar: el catálogo verificado se incorpora al paquete compilado. Las consultas actuales siguen siendo opcionales y no eliminan el catálogo anterior cuando fallan.

Cada adquisición usa el mismo componente `RadarPreview`:

- Intenta la imagen local, si existe para ese identificador exacto, y después la URL oficial.
- Limita cada intento a seis segundos y permite reintentar manualmente.
- Mientras carga o si falla, dibuja el contorno de la adquisición usando su geometría oficial. No necesita teselas ni imágenes externas. Se identifica expresamente como cobertura geográfica, **no imagen radar**.
- Mantiene fechas, trayectoria, cuadro, polarización y enlace de procedencia.
- Reinicia el estado al cambiar de adquisición y conserva las imágenes locales de identificadores coincidentes al actualizar.
- Rechaza geometrías vacías o coordenadas inválidas recibidas del catálogo externo.

Verificación: ocho pruebas unitarias y comprobación en Chromium de las cuatro regiones con peticiones de imagen bloqueadas, modos de comparación, consulta fallida y reintento con fuente disponible. El caso de recuperación usa una imagen de prueba temporal; no sustituye ni se incorpora a las observaciones oficiales.

Esta recuperación evita paneles vacíos, pero no repara la cadena de certificados del proveedor/red ni constituye disponibilidad garantizada de sus imágenes. La visualización radar real y la medición científica siguen sujetas a las limitaciones descritas anteriormente.

## Desplegar esta versión en Vercel

El repositorio incluye `vercel.json`, `.nvmrc` y `engines.node` para declarar:

| Ajuste | Valor |
| --- | --- |
| Framework | Vite |
| Node.js | 24.x |
| Install Command | `npm ci` |
| Build Command | `npm run check` |
| Output Directory | `dist` |
| Root Directory | Carpeta que contiene `package.json` y `vercel.json` |

`npm run check` ejecuta las ocho pruebas y compila TypeScript/Vite. No consulta ASF ni descarga imágenes durante el despliegue. No requiere variables de entorno para este frontend. No uses `npm run dev` ni `npm run preview` como comando de compilación.

1. Sube los cambios al repositorio conectado a Vercel, incluidos `public`, `src`, `scripts`, `tests`, el lockfile y los archivos de configuración.
2. Confirma que Root Directory apunta a esta carpeta. No selecciones `src`, `public` ni `dist`.
3. Genera un nuevo despliegue desde el commit actualizado. Volver a desplegar un commit antiguo no incorpora estos cambios locales.
4. Abre el dominio de producción que muestra Vercel. Si solicita iniciar sesión, revisa Settings → Deployment Protection y la audiencia que quieres permitir. No es un error de React.
5. Si falla la compilación, copia el primer error de Build Logs. Un aviso de tamaño del bundle de Vite no equivale a un fallo.

Comprobaciones realizadas: instalación limpia con `npm ci` usando caché local, ocho pruebas, compilación y recorridos de bienvenida/libro/laboratorio en Chromium. Las imágenes y mapas externos continúan dependiendo de sus proveedores.

La interfaz incluye recuperación ante excepciones de renderizado y texto inicial cuando JavaScript no termina de cargar. Esto no sustituye la revisión de logs ni garantiza ausencia de fallos.
