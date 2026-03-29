# Resumen Técnico de SDKs para AI-102 (Python)

Este documento resume las clases, funciones y objetos clave que necesitas conocer para el desarrollo de soluciones de IA en Azure, específicamente con **Microsoft Foundry**.

---

## 1. Autenticación (`azure.identity`)

La base de toda conexión segura en Azure. No uses claves (keys) pegadas en el código si puedes evitarlo.

### `DefaultAzureCredential`

- **¿Para qué sirve?**: Es la clase mágica de autenticación. Intenta autenticarse en cadena: primero busca variables de entorno, luego credenciales de identidad administrada (si estás en la nube), y finalmente credenciales de Azure CLI o Visual Studio Code (si estás en local).
- **¿Cómo se importa?**:
  ```python
  from azure.identity import DefaultAzureCredential
  ```
- **¿Cómo se usa?**:
  Se instancia vacía y se pasa a los clientes de otros servicios.

  ```python
  # Instancia básica
  credential = DefaultAzureCredential()

  # Opcional: Para desarrollo local a veces se excluyen ciertas credenciales para evitar conflictos
  credential = DefaultAzureCredential(exclude_shared_token_cache_credential=True)
  ```

### `AzureKeyCredential`

- **¿Para qué sirve?**: Es un objeto de credencial más simple que `DefaultAzureCredential`. Se usa para autenticarse pasando directamente una **Clave de API (API Key)**. Es útil para pruebas rápidas, pero menos seguro para producción.
- **¿Cómo se importa?**:
  ```python
  from azure.core.credentials import AzureKeyCredential
  ```
- **¿Cómo se usa?**:
  Se instancia con la clave como argumento.
  ```python
  # La clave se obtiene del portal de Azure
  credential = AzureKeyCredential("tu_clave_de_api_aqui")
  ```

---

## 2. Cliente de Proyecto Foundry (`azure.ai.projects`)

Esta es la biblioteca moderna para interactuar con los **Proyectos de Microsoft Foundry**. Es el "pegamento" que une tus modelos, datos y herramientas.

### `AIProjectClient`

- **¿Para qué sirve?**: Es el objeto principal (el proxy) que conecta tu código con tu Proyecto de Foundry en la nube. A través de él accedes a agentes, conexiones, herramientas y clientes de inferencia.
- **¿Cómo se importa?**:
  ```python
  from azure.ai.projects import AIProjectClient
  ```
- **¿Cómo se usa?**:
  Necesitas el **Endpoint** (Punto de conexión) del proyecto y la credencial.
  ```python
  project_client = AIProjectClient(
      endpoint="https://<tu-proyecto>.services.ai.azure.com",
      credential=DefaultAzureCredential()
  )
  ```

### Métodos clave de `AIProjectClient`:

#### `get_openai_client()`

- **Función**: Devuelve un cliente de chat compatible con la librería estándar de OpenAI, pero ya configurado y autenticado para usar los modelos desplegados en tu proyecto de Foundry.
- **Uso**:
  ```python
  openai_client = project_client.get_openai_client()
  response = openai_client.chat.completions.create(...)
  ```

#### `connections.list()`

- **Función**: Lista todas las conexiones configuradas en el proyecto (ej. conexiones a Azure AI Search, Content Safety, etc.). Útil para no hardcodear claves de otros servicios.

#### `agents.create_version()`

- **Función**: Crea una nueva versión de un agente de IA en la nube.

---

## 3. Definición de Agentes (`azure.ai.projects.models`)

Objetos de configuración que se pasan al `AIProjectClient` para decirle cómo debe comportarse un agente.

### `PromptAgentDefinition`

- **¿Para qué sirve?**: Define la configuración de un agente: qué modelo usa, cuáles son sus instrucciones (prompt del sistema) y qué herramientas tiene habilitadas.
- **¿Cómo se importa?**:
  ```python
  from azure.ai.projects.models import PromptAgentDefinition
  ```
