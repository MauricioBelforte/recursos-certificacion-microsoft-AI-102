# Temas Pendientes (Extras)

Este archivo contiene los módulos o temas identificados a partir de las evaluaciones de práctica que requieren un estudio profundo posterior.

- **Detección, análisis y reconocimiento de caras (Azure AI Face)**: [Módulo de Microsoft Learn](https://learn.microsoft.com/es-es/training/modules/detect-analyze-recognize-faces/)
  - *Identificado en:* Evaluación de práctica (Pregunta 2 de 50).
  - *Nota técnica clave:* Dominar las versiones de los modelos de detección (ej. `detection_03` para caras borrosas/laterales).

- **Análisis de vídeo con Azure Video Indexer**: [Módulo de Microsoft Learn](https://learn.microsoft.com/en-us/training/modules/analyze-video/) | [Documentación específica](https://learn.microsoft.com/es-es/azure/azure-video-indexer/customize-language-model-how-to?tabs=customizewebportal)
  - *Identificado en:* Evaluación de práctica (Pregunta 5, 6, 7 y 8).
  - *Importancia:* **4** (¡Se repite constantemente!).
  - *Notas técnicas clave:*
    - Flujo API miniaturas: Cargar -> Obtener Índice -> Obtener Miniaturas.
    - **Identificación Multilingüe:** Para contenido en varios idiomas, el parámetro `sourceLanguage` debe establecerse en **"multi-language detection"**.
    - **Reglas de Entrenamiento (Modelo Lenguaje):**
      - Evitar caracteres especiales (~, #, @, %, &), ya que se descarta la oración completa.
      - **NO** poner entradas masivas (ej. 500k frases) porque diluyen el efecto del refuerzo.
      - **NO** repetir frases idénticas (genera sesgo).
      - **SÍ**: Una frase por línea y múltiples ejemplos de oraciones habladas.
      - **SÍ**: Proporcionar múltiples opciones de adaptación.

- **Azure AI Document Intelligence (antiguo Form Recognizer)**: [Ruta de aprendizaje de Microsoft](https://learn.microsoft.com/en-us/training/paths/ai-extract-information/)
  - *Identificado en:* Evaluación de práctica (Pregunta 29 de 50).
  - *Importancia:* **4** (Tema core de extracción).
  - *Notas técnicas clave:*
    - **Modelos Precompilados (Prebuilt Models)**: Son la opción **óptima** cuando el tipo de documento ya existe en Azure (ej. Facturas, IDs, Tarjetas de seguro médico, W-2, Recibos). Minimizan esfuerzo y maximizan precisión.
    - **Modelo de Diseño (Layout Model)**: Extrae texto, marcas de selección, tablas y estructuras, pero no es específico para un tipo de documento.
    - **Azure OCR**: Solo extrae texto sin contexto ni estructura de pares clave-valor.
    - **Modelo Compuesto**: Se usa para agrupar múltiples modelos personalizados, no es necesario si ya hay precompilados disponibles.
  - *Guía de Decisión (Frecuente en Examen):*
    - ¿El documento es una factura/recibo/ID estándar? -> **Prebuilt Model** (Menor esfuerzo).
    - ¿Es un formulario único/específico de la empresa? -> **Custom Model** (Requiere etiquetado/labeling).
    - ¿Solo quieres la estructura de tabla o texto de cualquier documento? -> **Layout Model**.
    - ¿Quieres sacar nombres de personas/lugares de un texto libre? -> **General Document Model**.

- **Azure Content Understanding**: [Intro Microsoft](https://learn.microsoft.com/en-us/azure/ai-services/content-understanding/overview)
  - *Identificado en:* Evaluación de práctica (Pregunta 32 de 50).
  - *Importancia:* **5 (NUEVO TEMA CLAVE)**.
  - *Notas técnicas clave:*
    - **Multimodal**: A diferencia de *Document Intelligence* (solo documentos/PDFs), este saca información de **Video, Audio, Imágenes Y Documentos** en una sola API.
    - **Escenario**: Si el examen dice "vídeo y audio" o "múltiples tipos de contenido con mínimo esfuerzo", la respuesta es **Content Understanding**.
    - **Diferencia Crítica**:
      - **Document Intelligence**: Especialista profundo en formularios, tablas y campos específicos de PDF/Imágenes.
      - **Content Understanding**: Generalista multimodal (Vídeo, Audio, Doc). Orquestador que simplifica extraer de todo un poco.

- **Requisitos Técnicos Document Intelligence**:
  - *Identificado en:* Evaluación de práctica (Pregunta 33 de 50).
  - *Importancia:* **5 (CRÍTICO)**.
  - *Notas técnicas clave:*
    - **Calidad de Entrada**: Se deben usar imágenes de **Alta Resolución** (NO baja calidad).
    - **Formato Recomendado**: El uso de **PDF basados en texto** (en lugar de solo escaneos/imágenes) mejora drásticamente la precisión del OCR porque no tiene que "adivinar" los caracteres.
    - **Especificaciones**: Los archivos deben cumplir requisitos de tamaño (ej. < 500 MB) y formatos (PDF, JPEG, PNG, BMP, TIFF, HEIF).
    - **Selección de Modelo**: Siempre elegir el modelo de dominio específico (ej. **Invoice Model** para facturas), NUNCA modelos genéricos si existe uno especializado.

- **Ingeniería de Preguntas (Prompt Engineering)**: [Mejores prácticas de Microsoft](https://learn.microsoft.com/es-es/azure/foundry/openai/concepts/prompt-engineering)
  - *Identificado en:* Evaluación de práctica (Pregunta 11 de 50).
  - *Notas técnicas clave (Los 3 pilares):*
    - **Sé específico**: Dejar lo menos posible a la interpretación.
    - **Sé descriptivo**: Usar analogías y detalles.
    - **El orden importa**: La posición de las instrucciones y datos afecta la salida.

- **Generación de Imágenes (DALL-E 3 API)**: [Referencia de API de Microsoft](https://learn.microsoft.com/es-es/azure/foundry/openai/reference#image-generation)
  - *Identificado en:* Evaluación de práctica (Pregunta 13 de 50).
  - *Nota técnica clave:* En una solicitud HTTP POST para generación de imágenes:
    - **Cuerpo (Body)**: La única propiedad obligatoria es la **"pregunta" (prompt)**.
    - **Encabezado (Header)/URL**: Las propiedades como `api-key`, `api-version` y el nombre del recurso van en el encabezado o en la URL del endpoint, **no** en el cuerpo.

- **Seguimiento y FeedBack en Implementaciones (Prompt Flow)**: [Guía de Microsoft](https://learn.microsoft.com/en-us/azure/machine-learning/prompt-flow/how-to-enable-trace-feedback-for-deployment?view=azureml-api-2)
  - *Identificado en:* Evaluación de práctica (Pregunta 17 de 50).
  - *Nota técnica clave:* Para recopilar **datos de seguimiento (traces)**, métricas y **comentarios de usuario (feedback)** en una aplicación de IA generativa desplegada (flujo):
    - Se debe usar la integración nativa con **Application Insights**.
    - La configuración se realiza en las **Opciones de diagnóstico** de la implementación.
    - Esto minimiza el esfuerzo administrativo ya que no requiere modificar el archivo YAML del flujo ni variables de entorno manuales.

- **Azure Translator Custom (Puntuación BLEU)**: [Traducción de texto con Translator](https://learn.microsoft.com/es-es/training/modules/translate-text-with-translator-service/)
  - *Identificado en:* Evaluación de práctica (Pregunta 19 de 50).
  - *Nota técnica clave:* La escala de puntuación **BLEU (Bilingual Evaluation Understudy)** mide la calidad de la traducción automática (0 a 100):
    - **40 a 60**: Indica una traducción de **alta calidad**.
    - **60+**: Muy raro, indica que el modelo es casi idéntico a una traducción humana perfecta.
    - **Menos de 20**: Calidad baja/pobre.

- **Azure Speech - Errores en Voz a Texto (WER)**: [Creación de aplicaciones habilitadas para voz](https://learn.microsoft.com/es-es/training/modules/create-speech-enabled-apps/)
  - *Identificado en:* Evaluación de práctica (Pregunta 20 de 50).
  - *Nota técnica clave:* La **Tasa de Errores de Palabra (WER - Word Error Rate)** se descompone en:
    - **Errores de Sustitución**: El modelo confunde una palabra por otra (ej. "casa" por "masa"). **Solución**: Entrenar con nombres personalizados de productos y personas.
    - **Errores de Eliminación**: El modelo omite palabras. **Causa común**: Oradores superpuestos.
    - **Errores de Inserción**: El modelo añade palabras que no existen. **Causa común**: Ruido de fondo (personas hablando detrás).

- **Azure Speech - Transcripción por lotes (Batch Transcription)**: [Guía de Microsoft](https://learn.microsoft.com/es-es/training/modules/create-speech-enabled-apps/)
  - *Identificado en:* Evaluación de práctica (Pregunta 22 de 50).
  - *Nota técnica clave:* Para transcribir grandes volúmenes de audio de forma asíncrona:
    - Se requiere el uso exclusivo de **Azure Storage (Blob Storage)** para almacenar los archivos de entrada.
    - Se proporcionan los URIs de los archivos (SAS tokens comúnmente) a la API de transcripción.
    - Otras bases de datos como Cosmos DB o SQL no son compatibles de forma nativa para esta característica.

- **Azure AI Search - Atributos de campo e Índices**: [Módulo de búsqueda de Azure AI](https://learn.microsoft.com/es-es/training/modules/create-azure-cognitive-search-solution/)
  - *Identificado en:* Evaluación de práctica (Pregunta 26 de 50).
  - *Nota técnica clave:* Dominar los atributos de campo es vital:
    - **facetable**: Permite agrupar resultados por categorías y mostrar el **recuento de aciertos** por cada una (ej. Filtros laterales en Amazon).
    - **filterable**: Permite usar el campo en consultas con el parámetro `$filter`.
    - **key**: Identificador único del documento (solo un campo por índice).
    - **searchable**: El contenido del campo se procesa para búsqueda de texto completo (tokenización).
    - **sortable**: Permite orden ordenar los resultados por este campo.

- **Azure AI Search - Almacén de conocimiento (Knowledge Store)**: [Creación de un almacén de conocimiento](https://learn.microsoft.com/es-es/training/modules/create-knowledge-store-azure-cognitive-search/)
  - *Identificado en:* Evaluación de práctica (Pregunta 27 de 50).
  - *Nota técnica clave:* Se utiliza para minería de conocimiento fuera del buscador:
    - **Propósito**: Almacenar la salida enriquecida de los *skillsets* para **aplicaciones de bajada** (Power BI, ciencia de datos, análisis).
    - **Ubicación**: Se guardan en **Azure Storage** (como tablas, objetos o archivos).
    - **Definición**: Se define dentro de un **conjunto de aptitudes (skillset)**.

- **Azure AI Search - Aptitudes Integradas (Skills)**: [Módulo de Microsoft Learn](https://learn.microsoft.com/es-es/training/modules/create-azure-cognitive-search-solution/)
  - *Identificado en:* Evaluación de práctica (Pregunta 28 de 50).
  - *Aptitud Clave:* **Microsoft.Skills.Util.DocumentExtractionSkill**:
    - Se utiliza para **extraer contenido de un archivo** (como un PDF o Word) *dentro* de la canalización de enriquecimiento. 
    - A diferencia de la extracción inicial del indexador, esta ocurre *durante* el paso de enriquecimiento (Skillset).
  - *Otras aptitudes mencionadas:*
    - **SplitSkill**: Divide el texto en fragmentos (páginas/párrafos).
    - **EntityRecognitionSkill**: Identifica personas, lugares, fechas.
    - **KeyPhraseExtractionSkill**: Extrae frases clave importantes.

- **Azure Language - Respuesta a preguntas (Question Answering)**: [Creación de una solución de respuesta a preguntas](https://learn.microsoft.com/es-es/training/modules/create-question-answer-solution-ai-language/)
  - *Identificado en:* Evaluación de práctica (Preguntas 23, 24 y 25 de 50).
  - *Importancia:* **3** (Ya van 3 puntos seguidos sobre este servicio).
  - *Nota técnica clave:* Este servicio está diseñado para **escenarios estáticos**:
    - **SÍ**: Conversaciones de bot con información estática (FAQs).
    - **SÍ**: Bases de conocimiento con respuestas predefinidas.
    - **SÍ**: Proporcionar la **misma respuesta** a la misma pregunta/comando.
    - **NO**: Información dinámica que cambia en tiempo real o respuestas únicas personalizadas por cada solicitud.
  - *Extracción e Importación de Documentos (FAQ):*
    - **Qué extrae:** Solo **texto con formato**, **direcciones URL** y **listas** (numeradas/viñetas).
    - **Qué NO extrae:** **Imágenes o diagramas** dentro del documento (estos deben ser accesibles vía URL pública).
  - *Costos operativos:*
    - **Rendimiento necesario** (throughput).
    - **Tamaño y número de bases de conocimiento**.
    - (Nota: El nº de idiomas o de editores NO afecta al costo directamente).
