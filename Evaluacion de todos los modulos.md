# 1) Planeamiento y preparación para desarrollar soluciones de inteligencia artificial en Azure

## Evaluación del módulo


1. ¿Qué recurso de Azure proporciona servicios de lenguaje y visión desde un único punto de conexión?

    a. Idioma de Azure

    b. Azure Vision

    c. Herramientas de fundición ✅

**Justificación:** Los recursos de "Herramientas de fundición" (Foundry Tools) están diseñados para agrupar varios servicios de IA de Azure, como Lenguaje y Visión, bajo un único punto de conexión y una sola clave de API, simplificando su gestión y uso en las aplicaciones.

**Glosario / Comentarios:**
*   **"Idioma de Azure"**: Traducción de *Azure AI Language*.
*   **"Herramientas de fundición"**: Traducción de *Foundry Tools* (En el ecosistema Azure real, esto equivale a un recurso multi-servicio de **Azure AI Services**).

2. Tiene previsto crear una aplicación de chat sencilla que use un modelo de IA generativa. ¿Qué tipo de proyecto debe crear?

    a. Proyecto de Microsoft Foundry. ✅

    b. Proyecto basado en azure AI Hub.

    c. Proyecto de Custom Vision de Azure AI.

**Justificación:** Microsoft Foundry es la plataforma recomendada para el desarrollo de proyectos de IA, ya que integra modelos avanzados como los de Azure OpenAI (necesarios para IA generativa) y proporciona herramientas para la gestión del ciclo de vida del proyecto.

**Glosario / Comentarios:**
*   **"Proyecto basado en azure AI Hub"**: Traducción de *Azure AI Hub-based project*.

3. ¿Qué SDK le permite conectarse a los recursos de un proyecto?

    a. Foundry Tools SDK

    b. SDK de núcleo semántico

    c. Microsoft Foundry SDK ✅

**Justificación:** El SDK de Microsoft Foundry está específicamente diseñado para interactuar mediante programación con los recursos que se encuentran dentro de un proyecto de Microsoft Foundry, permitiendo la creación y gestión de soluciones de IA.

**Glosario / Comentarios:**
*   **"SDK de núcleo semántico"**: Traducción literal de *Semantic Kernel SDK*.

---

# 2) Selección, implementación y evaluación de modelos en el catálogo en el portal de Microsoft Foundry

## Evaluación del módulo

1.  **¿Dónde puede probar un modelo implementado en el portal de Microsoft Foundry?**

    a. Área de juegos de chat ✅

    b. Espacio aislado

    c. Cuadro de herramientas de desarrollo

**Justificación:** El "Área de juegos de chat" (Chat playground) en el portal de Microsoft Foundry es la interfaz diseñada específicamente para probar e interactuar con los modelos de lenguaje implementados.

**Glosario / Comentarios:**
*   **"Área de juegos de chat"**: Traducción de *Chat playground*.
*   **"Implementado"**: Traducción de *Deployed*.
*   **"Espacio aislado"**: Traducción de *Sandbox*.

2.  **Quiere especificar el tono, el formato y el contenido para cada interacción con el modelo en el área de juegos. ¿Qué debe usar para personalizar la respuesta del modelo?**

    a. Puntos de referencia

    b. Base

    c. Mensaje del sistema ✅

**Justificación:** El "Mensaje del sistema" (system message) se utiliza para proporcionar instrucciones, contexto y directrices al modelo antes de la interacción. Esto permite definir el tono, el formato de la respuesta y el comportamiento general del modelo.

**Glosario / Comentarios:**
*   **"Mensaje del sistema"**: Traducción de *System message*.
*   **"Puntos de referencia"**: Traducción de *Benchmarks*.
*   **"Base"**: Traducción de *Grounding* (o *Baseline* en otros contextos).

3.  **¿Qué opción de implementación debe elegir para hospedar un modelo de OpenAI en un recurso de Microsoft Foundry?**

    a. Implementación estándar ✅

    b. Proceso sin servidor

    c. Cómputo gestionado

**Justificación:** Para alojar un modelo de OpenAI dentro de un recurso de Microsoft Foundry, la opción correcta es la "Implementación estándar" (Standard deployment). Esta opción gestiona la infraestructura necesaria para servir el modelo a través de un punto de conexión.

**Glosario / Comentarios:**
*   **"Implementación estándar"**: Traducción de *Standard deployment*.
*   **"Proceso sin servidor"**: Traducción de *Serverless compute*.

---

# 3) Desarrollo de una aplicación de IA con el SDK de Microsoft Foundry 

## Evaluación del módulo

1.  **¿Qué clase del SDK de Microsoft Foundry proporciona un objeto proxy para un proyecto?**

    a. Propiedades de conexión (Connection properties)

    b. AIProjectClient ✅

    c. ChatCompletionsClient

**Justificación:** La clase `AIProjectClient` es el punto de entrada principal del SDK. Se inicializa con la cadena de conexión o el punto de conexión (endpoint) y actúa como un proxy para interactuar con los recursos y configuraciones del proyecto.

**Glosario / Comentarios:**
*   **"Propiedades de conexión"**: Traducción de *Connection properties*.
*   **"Objeto proxy"**: Refiere al patrón de diseño donde el cliente actúa como intermediario local para el servicio remoto.

2.  **¿Qué valor se necesita para crear una instancia de un objeto AIProjectClient?**

    a. Punto de conexión del proyecto (Project Endpoint) ✅

    b. La clave de autorización de Azure OpenAI

    c. Identificador de suscripción de Azure