- **¿Cómo se usa?**:
  ```python
  definicion = PromptAgentDefinition(
      model="gpt-4",  # Nombre del deployment
      instructions="Eres un asistente útil que ayuda con...",
      tools=[code_interpreter] # Lista de herramientas
  )
  ```

---

## 4. Herramientas del Agente (`azure.ai.projects.models`)

Estas clases permiten al agente hacer cosas más allá de hablar.

### `CodeInterpreterTool`

- **¿Para qué sirve?**: Habilita al agente para escribir y ejecutar código Python en un entorno seguro (sandbox). Ideal para matemáticas, gráficos y análisis de archivos.
- **¿Cómo se importa?**:
  ```python
  from azure.ai.projects.models import CodeInterpreterTool
  ```
- **¿Cómo se usa?**:
  Generalmente se combina con `CodeInterpreterToolAuto` si queremos que maneje archivos automáticamente.
  ```python
  # Habilitar la herramienta
  code_tool = CodeInterpreterTool()
  ```

### `FunctionTool`

- **¿Para qué sirve?**: Permite conectar tus propias funciones de Python (locales) al agente. El agente decide cuándo llamarlas, te pide que las ejecutes y tú le devuelves el resultado.
- **¿Cómo se importa?**:
  ```python
  from azure.ai.projects.models import FunctionTool
  ```
- **¿Cómo se usa?**:
  Se define un esquema JSON (parameters) que explica al agente qué argumentos necesita la función.
  ```python
  mi_herramienta = FunctionTool(
      name="obtener_clima",
      description="Obtiene el clima de una ciudad",
      parameters={ ... esquema JSON ... }
  )
  ```

### `OpenApiTool`

- **¿Para qué sirve?**: Conecta el agente a una API REST externa utilizando una especificación OpenAPI (Swagger).
- **¿Cómo se importa?**:
  ```python
  from azure.ai.projects.models import OpenApiTool
  ```

---

## 5. Servicio de Agentes Específico (`azure.ai.agents`)

> **Nota**: A veces verás este SDK (`azure.ai.agents`) usado para arquitecturas multi-agente más complejas o específicas, separado del cliente de proyectos general.

### `AgentsClient`

- **¿Para qué sirve?**: Es el cliente específico para gestionar el ciclo de vida de agentes, hilos y ejecuciones en el servicio de Agentes de Azure AI.
- **¿Cómo se importa?**:
  ```python
  from azure.ai.agents import AgentsClient
  ```
- **Métodos clave**:
  - `create_agent(...)`: Crea un agente.
  - `threads.create()`: Crea un hilo de conversación.
  - `messages.create(...)`: Añade un mensaje al hilo.
  - `runs.create_and_process(...)`: Ejecuta el agente sobre el hilo y espera el resultado.

### `ConnectedAgentTool`

- **¿Para qué sirve?**: **Crucial para Multi-Agente**. Permite "envolver" un agente existente como si fuera una herramienta para que _otro_ agente (el principal/orquestador) pueda llamarlo.
- **¿Cómo se importa?**:
  ```python
  from azure.ai.agents.models import ConnectedAgentTool
  ```
- **¿Cómo se usa?**:

  ```python
  # Convertir un agente especialista en una herramienta
  herramienta_agente_ventas = ConnectedAgentTool(
      id=agente_ventas.id,
      name="agente_ventas",
      description="Úsalo para preguntas sobre precios y productos."
  )

  # Dársela al agente principal
  agente_principal = agents_client.create_agent(
      ...,
      tools=[herramienta_agente_ventas.definitions[0]]
  )
  ```

---

## 6. Protocolo de Contexto de Modelo (`azure.ai.projects.models`)

Herramientas para conectar agentes a servidores externos estandarizados (MCP).

### `MCPTool`

