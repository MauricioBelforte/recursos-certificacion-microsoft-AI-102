# 1) Planeamiento y preparación para desarrollar soluciones de inteligencia artificial en Azure

## Evaluación del módulo


1. ¿Qué recurso de Azure proporciona servicios de lenguaje y visión desde un único punto de conexión?

    a. Idioma de Azure

    b. Azure Vision

    c. Herramientas de fundición ✅

**Justificación:** Los recursos de "Herramientas de fundición" (Foundry Tools) están diseñados para agrupar varios servicios de IA de Azure, como Lenguaje y Visión, bajo un único punto de conexión y una sola clave de API, simplificando su gestión y uso en las aplicaciones.

2. Tiene previsto crear una aplicación de chat sencilla que use un modelo de IA generativa. ¿Qué tipo de proyecto debe crear?

    a. Proyecto de Microsoft Foundry. ✅

    b. Proyecto basado en azure AI Hub.

    c. Proyecto de Custom Vision de Azure AI.

**Justificación:** Microsoft Foundry es la plataforma recomendada para el desarrollo de proyectos de IA, ya que integra modelos avanzados como los de Azure OpenAI (necesarios para IA generativa) y proporciona herramientas para la gestión del ciclo de vida del proyecto.

3. ¿Qué SDK le permite conectarse a los recursos de un proyecto?

    a. Foundry Tools SDK

    b. SDK de núcleo semántico

    c. Microsoft Foundry SDK ✅

**Justificación:** El SDK de Microsoft Foundry está específicamente diseñado para interactuar mediante programación con los recursos que se encuentran dentro de un proyecto de Microsoft Foundry, permitiendo la creación y gestión de soluciones de IA.

---

# 2) Selección, implementación y evaluación de modelos en el catálogo en el portal de Microsoft Foundry

## Evaluación del módulo

1.  **¿Dónde puede probar un modelo implementado en el portal de Microsoft Foundry?**

    a. Área de juegos de chat ✅

    b. Espacio aislado

    c. Cuadro de herramientas de desarrollo

**Justificación:** El "Área de juegos de chat" (Chat playground) en el portal de Microsoft Foundry es la interfaz diseñada específicamente para probar e interactuar con los modelos de lenguaje implementados.

2.  **Quiere especificar el tono, el formato y el contenido para cada interacción con el modelo en el área de juegos. ¿Qué debe usar para personalizar la respuesta del modelo?**

    a. Puntos de referencia

    b. Base

    c. Mensaje del sistema ✅

**Justificación:** El "Mensaje del sistema" (system message) se utiliza para proporcionar instrucciones, contexto y directrices al modelo antes de la interacción. Esto permite definir el tono, el formato de la respuesta y el comportamiento general del modelo.

3.  **¿Qué opción de implementación debe elegir para hospedar un modelo de OpenAI en un recurso de Microsoft Foundry?**

    a. Implementación estándar ✅

    b. Proceso sin servidor

    c. Cómputo gestionado

**Justificación:** Para alojar un modelo de OpenAI dentro de un recurso de Microsoft Foundry, la opción correcta es la "Implementación estándar" (Standard deployment). Esta opción gestiona la infraestructura necesaria para servir el modelo a través de un punto de conexión.

---

# 3) Desarrollo de una aplicación de IA con el SDK de Microsoft Foundry 

## Evaluación del módulo

1.  **¿Qué clase del SDK de Microsoft Foundry proporciona un objeto proxy para un proyecto?**

    a. Propiedades de conexión (Connection properties)

    b. AIProjectClient ✅

    c. ChatCompletionsClient

**Justificación:** La clase `AIProjectClient` es el punto de entrada principal del SDK. Se inicializa con la cadena de conexión o el punto de conexión (endpoint) y actúa como un proxy para interactuar con los recursos y configuraciones del proyecto.

2.  **¿Qué valor se necesita para crear una instancia de un objeto AIProjectClient?**

    a. Punto de conexión del proyecto (Project Endpoint) ✅

    b. La clave de autorización de Azure OpenAI

    c. Identificador de suscripción de Azure

**Justificación:** Como se observa en los ejemplos de código, el constructor de `AIProjectClient` requiere el parámetro `endpoint` (el punto de conexión del proyecto, a veces mal traducido como "final del proyecto") y un objeto de credenciales (como `DefaultAzureCredential`).

3.  **¿Qué SDK debe usar para chatear con un modelo que se implementa en un recurso de Microsoft Foundry?**

    a. Azure OpenAI ✅

    b. Azure Machine Learning

    c. Lenguaje de Azure (Azure AI Language)

**Justificación:** Aunque se utiliza el SDK de Microsoft Foundry para configurar el cliente, la interacción de chat real se realiza utilizando un cliente compatible con **Azure OpenAI**. El método `project_client.get_openai_client()` devuelve un cliente configurado que utiliza la librería `openai` para enviar los prompts al modelo.
