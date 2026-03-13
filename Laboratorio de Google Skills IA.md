# Guía de Estudio: SDK de IA Generativa de Google en Vertex AI

Este documento sirve como una guía de estudio consolidada basada en el laboratorio "Comienza a usar la IA generativa de Google con el SDK de IA generativa". Aquí encontrarás los conceptos clave y los fragmentos de código en Python necesarios para interactuar con los modelos de Gemini a través del SDK de Vertex AI.

---

## 1. Configuración del Entorno y Aprovisionamiento

Antes de interactuar con el modelo, es necesario configurar el entorno de Google Cloud y el SDK de Python.

### Aprovisionamiento de Recursos
Para usar Vertex AI, la API correspondiente debe estar habilitada en tu proyecto de Google Cloud. Aunque en un entorno de producción esto se automatizaría con herramientas como Terraform, puedes hacerlo manualmente con `gcloud`:

```bash
# Habilita la API de Vertex AI en tu proyecto
gcloud services enable aiplatform.googleapis.com
```

### Instalación de Librerías
Necesitarás la librería de Python para la plataforma de Vertex AI.

```bash
pip install google-cloud-aiplatform
```

### Inicialización en Python
En tu script o notebook, inicializa el SDK con tu ID de proyecto y la ubicación deseada. Dentro de un entorno de Vertex AI (como un notebook de Workbench), la autenticación es automática.

```python
import vertexai

# Reemplaza con los datos de tu proyecto
PROJECT_ID = "qwiklabs-gcp-01-128b8ae317b5"  
LOCATION = "us-central1"

vertexai.init(project=PROJECT_ID, location=LOCATION)
```

---

## 2. Interactuando con el Modelo Gemini

Una vez configurado el entorno, puedes comenzar a interactuar con los modelos de Gemini.

### Cargar un Modelo
Instancia un objeto `GenerativeModel` para el modelo que deseas usar. `gemini-1.0-pro-vision` es multimodal (texto, imágenes, video), mientras que `gemini-1.0-pro` es solo para texto.

```python
from vertexai.generative_models import GenerativeModel, Part

# Carga un modelo multimodal
multimodal_model = GenerativeModel("gemini-1.0-pro-vision")
```

### Enviar Prompts de Texto
Usa el método `generate_content` para enviar un prompt de texto simple.

```python
response = multimodal_model.generate_content("Explica la computación cuántica en términos sencillos.")
print(response.text)
```

### Enviar Prompts Multimodales
Puedes combinar texto con otros tipos de datos, como imágenes alojadas en Google Cloud Storage (GCS).

```python
image_uri = "gs://cloud-samples-data/generative-ai/image/office-desk.jpeg"
image_part = Part.from_uri(image_uri, mime_type="image/jpeg")

prompt_text = "Describe esta imagen. ¿Qué objetos ves en el escritorio?"

response = multimodal_model.generate_content([prompt_text, image_part])
print(response.text)
```

### Usar Instrucciones de Sistema
Define el comportamiento o rol del modelo para guiar sus respuestas a lo largo de una sesión.

```python
system_instruction = "Eres un asistente de viajes experto. Tus respuestas deben ser concisas y útiles para un turista."

model_with_system_instructions = GenerativeModel(
    "gemini-1.0-pro",
    system_instruction=[system_instruction]
)

response = model_with_system_instructions.generate_content("¿Cuáles son los 3 mejores lugares para visitar en Roma?")
print(response.text)
```

---

## 3. Configuración y Control del Modelo

Puedes ajustar cómo el modelo genera las respuestas y qué tipo de contenido está permitido.

### Parámetros de Generación (`GenerationConfig`)
Controla la creatividad y longitud de la respuesta con parámetros como `temperature`, `top_p` y `max_output_tokens`.

```python
from vertexai.generative_models import GenerationConfig

generation_config = GenerationConfig(
    temperature=0.9,          # Más alto = más creativo
    max_output_tokens=2048,   # Longitud máxima de la respuesta
    top_p=1.0,
)

response = multimodal_model.generate_content(
    "Escribe un haiku sobre la programación.",
    generation_config=generation_config
)
print(response.text)
```

### Filtros de Seguridad (`SafetySettings`)
Define umbrales para bloquear contenido potencialmente dañino.

```python
from vertexai.generative_models import HarmCategory, HarmBlockThreshold

safety_settings = {
    HarmCategory.HARM_CATEGORY_DANGEROUS_CONTENT: HarmBlockThreshold.BLOCK_ONLY_HIGH,
    HarmCategory.HARM_CATEGORY_HATE_SPEECH: HarmBlockThreshold.BLOCK_ONLY_HIGH,
    HarmCategory.HARM_CATEGORY_HARASSMENT: HarmBlockThreshold.BLOCK_ONLY_HIGH,
    HarmCategory.HARM_CATEGORY_SEXUALLY_EXPLICIT: HarmBlockThreshold.BLOCK_ONLY_HIGH,
}

response = multimodal_model.generate_content(
    "Un prompt que podría ser riesgoso", # Reemplazar con un prompt de prueba
    safety_settings=safety_settings
)

# Si el prompt fue bloqueado, la respuesta no contendrá texto.
print(response.text)
```

---

## 4. Gestión de Conversaciones (Chat)

Para diálogos de múltiples turnos, el SDK proporciona una interfaz de chat que mantiene el historial de la conversación.

