# Creación de una solución de análisis multimodal con Azure Content Understanding

## Introducción

Las organizaciones actuales manejan una gran cantidad de información que habitualmente se encuentra diversificada en múltiples formatos, tales como documentos, imágenes, videos y grabaciones de audio. Extraer datos y contexto valioso de este contenido multiformato suele ser un proceso difícil, laborioso y que consume mucho tiempo. Por esta razón, las organizaciones frecuentemente se ven obligadas a desarrollar soluciones complejas que integran múltiples tecnologías de análisis especializadas para cada tipo de archivo.

**Azure Content Understanding** es un servicio integral diseñado para simplificar este flujo: permite crear consolidadores y analizadores impulsados por Inteligencia Artificial capaces de extraer información estructurada a partir de contenido en prácticamente cualquier formato.

*(Diagrama de Azure Content Understanding extrayendo información de documentos, archivos de audio, vídeos e imágenes).*

En este módulo, exploraremos las capacidades de **Azure Content Understanding** y aprenderemos cómo utilizar este servicio para construir analizadores personalizados de extremo a extremo.

## ¿Qué es Azure Content Understanding?

**Azure Content Understanding** es un servicio de IA generativa que permite extraer información y datos valiosos a partir de múltiples tipos de contenido. Al utilizar este servicio, puedes construir rápidamente aplicaciones capaces de analizar datos complejos y generar salidas estructuradas, lo cual es fundamental para automatizar y optimizar procesos comerciales.

Este servicio está disponible a través de **Microsoft Foundry**. Para utilizarlo, primero debes aprovisionar un recurso de Microsoft Foundry dentro de tu suscripción de Azure. Una vez aprovisionado, puedes desarrollar y gestionar tus soluciones de análisis mediante tres opciones diferentes:

- En el **portal de Microsoft Foundry**.
- A través de **Content Understanding Studio**.
- Mediante la **API de Azure Content Understanding (Content Understanding API)**.

### Análisis de Contenido Multimodal

La gran ventaja de **Azure Content Understanding** es su capacidad para extraer información desde los formatos de contenido más comunes. Esto te permite unificar esfuerzos utilizando un único servicio, lo que resulta en un proceso de desarrollo ordenado y coherente para crear verdaderas soluciones de análisis multiplataforma.

A continuación, se detallan los tipos de contenido compatibles:

#### 1. Documentos y Formularios
Puedes utilizar este servicio para analizar documentos y formularios estructurados o no estructurados, con el fin de recuperar valores de campos específicos. Por ejemplo, podrías extraer los datos clave (como monto total, fecha y proveedor) de una factura para, posteriormente, automatizar su procesamiento de pagos.

#### 2. Imágenes
Permite analizar imágenes para deducir información a partir de objetos visuales (como gráficos o esquemas). Por ejemplo, puedes identificar defectos físicos en productos, detectar la presencia de objetos o personas específicas, o extraer otro tipo de información que solo esté disponible visualmente.

#### 3. Audio / Sonido
El análisis de audio es ideal para automatizar tareas centradas en la voz. Puedes resumir llamadas de conferencia, determinar el sentimiento de los clientes en conversaciones grabadas, o incluso extraer datos relevantes que se hayan dictado en mensajes telefónicos.

#### 4. Video
Hoy en día, el formato en video representa un inmenso volumen de los datos capturados. Con **Azure Content Understanding**, puedes analizar y extraer perspectivas e información clave directamente de los videos. Algunos ejemplos útiles incluyen: extraer puntos importantes de las grabaciones de una videoconferencia, realizar resúmenes automáticos de presentaciones o detectar actividades específicas a partir de material de cámaras de seguridad.

## Creación de un Analizador de Azure Content Understanding

Las soluciones de **Azure Content Understanding** se basan en la creación de un recurso conocido como **analizador (Analyzer)**. Un analizador se entrena para extraer información específica a partir de un tipo determinado de contenido, basándose de forma estricta en un **esquema (Schema)** que el usuario define.

El proceso general para crear una solución cuenta con los siguientes cuatro pasos:

