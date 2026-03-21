# Laboratorio: Uso de una función personalizada en un agente de IA

En este ejercicio, explorarás la creación de un agente que puede usar funciones personalizadas para completar tareas. El agente actuará como asistente de astronomía, proporcionando información sobre eventos astronómicos y calculando el costo del alquiler de telescopios según la información ingresada por el usuario. Definirás las herramientas de función e implementarás la lógica para procesar las llamadas a funciones realizadas por el agente.

> **Consejo:** El código utilizado en este ejercicio se basa en el SDK de Microsoft Foundry para Python.

Este ejercicio debería completarse en aproximadamente 50 minutos.

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

Para este ejercicio, utilizarás un código de inicio que te ayudará a conectarte a tu proyecto de Foundry y crear un agente que use herramientas de funciones personalizadas.

1.  Navegue a la pestaña **Bienvenido** en VS Code (puede abrirla seleccionando **Ayuda > Bienvenido** en la barra de menú).
2.  Seleccione **Clonar repositorio git** e introduzca la URL del repositorio de código inicial: `https://github.com/MicrosoftLearning/mslearn-ai-agents.git`.
3.  Cree una nueva carpeta y elija **Seleccionar como destino del repositorio**, luego abra el repositorio clonado cuando se le solicite.
4.  En la vista Explorador, navegue a la carpeta `Labfiles/02-agent-custom-tools/Python` para encontrar el código de inicio para este ejercicio.
5.  Haz clic con el botón derecho en el archivo `requirements.txt` y selecciona **Abrir en terminal integrado**.
6.  En la terminal, introduce el siguiente comando para instalar los paquetes de Python necesarios en un entorno virtual:
    ```bash
    python -m venv labenv
    .\labenv\Scripts\Activate.ps1
    pip install -r requirements.txt
    ```
7.  Abre el archivo `.env`, reemplaza el marcador de posición `your_project_endpoint` con el **Punto de conexión del proyecto** (copiado del recurso de implementación del proyecto en la extensión Microsoft Foundry) y asegúrate de que la variable `MODEL_DEPLOYMENT_NAME` esté configurada con el nombre de tu implementación de modelo. Usa `Ctrl+S` para guardar el archivo después de hacer estos cambios.

Ahora estás listo para crear un agente de IA que use herramientas de servidor MCP para acceder a fuentes de datos externas y API.

## Crea una función para que el agente la utilice

1.  Abre el archivo `functions.py` y revisa el código existente.
    Este archivo incluye varias funciones que puedes usar como herramientas para tu agente. Las funciones usan archivos de muestra ubicados en la carpeta `data` para recuperar información sobre eventos astronómicos y ubicaciones.
2.  Encuentra el comentario `Determine the next visible astronomical event for a given location` y agrega el siguiente código:

    ```python
    # Determinar el próximo evento astronómico visible para una ubicación dada
    def next_visible_event(location: str) -> str:
        """Devuelve el próximo evento astronómico visible para una ubicación."""
        today = int(datetime.now().strftime("%m%d"))
        loc = location.lower().replace(" ", "_")

        # Recuperar el próximo evento visible desde la ubicación, comenzando con eventos más tarde este año
        for name, event_type, date, date_str, locs in EVENTS:
            if loc in locs and date >= today:
                return json.dumps({"event": name, "type": event_type, "date": date_str, "visible_from": sorted(locs)})

        return json.dumps({"message": f"No upcoming events found for {location}."})
    ```

Esta función verifica los datos de eventos de muestra para encontrar el próximo evento astronómico que sea visible desde una ubicación específica y devuelve los detalles del evento como una cadena JSON. A continuación, vamos a crear un agente que pueda usar esta función.

## Conéctate al proyecto Foundry

1.  Abre el archivo `agent.py`.
    > **Consejo:** A medida que agregues código, asegúrate de mantener la sangría correcta. Usa los niveles de sangría de los comentarios como guía.