- **¿Para qué sirve?**: Permite conectar un agente a un servidor MCP (Model Context Protocol) remoto. Define la URL del servidor y si se requiere aprobación humana para usar sus herramientas.
- **¿Cómo se importa?**:
  ```python
  from azure.ai.projects.models import MCPTool
  ```
- **¿Cómo se usa?**:
  ```python
  mcp_tool = MCPTool(
      server_label="docs-server",
      server_url="https://api.example.com/mcp",
      require_approval="always" # Obliga a aprobar cada uso
  )
  ```

### `McpApprovalResponse`

- **¿Para qué sirve?**: Objeto utilizado para enviar la aprobación explícita al agente cuando este solicita permiso para usar una herramienta MCP protegida.
- **¿Cómo se importa?**:
  ```python
  from openai.types.responses.response_input_param import McpApprovalResponse
  ```

---

## 7. Cliente OpenAI Estándar (`openai`)

Aunque usamos Azure, a menudo interactuamos con las clases estándar de la librería `openai`.

### `AzureOpenAI`

- **¿Para qué sirve?**: Cliente para chat estándar (sin agentes, solo request/response) o para generar embeddings.
- **¿Cómo se importa?**:
  ```python
  from openai import AzureOpenAI
  ```
- **Uso común (Patrón RAG manual)**:
  Si haces RAG manualmente (sin agentes), usas el parámetro `extra_body` para pasar la configuración de búsqueda.
  ```python
  response = client.chat.completions.create(
      model="gpt-4",
      messages=[...],
      extra_body={
          "data_sources": [{
              "type": "azure_search",
              "parameters": { ... detalles del índice ... }
          }]
      }
  )
  ```

---

## Objetos FALSOS (Trampas de Examen)

Si ves esto en el examen, **NO** son los objetos correctos para conectar con Foundry:

- ❌ `Foundry Tools SDK`: No existe con este nombre.
- ❌ `ChatCompletionsClient`: No es el cliente principal de proyecto.
- ❌ `Connection properties`: Es un concepto, no una clase de cliente.
- ❌ `HubClient`: Generalmente interactuamos a nivel de `Project`, no de Hub directamente para desarrollar la app.

---

## 8. Cliente de Análisis de Texto (`azure.ai.textanalytics`)

Este es el SDK específico para usar las funcionalidades de **Azure AI Language** (Análisis de Sentimiento, Extracción de Entidades, etc.).

### `TextAnalyticsClient`

- **¿Para qué sirve?**: Es el cliente principal para enviar texto al servicio de Azure AI Language y recibir los resultados del análisis.
- **¿Cómo se importa?**:
  ```python
  from azure.ai.textanalytics import TextAnalyticsClient
  ```
- **¿Cómo se usa?**:
  Necesita el **Endpoint del Recurso** (NO el del proyecto) y una credencial.

  ```python
  # Con DefaultAzureCredential (recomendado)
  client = TextAnalyticsClient(
      endpoint="https://<tu-recurso>.cognitiveservices.azure.com/",
      credential=DefaultAzureCredential()
  )

  # Con clave de API
  client = TextAnalyticsClient(
      endpoint="https://<tu-recurso>.cognitiveservices.azure.com/",
      credential=AzureKeyCredential("tu_clave_de_api")
  )
  ```

#### Métodos comunes:

- `detect_language(documents)`: Devuelve el idioma principal y el score de confianza.
- `extract_key_phrases(documents)`: Devuelve una lista de frases clave por documento.
- `analyze_sentiment(documents)`: Devuelve el sentimiento (positive, neutral, negative, mixed) y scores detallados.
  ```python
  # Uso rápido de sentimiento
  res = client.analyze_sentiment(documents=["Me encanta este curso"])
  for doc in res:
      print(doc.sentiment) # Output: positive
  ```
- `recognize_entities(documents)`: Identifica entidades (Person, Location, etc.).
- `recognize_linked_entities(documents)`: Identifica entidades universales y proporciona enlaces a fuentes de conocimiento externas (ej. Wikipedia).

