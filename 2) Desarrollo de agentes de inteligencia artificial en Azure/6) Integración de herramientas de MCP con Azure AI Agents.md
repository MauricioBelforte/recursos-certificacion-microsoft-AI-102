# Integración de herramientas de MCP con Azure AI Agents

## Introducción

Los agentes de inteligencia artificial son capaces de realizar una amplia gama de tareas, pero muchas de ellas requieren interactuar con herramientas externas al Modelo de Lenguaje Grande (**LLM**). Es común que los agentes necesiten acceder a APIs, bases de datos o servicios internos. La integración y el mantenimiento manual de estas conexiones pueden volverse complejos rápidamente, especialmente a medida que el sistema crece o cambia con frecuencia.

Los servidores del **Protocolo de Contexto de Modelo (Model Context Protocol - MCP)** pueden ayudar a resolver este problema simplificando la integración con los agentes de IA. Conectar un agente de Azure AI a un servidor MCP proporciona al agente un catálogo de herramientas accesible bajo demanda. Este enfoque hace que la solución de inteligencia artificial sea más robusta, escalable y fácil de mantener.

### Escenario de Ejemplo: Gestión de Inventario

Suponga que trabaja para un minorista especializado en cosméticos. Su equipo desea crear un asistente de IA que ayude a administrar el inventario verificando los niveles de existencias de productos y analizando las tendencias de ventas recientes.

Con un servidor MCP, puede conectar el asistente a un conjunto de herramientas estandarizadas capaces de realizar evaluaciones de inventario y proporcionar recomendaciones al equipo, sin necesidad de codificar cada integración de forma rígida en el agente.

En este módulo, aprenderá a:

- Configurar un cliente y un servidor MCP.
- Conectar herramientas a un agente de Azure AI de forma dinámica.

---

# Comprender el descubrimiento de herramientas de MCP

A medida que los agentes de inteligencia artificial se vuelven más capaces, la gama de herramientas y servicios a los que pueden acceder también crece. Sin embargo, el registro, la administración, la actualización y la integración de nuevas herramientas pueden volverse tareas complejas y lentas rápidamente. La **Detección dinámica de herramientas (Dynamic Tool Discovery)** ayuda a resolver este problema al permitir que los agentes busquen y utilicen herramientas automáticamente en tiempo de ejecución.

## Ventajas del Protocolo de Contexto de Modelo para agentes de IA

El **Protocolo de Contexto de Modelo (Model Context Protocol - MCP)** proporciona varias ventajas que mejoran las funcionalidades y la flexibilidad de los agentes de IA:

- **Detección dinámica de herramientas:** Los agentes de IA pueden recibir automáticamente una lista de las herramientas disponibles desde un servidor, junto con descripciones de sus funciones. A diferencia de las APIs tradicionales, que a menudo requieren codificación manual para cada integración y actualizaciones cada vez que cambia la API, MCP habilita un enfoque de "integrar una vez" que mejora la adaptabilidad y reduce el mantenimiento.
- **Interoperabilidad entre LLM:** MCP funciona perfectamente con diferentes **Modelos de Lenguaje Grande (LLM)**, lo que permite a los desarrolladores cambiar o evaluar modelos principales para mejorar el rendimiento sin tener que volver a trabajar en las integraciones.
- **Seguridad estandarizada:** MCP proporciona un método de autenticación coherente, lo que simplifica el acceso seguro entre varios servidores MCP. Esto elimina la necesidad de administrar claves independientes o protocolos de autenticación para cada API, facilitando el escalado de las implementaciones de agentes de IA.

## ¿Qué es la detección dinámica de herramientas?

La detección dinámica de herramientas es un mecanismo que permite a un agente de IA descubrir herramientas externas disponibles sin necesidad de tener conocimiento codificado de forma rígida (**hard-coded**) de cada una. En lugar de agregar o actualizar manualmente todas las herramientas que el agente puede usar, este consulta un servidor MCP centralizado. Este servidor actúa como un catálogo activo, exponiendo herramientas que el agente puede comprender y llamar.

Este enfoque significa que:

- Las herramientas se pueden agregar, actualizar o quitar de forma centralizada sin modificar el código del agente.
- Los agentes siempre pueden usar la versión más reciente de una herramienta, mejorando la precisión y la confiabilidad.
- La complejidad de administrar herramientas se traslada del agente a un servicio dedicado.

## ¿Cómo habilita MCP la detección dinámica de herramientas?

Un servidor MCP hospeda un conjunto de funciones que se exponen como herramientas mediante el decorador `@mcp.tool`. Las herramientas son un tipo primitivo en el MCP que permite a los servidores exponer funcionalidad ejecutable a los clientes.

Un cliente puede conectarse al servidor y capturar estas herramientas de forma dinámica. A continuación, el cliente genera envoltorios (**wrappers**) de funciones que se agregan a las definiciones de herramientas del agente de Azure AI. Esta configuración crea una canalización flexible:

1.  El **Servidor MCP** hospeda las herramientas disponibles.
2.  El **Cliente MCP** detecta dinámicamente las herramientas.
3.  El **Agente de Azure AI** utiliza las herramientas disponibles para responder a las solicitudes del usuario.

## ¿Por qué usar la detección dinámica de herramientas con MCP?

Este enfoque proporciona varias ventajas clave:

- **Escalabilidad:** Agregue fácilmente nuevas herramientas o actualice las existentes sin volver a implementar los agentes.
- **Modularidad:** Los agentes pueden mantenerse simples, centrándose en la delegación en lugar de administrar los detalles de cada herramienta.
- **Mantenibilidad:** La administración centralizada de herramientas reduce la duplicación de código y los errores.
- **Flexibilidad:** Admite diversos tipos de herramientas y flujos de trabajo complejos agregando funcionalidades según sea necesario.

La detección dinámica de herramientas es especialmente útil en entornos donde las herramientas evolucionan rápidamente o donde muchos equipos administran diferentes APIs y servicios. El uso de herramientas permite a los agentes de IA adaptarse a funcionalidades cambiantes en tiempo real, interactuar con sistemas externos de forma segura y realizar acciones que van más allá de la simple generación de lenguaje.

---

# Integración de herramientas de agente mediante un servidor MCP y un cliente

Para conectar dinámicamente las herramientas al agente de Azure AI, primero necesita una configuración del **Protocolo de Contexto de Modelo (Model Context Protocol - MCP)** en funcionamiento. Esto incluye tanto el **Servidor MCP**, que hospeda el catálogo de herramientas, como el **Cliente MCP**, que captura esas herramientas y las hace utilizables por el agente.

## ¿Qué es el servidor MCP?

El **Servidor MCP** actúa como un registro para las herramientas que puede usar el agente. Puede inicializar el servidor MCP utilizando `FastMCP("server-name")`. La clase `FastMCP` utiliza sugerencias de tipo de Python (**type hints**) y cadenas de documentación (**docstrings**) para generar automáticamente definiciones de herramientas, lo que facilita la creación y el mantenimiento de herramientas de MCP.

A continuación, estas definiciones se sirven a través de HTTP cuando el cliente lo solicita. Dado que las definiciones de herramientas residen en el servidor, puede actualizar o agregar nuevas herramientas en cualquier momento, sin tener que modificar ni volver a implementar (**redeploy**) el agente.

## ¿Qué es el cliente MCP?

Un **Cliente MCP** estándar actúa como un puente entre el servidor MCP y el **Servicio de Agentes de Azure AI (Azure AI Agent Service)**. El cliente inicializa una sesión y se conecta al servidor. Después, realiza tres tareas clave:

1.  **Detectar herramientas:** Identifica las herramientas disponibles del servidor MCP mediante `session.list_tools()`.
2.  **Generar envoltorios:** Crea códigos auxiliares (**stubs**) de funciones de Python que encapsulan las herramientas.
3.  **Registrar funciones:** Registra esas funciones con el agente.

Esto permite al agente llamar a cualquier herramienta que aparezca en el catálogo de MCP como si fuera una función nativa, todo ello sin lógica codificada de forma rígida (**hard-coded**).

## Registro de herramientas con un agente de Azure AI

Cuando se inicializa una sesión de cliente MCP, el cliente puede extraer dinámicamente herramientas del servidor. Se puede invocar una herramienta MCP mediante `session.call_tool(tool_name, tool_args)`.

Las herramientas deben encapsularse en una función asincrónica para que el agente pueda invocarlas. Por último, esas funciones se agrupan en un `FunctionTool`, pasan a formar parte del conjunto de herramientas del agente y están disponibles durante el tiempo de ejecución para cualquier solicitud de usuario.

### Resumen del flujo de integración

1.  El **Servidor MCP** hospeda definiciones de herramientas decoradas con `@mcp.tool`.
2.  El **Cliente MCP** inicializa una conexión al servidor.
3.  El **Cliente MCP** captura las definiciones de herramientas disponibles con `session.list_tools()`.
4.  Cada herramienta está encapsulada en una función asincrónica que invoca `session.call_tool`.
5.  Las funciones de la herramienta se agrupan en un objeto `FunctionTool` que el agente puede usar.
6.  El `FunctionTool` se registra en el conjunto de herramientas del agente.

Ahora el agente puede acceder a las herramientas e invocarlas a través de la interacción con lenguaje natural. Al configurar el cliente y el servidor MCP, se crea una separación limpia entre la administración de herramientas y la lógica del agente, lo que permite que el sistema se adapte rápidamente a medida que estén disponibles nuevas herramientas.

---

# Uso de agentes de Azure AI con servidores MCP

Para mejorar el agente de Microsoft Foundry, conéctelo a los servidores del **Protocolo de Contexto de Modelo (Model Context Protocol - MCP)**. Los servidores MCP proporcionan herramientas y datos contextuales que el agente puede usar para realizar tareas, ampliando sus funcionalidades más allá de las funciones integradas. **Azure AI Agent Service** incluye compatibilidad con servidores MCP remotos, lo que permite al agente conectarse rápidamente al servidor y acceder a sus herramientas.

Cuando se usa el **Servicio de Agentes de Microsoft Foundry (Microsoft Foundry Agent Service)** para conectarse al servidor MCP, no es necesario crear manualmente una sesión de cliente MCP ni agregar ninguna herramienta de función al agente. En su lugar, cree un objeto de herramienta MCP que se conecte al servidor MCP.

## Integración de servidores MCP remotos

Para conectarse a un servidor MCP, necesita:

- Un **Punto de conexión (Endpoint)** de servidor MCP remoto (por ejemplo, `https://api.githubcopilot.com/mcp/`).
- Un agente de Microsoft Foundry configurado para usar la herramienta MCP.

Puede conectarse a varios servidores MCP agregándolos al agente como herramientas independientes. Cada objeto `MCPTool` puede incluir los siguientes parámetros:

- `server_label`: Un identificador único para el servidor MCP (por ejemplo, "GitHub").
- `server_url`: La dirección URL del servidor MCP.
- `allowed_tools` (opcional): Una lista de herramientas específicas a las que puede acceder el agente.
- `require_approval` (opcional): Valor booleano que determina si las invocaciones de herramientas requieren aprobación humana. Si se establece en `true`, el agente se pausará y esperará la aprobación antes de invocar cualquier herramienta en el servidor MCP.

La herramienta MCP también admite encabezados personalizados, lo que le permite pasar:

- Claves de autenticación (claves de API, tokens de OAuth).
- Otros encabezados necesarios para el servidor MCP.

## Uso de herramientas

Al usar el objeto de herramienta MCP de Azure, no es necesario encapsular las herramientas de función ni invocar `session.call_tool`. En su lugar, las herramientas se invocan automáticamente cuando es necesario durante la ejecución de un agente.

Para invocar automáticamente las herramientas de MCP:

1.  Cree el objeto `MCPTool` con la etiqueta y la dirección URL del servidor.
2.  Use `update_headers` para aplicar los encabezados requeridos por el servidor.
3.  Use el parámetro `require_approval` para determinar si se requiere aprobación. Los valores admitidos son:
    - `always`: Un desarrollador debe proporcionar aprobación para cada llamada.
    - `never`: No se requiere ninguna aprobación.
4.  Agregue el objeto `MCPTool` a la lista de herramientas del agente.
5.  Invoque una solicitud en el agente; debería ver los resultados de las herramientas invocadas en la respuesta.

### Aprobación de Herramientas

Si el modelo intenta invocar una herramienta en el servidor MCP con la aprobación necesaria, obtendrá una `mcp_approval_request` en la respuesta del agente. Esto incluye información sobre qué herramienta se está invocando y puede usar esta información para decidir si aprobar la solicitud.

Para aprobar, envíe un mensaje de seguimiento con el objeto `mcp_approval_response`, que incluye un valor `approval_request_id` y un booleano `approve`.

La integración de MCP es un paso clave para crear agentes de IA más enriquecidos y conscientes del contexto. A medida que crece el ecosistema de MCP, tendrá aún más oportunidades para incorporar herramientas especializadas a los flujos de trabajo y ofrecer soluciones más inteligentes y dinámicas.

---

# Laboratorio: Desarrollar un agente de IA con herramientas del Protocolo de Contexto de Modelo (MCP)


En este ejercicio, utilizarás la extensión **Microsoft Foundry** para VS Code para crear un agente que pueda usar las herramientas del servidor del **Protocolo de Contexto de Modelo (MCP)** para acceder a fuentes de datos externas y API. El agente podrá recuperar información actualizada e interactuar con servicios personalizados a través de las herramientas MCP.

Este ejercicio debería completarse en aproximadamente **60 minutos**.

> **Nota:** Algunas de las tecnologías utilizadas en este ejercicio están en fase de vista previa o en desarrollo activo. Es posible que experimente comportamientos inesperados, advertencias o errores.

## Requisitos previos

Antes de comenzar este ejercicio, asegúrese de tener:

- Visual Studio Code instalado en su máquina local.
- Una suscripción activa a Azure.
- Python 3.13 o posterior instalado.
- Git instalado en tu máquina local.

## Instala la extensión Microsoft Foundry para VS Code

Comencemos por instalar y configurar la extensión de VS Code.

1.  Abre Visual Studio Code.
2.  Seleccione **Extensiones** en el panel izquierdo (o pulse `Ctrl+Mayús+X`).
3.  En la barra de búsqueda, escribe **Microsoft Foundry** y pulsa Intro.
4.  Seleccione la extensión **Microsoft Foundry** de Microsoft y haga clic en **Instalar**.
5.  Una vez finalizada la instalación, compruebe que la extensión aparece en la barra de navegación principal del lado izquierdo de Visual Studio Code.

## Inicia sesión en Azure y crea un proyecto

Ahora te conectarás a tus recursos de Azure y crearás un nuevo proyecto de Microsoft Foundry.

1.  En la barra lateral de VS Code, seleccione el icono de la extensión **Microsoft Foundry**.
2.  En la vista **Recursos**, seleccione **Iniciar sesión en Azure…** y siga las instrucciones de autenticación.
    > **Nota:** Esta opción no aparecerá si ya has iniciado sesión.
3.  Crea un nuevo proyecto de Foundry seleccionando el icono `+` (más) que aparece junto a **Recursos** en la vista de la extensión de Foundry.
4.  Seleccione su suscripción de Azure en el menú desplegable.
5.  Elija si desea crear un nuevo grupo de recursos o utilizar uno existente:
    - Para crear un nuevo grupo de recursos:
      - Seleccione **Crear nuevo grupo de recursos** y pulse Intro.
      - Introduzca un nombre para su grupo de recursos (por ejemplo, `rg-ai-agents-lab`) y pulse Intro.
      - Seleccione una ubicación de las opciones disponibles y pulse Intro.
    - Para utilizar un grupo de recursos existente:
      - Seleccione el grupo de recursos que desea utilizar de la lista y pulse Intro.