2.  Encuentra el comentario `Add references` y agrega el siguiente código para importar las clases que necesitarás para construir un agente de Azure AI que use una herramienta de función:

    ```python
    # Agregar referencias
    from azure.ai.projects import AIProjectClient
    from azure.ai.projects.models import FunctionTool
    from azure.identity import DefaultAzureCredential
    from azure.ai.projects.models import PromptAgentDefinition, FunctionTool
    from openai.types.responses.response_input_param import FunctionCallOutput, ResponseInputParam
    from functions import next_visible_event, calculate_observation_cost, generate_observation_report
    ```

Observa que las funciones que definiste en el archivo `functions.py` se importan para que puedan usarse como herramientas para tu agente.

3.  Encuentra el comentario `Connect to the project client` y agrega el siguiente código:

    ```python
    # Conectar al cliente del proyecto
    with (
        DefaultAzureCredential() as credential,
        AIProjectClient(endpoint=project_endpoint, credential=credential) as project_client,
        project_client.get_openai_client() as openai_client,
    ):
    ```

## Defina las herramientas de función

En esta tarea, definirás cada una de las herramientas de función que el agente puede usar. Los parámetros para cada herramienta de función se definen usando un esquema JSON, que especifica el nombre, tipo, descripción y otros atributos para cada parámetro de la función.

1.  Encuentra el comentario `Define the event function tool` y agrega el siguiente código:

    ```python
    # Definir la herramienta de función de evento
    event_tool = FunctionTool(
        name="next_visible_event",
        description="Obtener el próximo evento visible en una ubicación dada.",
        parameters={
            "type": "object",
            "properties": {
                "location": {
                    "type": "string",
                    "description": "continente para encontrar el próximo evento visible en (ej. 'north_america', 'south_america', 'australia')",
                },
            },
            "required": ["location"],
            "additionalProperties": False,
        },
        strict=True,
    )
    ```

2.  Encuentra el comentario `Define the observation cost function tool` y agrega el siguiente código:

    ```python
    # Definir la herramienta de función de costo de observación
    cost_tool = FunctionTool(
        name="calculate_observation_cost",
        description="Calcular el costo de una observación basado en el nivel del telescopio, número de horas y nivel de prioridad.",
        parameters={
            "type": "object",
            "properties": {
                "telescope_tier": {
                    "type": "string",
                    "description": "el nivel del telescopio (ej. 'standard', 'advanced', 'premium')",
                },
                "hours": {
                    "type": "number",
                    "description": "el número de horas para la observación",
                },
                "priority": {
                    "type": "string",
                    "description": "el nivel de prioridad de la observación (ej. 'low', 'normal', 'high')",
                },
            },
            "required": ["telescope_tier", "hours", "priority"],
            "additionalProperties": False,
        },
        strict=True,
    )
    ```

3.  Encuentra el comentario `Define the observation report generation function tool` y agrega el siguiente código:

    ```python
    # Definir la herramienta de función de generación de informe de observación
    report_tool = FunctionTool(
        name="generate_observation_report",
        description="Generar un informe resumiendo una observación astronómica",
        parameters={
            "type": "object",
            "properties": {
                "event_name": {
                    "type": "string",
                    "description": "el nombre del evento astronómico siendo observado",
                },
                "location": {
                    "type": "string",
                    "description": "la ubicación del observador",
                },
                "telescope_tier": {
                    "type": "string",
                    "description": "el nivel del telescopio usado para la observación (ej. 'standard', 'advanced', 'premium')",
                },
                "hours": {
                    "type": "number",
                    "description": "el número de horas que el telescopio fue usado para la observación",
                },
                "priority": {
                    "type": "string",
                    "description": "el nivel de prioridad de la observación (ej. 'low', 'normal', 'high')",
                },
                "observer_name": {
                    "type": "string",
                    "description": "el nombre de la persona que realizó la observación",
                },
            },
            "required": ["event_name", "location", "telescope_tier", "hours", "priority", "observer_name"],
            "additionalProperties": False,
        },
        strict=True,
    )
    ```

## Crea el agente que utiliza las herramientas de función

Ahora que has definido las herramientas de función, puedes crear un agente que pueda usar esas herramientas para completar tareas.