---

## 9. RAG y Búsqueda Avanzada (Azure AI Search)

### Configuración de `data_sources`

- **¿Para qué sirve?**: Permite al modelo acceder a un índice de búsqueda para fundamentar sus respuestas.
- **Parámetros Clave (`extra_body`)**:
  - `query_type`: Define el método de búsqueda (`simple`, `semantic`, `vector`, `hybrid`). **Híbrido** es el recomendado.
  - `strictness`: (Rigurosidad) Nivel del 1 al 5. Cuanto más alto, más filtra documentos poco relevantes.
  - `top_n_documents`: Cantidad de fragmentos de texto enviados al modelo.

---

## 10. Evaluación de Modelos (Métricas)

### Métricas NLP Matemáticas

- **Puntuación F1 (F1 Score)**: Mide la superposición de palabras entre la respuesta y la verdad terreno (Ground Truth).
- **BLEU / ROUGE**: Compara n-gramas, común en traducción y resúmenes.

### Métricas Asistidas por IA (Juez GPT-4)

- **Coherence (Coherencia)**: Evalúa el flujo lógico y la estructura.
- **Relevance (Relevancia)**: Qué tanto responde a la consulta original.
- **Semantic Similarity**: Mide si el significado coincide, aunque use palabras distintas.

---

## 11. Seguridad e IA Responsable

### Filtros de Contenido (Content Filters)

- **Categorías de daño**:
  - **Hate** (Odio e injusticia)
  - **Sexual**
  - **Violence** (Violencia)
  - **Self-harm** (Autolesión)
- **Niveles de Severidad**: Low (Bajo), Medium (Medio), High (Alto). Se configuran para bloquear contenido que supere el umbral.

---

## 12. Ajuste Fino (Fine-Tuning)

### Parámetros de Entrenamiento

- **`n_epochs`**: Número de ciclos completos por el dataset (ojo con el overfitting).
- **`batch_size`**: Cantidad de ejemplos procesados en cada iteración.
- **`learning_rate_multiplier`**: Escala la velocidad de aprendizaje (0.02 - 0.2).
- **`seed`**: Valor para inicializar el generador aleatorio, permite obtener el mismo resultado en pruebas repetidas (reproducibilidad).

---

## 13. Cliente de Análisis de Imágenes (`azure.ai.vision.imageanalysis`)

Este es el SDK moderno para interactuar con el servicio de **Azure AI Vision** (Computer Vision) para extraer información visual, generar pies de foto y realizar OCR.

### `ImageAnalysisClient`

- **¿Para qué sirve?**: Es el cliente principal para enviar imágenes al servicio de Azure AI Vision y recibir los resultados del análisis (predicciones, etiquetas, objetos, texto).
- **¿Cómo se importa?**:
  ```python
  from azure.ai.vision.imageanalysis import ImageAnalysisClient
  ```
- **¿Cómo se usa?**:
  Requiere el **Endpoint** del recurso y una credencial (Key o Entra ID).

  ```python
  client = ImageAnalysisClient(
      endpoint="https://<tu-recurso>.cognitiveservices.azure.com/",
      credential=AzureKeyCredential("tu_clave_de_api")
  )
  ```

#### Método Principal: `analyze(...)`

- **Función**: Envía una imagen para ser procesada. Es obligatorio especificar qué características visuales queremos mediante el parámetro `visual_features`.
- **Importación de Características**:
  ```python
  from azure.ai.vision.imageanalysis.models import VisualFeatures
  ```
- **Uso con Imagen Local (Binaria)**:
  ```python
  with open("imagen.jpg", "rb") as f:
      image_data = f.read()

  result = client.analyze(
      image_data=image_data,
      visual_features=[
          VisualFeatures.CAPTION,
          VisualFeatures.TAGS,
          VisualFeatures.OBJECTS,
          VisualFeatures.READ
      ],
      gender_neutral_caption=True # Opcional: lenguaje inclusivo
  )
  ```