**Justificación:** Como se observa en los ejemplos de código, el constructor de `AIProjectClient` requiere el parámetro `endpoint` (el punto de conexión del proyecto, a veces mal traducido como "final del proyecto") y un objeto de credenciales (como `DefaultAzureCredential`).

**Glosario / Comentarios:**
*   **"Punto de conexión del proyecto"**: Traducción de *Project Endpoint*.
*   **"Identificador de suscripción"**: Traducción de *Subscription ID*.
*   **"Clave de autorización"**: Traducción de *Authorization Key*.

3.  **¿Qué SDK debe usar para chatear con un modelo que se implementa en un recurso de Microsoft Foundry?**

    a. Azure OpenAI ✅

    b. Azure Machine Learning

    c. Lenguaje de Azure (Azure AI Language)

**Justificación:** Aunque se utiliza el SDK de Microsoft Foundry para configurar el cliente, la interacción de chat real se realiza utilizando un cliente compatible con **Azure OpenAI**. El método `project_client.get_openai_client()` devuelve un cliente configurado que utiliza la librería `openai` para enviar los prompts al modelo.

**Glosario / Comentarios:**
*   **"Lenguaje de Azure"**: Traducción frecuente y confusa de *Azure AI Language* (el servicio de NLP).
*   **"Recurso"**: Traducción de *Resource*.


---

# 4) Introducción al flujo de mensajes para desarrollar aplicaciones de modelo de lenguaje en Microsoft Foundry

# Evaluación del Módulo: Introducción al Flujo de Mensajes (Prompt Flow)

1.  **Un flujo usa una herramienta LLM para generar texto con un modelo de GPT-3.5. ¿Qué necesita crear para asegurarse de que el flujo de mensajes puede llamar de forma segura al modelo implementado desde Azure OpenAI?**

    a. Runtimes

    b. Conexiones (Connections) ✅

    c. Herramientas (Tools)

**Justificación:** Las **Conexiones (Connections)** almacenan de forma segura las credenciales y los puntos de conexión necesarios para que el flujo se comunique con servicios externos como Azure OpenAI, evitando exponer claves API en el código.

**Glosario / Comentarios:**
*   **"Flujo de mensajes"**: Traducción de *Prompt Flow*.
*   **"Conexiones"**: Traducción de *Connections*.
*   **"Herramienta LLM"**: Traducción de *LLM Tool*.

2.  **Quiere integrar el flujo con un sitio web en línea. ¿Qué debe hacer para integrar fácilmente el flujo?**

    a. Crear un entorno personalizado.

    b. Crear un entorno de ejecución (Runtime).

    c. Implementar el flujo en un punto de conexión (Endpoint). ✅

**Justificación:** Para que una aplicación externa (como un sitio web) consuma el flujo, este debe ser **Implementado (Deployed)** en un **Punto de conexión en línea (Online Endpoint)**, el cual proporciona una URL estable y segura para realizar peticiones.

**Glosario / Comentarios:**
*   **"Punto de conexión"**: Traducción de *Endpoint* (a veces traducido como "Extremo" o "Punto final").
*   **"Entorno de ejecución"**: Traducción de *Runtime*.
*   **"Implementar"**: Traducción de *Deploy*.

3.  **Después de la implementación, observa que el flujo tiene un rendimiento inferior. ¿A qué fase del ciclo de vida de desarrollo debe revertir?**

    a. Experimentación ✅

    b. Evaluación y refinamiento

    c. Producción

**Justificación:** Según el ciclo de vida de desarrollo de aplicaciones LLM, si se detectan problemas de rendimiento en producción o evaluación, se debe regresar a la fase de **Experimentación** para ajustar los prompts, cambiar la lógica del flujo o probar nuevas configuraciones hasta obtener los resultados deseados.

**Glosario / Comentarios:**
*   **"Implementación"**: Traducción de *Deployment*.

---

# 5) Desarrollo de una solución basada en RAG con sus propios datos mediante Microsoft Foundry

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

# 6) Ajuste de un modelo de lenguaje con Microsoft Foundry

## Evaluación del módulo

1.  **¿Cómo se deben dar formato a los datos para optimizarlos?**

    a. JSONL ✅

    b. YAML

    c. HTML

**Justificación:** El proceso de ajuste fino (fine-tuning) para modelos de chat en Azure AI requiere que los datos de entrenamiento estén en formato **JSON Lines (JSONL)**, donde cada línea es un objeto JSON que representa una conversación.

**Glosario / Comentarios:**
*   **"Optimizar"**: Traducción de *Fine-tuning* (Ajuste fino).

2.  **¿Qué optimiza la optimización en el modelo?**

    a. Qué necesita saber el modelo.

    b. Cómo debe actuar el modelo. ✅

    c. Qué palabras no se permiten.

**Justificación:** Mientras que el patrón RAG se enfoca en proveer conocimiento al modelo ("qué necesita saber"), el ajuste fino (fine-tuning) se utiliza principalmente para modificar el **comportamiento** del modelo ("cómo debe actuar"), como su estilo, tono y formato de respuesta.

**Glosario / Comentarios:**
*   **"Optimización"**: Traducción de *Fine-tuning*.

3.  **¿Qué opción avanzada hace referencia a un ciclo completo a través del conjunto de datos de entrenamiento?**

    a. seed

    b. batch_size

    c. n_epochs ✅

**Justificación:** El hiperparámetro **n_epochs** (Número de Épocas) define cuántas veces el algoritmo de entrenamiento procesará el conjunto de datos completo. `seed` es para la reproducibilidad y `batch_size` es el número de muestras por iteración.

**Glosario / Comentarios:**
*   **"Opción avanzada"**: Se refiere a los Hiperparámetros.
