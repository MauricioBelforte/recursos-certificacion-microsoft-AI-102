# Resumen Técnico de SDKs para AI-102 (Python)

Este documento resume las clases, funciones y objetos clave que necesitas conocer para el desarrollo de soluciones de IA en Azure, específicamente con **Microsoft Foundry**.

---

## 1. Autenticación (`azure.identity`)

La base de toda conexión segura en Azure. No uses claves (keys) pegadas en el código si puedes evitarlo.

### `DefaultAzureCredential`

*   **¿Para qué sirve?**: Es la clase mágica de autenticación. Intenta autenticarse en cadena: primero busca variables de entorno, luego credenciales de identidad administrada (si estás en la nube), y finalmente credenciales de Azure CLI o Visual Studio Code (si estás en local).
*   **¿Cómo se importa?**:
    ```python
    from azure.identity import DefaultAzureCredential
    ```
*   **¿Cómo se usa?**:
    Se instancia vacía y se pasa a los clientes de otros servicios.
    ```python
    # Instancia básica
    credential = DefaultAzureCredential()
    
    # Opcional: Para desarrollo local a veces se excluyen ciertas credenciales para evitar conflictos
    credential = DefaultAzureCredential(exclude_shared_token_cache_credential=True)
    ```

---

## 2. Cliente de Proyecto Foundry (`azure.ai.projects`)

Esta es la biblioteca moderna para interactuar con los **Proyectos de Microsoft Foundry**. Es el "pegamento" que une tus modelos, datos y herramientas.

### `AIProjectClient`

*   **¿Para qué sirve?**: Es el objeto principal (el proxy) que conecta tu código con tu Proyecto de Foundry en la nube. A través de él accedes a agentes, conexiones, herramientas y clientes de inferencia.
*   **¿Cómo se importa?**:
    ```python
    from azure.ai.projects import AIProjectClient
    ```
*   **¿Cómo se usa?**:
    Necesitas el **Endpoint** (Punto de conexión) del proyecto y la credencial.
    ```python
    project_client = AIProjectClient(
        endpoint="https://<tu-proyecto>.services.ai.azure.com",
        credential=DefaultAzureCredential()
    )
    ```

### Métodos clave de `AIProjectClient`:

#### `get_openai_client()`
*   **Función**: Devuelve un cliente de chat compatible con la librería estándar de OpenAI, pero ya configurado y autenticado para usar los modelos desplegados en tu proyecto de Foundry.
*   **Uso**:
    ```python
    openai_client = project_client.get_openai_client()
    response = openai_client.chat.completions.create(...)
    ```

#### `connections.list()`
*   **Función**: Lista todas las conexiones configuradas en el proyecto (ej. conexiones a Azure AI Search, Content Safety, etc.). Útil para no hardcodear claves de otros servicios.

#### `agents.create_version()`
*   **Función**: Crea una nueva versión de un agente de IA en la nube.

---

## 3. Definición de Agentes (`azure.ai.projects.models`)

Objetos de configuración que se pasan al `AIProjectClient` para decirle cómo debe comportarse un agente.

### `PromptAgentDefinition`

*   **¿Para qué sirve?**: Define la configuración de un agente: qué modelo usa, cuáles son sus instrucciones (prompt del sistema) y qué herramientas tiene habilitadas.
*   **¿Cómo se importa?**:
    ```python
    from azure.ai.projects.models import PromptAgentDefinition
    ```
*   **¿Cómo se usa?**:
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

*   **¿Para qué sirve?**: Habilita al agente para escribir y ejecutar código Python en un entorno seguro (sandbox). Ideal para matemáticas, gráficos y análisis de archivos.
*   **¿Cómo se importa?**:
    ```python
    from azure.ai.projects.models import CodeInterpreterTool
    ```
*   **¿Cómo se usa?**:
    Generalmente se combina con `CodeInterpreterToolAuto` si queremos que maneje archivos automáticamente.
    ```python
    # Habilitar la herramienta
    code_tool = CodeInterpreterTool()
    ```

