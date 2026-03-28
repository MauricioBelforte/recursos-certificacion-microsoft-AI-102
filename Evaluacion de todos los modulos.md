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

# 1) Introducción al desarrollo de agentes de IA en Azure

## Evaluación del módulo

1.  **¿Cuál de las siguientes describe mejor un agente de IA?**

    a. Desarrollador que se especializa en la creación de soluciones de inteligencia artificial generativas.

    b. Un servicio de software que usa inteligencia artificial para ayudar a los usuarios con información y automatización de tareas. ✅

    c. Marketplace para componentes de software de inteligencia artificial listos para usar.

**Justificación:** Un **Agente de IA** se define como un sistema de software autónomo que utiliza modelos de IA para percibir su entorno, razonar y tomar medidas para ayudar a los usuarios o completar tareas, a diferencia de un simple chatbot o un rol humano.

2.  **¿Qué servicio de desarrollo de agentes de IA ofrece una elección de modelos de IA generativos de varios proveedores en el catálogo de modelos de Microsoft Foundry?**

    a. Servicio microsoft Foundry Agent ✅

    b. API de asistentes de OpenAI

    c. Microsoft 365 Copilot Chat

**Justificación:** El **Servicio de Agentes de Microsoft Foundry (Microsoft Foundry Agent Service)** permite utilizar una amplia gama de modelos disponibles en el catálogo de Foundry (incluyendo modelos de OpenAI y otros de código abierto), mientras que la API de Asistentes de OpenAI se limita a los modelos de OpenAI.

**Glosario / Comentarios:**
*   **"Servicio microsoft Foundry Agent"**: Traducción de *Microsoft Foundry Agent Service*.

3.  **¿Qué elemento de un agente de Foundry Agent Service permite fundamentar con datos contextuales?**

    a. Modelo

    b. Herramienta de intérprete de código

    c. Conocimientos ✅

**Justificación:** El componente de **Conocimientos (Knowledge)** es el encargado específicamente de almacenar y gestionar los datos externos (archivos, índices de búsqueda) que el agente utiliza para **fundamentar (ground)** sus respuestas y evitar alucinaciones.

**Glosario / Comentarios:**
*   **"Conocimientos"**: Traducción de *Knowledge*.
*   **"Herramienta de intérprete de código"**: Traducción de *Code Interpreter Tool*.

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

---

# 7) Implementación de una solución de IA generativa responsable en Microsoft Foundry

## Evaluación del módulo

1.  **¿Por qué debe considerar la posibilidad de crear una valoración de impacto de la IA al diseñar una solución de IA generativa?**

    a. Para construir un caso legal que le permita eximirse de responsabilidad por los daños provocados por la solución

    b. Para documentar el propósito, el uso esperado y los daños posibles para la solución ✅

    c. Para evaluar el costo de los servicios en la nube necesarios para implementar la solución

**Justificación:** La **Evaluación de Impacto de la IA (AI Impact Assessment)** es una herramienta clave en la fase de identificación para documentar los usos previstos, los riesgos y los daños potenciales, no para temas legales o de costos.

**Glosario / Comentarios:**
*   **"Valoración de impacto de la IA"**: Traducción de *AI Impact Assessment*.

2.  **¿Qué funcionalidad de Microsoft Foundry ayuda a mitigar la generación de contenido perjudicial en el nivel del sistema de seguridad?**

    a. soporte para el modelo DALL-E

    b. Ajuste preciso

    c. Filtros de contenido ✅

**Justificación:** Los **Filtros de contenido (Content Filters)** actúan en la capa del sistema de seguridad interceptando entradas y salidas dañinas. El "Ajuste preciso" (Fine-tuning) actúa en la capa del modelo.

**Glosario / Comentarios:**
*   **"Filtros de contenido"**: Traducción de *Content Filters*.
*   **"Ajuste preciso"**: Traducción de *Fine-tuning*.

3.  **¿Por qué debe considerar un plan de entrega por fases para la solución de IA generativa?**

    a. Para permitirle recopilar comentarios e identificar problemas antes de publicar la solución de forma más amplia ✅

    b. Para eliminar la necesidad de mapear, medir, mitigar y gestionar posibles daños

    c. Para permitirle cobrar más por la solución

**Justificación:** La **Entrega por fases (Phased Delivery)** es una estrategia de operación que permite detectar problemas en un grupo reducido antes del lanzamiento general. No elimina la necesidad del proceso de IA responsable.

**Glosario / Comentarios:**
*   **"Plan de entrega por fases"**: Traducción de *Phased Delivery*.

---

# 8) Evalúa el rendimiento de IA generativa en el portal de Microsoft Foundry

## Evaluación del módulo

