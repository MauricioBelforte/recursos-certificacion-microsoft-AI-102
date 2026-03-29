

# Desarrollo de una solución basada en RAG con sus propios datos mediante Microsoft Foundry

**Retrieval Augmented Generation (RAG)** es un patrón arquitectónico fundamental en las soluciones de IA generativa. Permite "fundamentar" (**Ground**) las respuestas del modelo en sus propios datos empresariales, en lugar de depender únicamente del conocimiento pre-entrenado del modelo.

**Microsoft Foundry** facilita este proceso proporcionando herramientas para:
*   Agregar e ingerir datos.
*   Crear índices de búsqueda.
*   Integrar estos índices con modelos de IA generativa.

---

## Objetivos de Aprendizaje

Al finalizar este módulo, usted será capaz de:

*   **Identificar** la necesidad de fundamentar (**Grounding**) los modelos de lenguaje mediante RAG.
*   **Indexar** datos utilizando **Azure AI Search** para hacerlos accesibles (buscables) para los modelos de lenguaje.
*   **Construir** un agente que utilice RAG sobre sus propios datos dentro del portal de **Microsoft Foundry**.
*   **Implementar** el patrón RAG dentro de un **Flujo de mensajes (Prompt Flow)**.

---


## Contenido del Módulo

1.  **Introducción:** Conceptos básicos de RAG.
2.  **Fundamentación (Grounding):** Cómo conectar el modelo con datos reales.
3.  **Búsqueda (Searchability):** Configuración de **Azure AI Search**.
4.  **Desarrollo de Cliente:** Creación de aplicaciones que consumen el patrón RAG.
5.  **Prompt Flow:** Implementación técnica del flujo RAG.
6.  **Ejercicio Práctico:** Creación de una aplicación "Chat with your data".

---


# Fundamentación de Modelos de Lenguaje (Grounding)

Un desafío común al usar **Modelos de Lenguaje Grandes (LLM)** es que sus respuestas, aunque coherentes, pueden no basarse en hechos reales o en un contexto específico. Este problema se conoce como "alucinación".

La **Fundamentación (Grounding)** es la técnica que resuelve este problema al anclar o conectar la respuesta del modelo a una fuente de información fáctica y relevante.

---

## 1. Escenario Sin Fundamentación (Ungrounded)

Cuando un modelo recibe una pregunta, su única fuente de información son los datos masivos (generalmente de internet) con los que fue entrenado.

*   **Problema:** La respuesta puede ser gramaticalmente correcta y lógica, pero carecer de contexto y ser inexacta. El modelo puede "inventar" detalles, como productos o características que no existen.
*   **Ejemplo:** Al preguntar `¿Qué producto debo usar para hacer X?`, el modelo podría describir un producto ficticio.

---

## 2. Escenario Con Fundamentación (Grounded)

En este enfoque, junto con la pregunta del usuario, se proporciona al modelo un contexto fáctico extraído de una fuente de datos relevante.

*   **Solución:** El modelo utiliza tanto la pregunta como los datos de fundamentación para generar una respuesta que es contextualizada, relevante y precisa.
*   **Ejemplo:** Si se le proporciona un catálogo de productos junto con la pregunta `¿Qué producto debo usar para hacer X?`, la respuesta se basará en los productos reales de ese catálogo.

En este módulo, explorará cómo construir una aplicación de chat que utiliza sus propios datos para fundamentar las respuestas del modelo.


---

# Fundamentación y Patrón RAG (Retrieval Augmented Generation)

Los modelos de lenguaje son excelentes generando texto, pero para ser útiles en entornos empresariales, deben estar **fundamentados (Grounded)** en información objetiva y relevante, en lugar de depender solo de sus datos de entrenamiento.

La **Generación Aumentada por Recuperación (RAG)** es la técnica utilizada para lograr esto.

---

## Descripción de RAG

RAG es un proceso que mejora la precisión de las respuestas del modelo al proporcionarle contexto externo. El patrón general consta de tres pasos:

1.  **Recuperar (Retrieve):** Buscar datos relevantes basados en la consulta inicial del usuario (Prompt).
2.  **Aumentar (Augment):** Combinar la consulta original con los datos recuperados en un nuevo Prompt.
3.  **Generar (Generate):** Enviar el Prompt enriquecido al modelo de lenguaje para obtener una respuesta fundamentada.

Al utilizar RAG, se asegura que el modelo responda utilizando información específica y actual, reduciendo las alucinaciones.