6.  Introduzca un nombre para su proyecto de Foundry (por ejemplo, `ai-agents-project`) en el cuadro de texto y pulse Intro.
7.  Espere a que finalice la implementación del proyecto. Aparecerá una ventana emergente con el mensaje "Proyecto implementado correctamente".

## Implementar un modelo

En esta tarea, implementarás un modelo desde el Catálogo de modelos para usarlo con tu agente.

1.  Cuando aparezca la ventana emergente "Proyecto implementado correctamente", seleccione el botón **Implementar un modelo**. Esto abre el Catálogo de modelos.
    > **Consejo:** También puede acceder al Catálogo de modelos seleccionando el icono `+` junto a **Modelos** en la sección **Recursos**, o pulsando `F1` y ejecutando el comando **Microsoft Foundry: Abrir catálogo de modelos**.
2.  En el Catálogo de modelos, localice el modelo `gpt-4.1` (puede usar la barra de búsqueda para encontrarlo rápidamente).
3.  Seleccione **Implementar** junto al modelo `gpt-4.1`.
4.  Configure los ajustes de implementación:
    - **Nombre de la implementación:** Introduzca un nombre como `gpt-4.1`.
    - **Tipo de despliegue:** Seleccione **Estándar global** (o Estándar si Estándar global no está disponible).
    - **Versión del modelo:** Dejar como está por defecto.
    - **Tokens por minuto:** Dejar como está por defecto.
5.  Seleccione **Implementar en Microsoft Foundry** en la esquina inferior izquierda.
6.  En el cuadro de diálogo de confirmación, seleccione **Implementar** para implementar el modelo.
7.  Espere a que finalice el despliegue. Su modelo desplegado aparecerá en la sección **Modelos** de la vista **Recursos**.
8.  Haz clic con el botón derecho en el nombre del despliegue del proyecto y selecciona **Copiar punto final del proyecto** (Copy Project Endpoint). Necesitarás esta URL para conectar tu agente al proyecto Foundry en los siguientes pasos.

## Clona el repositorio de código inicial

Para este ejercicio, utilizarás un código de inicio que te ayudará a conectarte a tu proyecto de Foundry y crear un agente que use herramientas de servidor MCP.

1.  Navegue a la pestaña **Bienvenido** en VS Code (puede abrirla seleccionando **Ayuda > Bienvenido** en la barra de menú).
2.  Seleccione **Clonar repositorio git** e introduzca la URL del repositorio de código inicial: `https://github.com/MicrosoftLearning/mslearn-ai-agents.git`.
3.  Cree una nueva carpeta y elija **Seleccionar como destino del repositorio**, luego abra el repositorio clonado cuando se le solicite.
4.  En la vista Explorador, navegue a la carpeta `Labfiles/03-mcp-integration/Python` para encontrar el código de inicio para este ejercicio.
5.  Haz clic con el botón derecho en el archivo `requirements.txt` y selecciona **Abrir en terminal integrado**.
6.  En la terminal, introduce el siguiente comando para instalar los paquetes de Python necesarios en un entorno virtual:
    ```bash
    python -m venv labenv
    .\labenv\Scripts\Activate.ps1
    pip install -r requirements.txt
    ```
7.  Abre el archivo `.env`, reemplaza el marcador de posición `your_project_endpoint` con el **Punto de conexión del proyecto** (copiado del recurso de implementación del proyecto en la extensión Microsoft Foundry) y asegúrate de que la variable `MODEL_DEPLOYMENT_NAME` esté configurada con el nombre de tu implementación de modelo. Usa `Ctrl+S` para guardar el archivo después de hacer estos cambios.

Ahora estás listo para crear un agente de IA que use herramientas de servidor MCP para acceder a fuentes de datos externas y API.

## Conecte un agente de IA de Azure a un servidor MCP remoto

En esta tarea, te conectarás a un servidor MCP remoto, prepararás el agente de IA y ejecutarás un prompt de usuario.

1.  Abre el archivo `agent.py` en el editor de código.
    > **Consejo:** A medida que agregues código, asegúrate de mantener la sangría correcta. Usa los niveles de sangría de los comentarios como guía.
2.  Busque el comentario `Add references` (Agregar referencias) y agregue el siguiente código para importar las clases:

    ```python
    # Agregar referencias
    from azure.identity import DefaultAzureCredential
    from azure.ai.projects import AIProjectClient
    # MCPTool es la clase clave para conectar servidores remotos
    from azure.ai.projects.models import PromptAgentDefinition, MCPTool
    from openai.types.responses.response_input_param import McpApprovalResponse, ResponseInputParam
    ```

3.  Busque el comentario `Connect to the agents client` (Conectar con el cliente de agentes) y agregue el siguiente código para conectarse al proyecto Azure AI usando las credenciales actuales.

    ```python
    # Conectar con el cliente de agentes
    with (
        DefaultAzureCredential() as credential,
        AIProjectClient(endpoint=project_endpoint, credential=credential) as project_client,
        project_client.get_openai_client() as openai_client,
    ):
    ```