1.  **¿Qué técnica de evaluación puede usar para aplicar su propio juicio sobre la calidad de las respuestas a un conjunto de indicaciones específicas?**

    a. Puntos de referencia de modelos

    b. Evaluaciones manuales ✅

    c. Evaluaciones automatizadas

**Justificación:** Las **Evaluaciones manuales (Manual Evaluations)** permiten a los evaluadores humanos revisar y calificar las respuestas basándose en su propio juicio, lo cual es útil para capturar matices que las métricas automáticas pierden.

**Glosario / Comentarios:**
*   **"Puntos de referencia de modelos"**: Traducción de *Model Benchmarks*.
*   **"Indicaciones"**: Traducción de *Prompts*.

2.  **¿Qué evaluador compara las respuestas generadas a la verdad básica en función de las métricas estándar?**

    a. Coherencia

    b. Puntuación F1 ✅

    c. Material protegido

**Justificación:** La **Puntuación F1 (F1 Score)** es una métrica estándar de NLP que mide la superposición de palabras entre la respuesta generada y la verdad terreno (ground truth/verdad básica).

**Glosario / Comentarios:**
*   **"Verdad básica"**: Traducción de *Ground Truth*.

3.  **¿Qué métrica del evaluador usa un modelo de IA para juzgar la estructura y el flujo lógico de ideas en una respuesta?**

    a. Coherencia ✅

    b. Puntuación F1

    c. material protegido

**Justificación:** La métrica de **Coherencia (Coherence)** utiliza un modelo de IA para evaluar qué tan bien fluye el texto y si tiene una estructura lógica.

---

# 2) Desarrollo de un agente de IA con el servicio Microsoft Foundry Agent

## Evaluación del módulo

1.  **¿Cuál es el primer paso para configurar microsoft Foundry Agent Service?**

    a. Implementación de un modelo compatible

    b. Creación de un proyecto de Microsoft Foundry ✅

    c. Realización de llamadas de API mediante SDK

**Justificación:** El servicio de agentes opera dentro del contexto de un **Proyecto de Microsoft Foundry**. Antes de poder implementar modelos o llamar APIs, se debe establecer este contenedor lógico que agrupa los recursos y configuraciones.

**Glosario / Comentarios:**
*   **"Implementación"**: Traducción de *Deployment*.

2.  **¿Qué elemento de una definición de agente se usa para especificar su comportamiento y restricciones?**

    a. Modelo

    b. Instrucciones ✅

    c. Herramientas

**Justificación:** Las **Instrucciones (Instructions)** actúan como el prompt del sistema del agente, definiendo su personalidad, objetivos y, crucialmente, sus límites y restricciones de comportamiento.

**Glosario / Comentarios:**
*   **"Instrucciones"**: Traducción de *Instructions*.

3.  **¿Qué herramienta debe usar para permitir que un agente genere código dinámicamente para realizar tareas o acceder a datos de archivos?**

    a. Intérprete de código ✅

    b. Funciones de Azure

    c. Búsqueda de Azure AI

**Justificación:** El **Intérprete de código (Code Interpreter)** es la herramienta diseñada específicamente para proporcionar un entorno aislado (sandbox) donde el agente puede escribir y ejecutar código (generalmente Python) para procesar archivos y realizar cálculos.

**Glosario / Comentarios:**
*   **"Intérprete de código"**: Traducción de *Code Interpreter*.

---

# 3) Desarrollo de agentes de IA con la extensión Microsoft Foundry en Visual Studio Code

## Evaluación del módulo

1.  **Al crear un nuevo agente en la extensión Microsoft Foundry, ¿qué dos vistas se abren automáticamente?**

    a. El archivo YAML y la vista del entorno de pruebas

    b. El archivo YAML y la vista Diseñador ✅

    c. La vista Diseñador y la vista Generación de código

**Justificación:** Al crear un agente, la extensión abre automáticamente tanto el archivo de configuración `.yaml` (para edición de código) como la vista del **Diseñador (Designer)** (para edición visual) lado a lado.

2.  **¿Cuál es una ventaja clave del uso de servidores del Protocolo de contexto de modelo (MCP) para las herramientas del agente?**

    a. Solo funcionan con herramientas creadas por Microsoft

    b. Proporcionan componentes reutilizables que funcionan en distintos agentes. ✅

    c. Reemplazan la necesidad de todas las herramientas integradas

**Justificación:** El **Protocolo de Contexto de Modelo (Model Context Protocol - MCP)** es un estándar abierto que permite conectar herramientas de manera consistente, fomentando la creación de componentes reutilizables por la comunidad que funcionan en diferentes implementaciones de agentes.

**Glosario / Comentarios:**
*   **"Protocolo de contexto de modelo"**: Traducción de *Model Context Protocol (MCP)*.