### `FunctionTool`

*   **¿Para qué sirve?**: Permite conectar tus propias funciones de Python (locales) al agente. El agente decide cuándo llamarlas, te pide que las ejecutes y tú le devuelves el resultado.
*   **¿Cómo se importa?**:
    ```python
    from azure.ai.projects.models import FunctionTool
    ```
*   **¿Cómo se usa?**:
    Se define un esquema JSON (parameters) que explica al agente qué argumentos necesita la función.
    ```python
    mi_herramienta = FunctionTool(
        name="obtener_clima",
        description="Obtiene el clima de una ciudad",
        parameters={ ... esquema JSON ... }
    )
    ```

### `OpenApiTool`

*   **¿Para qué sirve?**: Conecta el agente a una API REST externa utilizando una especificación OpenAPI (Swagger).
*   **¿Cómo se importa?**:
    ```python
    from azure.ai.projects.models import OpenApiTool
    ```

---

## 5. Servicio de Agentes Específico (`azure.ai.agents`)

> **Nota**: A veces verás este SDK (`azure.ai.agents`) usado para arquitecturas multi-agente más complejas o específicas, separado del cliente de proyectos general.

### `AgentsClient`

*   **¿Para qué sirve?**: Es el cliente específico para gestionar el ciclo de vida de agentes, hilos y ejecuciones en el servicio de Agentes de Azure AI.
*   **¿Cómo se importa?**:
    ```python
    from azure.ai.agents import AgentsClient
    ```
*   **Métodos clave**:
    *   `create_agent(...)`: Crea un agente.
    *   `threads.create()`: Crea un hilo de conversación.
    *   `messages.create(...)`: Añade un mensaje al hilo.
    *   `runs.create_and_process(...)`: Ejecuta el agente sobre el hilo y espera el resultado.

### `ConnectedAgentTool`

*   **¿Para qué sirve?**: **Crucial para Multi-Agente**. Permite "envolver" un agente existente como si fuera una herramienta para que *otro* agente (el principal/orquestador) pueda llamarlo.
*   **¿Cómo se importa?**:
    ```python
    from azure.ai.agents.models import ConnectedAgentTool
    ```
*   **¿Cómo se usa?**:
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

*   **¿Para qué sirve?**: Permite conectar un agente a un servidor MCP (Model Context Protocol) remoto. Define la URL del servidor y si se requiere aprobación humana para usar sus herramientas.
*   **¿Cómo se importa?**:
    ```python
    from azure.ai.projects.models import MCPTool
    ```
*   **¿Cómo se usa?**:
    ```python
    mcp_tool = MCPTool(
        server_label="docs-server",
        server_url="https://api.example.com/mcp",
        require_approval="always" # Obliga a aprobar cada uso
    )
    ```

### `McpApprovalResponse`

*   **¿Para qué sirve?**: Objeto utilizado para enviar la aprobación explícita al agente cuando este solicita permiso para usar una herramienta MCP protegida.
*   **¿Cómo se importa?**:
    ```python
    from openai.types.responses.response_input_param import McpApprovalResponse
    ```

---

## 7. Cliente OpenAI Estándar (`openai`)

Aunque usamos Azure, a menudo interactuamos con las clases estándar de la librería `openai`.

### `AzureOpenAI`

*   **¿Para qué sirve?**: Cliente para chat estándar (sin agentes, solo request/response) o para generar embeddings.
*   **¿Cómo se importa?**:
    ```python
    from openai import AzureOpenAI
    ```
*   **Uso común (Patrón RAG manual)**:
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

*   ❌ `Foundry Tools SDK`: No existe con este nombre.
*   ❌ `ChatCompletionsClient`: No es el cliente principal de proyecto.
*   ❌ `Connection properties`: Es un concepto, no una clase de cliente.
*   ❌ `HubClient`: Generalmente interactuamos a nivel de `Project`, no de Hub directamente para desarrollar la app.