4.  Debajo del comentario `Initialize agent MCP tool` (Inicializar herramienta MCP del agente), agregue el siguiente código:

    ```python
    # Inicializar herramienta MCP del agente
    # Configuramos la conexión a la API de Microsoft Learn (servidor MCP público)
    # 'require_approval="always"' significa que el humano debe confirmar antes de usar la herramienta.
    mcp_tool = MCPTool(
        server_label="api-specs",
        server_url="https://learn.microsoft.com/api/mcp",
        require_approval="always",
    )
    ```

5.  Debajo del comentario `Create a new agent with the MCP tool` (Crear un nuevo agente con la herramienta MCP) agregue el siguiente código:

    ```python
    # Crear un nuevo agente con la herramienta MCP
    agent = project_client.agents.create_version(
        agent_name="MyAgent",
        definition=PromptAgentDefinition(
            model=model_deployment,
            instructions="Eres un agente útil que puede usar herramientas MCP para ayudar a los usuarios. Usa las herramientas MCP disponibles para responder preguntas y realizar tareas.",
            tools=[mcp_tool],
        ),
    )
    print(f"Agente creado (id: {agent.id}, nombre: {agent.name}, versión: {agent.version})")
    ```

6.  Busque el comentario `Create a conversation thread` (Crear un hilo de conversación) y agregue:

    ```python
    # Crear un hilo de conversación
    conversation = openai_client.conversations.create()
    print(f"Conversación creada (id: {conversation.id})")
    ```

7.  Busque el comentario `Send initial request that will trigger the MCP tool` (Enviar solicitud inicial que activará la herramienta MCP) y agregue:

    ```python
    # Enviar solicitud inicial que activará la herramienta MCP
    # Esta pregunta específica sobre Azure Container Apps debería disparar el uso de la herramienta de documentación
    response = openai_client.responses.create(
        conversation=conversation.id,
        input="Dame los comandos de Azure CLI para crear una Azure Container App con una identidad administrada.",
        extra_body={"agent": {"name": agent.name, "type": "agent_reference"}},
    )
    ```

8.  Busque el comentario `Process any MCP approval requests that were generated` (Procesar cualquier solicitud de aprobación MCP generada) y agregue:

    ```python
    # Procesar cualquier solicitud de aprobación MCP generada
    input_list: ResponseInputParam = []
    for item in response.output:
        # Si el agente quiere usar una herramienta que requiere aprobación, nos avisa con este tipo
        if item.type == "mcp_approval_request":
            if item.server_label == "api-specs" and item.id:
                # Aprobar automáticamente la solicitud MCP para permitir que el agente proceda
                input_list.append(
                    McpApprovalResponse(
                        type="mcp_approval_response",
                        approve=True,
                        approval_request_id=item.id,
                    )
                )

    print("Entrada final (Aprobaciones):")
    print(input_list)
    ```

9.  Busque el comentario `Send the approval response back and retrieve a response` (Enviar la respuesta de aprobación de vuelta y recuperar una respuesta) y agregue:

    ```python
    # Enviar la respuesta de aprobación de vuelta y recuperar una respuesta
    response = openai_client.responses.create(
        input=input_list,
        previous_response_id=response.id,
        extra_body={"agent": {"name": agent.name, "type": "agent_reference"}},
    )

    print(f"\nRespuesta del agente: {response.output_text}")
    ```

10. Busque el comentario `Clean up resources by deleting the agent version` (Limpiar recursos eliminando la versión del agente) y agregue:

    ```python
    # Limpiar recursos eliminando la versión del agente
    project_client.agents.delete_version(agent_name=agent.name, agent_version=agent.version)
    print("Agente eliminado")
    ```

11. Guarde el archivo de código (`CTRL+S`) cuando haya terminado.

## Ejecutar la aplicación

1.  En la terminal integrada, introduzca el siguiente comando para ejecutar la aplicación:

    ```bash
    python agent.py
    ```

2.  Espere a que el agente procese el prompt, utilizando el servidor MCP para encontrar una herramienta adecuada para recuperar la información solicitada.
    Observe que el agente pudo invocar la herramienta MCP para cumplir automáticamente con la solicitud.

## Conecte un agente de IA de Azure a herramientas de servidor MCP personalizadas

Además de conectarse a servidores MCP remotos, también puede crear sus propias herramientas de servidor MCP personalizadas y conectarlas a su agente. En esta tarea, agregará herramientas que permitirán a un agente realizar consultas de inventario y recomendaciones.

### Crear un servidor MCP con herramientas personalizadas

1.  Abra el archivo `server.py` en el editor de código.
    En este archivo de código, definirá las herramientas que el agente puede usar para simular un servicio backend para la tienda minorista.
2.  Debajo del comentario `Add references` (Agregar referencias), agregue el siguiente código:

    ```python
    # Agregar referencias
    # FastMCP permite crear servidores rápidamente con Python
    from mcp.server.fastmcp import FastMCP
    ```

3.  Debajo del comentario `Create an MCP server` (Crear un servidor MCP), agregue:

    ```python
    # Crear un servidor MCP
    mcp = FastMCP(name="Inventory")
    ```

4.  Busque el comentario `Add an inventory check mcp tool` (Agregar herramienta MCP de verificación de inventario) y agregue el decorador y la función:

    ```python
    # Agregar herramienta MCP de verificación de inventario
    @mcp.tool()
    def get_inventory_levels() -> dict:
        """Recupera los niveles de inventario actuales."""
        return {
            "Moisturizer": 6,
            "Shampoo": 8,
            "Body Spray": 28,
            "Conditioner": 14,
            "Face Wash": 4
        }
    ```

