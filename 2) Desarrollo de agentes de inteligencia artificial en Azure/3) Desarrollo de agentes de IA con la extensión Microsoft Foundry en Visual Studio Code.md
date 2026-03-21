# Desarrollo de agentes de IA con la extensión Microsoft Foundry en Visual Studio Code

## Introducción

A medida que los modelos de inteligencia artificial generativa se vuelven más eficaces y accesibles, los desarrolladores están yendo más allá de las aplicaciones de chat sencillas para crear **agentes inteligentes**. Estos agentes pueden automatizar tareas complejas combinando **Modelos de Lenguaje Grande (LLM)** con herramientas especializadas para acceder a datos, realizar acciones y completar flujos de trabajo enteros con una intervención humana mínima.

**Visual Studio Code** se ha convertido en una plataforma eficaz para el desarrollo de estos agentes, especialmente gracias a la **Extensión de Microsoft Foundry**. Esta extensión integra funcionalidades de IA de nivel empresarial directamente en el entorno de desarrollo.

Con la extensión de Microsoft Foundry para Visual Studio Code, es posible:

- Detectar e **implementar (deploy)** modelos.
- Crear y configurar agentes.
- Probar agentes en **áreas de juegos (playgrounds)** interactivas.
- Todo ello sin necesidad de salir del editor de código.

En este módulo, se introducen los conceptos básicos del desarrollo de agentes de IA y se demuestra cómo utilizar la extensión de Microsoft Foundry para Visual Studio Code para compilar, probar e implementar agentes inteligentes. Aprenderá a utilizar el **Servicio de Agentes de Microsoft Foundry (Microsoft Foundry Agent Service)** para crear agentes capaces de controlar tareas complejas, utilizar diversas herramientas e integrarse con aplicaciones existentes.

---

# Introducción a la extensión Microsoft Foundry

La extensión **Microsoft Foundry para Visual Studio Code** transforma su entorno de desarrollo local en una plataforma robusta para compilar, probar e implementar agentes de Inteligencia Artificial. Esta herramienta proporciona acceso directo a las capacidades del **Servicio de Agentes de Microsoft Foundry (Microsoft Foundry Agent Service)** sin necesidad de abandonar el editor de código, optimizando significativamente el flujo de trabajo de desarrollo.

---

## ¿Qué es la extensión Microsoft Foundry para Visual Studio Code?

Es una herramienta diseñada para llevar las funcionalidades de desarrollo de agentes de nivel empresarial directamente a VS Code. Ofrece una experiencia integrada que permite:

- **Detección y administración:** Explorar, crear y gestionar agentes de IA dentro de sus proyectos de Microsoft Foundry.
- **Diseñador visual de agentes:** Utilizar una interfaz gráfica intuitiva para configurar las **Instrucciones (Instructions)**, las herramientas y las capacidades del agente.
- **Pruebas integradas:** Validar el comportamiento de los agentes en tiempo real utilizando un **Área de juegos (Playground)** integrada, evitando el cambio de contexto entre el navegador y el editor.
- **Generación de código:** Producir automáticamente código de ejemplo para integrar los agentes con sus aplicaciones.
- **Canalización de implementación (Deployment Pipeline):** Publicar agentes directamente en Microsoft Foundry para su uso en producción.

---

## Características y Funcionalidades Clave

La extensión acelera el desarrollo de agentes mediante varias características esenciales:

### Interfaz del Diseñador de Agentes

Un entorno visual que simplifica la configuración del agente. Permite definir las **Instrucciones (Instructions)**, seleccionar el modelo adecuado y añadir herramientas mediante una interfaz gráfica.

### Área de Juegos Integrada (Integrated Playground)

Un entorno de pruebas dentro del editor donde puede chatear con sus agentes en tiempo real para verificar diferentes escenarios y refinar su comportamiento antes de la **Implementación (Deployment)**.

### Integración de Herramientas

Soporte fluido para diversas capacidades, incluyendo:

- **RAG (Recuperación y Generación Aumentada):** Para respuestas fundamentadas en bases de conocimiento.
- **Búsqueda:** Para recuperación de información en tiempo real.
- **Acciones personalizadas:** Para ejecutar lógica de negocio específica.
- **Servidores del Protocolo de Contexto de Modelo (MCP):** Para extender la funcionalidad del agente.

### Integración de Proyectos

Conexión directa con los proyectos de Microsoft Foundry en la nube, permitiendo trabajar con recursos existentes e implementar nuevos agentes en la infraestructura establecida.

---

## Instalación de la Extensión

Para comenzar, asegúrese de tener Visual Studio Code instalado.

1.  Abra Visual Studio Code.
2.  Seleccione el icono de **Extensiones** en la barra lateral izquierda o presione `Ctrl+Mayús+X`.
3.  Busque **Microsoft Foundry**.
4.  Seleccione **Instalar**.
5.  Verifique que la instalación se haya completado correctamente revisando los mensajes de estado.

