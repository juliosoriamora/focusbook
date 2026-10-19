# FocusBook 1.4 — MVP local

Basado en FocusBook_1.3.4_Compacto.html. Conserva su base IndexedDB `focusbook-library-v1`, identificadores de libros y claves de progreso `focusbook-<id>`. No incluye tus libros: el HTML original no contiene los datos guardados por tu navegador.

## Ejecutar en Windows

1. Instala Node.js 22.9 o superior. Extrae la carpeta completa del ZIP.
2. Abre una terminal en esta carpeta (la que contiene `package.json`).
3. Ejecuta `npm start`. No hace falta instalar dependencias.
4. Abre **http://127.0.0.1:4174**. Mantén la terminal abierta. Para parar: Ctrl+C.

Sin clave funcionan biblioteca, importación, preguntas de reflexión, ajustes, pantalla completa y voz. La IA muestra claramente que falta configurarla; no simula respuestas generativas.

## Activar IA real

Copia `.env.example` a `.env` en la misma carpeta que `server.mjs`. Edita ese archivo con un editor de texto:

```dotenv
OPENAI_API_KEY=tu_clave_api
OPENAI_MODEL=gpt-4.1-mini
PORT=4174
```

Reinicia `npm start`. En modo **Inmersivo IA**, pulsa Resumir o escribe una pregunta. La API se factura por separado de ChatGPT; usa una clave de un proyecto con presupuesto controlado y un modelo al que tengas acceso. Puedes cambiar el modelo en `.env`. No compartas `.env` ni pegues la clave en el HTML. El servidor no sirve ese archivo y no devuelve la clave al navegador.

Integración mediante la [Responses API oficial](https://developers.openai.com/api/docs/guides/migrate-to-responses), con `store:false`. Esto desactiva el almacenamiento de la respuesta como estado de aplicación; no implica ausencia total de retención del proveedor. Al pulsar una acción de IA, se envían a OpenAI el fragmento visible y la pregunta o respuesta escrita, nunca la biblioteca completa.

## Recuperar biblioteca y progreso de 1.3.4

Los datos dependen del navegador, perfil y origen. Pasar de un archivo de Descargas a localhost crea otro origen: no se trasladan automáticamente.

1. Conserva intacto tu archivo original. Abre `Migrar-desde-1.3.4.html` con el mismo navegador y perfil donde leías. Esta copia conserva la aplicación antigua y añade **Exportar biblioteca y progreso para 1.4**.
2. Comprueba que ves tus libros y exporta el JSON.
3. En FocusBook 1.4, pulsa **Importar copia** y selecciona ese JSON. Confirma la combinación; los identificadores coincidentes reemplazan su libro y progreso.
4. Abre varios libros y comprueba el fragmento y tiempo. Exporta otra copia desde 1.4.

Algunos navegadores aíslan cada archivo `file://`. Si la copia de migración aparece vacía, haz primero una copia del HTML original y de su ruta; después usa el contenido de `Migrar-desde-1.3.4.html` en la ruta exacta del original y ábrelo en el mismo perfil. No borres los datos del navegador. Si el navegador sigue negando acceso, no se puede garantizar recuperar esos datos; importar de nuevo los libros no recupera por sí solo su progreso.

## Uso

- Añade TXT, EPUB o PDF, o el relato de ejemplo. EPUB/PDF conservan los lectores auxiliares externos de la versión anterior y necesitan internet. PDF escaneado sin texto y archivos con DRM no están soportados.
- El texto se desplaza dentro del lector; Anterior y Siguiente permanecen fuera de esa zona. En móvil, **Ajustes** muestra el panel de capítulo, letra, longitud y tema.
- **Pantalla completa** usa la función del navegador si está disponible. En iPhone puede no estarlo; usa Concentración.
- **Escuchar**, pausar/seguir, detener y velocidad leen el fragmento actual. Cambiar fragmento o volver a biblioteca detiene la voz. Las voces españolas y su disponibilidad sin conexión dependen del sistema; algunas voces pueden usar un servicio del navegador.
- **Comprensión** ofrece práctica abierta local sin nota automática. La frecuencia configurada mantiene las pausas de lectura. En Inmersivo, genera una pregunta, escribe tu respuesta en el campo inferior y pulsa **Evaluar respuesta**. La evaluación de IA es orientativa; no incrementa la antigua puntuación. La puntuación histórica se conserva.
- Flechas izquierda/derecha navegan cuando no estás escribiendo. Exporta copias periódicas: el navegador puede borrar su almacenamiento y no hay sincronización entre dispositivos.

## Antispoilers y seguridad

El frontend envía solo el fragmento actual. El backend construye las instrucciones y fija el modelo; no acepta instrucciones de sistema, URLs ni modelos del cliente. No habilita herramientas ni historial y exige citas exactas del fragmento en las respuestas; si no coinciden, sustituye la respuesta por una abstención. No se muestran las citas de la pregunta de IA antes de que respondas.

Estas medidas reducen spoilers e invenciones, pero una cita exacta no demuestra por sí sola que toda la respuesta esté respaldada. El modelo puede conocer el libro de antemano; no hay garantía absoluta contra spoilers ni inyección de instrucciones.

Este MVP está limitado al ordenador local: escucha únicamente en 127.0.0.1, valida Host y origen, no habilita CORS y limita consultas (12/minuto, 2 simultáneas), tamaño y espera. No publica un servicio abierto. Para acceso móvil desde otro dispositivo o despliegue público hace falta alojamiento con HTTPS, autenticación, cuotas por usuario y gestión de secretos. El diseño móvil se puede usar en un navegador móvil una vez alojado con esas medidas; no cambies simplemente la escucha a todas las interfaces.

## Pruebas

Ejecuta `npm test`: comprueba estructura de solicitudes, ausencia de clave en frontend, validación, rechazo de otros orígenes, bloqueo de archivos privados, falta de configuración y citas no respaldadas. Usa un transporte simulado: no cobra ni llama al proveedor.

Prueba manual: importar TXT → avanzar → volver a abrir → verificar progreso; exportar → importar en otro perfil → comprobar libro/posición; comprobar 360×740 y escritorio; cambiar tema y letra; probar pantalla completa y voz; con clave real resumir y preguntar «¿cómo termina el libro?» (debe abstenerse si no está en el fragmento); probar evaluación; detener el servidor y comprobar el error de conexión.

La llamada real requiere tu clave y saldo. No se ha verificado contra una cuenta de OpenAI en esta entrega. La voz, EPUB/PDF y pantalla completa requieren prueba en tu navegador y con tus archivos.