1. **Crear un recurso de Foundry**: Aprovisionar un recurso base en Microsoft Foundry.
2. **Definir un esquema**: Establecer un esquema que detallará la información que se desea extraer. Esto se suele realizar cargando un archivo de contenido de ejemplo y aplicando una "plantilla de analizador".
3. **Compilar el analizador**: Entrenar o construir el analizador con la ayuda del esquema y los tipos de datos definidos.
4. **Usar el analizador**: Desplegarlo para extraer campos estructurados a partir de nuevos contenidos de forma continua.

Para acelerar el desarrollo, Microsoft proporciona numerosas plantillas de analizadores. Además, gracias a las capacidades subyacentes de inteligencia artificial generativa, el servicio a menudo puede identificar con precisión los valores de datos del contenido de ejemplo y asignarlos automáticamente a los elementos del esquema (sin entrenamiento intensivo previo). Aún así, es posible etiquetar manualmente algunos campos (como en facturas o documentos complejos) para perfeccionar y mejorar el rendimiento del analizador.

### Creación de un analizador en Content Understanding Studio

Aunque es posible desarrollar toda la solución programáticamente a través de una API REST o un SDK, el uso de **Content Understanding Studio** proporciona una interfaz completamente visual para crear proyectos, definir esquemas y probar analizadores de manera sencilla.

> **Sugerencia:** En el portal general de Microsoft Foundry, solo hay disponible un puñado de modelos preconstruidos. Para poder crear, etiquetar y probar **analizadores personalizados**, siempre debes recurrir a *Content Understanding Studio*.

#### 1. Creación del Proyecto
Dentro de *Content Understanding Studio*, se puede crear un nuevo proyecto vinculándolo a un recurso de Foundry existente. Esta acción aprovisionará automáticamente otros recursos subyacentes en tu suscripción de Azure necesarios para que funcione:
- **Cuenta de Almacenamiento (Storage)**: Para guardar los documentos subidos.
- **Key Vault**: Para almacenar de forma segura claves y credenciales.

> **Nota:** Los esquemas solo se pueden crear en aquellas regiones concretas de Azure donde el servicio de Azure Content Understanding esté admitido oficialmente.

#### 2. Definición del Esquema
El paso inicial dentro del proyecto consiste en preparar el esquema que instruirá al analizador.
Se debe cargar un archivo inicial representativo (puede ser un documento PDF, una imagen JPEG, un audio MP3 o un vídeo MP4). Luego, en una vistosa interfaz de edición, se especifica qué información o campos el analizador debe aprender a reconocer y recuperar. 

> **Nota Adicional:** El tipo de campos y plantillas disponibles variarán dependiendo del formato del archivo cargado. Por ejemplo, en archivos de documentos, es posible habilitar la extracción opcional de códigos de barras (Barcodes) y fórmulas matemáticas, funcionalidad que no estaría presente en un archivo de audio.

#### 3. Pruebas Continuas
Una gran ventaja del Studio es que permite probar el rendimiento del analizador en cualquier momento de su ciclo vital. Puedes ejecutar pruebas instantáneas usando un nuevo documento de prueba y verificar *in-situ* la salida en formato JSON que devolverá a las aplicaciones cliente; comprobando si ha podido extraer los campos del esquema configurado con anterioridad.

#### 4. Compilación del Analizador
Una vez que el esquema sea satisfactorio en las pruebas visuales, se procede a "construir" (Build) el analizador. 
Esta acción estabilizará su forma y lo hará accesible públicamente para cualquier aplicación cliente utilizando un punto de conexión (Endpoint) API bajo el paraguas de tu recurso en Foundry. Posteriormente, si las necesidades de metadatos cambian, es habitual crear "nuevas versiones" con un esquema refinado, cada una conviviendo bajo distintos nombres sin afectar a las otras versiones en producción.

## Uso de la API de Azure Content Understanding

La **API de Azure Content Understanding** proporciona una interfaz de programación completa para crear, administrar y consumir analizadores personalizados desde tu propio código.

Para utilizar esta API, la aplicación cliente debe enviar solicitudes HTTP al punto de conexión (*Endpoint*) de Content Understanding asociado a tu recurso de Microsoft Foundry, incluyendo siempre una de las claves de autorización en el encabezado de la petición. Tanto el Endpoint como las claves (Keys) pueden consultarse desde Azure Portal o directamente en el portal de Microsoft Foundry.