5.  Busque el comentario `Add a weekly sales mcp tool` (Agregar herramienta MCP de ventas semanales) y agregue:

    ```python
    # Agregar herramienta MCP de ventas semanales
    @mcp.tool()
    def get_weekly_sales() -> dict:
        """Recupera las cifras de ventas de la última semana."""
        return {
            "Moisturizer": 20,
            "Shampoo": 12,
            "Body Spray": 4,
            "Conditioner": 10,
            "Face Wash": 15
        }
    ```

6.  Busque el comentario `Run the MCP server` (Ejecutar el servidor MCP) y agregue:

    ```python
    # Ejecutar el servidor MCP
    if __name__ == "__main__":
        mcp.run()
    ```

7.  Guarde el archivo (`CTRL+S`).

### Implementar un Cliente MCP

Un cliente MCP es el componente que se conecta al servidor MCP para descubrir y llamar a herramientas.

1.  Navegue al archivo `client.py`.
2.  Busque el comentario `Add references` (Agregar referencias) y agregue:

    ```python
    # Agregar referencias
    from mcp import ClientSession, StdioServerParameters
    from mcp.client.stdio import stdio_client
    ```

3.  En el método `connect_to_server` (o dentro de `main`), busque el comentario `Start the MCP server` (Iniciar el servidor MCP) y agregue:

    ```python
    # Iniciar el servidor MCP
    # Configuramos el cliente para ejecutar el script server.py como un subproceso
    stdio_transport = await exit_stack.enter_async_context(stdio_client(server_params))
    stdio, write = stdio_transport
    ```

4.  Busque el comentario `Create an MCP client session` (Crear sesión de cliente MCP) y agregue:

    ```python
    # Crear sesión de cliente MCP
    session = await exit_stack.enter_async_context(ClientSession(stdio, write))
    await session.initialize()
    ```

5.  Debajo del comentario `List available tools` (Listar herramientas disponibles), agregue:

    ```python
    # Listar herramientas disponibles
    # El cliente descubre dinámicamente qué puede hacer el servidor
    response = await session.list_tools()
    tools = response.tools
    print("\nConectado al servidor con herramientas:", [tool.name for tool in tools])
    ```

### Conecte las herramientas MCP a su agente

1.  En el bucle de chat o función principal, busque el comentario `Build a function for each tool` (Construir una función para cada herramienta) y agregue:

    ```python
    # Construir una función para cada herramienta
    # Creamos 'wrappers' asíncronos para llamar al cliente MCP
    def make_tool_func(tool_name):
        async def tool_func(**kwargs):
            result = await session.call_tool(tool_name, kwargs)
            return result

        tool_func.__name__ = tool_name
        return tool_func

    # Almacenar las funciones en un diccionario para acceso fácil al procesar llamadas
    functions_dict = {tool.name: make_tool_func(tool.name) for tool in tools}
    ```

2.  Busque el comentario `Create FunctionTool definitions for the agent` (Crear definiciones FunctionTool para el agente) y agregue:

    ```python
    # Crear definiciones FunctionTool para el agente
    # Convertimos las herramientas MCP al formato que entiende Azure AI Agent
    mcp_function_tools = []
    for tool in tools:
        function_tool = FunctionTool(
            name=tool.name,
            description=tool.description,
            parameters={
                "type": "object",
                "properties": {}, # Simplificado para este lab
                "additionalProperties": False,
            },
            strict=True
        )
        mcp_function_tools.append(function_tool)
    ```

3.  Busque el comentario `Create the agent` (Crear el agente) y agregue:

    ```python
    # Crear el agente
    agent = project_client.agents.create_version(
        agent_name="inventory-agent",
        definition=PromptAgentDefinition(
            model=model_deployment,
            instructions="""
            Eres un asistente de inventario. Aquí tienes algunas pautas generales:
            - Recomienda reabastecer si el inventario < 10 y las ventas semanales > 15
            - Recomienda liquidación si el inventario > 20 y las ventas semanales < 5
            """,
            tools=mcp_function_tools
        ),
    )
    ```

4.  Localice el comentario `Process function calls` (Procesar llamadas a funciones) y agregue:

    ```python
    # Procesar llamadas a funciones
    for item in response.output:
        if item.type == "function_call":
            # Recuperar la herramienta de función correspondiente
            function_name = item.name
            kwargs = json.loads(item.arguments)
            required_function = functions_dict.get(function_name)

            # Invocar la función (que llama al servidor MCP)
            output = await required_function(**kwargs)

            # Anexar el texto de salida
            input_list.append(
                FunctionCallOutput(
                    type="function_call_output",
                    call_id=item.call_id,
                    output=output.content[0].text,
                )
            )
    ```

5.  Busque el comentario `Send function call outputs back to the model and retrieve a response` (Enviar salidas de llamadas a funciones de vuelta al modelo y recuperar respuesta) y agregue:

    ```python
    # Enviar salidas de llamadas a funciones de vuelta al modelo y recuperar respuesta
    if input_list:
        response = openai_client.responses.create(
                input=input_list,
                previous_response_id=response.id,
                extra_body={"agent": {"name": agent.name, "type": "agent_reference"}},
        )
    print(f"Respuesta del agente: {response.output_text}")
    ```