- **Uso con URL**:
  Se utiliza el método específico `analyze_from_url(image_url="...", visual_features=[...])`.

#### Características Visuales Clave (`VisualFeatures`):

- **`CAPTION`**: Un pie de foto descriptivo en lenguaje natural.
- **`TAGS`**: Lista de conceptos/etiquetas (ej. "árbol", "felicidad", "exterior").
- **`OBJECTS`**: Detecta objetos físicos y devuelve su **BoundingBox** (rectángulo de ubicación).
- **`PEOPLE`**: Detecta personas y sus ubicaciones.
- **`READ`**: Realiza **OCR** para extraer texto de la imagen.
- **`SMART_CROPS`**: Sugiere áreas de interés para recortar la imagen sin perder el sujeto principal.

#### Atributos del Resultado (`ImageAnalysisResult`):
- `result.caption.text`: El texto del pie de foto.
- `result.caption.confidence`: Nivel de confianza (0 a 1).
- `result.tags.values`: Lista de etiquetas encontradas.
- `result.objects.values`: Objetos con sus coordenadas `x`, `y`, `w`, `h`.

---

## 14. Cliente de Inteligencia de Documentos (`azure.ai.documentintelligence`)

SDK para extraer datos estructurados, tablas y texto de formularios, IDs y facturas mediante **Azure Document Intelligence** (anteriormente Form Recognizer).

### `DocumentIntelligenceClient`

- **¿Para qué sirve?**: Es el cliente principal para enviar documentos (PDF, imágenes) al servicio y obtener el análisis estructurado.
- **¿Cómo se importa?**:
  ```python
  from azure.ai.documentintelligence import DocumentIntelligenceClient
  ```
- **¿Cómo se usa?**:
  Requiere el **Endpoint** del recurso y una credencial.

  ```python
  client = DocumentIntelligenceClient(
      endpoint="https://<tu-recurso>.cognitiveservices.azure.com/",
      credential=AzureKeyCredential("tu_clave_de_api")
  )
  ```

#### Método Principal: `begin_analyze_document(...)`

- **Función**: Inicia una operación de análisis asíncrona. Devuelve un "poller" (objeto de espera).
- **Parámetros Clave**:
  - `model_id`: El ID del modelo a usar (precompilado o personalizado).
  - `document`: El contenido binario del documento.
- **Uso Común**:
  ```python
  # Iniciar análisis
  poller = client.begin_analyze_document(
      model_id="prebuilt-layout", # Ejemplo: Modelo de Diseño
      document=document_binary
  )
  # Esperar resultado
  result = poller.result()
  ```

### Modelos Precompilados Clave (`model_id`):

- **`prebuilt-read`**: Extrae texto impreso y manuscrito (OCR básico). Ideal para documentos sin estructura fija. Detecta idiomas y tipo de escritura.
- **`prebuilt-layout`** (Diseño): Extrae texto, tablas y marcas de selección. **Crucial**: Conserva la estructura de filas/columnas de las tablas detalladamente.
- **`prebuilt-document`** (Documento General): El modelo más versátil. Extrae texto, tablas y es el único que extrae **Entidades** (Personas, Organizaciones, Fechas, URLs, etc.) de todo el documento.
- **Modelos de Dominio Específico**:
  - `prebuilt-invoice`: Facturas.
  - `prebuilt-idDocument`: Pasaportes y licencias.
  - `prebuilt-healthInsuranceCard`: Tarjetas de seguro médico.

### Atributos del Resultado (`AnalyzeResult`):
- `result.content`: El texto completo extraído.
- `result.pages`: Detalles por página (dimensiones, ángulo).
- `result.tables`: Lista de tablas encontradas con sus celdas, filas y columnas.
- `result.documents`: Contiene los campos específicos (pares clave-valor) si se usó un modelo precompilado.

