# Desarrollar un agente de IA con herramientas del Protocolo de Contexto de Modelo (MCP)

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