---

## Agregar datos en Microsoft Foundry

**Microsoft Foundry** permite crear agentes personalizados que utilizan sus propios datos para fundamentar las respuestas.

La plataforma soporta diversas conexiones de datos para la ingesta, incluyendo:

*   **Azure Blob Storage**
*   **Azure Data Lake Storage Gen2**
*   **Microsoft OneLake**

También es posible cargar archivos o carpetas directamente en el almacenamiento gestionado por el proyecto.


---


# Hacer que los Datos sean Buscables con Azure AI Search

Para que un agente de IA pueda usar sus propios datos para generar respuestas precisas (patrón RAG), primero es necesario que esos datos se puedan buscar de manera eficiente. **Microsoft Foundry** se integra con **Azure AI Search** para indexar y recuperar contexto relevante.

---

## 1. Uso de un Índice Vectorial (Vector Index)

Aunque la búsqueda de texto tradicional es útil, una solución más avanzada y efectiva para la recuperación de datos se logra mediante un **índice vectorial (vector index)**. Este tipo de índice contiene **incrustaciones (embeddings)**, que son representaciones numéricas del texto.

Una **incrustación (embedding)** es un vector de números de punto flotante que captura el significado semántico de un texto.

**Ejemplo:**
*   Documento 1: "Los niños jugaron alegremente en el parque".
*   Documento 2: "Los niños corrieron felizmente por el parque infantil".

Aunque usan palabras diferentes, ambos textos están relacionados semánticamente. Al convertir estos textos en vectores, su relación se puede medir matemáticamente.

La distancia se calcula midiendo el ángulo entre **los dos vectores completos** (no entre dimensiones individuales). Se utiliza una operación llamada **Producto Punto** que combina todas las dimensiones (ej. 1536) para obtener un único valor de similitud.

Esta técnica se conoce como **similitud del coseno (cosine similarity)** y permite encontrar información conceptualmente similar, no solo coincidencias exactas de palabras.

Para crear estas incrustaciones, se utilizan modelos específicos, como los modelos de **Azure OpenAI** disponibles en Microsoft Foundry.

---

## 2. Creación de un Índice de Búsqueda

Un **índice de búsqueda (search index)** en Azure AI Search es como el catálogo de una biblioteca: organiza el contenido para que sea fácil y rápido de encontrar.

Microsoft Foundry simplifica este proceso. Puedes agregar tus datos y luego usar el asistente del portal para crear un índice con **Azure AI Search**, seleccionando un modelo para generar las **incrustaciones (embeddings)**. El activo del índice se almacena en Azure AI Search y se consulta desde los flujos de chat.

---

## 3. Tipos de Búsqueda en un Índice

Existen varias formas de consultar la información en un índice:

*   **Búsqueda de palabras clave (Keyword Search):** Encuentra coincidencias exactas de los términos de búsqueda.
*   **Búsqueda semántica (Semantic Search):** Comprende el significado de la consulta y busca contenido relacionado semánticamente, yendo más allá de las palabras clave.
*   **Búsqueda vectorial (Vector Search):** Utiliza las representaciones vectoriales (embeddings) para encontrar documentos similares en significado y contexto.
*   **Búsqueda híbrida (Hybrid Search):** Combina las técnicas anteriores (generalmente, palabras clave y vectorial) para obtener los mejores resultados. Las consultas se ejecutan en paralelo y los resultados se unifican.

### Búsqueda Híbrida: La Mejor Opción para IA Generativa

Al crear un índice en Microsoft Foundry para una aplicación de IA generativa, la **búsqueda híbrida (hybrid search)** es la que proporciona los resultados más precisos.

*   Es **precisa** cuando existen coincidencias exactas (gracias a la búsqueda por palabras clave).
*   Es **relevante** cuando solo se encuentra información conceptualmente similar (gracias a la búsqueda vectorial).


---

# Desarrollo de una Aplicación Cliente con Patrón RAG

Una vez que ha creado un índice en **Azure AI Search** con sus datos contextuales, el siguiente paso es consumirlo desde una aplicación cliente utilizando un modelo de **Azure OpenAI**.

El SDK de Azure OpenAI permite "fundamentar" (**Ground**) los **Prompts** enviando los detalles de conexión del índice junto con la solicitud. Esto permite que el modelo utilice su información específica para generar la respuesta.

---

## 1. Implementación Básica (Búsqueda por Palabras Clave)