1.  Encuentra el comentario `Create a new agent with the function tools` y agrega el siguiente código:

    ```python
    # Crear un nuevo agente con las herramientas de función
    agent = project_client.agents.create_version(
        agent_name="astronomy-agent",
        definition=PromptAgentDefinition(
            model=model_deployment,
            instructions=
                """Eres un asistente de observaciones astronómicas que ayuda a los usuarios a encontrar
                información sobre eventos astronómicos y calcular costos de alquiler de telescopios.
                Usa las herramientas disponibles para asistir a los usuarios con sus consultas.""",
            tools=[event_tool, cost_tool, report_tool],
        ),
    )
    ```

## Enviar un mensaje al agente y procesar la respuesta

Ahora que has creado el agente con las herramientas de función, puedes enviar mensajes al agente y procesar sus respuestas.

1.  Encuentra el comentario `Create a thread for the chat session` y agrega el siguiente código:

    ```python
    # Crear un hilo para la sesión de chat
    conversation = openai_client.conversations.create()
    ```

    Este código crea la sesión de chat con el agente.

2.  Encuentra el comentario `Create an input list to hold function call outputs to send back to the model` y agrega el siguiente código:

    ```python
    # Crear una lista para mantener las salidas de llamadas a funciones que serán enviadas de vuelta como entrada al agente
    input_list: ResponseInputParam = []
    ```

3.  Encuentra el comentario `Send a prompt to the agent` y agrega el siguiente código:

    ```python
    # Enviar un prompt al agente
    openai_client.conversations.items.create(
        conversation_id=conversation.id,
        items=[{"type": "message", "role": "user", "content": user_input}],
    )
    ```

4.  Encuentra el comentario `Retrieve the agent’s response, which may include function calls` y agrega el siguiente código:

    ```python
    # Recuperar la respuesta del agente, la cual puede incluir llamadas a funciones
    response = openai_client.responses.create(
        conversation=conversation.id,
        extra_body={"agent": {"name": agent.name, "type": "agent_reference"}},
        input=input_list,
    )

    # Verificar el estado de la ejecución para fallos
    if response.status == "failed":
        print(f"La respuesta falló: {response.error}")
    ```

En este código, envías un prompt de usuario al agente y recuperas la respuesta. También verificas si la respuesta indica un fallo e imprimes el error si es así.

## Procesar las llamadas a funciones y mostrar la respuesta del agente

1.  Encuentra el comentario `Process function calls` y agrega el siguiente código para manejar cualquier llamada a función realizada por el agente:

    ```python
    # Procesar llamadas a funciones
    for item in response.output:
        if item.type == "function_call":
            # Recuperar la herramienta de función correspondiente
            function_name = item.name
            result = None
            if item.name == "next_visible_event":
                result = next_visible_event(**json.loads(item.arguments))
            elif item.name == "calculate_observation_cost":
                result = calculate_observation_cost(**json.loads(item.arguments))
            elif item.name == "generate_observation_report":
                result = generate_observation_report(**json.loads(item.arguments))

            # Anexar el texto de salida
            input_list.append(
                FunctionCallOutput(
                    type="function_call_output",
                    call_id=item.call_id,
                    output=result,
                )
            )
    ```

Este código itera a través de los elementos en la respuesta del agente para verificar si hay llamadas a funciones. Si se encuentra una llamada a función, recupera la herramienta de función correspondiente, ejecuta la función con los argumentos proporcionados y anexa el resultado a la lista de entrada que se enviará de vuelta al agente.

2.  Encuentra el comentario `Send function call outputs back to the model and retrieve a response` y agrega el siguiente código:

    ```python
    # Enviar salidas de llamadas a funciones de vuelta al modelo y recuperar una respuesta
    if input_list:
        response = openai_client.responses.create(
            input=input_list,
            previous_response_id=response.id,
            extra_body={"agent": {"name": agent.name, "type": "agent_reference"}},
        )
    # Mostrar la respuesta del agente
    print(f"AGENTE: {response.output_text}")
    ```

Este código verifica si hay salidas de llamadas a funciones en la lista de entrada, y si es así, las envía de vuelta al agente como entrada para recuperar una respuesta actualizada. Finalmente, imprime la respuesta del agente.