> **Sugerencia:** De forma alternativa y más segura, puedes conectarte programáticamente autenticando el proyecto a través Microsoft Entra ID.

### Uso de la API para analizar contenido

El escenario más común es emplear la API para enviar archivos a un analizador que ya hemos creado previamente y recuperar los resultados del análisis estructurado. Dado que procesar archivos es pesado, la solicitud inicial es tratada como una **tarea asincrónica**, en donde la API primero devolverá un identificador de operación (Operation ID). 

La aplicación cliente luego es responsable de hacer un sondeo continuo (Polling), o de enviar una solicitud GET recurrente con dicho identificador de operación para verificar el estado de la tarea, hasta que finalmente marque completada y retorne toda la información esperada en formato JSON.

#### Ejemplo del flujo de solicitud de análisis

**1. Solicitud inicial (`POST`):**

Para analizar un documento alojado en internet, la aplicación cliente podría enviar una solicitud POST a la función `analyze` con el siguiente cuerpo en formato JSON:

```http
POST {endpoint}/contentunderstanding/analyzers/{analyzer}:analyze?api-version=2025-11-01
Content-Type: application/json

{
  "inputs": [
    {
      "url": "https://host.com/doc.pdf"
    }
  ]
}
```

> **Nota importante:** En el ejemplo anterior se especifica una URL pública del documento. Si en lugar de una URL necesitas cargar los datos binarios del archivo local directamente en la solicitud, es imperativo que utilices la operación `analyzeBinary` en lugar de `analyze`.

**2. Respuesta inicial (Pendiente):**

Suponiendo que la petición inicial se haya autenticado e iniciado correctamente, la API devolverá un estado indicando que la tarea no ha comenzado o está en curso, junto con el ID de la operación:

```http
Operation-Id: 1234abcd-1234-abcd-1234-abcd1234abcd
Operation-Location: {endpoint}/contentunderstanding/analyzerResults/1234abcd-1234-abcd-1234-abcd1234abcd?api-version=2025-11-01

{
  "id": "1234abcd-1234-abcd-1234-abcd1234abcd",
  "status": "NotStarted"
}
```

**3. Sondeo del estado (`GET`):**

Con el ID devuelto, la aplicación cliente debe enviar una solicitud GET constante al punto de conexión `analyzerResults` para verificar periódicamente cuándo finalizará el trabajo.

```http
GET {endpoint}/contentunderstanding/analyzerResults/1234abcd-1234-abcd-1234-abcd1234abcd?api-version=2025-11-01
```

Una vez que la operación marque como `Succeeded`, la respuesta revelará toda la carga JSON definitiva con los campos y contenido extraído según lo que se configuró en el esquema original del analizador.

## Laboratorio: Extraer información de contenido multimodal

En este laboratorio, utilizarás **Azure Content Understanding** para extraer información de diversos tipos de contenido, como una factura, la imagen de una diapositiva con gráficos, la grabación de audio de un mensaje de voz y la grabación de vídeo de una llamada de conferencia.

Este ejercicio dura aproximadamente 40 minutos.

### 1. Crear un recurso y un proyecto de Microsoft Foundry

Las funciones que vamos a utilizar en este ejercicio requieren un recurso y un proyecto de Microsoft Foundry.

1. En un navegador web, abre el portal de Microsoft Foundry en `https://ai.azure.com` e inicia sesión con tus credenciales de Azure. Cierra cualquier panel de consejos o de inicio rápido que se abra la primera vez que inicies sesión.
2. Asegúrate de que el interruptor de **New Foundry** esté activado para que estés usando **Foundry (nuevo)**.
3. Si no se te solicita que crees un nuevo proyecto automáticamente, selecciona el nombre del proyecto en la esquina superior izquierda y, a continuación, selecciona **Crear nuevo proyecto** (Create new project).
4. Asigna un nombre a tu proyecto y expande las opciones avanzadas para especificar la siguiente configuración:
   - **Nombre del proyecto**: Proporciona un nombre válido para tu proyecto.
   - **Recurso de Foundry**: Usar el valor predeterminado.
   - **Región**: Elige una región compatible (ej. Este de Estados Unidos).
   - **Suscripción**: Tu suscripción a Azure.
   - **Grupo de recursos**: Crear o seleccionar un grupo de recursos.