Para implementar el patrón RAG, se configuran los parámetros de la fuente de datos (`data_sources`) y se envían como un cuerpo extra (`extra_body`) en la llamada a la API de chat.

El siguiente ejemplo en **Python** muestra cómo configurar el cliente y realizar una solicitud que consulta el índice mediante **palabras clave**:

```python
from openai import AzureOpenAI

# 1. Configurar el cliente de Azure OpenAI
cliente_chat = AzureOpenAI(
    api_version = "2024-12-01-preview",
    azure_endpoint = "https://su-endpoint.openai.azure.com",
    api_key = "su-api-key"
)

# 2. Inicializar el Prompt con un mensaje del sistema
mensajes_chat = [
    {"role": "system", "content": "Eres un asistente de IA útil y amigable."}
]

# 3. Añadir la pregunta del usuario
pregunta_usuario = input("Ingresa tu pregunta: ")
mensajes_chat.append({"role": "user", "content": pregunta_usuario})

# 4. Definir los parámetros RAG para conectar con Azure AI Search
# Nota: En este caso, se usa la autenticación por Key y búsqueda por defecto (palabras clave)
# ¡OJO! Este código NO sube archivos. Solo conecta con un índice donde YA subiste los datos antes.
parametros_rag = {
    "data_sources": [
        {
            "type": "azure_search",
            "parameters": {
                "endpoint": "https://su-servicio-search.search.windows.net",
                "index_name": "nombre-del-indice", # <--- Aquí referencias los datos que ya cargaste previamente
                "authentication": {
                    "type": "api_key",
                    "key": "su-search-key",
                }
            }
        }
    ],
}

# 5. Enviar el prompt incluyendo la información del índice
respuesta = cliente_chat.chat.completions.create(
    model="nombre-del-deployment-gpt",
    messages=mensajes_chat,
    extra_body=parametros_rag
)

# 6. Imprimir la respuesta contextualizada
texto_final = respuesta.choices[0].message.content
print(texto_final)
```

---

## 2. Implementación Avanzada (Búsqueda Vectorial)

El ejemplo anterior utiliza la coincidencia de texto literal (palabras clave). Sin embargo, para obtener resultados más relevantes basados en el significado semántico, es recomendable utilizar la **Búsqueda Vectorial (Vector Search)**.

Para ello, se debe modificar la configuración de `rag_params` para especificar:
1.  `query_type`: Establecido en `"vector"`.
2.  `embedding_dependency`: Especifica el **Deployment** del modelo de embeddings (ej. `text-embedding-ada-002`) que se usará para vectorizar la consulta del usuario.

```python
parametros_rag = {
    "data_sources": [
        {
            "type": "azure_search",
            "parameters": {
                "endpoint": "https://su-servicio-search.search.windows.net",
                "index_name": "nombre-del-indice", # <--- Aquí referencias los datos que ya cargaste previamente
                "authentication": {
                    "type": "api_key",
                    "key": "su-search-key",
                },
                # Configuración para consulta basada en vectores (Vector Search)
                "query_type": "vector",
                "embedding_dependency": {
                    # Campo 1: 'type'. Le dice al sistema QUÉ clave buscar (el método)
                    "type": "deployment_name",
                    # Campo 2: 'deployment_name'. Es la clave prometida arriba con el VALOR real
                    "deployment_name": "nombre-del-deployment-embedding",
                },
            }
        }
    ],
}
```


---


# Implementación del Patrón RAG en Prompt Flow

Después de cargar sus datos y crear un índice en **Azure AI Search**, el siguiente paso es orquestar la interacción utilizando **Prompt Flow**.

**Prompt Flow** es una herramienta de desarrollo que permite crear flujos de trabajo ejecutables que conectan modelos de lenguaje (LLM), prompts y código Python para crear aplicaciones de IA complejas.

---

## Estructura de un Flujo

Un flujo típico consta de:
1.  **Entradas (Inputs):** Generalmente la pregunta del usuario y el historial de chat.
2.  **Nodos (Nodes):** Herramientas conectadas que procesan la información.
3.  **Salidas (Outputs):** La respuesta generada por el modelo.

### Herramientas Clave
*   **Python Tool:** Ejecuta código personalizado (útil para formatear datos).
*   **Index Lookup:** Busca valores específicos en un índice vectorial.
*   **Prompt Tool:** Permite crear y probar variantes de mensajes.
*   **LLM Tool:** Envía la solicitud al modelo (ej. GPT-4).