3.  Encuentra el comentario `Delete the conversation when done` y agrega el siguiente código:

    ```python
    # Eliminar el agente cuando termine
    project_client.agents.delete_version(agent_name=agent.name, agent_version=agent.version)
    print("Agente eliminado.")
    ```

Revise el código completo que ha agregado al archivo. Ahora debería incluir secciones que:

- Importen las bibliotecas necesarias.
- Se conecten al proyecto Foundry y al cliente OpenAI.
- Definan herramientas de función para que las use el agente.
- Creen un agente con esas herramientas de función.
- Envíen un mensaje al agente y recuperen la respuesta.
- Procesen cualquier llamada a función realizada por el agente y envíen las salidas de vuelta al agente.
- Muestren la respuesta del agente.
- Eliminen el agente cuando termine.

Guarde el archivo de código (`CTRL+S`) cuando haya terminado.

## Ejecutar la aplicación del agente

1.  En la terminal integrada, introduzca el siguiente comando para ejecutar la aplicación:

    ```bash
    python agent.py
    ```

2.  Cuando se le solicite, introduzca un prompt como:

    ```
    Find me the next event I can see from South America and give me the cost for 5 hours of premium telescope time at normal priority.
    ```

    _(Encuéntrame el próximo evento que pueda ver desde Sudamérica y dame el costo por 5 horas de tiempo de telescopio premium con prioridad normal.)_

    Observe que este prompt le pide al agente que use ambas herramientas de función que definió: `next_visible_event` y `calculate_observation_cost`. El agente es capaz de invocar ambas funciones en el mismo turno de conversación y usar las salidas de esas llamadas a funciones para proporcionar una respuesta útil al usuario.

    > **Consejo:** Si la aplicación falla porque se ha superado el límite de solicitudes, espere unos segundos e inténtelo de nuevo. Si su suscripción no tiene suficiente cuota disponible, es posible que el modelo no pueda responder.

    Deberías ver una salida similar a la siguiente:

    ```
    AGENTE: The next astronomical event you can observe from South America is the Jupiter-Venus Conjunction, taking place on May 1st.
    The cost for 5 hours of premium telescope time at normal priority for this observation will be $1,875.
    ```

    _(AGENTE: El próximo evento astronómico que puedes observar desde Sudamérica es la Conjunción Júpiter-Venus, que tendrá lugar el 1 de mayo. El costo por 5 horas de tiempo de telescopio premium con prioridad normal para esta observación será de $1,875.)_

3.  Introduzca una solicitud de seguimiento para generar un informe de observación, como por ejemplo:

    ```
    Generate that information in a report for Bellows College.
    ```

    _(Genera esa información en un informe para Bellows College.)_

    Deberías ver una respuesta similar a la siguiente:

    ```
    AGENTE: Here is your report for Bellows College:

    - Next visible astronomical event: Jupiter-Venus Conjunction
    - Date: May 1st
    - Visible from: South America
    - Observation details:
        - Telescope tier: Premium
        - Duration: 5 hours
        - Priority: Normal
    - Observation cost: $1,875

    A formal report has been generated for Bellows College.
    ```

4.  En el explorador de archivos, verá que se ha creado un nuevo archivo `report-<event-type>.txt` con el nombre especificado, que contiene el informe generado. Puede abrir este archivo para ver el contenido del informe.

5.  Introduzca `quit` para salir de la aplicación.

6.  También puedes usar `deactivate` para salir del entorno virtual de Python en la terminal.

## Limpiar

Cuando hayas terminado de explorar la extensión Foundry para VS Code, deberías limpiar los recursos para evitar incurrir en costes innecesarios de Azure.

1.  **Elimina tu modelo:** En VS Code, actualice la vista de **Recursos de Azure**. Amplíe la subsección **Modelos**. Haz clic con el botón derecho en el modelo implementado y selecciona **Eliminar**.
2.  **Eliminar el grupo de recursos:** Abra el portal de Azure. Navegue hasta el grupo de recursos que contiene sus recursos de Microsoft Foundry. Seleccione **Eliminar grupo de recursos** y confirme la eliminación.