---

## Flujo de Trabajo Inicial

El proceso típico de desarrollo con la extensión sigue estos pasos:

1.  **Configuración:** Instalación de la extensión y conexión a su **Proyecto de Microsoft Foundry**.
2.  **Creación:** Uso del diseñador visual para crear o importar un agente de IA.
3.  **Configuración:** Definición de las **Instrucciones (Instructions)** y adición de las herramientas necesarias (como búsqueda o intérprete de código).
4.  **Pruebas:** Validación del agente en el **Área de juegos (Playground)** integrada.
5.  **Iteración:** Refinamiento del diseño y los prompts basándose en los resultados de las pruebas.
6.  **Integración:** Generación de código para conectar el agente con su aplicación final.

Este flujo simplificado facilita la creación rápida de prototipos (rapid prototyping) y la implementación de agentes capaces de automatizar tareas complejas del mundo real.

---

# Desarrollo y Gestión de Agentes en VS Code

La creación y configuración de agentes mediante la extensión proporciona una experiencia simplificada que combina la potencia del **Servicio de Agentes de Microsoft Foundry** con la familiaridad de Visual Studio Code.

## Características del Servicio de Agentes

El **Servicio de Agentes de Microsoft Foundry (Microsoft Foundry Agent Service)** es un servicio administrado en Azure que proporciona un marco completo para el ciclo de vida de los agentes. Se basa en la API de Asistentes de OpenAI, pero añade capacidades empresariales críticas:

- **Selección de Modelos Ampliada:** Compatibilidad con diversos modelos de IA más allá de los exclusivos de OpenAI.
- **Seguridad Empresarial:** Características robustas para entornos de producción.
- **Integración de Datos:** Conexión directa a los servicios de datos de Azure para fundamentar las respuestas.
- **Ecosistema de Herramientas:** Acceso a herramientas integradas y personalizadas.

---

## Creación de un Agente

### Requisitos Previos

1.  Tener la extensión instalada y haber iniciado sesión en Azure.
2.  Tener un **Proyecto de Microsoft Foundry** (creado o seleccionado).
3.  Tener un modelo **implementado (deployed)** disponible para su uso.

### Pasos para Crear

1.  Abra la vista de la extensión **Microsoft Foundry** en la barra lateral.
2.  Navegue a la sección **Recursos**.
3.  Haga clic en el icono `+` (más) junto a la subsección **Agentes**.
4.  La extensión abrirá automáticamente el archivo de configuración `.yaml` del nuevo agente y la vista del **Diseñador de Agentes**.

---

## Configuración del Agente

Una vez creado, puede definir el comportamiento del agente mediante el diseñador visual o editando directamente el archivo YAML.

### Propiedades Básicas

- **Nombre:** Un identificador descriptivo para el agente.
- **Selección de Modelo:** Elija una **implementación (deployment)** existente en la lista desplegable.
- **Descripción:** Un resumen claro de la función del agente.
- **Instrucciones (Instructions):** El "prompt del sistema" que define la personalidad, el estilo y las reglas de comportamiento.

### Definición en YAML

Toda la configuración del agente se almacena en un archivo YAML. Esto permite el control de versiones y la edición directa.

```yaml
# Ejemplo de estructura de agente
version: 1.0.0
name: mi-agente-soporte
description: Agente para responder consultas de clientes
model:
  id: "gpt-4o"
  options:
    temperature: 0.7
instructions: "Eres un asistente útil y profesional..."
tools: []
```

---

## Diseño de Instrucciones (Instructions)

Las instrucciones son el núcleo de la inteligencia del agente. Para que sean efectivas:

- **Sea específico:** Defina claramente qué debe hacer el agente y qué no.
- **Proporcione contexto:** Explique el rol del agente y su entorno operativo.
- **Establezca límites:** Indique explícitamente las restricciones de seguridad y comportamiento.
- **Incluya ejemplos:** Muestre interacciones ideales (Few-shot prompting).
- **Defina la personalidad:** Establezca el tono (ej. formal, empático, técnico).

---

## Implementación (Deployment) y Pruebas

### Proceso de Implementación

1.  Seleccione el botón **"Crear en Microsoft Foundry"** en el diseñador.
2.  La extensión gestionará el proceso y el agente aparecerá en la lista de recursos en la nube.
3.  Una vez implementado, puede editarlo y volver a desplegarlo usando **"Actualizar en Microsoft Foundry"**.

### Pruebas en el Área de Juegos (Playground)

Utilice el **Playground** integrado para chatear con su agente en tiempo real. El sistema gestiona la conversación utilizando:

- **Hilos (Threads):** Sesiones de conversación que mantienen el estado y el contexto.
- **Mensajes:** Interacciones individuales (texto, imágenes, archivos).
- **Ejecuciones (Runs):** Procesos donde el agente procesa los mensajes del hilo basándose en su configuración.

---

# Ampliación de las Funcionalidades del Agente con Herramientas

Una de las características más potentes de los agentes de IA es su capacidad para utilizar herramientas que extienden sus capacidades más allá de la simple generación de texto. La extensión **Microsoft Foundry para Visual Studio Code** facilita la adición y configuración de estas herramientas, permitiendo a los agentes realizar acciones, acceder a datos e integrarse con sistemas externos.

## Comprensión de las Herramientas del Agente

Las herramientas son funciones programáticas que permiten a los agentes automatizar acciones y acceder a información fuera de sus datos de entrenamiento. Cuando un agente determina que necesita una herramienta para responder a una solicitud del usuario, puede invocarla automáticamente, procesar los resultados e incorporarlos en su respuesta.

Esta funcionalidad transforma a los agentes de simples generadores de texto en sistemas de automatización eficaces capaces de interactuar con datos y servicios del mundo real.

## Herramientas Integradas

Microsoft Foundry proporciona varias herramientas listas para producción que cubren casos de uso comunes:

- **Intérprete de código (Code Interpreter):** Permite a los agentes escribir y ejecutar código Python en un entorno seguro. Es ideal para cálculos matemáticos, análisis de datos, generación de gráficos y procesamiento de archivos.
- **Búsqueda de archivos (File Search):** Habilita la **Generación Aumentada por Recuperación (RAG)**. Permite cargar e indexar documentos (PDF, Word, TXT) para que el agente busque en bases de conocimiento específicas.
- **Búsqueda de Bing (Bing Search):** Permite buscar en internet datos en tiempo real, eventos actuales y temas de tendencia, proporcionando citas y fuentes.
- **Herramientas OpenAPI:** Conecta agentes a APIs y servicios externos mediante especificaciones estándar OpenAPI 3.0.
- **Protocolo de Contexto de Modelo (MCP):** Interfaces estandarizadas para funcionalidad extendida y herramientas impulsadas por la comunidad.

## Gestión de Herramientas en Visual Studio Code

La extensión ofrece una interfaz visual intuitiva para explorar, configurar y probar herramientas sin necesidad de escribir código complejo.

### Proceso de Adición

1.  Seleccione el agente en la vista de la extensión.
2.  Navegue a la sección **Herramientas (Tools)** en el panel de configuración.
3.  Examine la biblioteca de herramientas disponibles.
4.  Configure las opciones necesarias para la herramienta seleccionada.
5.  Pruebe la integración inmediatamente utilizando el **Área de juegos (Playground)**.

> **Nota:** Al agregar ciertas herramientas, como la **Búsqueda de archivos**, es posible que necesite provisionar recursos adicionales en Azure, como un almacén vectorial (Vector Store) para hospedar los índices de los archivos cargados.

## Servidores del Protocolo de Contexto de Modelo (MCP)

Los servidores **MCP (Model Context Protocol)** proporcionan una forma estandarizada de conectar herramientas a los agentes mediante un protocolo abierto. Este enfoque fomenta la reutilización y el uso de herramientas creadas por la comunidad.

### Ventajas Clave

- **Estandarización:** Protocolo consistente para la comunicación con diversas herramientas.
- **Reutilización:** Componentes que funcionan a través de diferentes implementaciones de agentes.
- **Comunidad:** Acceso a herramientas desarrolladas por terceros a través de registros MCP.
- **Integración:** Interfaces simplificadas para conectar nuevas capacidades.

## Mejores Prácticas en la Gestión de Herramientas

- **Selección:** Identifique claramente qué funcionalidades requiere el agente antes de añadir herramientas.
- **Prioridad:** Comience utilizando las herramientas integradas antes de desarrollar soluciones personalizadas.
- **Pruebas:** Valide exhaustivamente el comportamiento de las herramientas en diversos escenarios de uso en el **Playground**.

---

# Laboratorio: Desarrollo de un Agente de IA con Herramientas MCP en VS Code

En este ejercicio práctico, utilizaremos la **Extensión de Microsoft Foundry para Visual Studio Code** para crear un agente capaz de utilizar herramientas del servidor del **Protocolo de Contexto de Modelo (MCP)**. Este protocolo permite al agente acceder a fuentes de datos externas y APIs, como documentación técnica, para proporcionar respuestas actualizadas.

## 1. Configuración del Entorno y Proyecto