5. Selecciona **Crear** (Create) y espera a que se cree tu proyecto.

### 2. Descargar contenido

El contenido que vas a analizar se encuentra en un archivo `.zip`. Descárgalo y extráelo en una carpeta local.

1. En una nueva pestaña del navegador, descarga el archivo `content.zip` desde su repositorio (o utiliza la ruta proveída en el laboratorio) y guárdalo en una carpeta local.
2. Extrae el archivo `content.zip` descargado y visualiza los archivos que contiene. Utilizarás estos archivos para explorar los analizadores de Azure Content Understanding.

> **Nota:** Si solo te interesa explorar el análisis de una modalidad específica (documentos, imágenes, vídeo o audio), puedes saltar a la tarea relevante a continuación. Para obtener la mejor experiencia, se recomienda realizar todas las tareas.

### 3. Probar analizadores preconfigurados en Microsoft Foundry

Azure Content Understanding incluye analizadores preconfigurados de **Lectura** (Read) y **Diseño** (Layout) que pueden extraer texto y elementos estructurales de documentos sin requerir ninguna configuración personalizada. Estos analizadores preconfigurados están disponibles directamente en el portal de Foundry (nuevo) como modelos de *AI Services*.

#### Uso del analizador de "Diseño" (Layout) en el área de juegos (Playground)

1. En el portal de Microsoft Foundry, asegúrate de que el interruptor de New Foundry esté activado.
2. Selecciona **Construir** (Build) en el menú superior derecho y, a continuación, selecciona **Modelos** (Models) en el panel izquierdo.
3. Selecciona la pestaña **Servicios de IA** (AI Services) para ver los modelos preconfigurados proporcionados por Foundry Tools.
4. Busca y selecciona **Azure Content Understanding - Layout**.
5. Esto abre la página del Área de juegos (Playground) del analizador de Layout, donde puedes probar el modelo en datos de muestra o tus propios archivos.
6. En el Playground, usa la opción para cargar tus propios datos y carga el archivo `invoice-1234.pdf` desde la carpeta donde extrajiste los archivos de contenido.
7. Ejecuta el analizador y espera a que se complete el análisis.
8. Revisa los resultados. Puedes ver el contenido extraído como salida formateada o como datos JSON sin procesar. Observa que el analizador Layout extrae texto, tablas y elementos estructurales como párrafos y secciones del documento.

> **Nota:** Los analizadores preconfigurados *Read* y *Layout* extraen contenido de los documentos sin requerir un modelo de IA generativa. *Read* extrae elementos de texto (palabras, párrafos, fórmulas y códigos de barras), mientras que *Layout* extrae adicionalmente tablas, figuras, estructura del documento, hipervínculos y anotaciones. Son muy útiles para la extracción de contenido de propósito general, pero no extraen campos personalizados específicos como importes de facturas o nombres de proveedores.

9. Opcionalmente, vuelve a la pestaña de AI Services y prueba **Azure Content Understanding - Read** con el mismo archivo para comparar los resultados. Observa que *Read* extrae texto sin el análisis de diseño o estructura.

### 4. Configurar Content Understanding Studio para analizadores personalizados

Para extraer campos específicos de tu contenido (como importes de facturas, nombres de personas que llaman o participantes de una reunión), debes compilar **analizadores personalizados**. Estos se crean en **Content Understanding Studio**, una herramienta web independiente dedicada a construir y probar analizadores con esquemas (Schemas) personalizados.

1. En una nueva pestaña del navegador, abre **Content Understanding Studio** en `https://contentunderstanding.ai.azure.com`.
2. Si se te solicita, inicia sesión con las mismas credenciales de Azure que usaste para el portal de Foundry.
3. En la página de Configuración (Settings) (o si se te redirige para configurar tu recurso), selecciona el botón **+ Add resource** (+ Agregar recurso).
4. Selecciona el recurso de Foundry que creaste anteriormente y selecciona **Next > Save** (Siguiente > Guardar).