6.  Guarde el archivo (`CTRL+S`).

### Ejecutar la aplicación

1.  En la terminal integrada, introduzca:
    ```bash
    python client.py
    ```
2.  Cuando se le solicite, introduzca un prompt como:
    `Show me the current inventory levels for all products.`
    _(Muéstrame los niveles de inventario actuales para todos los productos.)_

3.  Debería ver que el agente utiliza las herramientas MCP para obtener los datos y responder.

## Limpiar

Cuando hayas terminado, elimina tu modelo y el grupo de recursos en Azure para evitar costos.

---

# Código Completo del Laboratorio: Herramientas MCP

Este documento contiene el código completo para los tres archivos principales utilizados en el laboratorio: `agent.py` (conexión a servidor remoto), `server.py` (servidor MCP personalizado) y `client.py` (cliente MCP e integración con el agente).

## 1. Agente con Servidor MCP Remoto (`agent.py`)

Este script conecta un agente de Azure AI a un servidor MCP público (Microsoft Learn) para obtener documentación técnica actualizada.

```python
# --- ARCHIVO: agent.py ---

import os
import time
# Importamos las credenciales y el cliente de proyectos de Azure AI
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient
# Importamos MCPTool para definir herramientas remotas
from azure.ai.projects.models import PromptAgentDefinition, MCPTool
# Importamos las clases para manejar la aprobación de herramientas MCP
from openai.types.responses.response_input_param import McpApprovalResponse, ResponseInputParam
from dotenv import load_dotenv

# Cargar variables de entorno
load_dotenv()
project_endpoint = os.getenv("PROJECT_ENDPOINT")
model_deployment = os.getenv("MODEL_DEPLOYMENT_NAME")

def main():
    # 1. CONEXIÓN AL CLIENTE
    # Usamos un bloque 'with' para gestionar la conexión de forma segura
    with (
        DefaultAzureCredential() as credential,
        AIProjectClient(endpoint=project_endpoint, credential=credential) as project_client,
        project_client.get_openai_client() as openai_client,
    ):
        # 2. DEFINICIÓN DE LA HERRAMIENTA MCP
        # Configuramos la conexión al servidor MCP de Microsoft Learn.
        # 'require_approval="always"' obliga al script a aprobar explícitamente cada uso.
        mcp_tool = MCPTool(
            server_label="api-specs",
            server_url="https://learn.microsoft.com/api/mcp",
            require_approval="always",
        )

        # 3. CREACIÓN DEL AGENTE
        print("Creando agente...")
        agent = project_client.agents.create_version(
            agent_name="MyAgent",
            definition=PromptAgentDefinition(
                model=model_deployment,
                # Instrucciones traducidas:
                instructions="Eres un agente útil que puede usar herramientas MCP para ayudar a los usuarios. Usa las herramientas MCP disponibles para responder preguntas y realizar tareas.",
                tools=[mcp_tool],
            ),
        )
        print(f"Agente creado (id: {agent.id}, nombre: {agent.name}, versión: {agent.version})")

        # 4. HILO DE CONVERSACIÓN
        conversation = openai_client.conversations.create()
        print(f"Conversación creada (id: {conversation.id})")

        # 5. ENVIAR SOLICITUD INICIAL
        # Pedimos algo que requiera buscar en la documentación técnica
        print("Enviando solicitud al agente...")
        response = openai_client.responses.create(
            conversation=conversation.id,
            input="Dame los comandos de Azure CLI para crear una Azure Container App con una identidad administrada.",
            extra_body={"agent": {"name": agent.name, "type": "agent_reference"}},
        )

        # 6. PROCESAR SOLICITUDES DE APROBACIÓN MCP
        # El agente intentará usar la herramienta, pero como requiere aprobación,
        # nos devolverá un 'mcp_approval_request'.
        input_list: ResponseInputParam = []
        for item in response.output:
            if item.type == "mcp_approval_request":
                if item.server_label == "api-specs" and item.id:
                    print(f"Aprobando solicitud de herramienta para: {item.server_label}")
                    # Aprobamos automáticamente la solicitud
                    input_list.append(
                        McpApprovalResponse(
                            type="mcp_approval_response",
                            approve=True,
                            approval_request_id=item.id,
                        )
                    )

        # 7. ENVIAR APROBACIÓN Y OBTENER RESPUESTA FINAL
        if input_list:
            print("Enviando aprobación al agente...")
            response = openai_client.responses.create(
                input=input_list,
                previous_response_id=response.id,
                extra_body={"agent": {"name": agent.name, "type": "agent_reference"}},
            )

        print(f"\nRespuesta del Agente:\n{response.output_text}")

        # 8. LIMPIEZA
        project_client.agents.delete_version(agent_name=agent.name, agent_version=agent.version)
        print("Agente eliminado")

if __name__ == "__main__":
    main()
```

## 2. Servidor MCP Personalizado (`server.py`)

Este script utiliza la librería `fastmcp` para crear un servidor local que expone funciones de inventario y ventas como herramientas.