---

## El Flujo RAG Paso a Paso

Para implementar RAG, se suele utilizar una plantilla como "Multi-round Q&A on your data" (Preguntas y respuestas de varios turnos sobre sus datos). El proceso lógico es el siguiente:

### 1. Modificación de la Consulta (Query Modification)
El primer nodo suele ser un LLM que toma el **historial del chat** y la **última pregunta del usuario**.
*   **Objetivo:** Reescribir la pregunta para que sea autónoma.
*   **Ejemplo:** Si el usuario dice "¿Cuánto cuesta?", y el historial hablaba de una bicicleta, el modelo reescribe la pregunta a "¿Cuánto cuesta la bicicleta X?".

### 2. Búsqueda de Información (Index Lookup)
Se utiliza la herramienta **Index Lookup** para consultar el índice de **Azure AI Search**.
*   Utiliza la pregunta reescrita para buscar vectores o palabras clave similares.
*   Devuelve los fragmentos de documentos más relevantes (Top N).

### 3. Generación de Contexto (Context Generation)
Los resultados de la búsqueda suelen venir en formato JSON crudo. Un nodo de **Python** se encarga de:
*   Iterar sobre los resultados.
*   Extraer el texto y la fuente.
*   Concatenarlos en un solo bloque de texto (string) que servirá como contexto.

### 4. Definición del Prompt y Variantes
Se construye el prompt final que se enviará al modelo. Este prompt incluye:
*   El **Mensaje del Sistema** (instrucciones de comportamiento).
*   El **Contexto** generado en el paso anterior.
*   La **Pregunta** del usuario.

**Uso de Variantes:** Puede crear múltiples versiones (variantes) de este prompt para probar qué instrucciones funcionan mejor (ej. "¿Eres un asistente formal o divertido?") y comparar los resultados.

### 5. Chat con Contexto (Chat with Context)
Finalmente, un nodo LLM recibe el prompt enriquecido y genera la respuesta natural para el usuario, fundamentada (**Grounded**) en los datos recuperados.


---


## Laboratorio: Crea una aplicación de IA generativa que utilice tus propios datos

*Nota: Este ejercicio está obsoleto pero se mantiene para fines de estudio del patrón RAG (Retrieval Augmented Generation).*

La **Generación Aumentada por Recuperación (RAG)** es una técnica utilizada para crear aplicaciones que integran datos de fuentes personalizadas en un mensaje para un modelo de IA generativa. En este ejercicio, utilizarás Microsoft Foundry para integrar datos personalizados de folletos de viaje en una solución de chat.

Este ejercicio dura aproximadamente 45 minutos.

**Nota:** El código se basa en una versión preliminar del SDK. Es posible que experimente comportamientos inesperados o errores.