3.  **¿Cómo administra el servicio Microsoft Foundry Agent las sesiones de conversación cuando los usuarios interactúan con agentes implementados?**

    a. El sistema crea subprocesos individuales para cada conversación para administrar el contexto y el historial de mensajes. ✅

    b. Cada mensaje se procesa de forma independiente sin historial de conversaciones

    c. Todas las conversaciones de usuario se combinan en una sesión global compartida

**Justificación:** El servicio gestiona el estado y el contexto mediante **Hilos (Threads)** (traducido aquí como "subprocesos"). Cada conversación es un hilo único que almacena el historial de mensajes e interacciones.

**Glosario / Comentarios:**
*   **"Subprocesos"**: Traducción de *Threads* (Hilos).

---

# 4) Integración de herramientas personalizadas en el agente

## Evaluación del módulo

1.  **¿Qué son las herramientas personalizadas y cómo pueden ayudarle a desarrollar agentes eficaces con el servicio Microsoft Foundry Agent?**

    a. Funciones disponibles que un agente puede usar para ampliar sus funcionalidades ✅

    b. Extensiones para Visual Studio Code que facilitan la creación e implementación de agentes.

    c. Modelos ajustados que el agente puede usar para generar una salida personalizada.

**Justificación:** Las **herramientas personalizadas (Custom tools)** son funciones que se proporcionan al agente para que pueda realizar acciones que van más allá de la generación de texto, como buscar datos o realizar cálculos, ampliando así sus capacidades operativas.

2.  **Debe integrar la funcionalidad de un servicio web basado en OpenAPI 3.0 en una solución de agente. ¿Qué debe hacer?**

    a. Agregue el esquema JSON del servicio web a las instrucciones del agente.

    b. Vuelve a escribir el servicio web como una función de Python y codifícalo de forma rígida en la aplicación del agente

    c. Adición del servicio web como una herramienta de especificación de OpenAPI a la definición del agente ✅

**Justificación:** El servicio de agentes soporta nativamente la integración de APIs externas mediante herramientas definidas con especificaciones **OpenAPI 3.0**. Esto permite conectar el agente directamente a la API existente sin reescribir la lógica.

**Glosario / Comentarios:**
*   **"Herramienta de especificación de OpenAPI"**: Traducción de *OpenAPI specification tool*.

3.  **El código de la aplicación del agente incluye una función local a la que desea que llame el agente. ¿Qué tipo de herramienta debe agregar a la definición del agente?**

    a. Llamada de funciones ✅

    b. Intérprete de código

    c. Funciones de Azure

**Justificación:** La **Llamada a funciones (Function Calling)** es el mecanismo diseñado específicamente para que el agente solicite la ejecución de funciones definidas localmente en el código de la aplicación cliente.

**Glosario / Comentarios:**
*   **"Llamada de funciones"**: Traducción de *Function calling*.

---

# 5) Desarrollo de una solución multiagente con el servicio Microsoft Foundry Agent

## Evaluación del módulo

1.  **¿Cuál es el rol del agente principal en un sistema de agente conectado?**

    a. Para realizar directamente todas las tareas mediante herramientas

    b. Para coordinar la entrada del usuario y enrutar tareas a los agentes conectados adecuados ✅

    c. Para supervisar el rendimiento del agente y generar registros

**Justificación:** En una arquitectura de agentes conectados, el agente principal actúa como **orquestador**. Su función no es realizar las tareas especializadas, sino interpretar la intención del usuario y delegar (enrutar) el trabajo al subagente (agente conectado) más adecuado para ello.

**Glosario / Comentarios:**
*   **"Agente conectado"**: Traducción de *Connected agent*.

2.  **¿Cómo se conecta un agente a un agente principal mediante la biblioteca cliente de proyectos de Azure AI?**

    a. Agregue el agente como ConnectedAgentTool a la definición de la herramienta del agente principal. ✅

    b. Use el link_agents() método para enlazar el subagente al agente principal.

    c. Establezca el parent_id del agente principal en el id. del agente secundario.

**Justificación:** Para conectar un agente secundario a uno principal utilizando el SDK, se debe envolver el agente secundario en un objeto `ConnectedAgentTool` y luego agregarlo a la lista de `tools` (herramientas) en la definición del agente principal. No existen métodos como `link_agents` o propiedades `parent_id` para este propósito.

**Glosario / Comentarios:**
*   **"ConnectedAgentTool"**: Clase del SDK utilizada para definir un agente como herramienta.

3.  **¿Cómo decide el agente principal qué agente conectado va a usar?**

    a. Usa instrucciones de aviso y comprensión del lenguaje natural. ✅

    b. Sigue un árbol de decisión fijo basado en código.

    c. Selecciona aleatoriamente los agentes conectados disponibles.