Para comenzar, trabajamos dentro de Visual Studio Code con la extensión de Microsoft Foundry instalada.
*   **Conexión a Azure:** Iniciamos sesión en Azure a través de la extensión para acceder a nuestras suscripciones.
*   **Creación del Proyecto:** Creamos un nuevo **Proyecto de Foundry**. Al hacerlo, definimos un grupo de recursos (ej. `rg-ai-agents-lab`) y una ubicación. La extensión maneja la creación de los recursos necesarios en la nube.

## 2. Implementación del Modelo Base

Un agente necesita un "cerebro". Utilizamos el **Catálogo de Modelos** integrado en la extensión para buscar e **implementar (deploy)** el modelo `gpt-4`.
*   Configuramos la implementación como **Estándar global** (o Estándar) y le asignamos un nombre, por ejemplo, `gpt-4-deployment`.

## 3. Creación y Diseño del Agente

Utilizamos el **Diseñador Visual** de la extensión para crear un nuevo "Agente Clásico". Esto genera un archivo de configuración `.yaml` y abre una interfaz gráfica para ajustar las preferencias.

### Configuración Básica
*   **Nombre:** Le damos un nombre descriptivo, como `agente-de-investigación-de-datos`.
*   **Modelo:** Seleccionamos la implementación de `gpt-4` que acabamos de crear.
*   **Instrucciones:** Definimos el rol del agente.
    > **Instrucción:** "You are an AI agent that helps users research information from various sources. Use the available tools to access up-to-date information and provide comprehensive responses based on external data sources."
    >
    > *(Traducción: Eres un agente de IA que ayuda a los usuarios a investigar información de diversas fuentes. Utiliza las herramientas disponibles para acceder a información actualizada y proporcionar respuestas completas basadas en fuentes de datos externas.)*

## 4. Integración de Herramientas MCP

Para dar superpoderes al agente, añadimos una herramienta de servidor **MCP (Model Context Protocol)**. Esto le permite consultar especificaciones de API externas.
*   Desde la sección de **Herramientas (Tools)** del diseñador, añadimos una herramienta "MCP Server".
*   **URL del Servidor:** Introducimos `https://gitmcp.io/Azure/azure-rest-api-specs`.
*   **Etiqueta:** Asignamos un identificador como `github_docs_server`.

## 5. Implementación y Pruebas

Una vez configurado, hacemos clic en **"Crear agente en Microsoft Foundry"** para desplegarlo en la nube.

### Prueba en el Área de Juegos (Playground)
Abrimos el **Playground** integrado en VS Code para interactuar con el agente.
*   **Prueba:** Le pedimos ayuda con documentación técnica:
    > "Can you help me find documentation about Azure Container Apps and provide an example of how to create one?"
    >
    > *(Traducción: ¿Puedes ayudarme a encontrar documentación sobre Azure Container Apps y proporcionar un ejemplo de cómo crear una?)*
*   **Autorización de Herramientas:** Al ejecutarse, el agente intentará usar la herramienta MCP. El sistema pedirá aprobación para conectar con el servidor externo. En este entorno de prueba, seleccionamos "Sin autenticación" y aprobamos el uso de la herramienta.
*   **Resultado:** El agente consulta el servidor MCP, recupera la información sobre las APIs de Azure y genera una respuesta con el ejemplo solicitado, citando sus fuentes en las **Anotaciones del Agente**.

## 6. Generación de Código y Revisión

### Generación de Código
La extensión facilita la integración del agente en aplicaciones.
*   Seleccionamos la opción **"Abrir archivo de código" (Open Code File)**.
*   Elegimos el lenguaje (ej. Python) y el método de autenticación.
*   La extensión genera automáticamente un script de ejemplo que muestra cómo instanciar el cliente del proyecto y llamar al agente programáticamente.

### Revisión de Hilos (Threads)
En la vista de **Recursos**, podemos explorar la sección de **Hilos (Threads)**. Esto nos permite auditar las conversaciones pasadas, ver los detalles de cada ejecución (Run) y analizar el JSON de las interacciones para depurar el comportamiento del agente y el uso de herramientas.

---

# Resumen del Módulo

En este módulo, ha aprendido a utilizar la extensión de **Microsoft Foundry para Visual Studio Code** para agilizar el desarrollo de agentes de IA.

Los puntos clave incluyen:
*   **Entorno Integrado:** Cómo crear, configurar y probar agentes sin salir de VS Code, utilizando tanto el **Diseñador Visual** como la edición directa de archivos **YAML**.
*   **Herramientas y Capacidades:** La integración de herramientas como el **Intérprete de código**, **Búsqueda de archivos (RAG)** y servidores **MCP (Model Context Protocol)** para extender la funcionalidad del agente.
*   **Ciclo de Vida:** El flujo de trabajo desde la creación y prueba en el **Playground** local hasta la **Implementación (Deployment)** en la nube y la generación de código para la integración en aplicaciones.

---

# Evaluación del módulo

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