### Chat de Múltiples Turnos
Usa `start_chat` para iniciar una conversación. El modelo recordará los mensajes anteriores.

```python
chat = multimodal_model.start_chat(history=[])

response = chat.send_message("Hola, mi color favorito es el azul.")
print(response.text)

response = chat.send_message("¿Recuerdas cuál es mi color favorito?")
print(response.text)
```

### Streaming de Respuestas
Para recibir la respuesta en fragmentos a medida que se genera (útil para interfaces interactivas), usa `stream=True`.

```python
chat = multimodal_model.start_chat()
response_stream = chat.send_message("Escribe un cuento corto sobre un robot que descubre la música.", stream=True)

print("Respuesta en streaming:")
for chunk in response_stream:
    print(chunk.text, end="")
```

### Solicitudes Asíncronas
Para aplicaciones que no pueden bloquearse mientras esperan una respuesta, usa los métodos asíncronos.

```python
import asyncio

async def send_async_message():
    chat = multimodal_model.start_chat()
    response = await chat.send_message_async("¿Cuál es la capital de Japón?")
    print(response.text)

# Para ejecutar la función asíncrona en un entorno adecuado
# asyncio.run(send_async_message())
```

---

## 5. Funciones Avanzadas del SDK

### Conteo de Tokens
Calcula cuántos tokens consumirá un prompt antes de enviarlo, útil para controlar costos y límites.

```python
prompt = "Este es un ejemplo de texto para contar tokens."
token_count = multimodal_model.count_tokens(prompt)
print(f"El prompt tiene {token_count.total_tokens} tokens.")
```

### Llamadas a Funciones (Function Calling)
Permite que el modelo solicite la ejecución de funciones externas que tú definas (ej. para consultar una API o base de datos).

```python
from vertexai.generative_models import Tool, FunctionDeclaration

# 1. Define la estructura de una función que el modelo puede "llamar"
get_weather_func = FunctionDeclaration(
    name="get_current_weather",
    description="Obtiene el clima actual para una ubicación específica.",
    parameters={
        "type": "object",
        "properties": {
            "location": {
                "type": "string",
                "description": "La ciudad, ej. 'San Francisco'"
            }
        },
        "required": ["location"]
    },
)

# 2. Agrega la función a una herramienta (Tool)
weather_tool = Tool(function_declarations=[get_weather_func])

# 3. Crea un modelo que conozca esta herramienta
model_with_tool = GenerativeModel("gemini-1.0-pro", tools=[weather_tool])

# 4. Envía un prompt que pueda activar la herramienta
prompt = "¿Qué tiempo hace en Boston?"
response = model_with_tool.generate_content(prompt)

# El modelo no ejecuta la función, sino que devuelve una solicitud para que tú la ejecutes
function_call = response.candidates.content.parts.function_call
print(function_call)
```

### Predicción por Lotes
Procesa una gran cantidad de prompts de forma asíncrona. Ideal para trabajos que no requieren una respuesta inmediata.

```python
# Debes tener un archivo JSONL en GCS con tus prompts.
# ej: gs://tu-bucket/prompts.jsonl
input_uri = "gs://..." 

# Define una carpeta en GCS para los resultados.
output_uri = "gs://..."

batch_job = multimodal_model.batch_predict(
    dataset=[input_uri],
    destination_uri_prefix=output_uri,
)

print("Trabajo de predicción por lotes iniciado.")
print(f"Estado: {batch_job.state}")
# Puedes monitorear el progreso en la consola de Vertex AI.
```

### Almacenamiento de Contexto en Caché (`Cached Content`)
Almacena en caché contextos grandes (como documentos largos) para reducir la latencia y el costo en solicitudes posteriores sobre el mismo contexto.

```python
from vertexai.generative_models import caching, Content, Part

# El siguiente código es ilustrativo.
# Debes definir las variables 'large_document_text' y 'model_name'.

# 1. Define el contenido a cachear
# large_document_text = "..." # Un string con un documento muy largo
# model_name = "gemini-1.5-flash-001" # Un modelo que soporte caching

# 2. Crea la caché
# cache = caching.CachedContent.create(
#     model_name=model_name,
#     contents=[Content(parts=[Part.from_text(large_document_text)])],
# )

# 3. Carga el modelo desde la caché
# model_from_cache = GenerativeModel.from_cached_content(cached_content=cache)

# 4. Haz preguntas sobre el contenido cacheado (más rápido y barato)
# response = model_from_cache.generate_content("Resume el documento en 3 puntos clave.")
# print(response.text)

# 5. Borra la caché cuando ya no se necesite
# cache.delete()

print("Ejemplo conceptual de Context Caching. Descomenta y adapta para usar.")
```

### Embeddings de Texto
Convierte texto en vectores numéricos (embeddings) para tareas como búsqueda semántica, clasificación o clustering.

```python
from vertexai.language_models import TextEmbeddingModel

embedding_model = TextEmbeddingModel.from_pretrained("text-embedding-004")

embeddings = embedding_model.get_embeddings([
    "Me encanta el helado de chocolate", 
    "Prefiero el helado de vainilla"
])

for embedding in embeddings:
    vector = embedding.values
    print(f"Dimensiones del vector de embedding: {len(vector)}")
    # print(vector[:5]) # Imprime los primeros 5 valores
```