### Crea un centro (Hub) y un proyecto de Microsoft Foundry
1. En un navegador web, abra el portal de Foundry en [https://ai.azure.com] e inicie sesión.
2. Cree un nuevo recurso de **Centro de IA (AI Hub)** y un proyecto con nombres alfanuméricos únicos.
3. Configure la suscripción, el grupo de recursos y la región (Este de EE. UU. 2 o Suecia Central).
4. Espere a que se cree el proyecto y navegue hacia él.

### Implementar modelos
Necesitas dos modelos para implementar tu solución RAG:
- Un modelo de **incrustación (embedding)** para vectorizar los datos de texto.
- Un modelo que genere respuestas en lenguaje natural (modelo de chat).

1. En su proyecto, vaya al catálogo de modelos y filtre por la colección **OpenAI**.
2. Busque e implemente `text-embedding-ada-002` (modelo de embeddings):
   - **Tipo de implementación:** Global Standard.
   - **Límite de tasa (TPM):** 50K.
3. Repita los pasos para implementar un modelo `gpt-4o` (modelo de chat):
   - **Tipo de implementación:** Global Standard.
   - **Límite de tasa (TPM):** 50K.

### Agrega datos a tu proyecto
Los datos consisten en un conjunto de folletos de viaje en formato PDF de la agencia ficticia *Margie’s Travel*.

1. Descargue el archivo de folletos y extráigalo localmente en una carpeta llamada `brochures`.
2. En el portal de Foundry, en su proyecto, vaya a la página de **Datos + índices**.
3. Seleccione **+ Nuevos datos** -> **Cargar archivos/carpetas**.
4. Cargue la carpeta `brochures` y espere a que se listen los archivos PDF.
5. Asigne el nombre `brochures` a los datos.

### Crea un índice para tus datos
Ahora vincularemos los datos a un recurso de **Azure AI Search** para crear el índice.

1. En la pestaña **Índices** de la página de Datos + índices, agregue un nuevo índice:
   - **Ubicación de origen:** Datos en Foundry (seleccione `brochures`).
   - **Configuración del índice:** Cree un nuevo recurso de **Azure AI Search** (Pricing tier: Basic).
2. Una vez creado el servicio de búsqueda, conéctelo en el asistente de Foundry.
3. Configure el índice vectorial:
   - **Nombre:** `brochures-index`.
   - **Configuración de búsqueda:** Agregar búsqueda vectorial.
   - **Modelo de incrustación (Embedding):** Seleccione su despliegue de `text-embedding-ada-002`.
4. Seleccione **Crear** y espere a que se complete el proceso (esto fragmentará los documentos, generará los vectores y registrará el índice).

### Prueba el índice en el área de juegos (Playground)
1. Vaya a la página de **Playgrounds** y abra el **Chat playground**.
2. Asegúrese de que el modelo `gpt-4o` esté seleccionado.
3. Primero, pregunte: `¿Dónde puedo alojarme en Nueva York?` (debería recibir una respuesta genérica).
4. En el panel de configuración, expanda **Agregar sus datos** y seleccione su índice `brochures-index` con tipo de búsqueda **Híbrida (Vector + Palabra clave)**.
5. Vuelva a preguntar lo mismo. Ahora la respuesta se basará en los folletos de *Margie’s Travel*.

### Crea una aplicación cliente RAG
Ahora implementaremos este mismo patrón usando el SDK de Azure OpenAI en Python.

**Preparar la configuración**
1. En el portal de Azure [https://portal.azure.com], abra un Cloud Shell de PowerShell.
2. Clone el repositorio de código:

```bash
rm -r mslearn-ai-foundry -f
git clone https://github.com/microsoftlearning/mslearn-ai-studio mslearn-ai-foundry
```

3. Navegue a la carpeta del proyecto e instale las dependencias:

```bash
cd mslearn-ai-foundry/labfiles/rag-app/python
python -m venv labenv
./labenv/bin/Activate.ps1
pip install -r requirements.txt openai
```

4. Edite el archivo `.env` con las claves y puntos de conexión correspondientes de su proyecto de Foundry y su recurso de Azure AI Search:

```bash
code .env
```

*Debe completar: endpoint y clave de OpenAI, nombres de despliegue de chat y embedding, endpoint y clave de Search, y el nombre del índice (`brochures-index`).*

**Ejecuta la aplicación de chat**
1. Ejecute la aplicación:

```bash
python rag-app.py
```

2. Realice preguntas basadas en los folletos, como: `¿A dónde debería ir de vacaciones para ver arquitectura?`
3. Verifique que la respuesta incluya referencias a las fuentes indexadas.
4. Escriba `quit` para salir.

### Limpieza
Elimine el grupo de recursos desde el portal de Azure una vez finalizado el ejercicio para evitar costos.


### Código Completo del Laboratorio: Aplicación Cliente RAG (Python)

```python
# rag-app.py
# Implementación del patrón RAG utilizando el SDK de OpenAI y Azure AI Search.

import os
from dotenv import load_dotenv
from openai import AzureOpenAI

def main():
    # Cargar variables de entorno del archivo .env
    load_dotenv()
    
    # Configuración de Azure OpenAI
    endpoint = os.getenv("AZURE_OPENAI_ENDPOINT")
    key = os.getenv("AZURE_OPENAI_KEY")
    chat_model = os.getenv("AZURE_OPENAI_CHAT_MODEL")
    embedding_model = os.getenv("AZURE_OPENAI_EMBEDDING_MODEL")

    # Configuración de Azure AI Search
    search_endpoint = os.getenv("AZURE_SEARCH_ENDPOINT")
    search_key = os.getenv("AZURE_SEARCH_KEY")
    search_index = os.getenv("AZURE_SEARCH_INDEX")

    # Inicializar el cliente de Azure OpenAI
    client = AzureOpenAI(
        base_url=f"{endpoint}/openai/deployments/{chat_model}/extensions",
        api_key=key,
        api_version="2023-08-01-preview",
    )

    # Definir el mensaje del sistema para el contexto del asistente
    messages = [
        {
            "role": "system", 
            "content": "Eres un asistente de viajes de Margie's Travel que ayuda a los clientes a planificar sus viajes basándose solo en los folletos proporcionados."
        }
    ]

    print("--- Margie's Travel Chat Bot ---")

    while True:
        user_input = input("Pregunta (o 'quit' para salir): ")
        if user_input.lower() == "quit":
            break

        messages.append({"role": "user", "content": user_input})

        # Configurar los parámetros RAG (data_sources)
        # Aquí definimos la conexión al índice de búsqueda y el modelo de embedding para la consulta vectorial.
        extension_config = {
            "dataSources": [
                {
                    "type": "AzureCognitiveSearch",
                    "parameters": {
                        "endpoint": search_endpoint,
                        "key": search_key,
                        "indexName": search_index,
                        "queryType": "vector",
                        "embeddingDependency": {
                            "type": "deployment_name",
                            "deploymentName": embedding_model
                        }
                    }
                }
            ]
        }

        # Realizar la llamada al modelo incluyendo las fuentes de datos externas
        response = client.chat.completions.create(
            model=chat_model,
            messages=messages,
            extra_body=extension_config
        )

        # Extraer la respuesta fundamentada (Grounded)
        assistant_response = response.choices[0].message.content
        print(f"\nRespuesta: {assistant_response}\n")
        messages.append({"role": "assistant", "content": assistant_response})

if __name__ == "__main__":
    main()
```

---

# Resumen del Módulo: Desarrollo de una solución RAG

En este módulo, ha aprendido a:

*   Identificar la necesidad de fundamentar (**Grounding**) un modelo de lenguaje utilizando la **Generación Aumentada por Recuperación (RAG)**.
*   Indexar datos utilizando **Azure AI Search** para que sean accesibles para los modelos de lenguaje.
*   Crear un agente que utilice RAG sobre sus propios datos en **Microsoft Foundry**.
*   Implementar el patrón RAG dentro de un **Flujo de mensajes (Prompt Flow)**.

---

# Evaluación del módulo

1.  **¿A qué se refiere la base en el contexto de la IA generativa?**

    a. Uso de un modelo de lenguaje implementado localmente.

    b. Uso de datos para contextualizar las solicitudes y garantizar las respuestas pertinentes. ✅

    c. Usar el número más bajo posible de tokens en un símbolo del sistema.

**Justificación:** En este contexto, "base" se refiere al concepto de **Grounding** (Fundamentación). Consiste en proporcionar datos propios al modelo para que las respuestas sean pertinentes y basadas en hechos, evitando alucinaciones.

**Glosario / Comentarios:**
*   **"La base"**: Traducción de *Grounding* (Fundamentación).
*   **"Símbolo del sistema"**: Traducción errónea clásica de *Prompt*.

2.  **¿Qué patrón puede usar para poner en tierra las indicaciones?**

    a. Símbolo del sistema optimizado para metadatos (MOP).

    b. Comprensión de datos admite texto (DUST).

    c. Generación aumentada de recuperación (RAG). ✅

**Justificación:** **RAG** (Retrieval-Augmented Generation) es el patrón estándar para "poner en tierra" (Ground) los prompts con datos externos recuperados de un índice de búsqueda.

**Glosario / Comentarios:**
*   **"Poner en tierra"**: Traducción literal de *To Ground* (Fundamentar).
*   **"Indicaciones"**: Traducción de *Prompts*.
*   **"Símbolo del sistema"**: Otra vez, traducción de *Prompt*.

3.  **¿Cómo puede usar el patrón RAG en una aplicación cliente que use el SDK de Azure OpenAI?**

    a. Agregue archivos de texto que contengan los datos de conexión a tierra a la carpeta de la aplicación.

    b. No es necesario hacer nada más. Microsoft Foundry genera automáticamente todos los mensajes mediante Bing Search.

    c. Agregue los detalles de conexión de índice a la configuración de ChatClient de OpenAI. ✅

**Justificación:** Para usar RAG en el SDK, se deben enviar los **detalles de conexión** del índice de Azure AI Search (endpoint, clave y nombre del índice) dentro de la solicitud (en el parámetro `extra_body` o `data_sources`), para que el modelo sepa dónde buscar la información.

**Glosario / Comentarios:**
*   **"Datos de conexión a tierra"**: Traducción de *Grounding data* (Datos de fundamentación).


---