> **Sugerencia:** Asegúrate de que la casilla "Habilitar la implementación automática de los modelos necesarios si no hay valores predeterminados disponibles" (Enable autodeployment for required models...) esté seleccionada. Esto garantiza que tu recurso se configure con los modelos necesarios (`GPT-4o`, `text-embedding-3`, etc.) que obligatoriamente requieren los analizadores personalizados.

5. Una vez que tu recurso esté conectado, estarás listo para crear analizadores personalizados. Selecciona **Content Understanding** en la navegación superior para ir a la página de inicio.

### 5. Extraer información de documentos de facturas

Vas a crear un analizador personalizado que pueda extraer campos específicos de facturas. Crearás un proyecto, definirás un esquema basado en una factura de muestra y, a continuación, construirás un analizador reutilizable.

#### Crear una Cuenta de Almacenamiento (Storage account)

El Studio requiere una cuenta de **Azure Blob Storage** para almacenar los datos utilizados. Debes crear una en el mismo grupo de recursos que tu recurso de Foundry.

1. En una nueva pestaña del navegador, abre el portal de Azure (`https://portal.azure.com`).
2. Selecciona **+ Crear un recurso** (+ Create a resource), busca **Storage account** y crea una nueva con la siguiente configuración:
   - **Suscripción**: Tu suscripción de Azure.
   - **Grupo de recursos**: El mismo de tu recurso de Foundry.
   - **Nombre de la cuenta de almacenamiento**: Un nombre único a nivel mundial.
   - **Región**: La misma región de tu recurso de Foundry.
   - **Tipo de almacenamiento preferido**: Azure Blob Storage o Data Lake Storage Gen 2.
   - **Rendimiento**: Estándar (Standard).
   - **Redundancia**: Almacenamiento redundante localmente (LRS).
3. Selecciona **Revisar y crear** (Review + create), y luego en **Crear**.

#### Definir un esquema (Schema) para el análisis de facturas

1. En **Content Understanding Studio**, en la sección de proyectos personalizados, selecciona el botón **Empezar** (Get started) y luego **Crear** (Create).
2. Selecciona **Extraer contenido y campos con un esquema personalizado** (Extract content and fields with a custom schema) y crea un proyecto con esta configuración:
   - **Nombre del proyecto**: Invoice analysis.
   - **Descripción**: Extract data from an invoice.
   - **Configuración avanzada**:
     - **Recurso conectado**: Confirma tu recurso de Foundry.
     - **Conectar cuenta de almacenamiento**: Selecciona la que acabas de crear.
     - **Contenedor de Blob**: Crea un nuevo contenedor llamado `content-understanding`.
3. Espera a que se cree el proyecto.
4. Carga el archivo `invoice-1234.pdf` de tus archivos de contenido extraídos.
5. El servicio clasificará tus datos y recomendará plantillas de analizador. En la ventana "Elegir una plantilla" (Choose a template), selecciona la plantilla **Factura** (Invoice) y pulsa en **Guardar** (Save).
6. La plantilla incluye campos comunes que se encuentran en las facturas. Elimina mediante la papelera los campos que no te sirvan, por ejemplo el campo `BillingAddress` (Dirección de facturación).
7. En la barra superior, pulsa **Sugerir** (Suggest). Esto analizará el documento base e intentará proponer campos que tengan sentido, como la matriz de ítems de la factura. Selecciona **Save**.
8. Usa el botón **+ Add new field** para añadir el siguiente campo:
   - **Nombre del campo**: `TotalQuantity`
   - **Descripción**: Número total de artículos en la factura
   - **Tipo de valor**: String
   - **Método**: Auto
9. Selecciona la pestaña **Test** (Probar) y luego en **Ejecutar análisis** (Run analysis) para probar cómo funciona el esquema sobre el archivo.
10. Revisa el valor extraído de los campos y verifica que estén correctos.

#### Construir y probar el analizador

Ahora que el esquema funciona bien en la prueba, vas a "compilar" (Build) el analizador para usarlo frente a documentos *similares*, pero nuevos.

1. Selecciona el botón **Construir analizador** (Build analyzer) en la parte superior, con estas propiedades:
   - **Nombre**: `invoiceanalyzer`
   - **Descripción**: Invoice analyzer