```python
# --- ARCHIVO: server.py ---

# Importamos FastMCP para crear servidores de herramientas rápidamente
from mcp.server.fastmcp import FastMCP

# Crear una instancia del servidor MCP llamada "Inventory"
mcp = FastMCP(name="Inventory")

# Definir herramienta de inventario
# El decorador @mcp.tool() expone esta función al cliente MCP
@mcp.tool()
def get_inventory_levels() -> dict:
    """Recupera los niveles actuales de inventario para todos los artículos."""
    return {
        "Moisturizer": 6,
        "Shampoo": 8,
        "Body Spray": 28,
        "Conditioner": 14,
        "Face Wash": 4
    }

# Definir herramienta de ventas semanales
@mcp.tool()
def get_weekly_sales() -> dict:
    """Recupera las cifras de ventas de la última semana."""
    return {
        "Moisturizer": 20,
        "Shampoo": 12,
        "Body Spray": 4,
        "Conditioner": 10,
        "Face Wash": 15
    }

# Ejecutar el servidor
# Esto iniciará el servidor y escuchará conexiones (generalmente vía stdio para este lab)
if __name__ == "__main__":
    mcp.run()
```

## 3. Cliente MCP e Integración (`client.py`)

Este script actúa como puente: inicia el servidor MCP localmente, descubre sus herramientas y las conecta a un Agente de Azure AI.

```python
# --- ARCHIVO: client.py ---

import asyncio
import os
import json
from contextlib import AsyncExitStack

# Importamos las librerías del cliente MCP
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client

# Importamos las librerías de Azure AI
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient
from azure.ai.projects.models import PromptAgentDefinition, FunctionTool
from openai.types.responses.response_input_param import FunctionCallOutput

from dotenv import load_dotenv

load_dotenv()
project_endpoint = os.getenv("PROJECT_ENDPOINT")
model_deployment = os.getenv("MODEL_DEPLOYMENT_NAME")

async def main():
    async with AsyncExitStack() as exit_stack:
        # 1. CONFIGURACIÓN DEL SERVIDOR MCP
        # Definimos cómo ejecutar el servidor.py (asumiendo que está en la misma carpeta)
        server_params = StdioServerParameters(
            command="python",
            args=["server.py"],
            env=None
        )

        # 2. INICIAR CLIENTE MCP
        # Iniciamos el servidor y conectamos los canales de entrada/salida (stdio)
        stdio_transport = await exit_stack.enter_async_context(stdio_client(server_params))
        stdio, write = stdio_transport

        # Creamos la sesión del cliente e inicializamos
        session = await exit_stack.enter_async_context(ClientSession(stdio, write))
        await session.initialize()

        # 3. LISTAR HERRAMIENTAS DISPONIBLES
        # Preguntamos al servidor qué herramientas tiene (@mcp.tool)
        response = await session.list_tools()
        tools = response.tools
        print("\nConectado al servidor con herramientas:", [tool.name for tool in tools])

        # 4. CONVERTIR HERRAMIENTAS MCP A FUNCTION TOOLS DE AZURE
        # Creamos las definiciones que entiende el agente de Azure
        mcp_function_tools = []
        for tool in tools:
            function_tool = FunctionTool(
                name=tool.name,
                description=tool.description,
                parameters={
                    "type": "object",
                    "properties": {}, # En este ejemplo simple, no tienen parámetros complejos
                    "additionalProperties": False,
                },
                strict=True
            )
            mcp_function_tools.append(function_tool)

        # 5. CREAR AGENTE
        # Conectamos con Azure
        credential = DefaultAzureCredential()
        project_client = AIProjectClient(endpoint=project_endpoint, credential=credential)
        openai_client = project_client.get_openai_client()

        agent = project_client.agents.create_version(
            agent_name="inventory-agent",
            definition=PromptAgentDefinition(
                model=model_deployment,
                # Instrucciones: Reglas de negocio para reposición y liquidación
                instructions="""
                Eres un asistente de inventario. Aquí tienes algunas pautas generales:
                - Recomienda reabastecer si el inventario < 10 y las ventas semanales > 15
                - Recomienda liquidación si el inventario > 20 y las ventas semanales < 5
                """,
                tools=mcp_function_tools # Pasamos las herramientas dinámicas
            ),
        )

        # ... (El código continúa con el bucle de chat, envío de mensajes y procesamiento de function_calls
        # similar a laboratorios anteriores, pero invocando 'session.call_tool' cuando el agente lo pide) ...

if __name__ == "__main__":
    asyncio.run(main())
```


---
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



---
# Resumen del Módulo

En este módulo, ha aprendido a integrar herramientas externas con el **Servicio de Agentes de Microsoft Foundry (Microsoft Foundry Agent Service)** mediante el **Protocolo de Contexto de Modelo (Model Context Protocol - MCP)**.

Al conectar el agente a un servidor MCP, puede detectar y registrar herramientas dinámicamente en tiempo de ejecución sin necesidad de codificar las APIs manualmente ni volver a implementar (**redeploy**) el agente.

Con un **Cliente MCP**, ha generado envoltorios (**wrappers**) de funciones a partir de las herramientas detectadas y los ha conectado directamente al agente. Esta integración permite que el agente se adapte a conjuntos de herramientas en evolución y ayuda a crear soluciones de inteligencia artificial más flexibles y escalables.

- Crear su propia solución de herramientas MCP con el **Servicio de Agentes de Microsoft Foundry (Microsoft Foundry Agent Service)**.
