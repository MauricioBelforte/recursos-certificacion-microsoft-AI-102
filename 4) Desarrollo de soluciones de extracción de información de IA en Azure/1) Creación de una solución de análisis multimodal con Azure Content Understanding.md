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