2. Cuando el analizador esté compilado, selecciona **Ir a la lista de analizadores** (Jump to analyzer list) para ver todos los analizadores creados y selecciona tu `invoiceanalyzer`.
3. En la página de tu analizador, ve a la pestaña **Test**.
4. Ahora, carga un archivo diferente y nuevo, por ejemplo `invoice-1235.pdf` y ejecuta el análisis.
5. En el panel lateral, revisa y verifica que el analizador personal extrajo los campos correctos nuevamente. Revisa el panel de **Resultados** (Results) para entender la respuesta en código puro JSON que llegaría a una aplicación externa.

---

### 6. Extraer información de una imagen de presentación (Diapositiva)

Vas a construir un analizador personalizado para extraer datos o gráficas incrustadas directamente en una simple imagen estática (`.jpg`).

1. En tu lista de Proyectos del Studio, selecciona **Create** > **Extract content and fields with a custom schema**.
2. **Nombre**: Slide analysis. Mismos recursos avanzados que antes.
3. Sube el archivo `slide-1.jpg`. Selecciona la plantilla **Image analysis** y guarda.
4. La plantilla de análisis de imágenes viene en blanco por defecto. Debes agregar campos manualmente usando **+ Add new field**:
   - `Title` | Slide title | String | Generate
   - `Summary` | Summary of the slide | String | Generate
   - `Charts` | Number of charts on the slide | Integer | Generate
5. Añade un nuevo campo llamado `QuarterlyRevenue` con el tipo de valor "Lista de objetos" (List of objects). En el icono de tabla para subcampos, agrega:
   - `Quarter` | Which quarter? | String | Generate
   - `Revenue` | Revenue for the quarter | Number | Generate
6. Regresa al primer nivel, y añade un nuevo campo de tabla o matriz de objetos llamado `ProductCategories`:
   - `ProductCategory` | Product category name | String | Generate
   - `RevenuePercentage` | Percentage of revenue | Number | Generate
7. Guarda todo. Selecciona la pestaña **Test**, ejecuta el análisis (`Run analysis`) y verifica los subcampos detectados del gráfico.

#### Construir y probar el analizador de diapositiva

1. Haz clic en **Build analyzer**.
   - **Nombre**: `slideanalyzer`
   - **Descripción**: Slide image analyzer
2. Una vez hecho, ve a tu analizador en la lista y abre su propia pestaña **Test**.
3. Carga la imagen de prueba alternativa `slide-2.jpg` y ejecuta.
4. Revisa los resultados. Notarás que la diapositiva 2 *no* incluye categorías de producto, por ende esos campos quedarán nulos, pero el título o cuadros estadísticos trimestrales sí se recuperarán exitosamente. 

---

### 7. Extraer información de un buzón de voz grabado (Audio)

Ahora veremos la capacidad de extracción de datos a partir de llamadas o buzones de voz.

1. Crea otro proyecto, con el nombre **Voicemail analysis**.
2. Sube el archivo `call-1.mp3`. Selecciona la plantilla para audio (**Audio analysis template**).
3. En el panel derecho de contenido, selecciona **Get transcription preview** (Obtener vista previa de la transcripción) para ver el texto crudo transcrito del audio.
4. Crea los campos que quieras que el LLM extraiga del audio:
   - `Caller` | Person who left the message | String | Generate
   - `Summary` | Summary of the message | String | Generate
   - `Actions` | Requested actions | String | Generate
   - `CallbackNumber` | Telephone number to return the call | String | Generate
   - `AlternativeContacts` | Alternative contact details | List of strings | Generate
5. Selecciona **Run analysis**. *El análisis de audio es pesado y puede tardar un poco más en ejecutarse*.
6. Una vez completado, revisa los valores en el panel "Fields". Verás que la IA fue capaz de inferir quién llamó o qué teléfono dejó.
7. Construye el analizador (`Voicemailanalyzer`) y repite el proceso de pruebas usando el archivo extra `call-2.mp3`.

---

### 8. Extraer información de videoconferencias (Video)

La modalidad más compleja. Crearemos un esquema de video.