**Justificación:** La ventaja de los agentes de IA es que no requieren lógica rígida ("hard-coded"). El agente principal utiliza sus **Instrucciones** (System Prompt) y la descripción de las herramientas disponibles para razonar en lenguaje natural y decidir qué agente conectado es el mejor para la tarea actual.

**Glosario / Comentarios:**
*   **"Instrucciones de aviso"**: Traducción confusa de *Prompt instructions* o simplemente *Instructions*.
*   **"Intérprete de código"**: Traducción de *Code Interpreter*.

---

# 6) Integración de herramientas de MCP con Azure AI Agents

## Evaluación del módulo

1.  **¿Qué rol desempeña el servidor MCP en la integración de herramientas del agente de MCP?**

    a. Ejecuta directamente el agente de IA y procesa las solicitudes del usuario.

    b. Administra las conexiones de red entre varios agentes.

    c. Hospeda definiciones de herramientas y hace que estén disponibles para la detección por parte del cliente. ✅

**Justificación:** El **Servidor MCP** actúa como un catálogo o registro centralizado donde se definen y hospedan las herramientas. Su función principal es exponer estas capacidades para que los clientes (y por ende los agentes) puedan descubrirlas ("detección") y utilizarlas, pero no ejecuta la lógica del agente en sí.

**Glosario / Comentarios:**
*   **"Detección"**: Traducción de *Discovery*.

2.  **¿Cómo recupera un cliente MCP las herramientas disponibles del servidor MCP?**

    a. Llame a session.list_tools() para obtener el catálogo de herramientas actual. ✅

    b. Al leer un archivo JSON estático desde el directorio del servidor.

    c. Al suscribirse a eventos de servidor a través de una conexión WebSocket.

**Justificación:** Según el SDK de MCP, el método estándar para que un cliente descubra dinámicamente qué capacidades ofrece el servidor es invocar `session.list_tools()`. Esto devuelve la lista actualizada de herramientas disponibles.

3.  **¿Por qué se deben encapsular las herramientas de MCP en funciones asincrónicas en el lado cliente?**

    a. Para permitir que el agente espere la entrada del usuario.

    b. Para habilitar la invocación asincrónica para que el agente pueda llamar a herramientas sin bloqueo. ✅

    c. Para convertir las funciones en puntos de conexión de la API REST automáticamente.

**Justificación:** El uso de funciones asincrónicas (`async/await`) es crucial en arquitecturas de agentes para evitar bloquear el hilo principal de ejecución mientras se espera la respuesta de una herramienta externa (como una consulta a base de datos o API), permitiendo que el sistema sea más eficiente y receptivo.

**Glosario / Comentarios:**
*   **"Sin bloqueo"**: Traducción de *Non-blocking*.

---

# 1) Análisis de texto con lenguaje de Azure en Foundry Tools

## Evaluación del módulo

1.  **¿Cómo debe crear una aplicación que supervise los comentarios en el sitio web de su empresa y marque las publicaciones negativas?**

    a. Use el lenguaje de Azure en Foundry Tools para extraer frases clave.

    b. Use el lenguaje de Azure en Foundry Tools para realizar el análisis de opiniones de los comentarios. ✅

    c. Use el lenguaje de Azure en Foundry Tools para extraer entidades con nombre de los comentarios.

    **Justificación:** Para detectar si un texto o comentario es negativo, positivo o neutro, el servicio adecuado a utilizar es el de "Análisis de sentimiento" (mal traducido en el portal como "análisis de opiniones").

    **Glosario / Comentarios:**
    *   **"Análisis de opiniones"**: Traducción errónea de *Sentiment Analysis* (Análisis de sentimiento).
    *   **"Lenguaje de Azure en Foundry Tools"**: Traducción confusa de *Azure AI Language*.

2.  **Está analizando texto que contiene la palabra "París". ¿Cómo puede determinar si esta palabra hace referencia a la ciudad francesa o al carácter del "The Iliad" de Homer?**

    a. Use el lenguaje de Azure en Foundry Tools para extraer frases clave.

    b. Use el idioma de Azure en Foundry Tools para detectar el idioma del texto.

    c. Use el lenguaje de Azure en Foundry Tools para extraer entidades vinculadas. ✅

    **Justificación:** La "Vinculación de entidades" (Entity Linking) es la característica diseñada específicamente para la desambiguación de términos. Identificará si "París" se refiere a una ubicación geográfica o al personaje de la mitología, devolviendo el enlace a la base de conocimiento correspondiente (ej. Wikipedia).

    **Glosario / Comentarios:**
    *   **"Idioma de Azure"**: Traducción errónea de *Azure AI Language*.