1. Crea el último proyecto llamado **Conference call video analysis**.
2. Sube el archivo `meeting-1.mp4`. Utiliza el **Video analysis template**.
3. Obteniendo la transcripción, podrás ver los subtítulos generados del video. Define los campos:
   - `Summary` | Summary of discussion | String | Generate
   - `Participants` | Count of participants | Integer | Generate
   - `ParticipantNames` | Names of participants | List of Strings | Generate
   - `SharedSlides` | PowerPoint slides shown | List of Strings | Generate
   - `AssignedActions` | List of objects. Subcampos: `Task` (tarea, String) y `AssignedTo` (a quién se le asignó, String).
4. Ejecuta un "Test" contra el video 1. El modelo no solo observará el audio de cada segmento, sino si se compartieron o leyeron documentos en las "Diapositivas compartidas" (SharedSlides).
5. Como con todos los anteriores, construye el analizador (`meetinganalyzer`) y pruébalo en la lista usando el archivo secundario `meeting-2.mp4` interactuando finalmente con el JSON resultante de los "shots" o segmentos de la llamada.

### 9. Limpiar recursos

Si ya has terminado de trabajar con el servicio API de Content Understanding y las herramientas del Studio, elimina todos tus recursos del laboratorio, especialmente el grupo de recursos de Azure (Resource Group) completo. Esto asegurará que no te cobren costos de almacenamiento permanente de Microsoft Foundry.

---

## Evaluación del Módulo

1.  **¿Para qué tipos de soluciones de inteligencia artificial está diseñada Azure Content Understanding para ayudarle a crear?**

    a. Bots de chat que traducen automáticamente entre varios idiomas hablados y escritos.

    b. Analizadores que extraen información de documentos, imágenes, vídeos y archivos de audio. ✅

    c. Generadores de imágenes que crean visualizaciones basadas en descripciones.

    **Justificación:** Azure Content Understanding es el servicio específico diseñado para construir analizadores que extraen estructuras, texto y esquemas a partir de múltiples modalidades como archivos de video, audios, documentos o imágenes estáticas.

    **Glosario / Comentarios:** (Ninguno)

2.  **¿Qué herramienta gráfica debe usar para crear un proyecto de Azure Content Understanding?**

    a. Microsoft Visual Studio.

    b. Azure Machine Learning Studio.

    c. Estudio de Comprensión de Contenidos. ✅

    **Justificación:** Aunque la API se puede usar con otras herramientas programáticas, la herramienta gráfica (interfaz visual) designada específicamente para crear proyectos definidos, construir esquemas y probar analizadores es *Content Understanding Studio* (traducido como "Estudio de Comprensión de Contenidos" en el examen).

    **Glosario / Comentarios:**
    *   **"Estudio de Comprensión de Contenidos"**: Traducción oficial al español de *Content Understanding Studio*.

3.  **¿Qué debe definir para la información que desea extraer del contenido?**

    a. Esquema. ✅

    b. Un índice.

    c. Un clúster.

    **Justificación:** Se requiere la definición de un "Esquema" (Schema) que le indica a la Inteligencia Artificial cuáles son los campos (y sus tipos de datos esperados) que el Analizador tendrá que identificar y extraer tras ser construido.

    **Glosario / Comentarios:**
    *   **"Esquema"**: Traducción de *Schema*.

---

## Resumen

**Azure Content Understanding** es un servicio de inteligencia artificial horizontal diseñado para simplificar la extracción de información estructurada a partir de una amplia variedad de formatos de contenido.

En este módulo, hemos aprendido los conceptos fundamentales para dominar este servicio:

- **Versatilidad Multimodal**: Capacidad para procesar documentos, imágenes, audio y video bajo un mismo marco de trabajo.
- **Flujo de Trabajo en el Studio**: El uso de **Content Understanding Studio** como la herramienta visual clave para crear proyectos y definir esquemas de forma intuitiva.
- **Analizadores y Esquemas**: La importancia de definir un **Esquema (Schema)** preciso que sirva como plano para que el **Analizador (Analyzer)** extraiga exactamente los campos necesarios.
- **Integración Programática**: Cómo consumir los resultados de forma asincrónica a través de la **API de Content Understanding**, utilizando identificadores de operación para realizar el sondeo de resultados.

Con estas herramientas, es posible transformar flujos de datos no estructurados en información accionable para cualquier aplicación empresarial.
