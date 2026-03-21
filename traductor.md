
Desarrollar agentes de IA en Azure
 Hogar
Requisitos previos
Instala la extensión Microsoft Foundry para VS Code.
Inicia sesión en Azure y crea un proyecto.
Implementar un modelo
Clona el repositorio de código inicial.
Crea una función para que el agente la utilice.
Conéctate al proyecto Foundry
Defina las herramientas de función
Crea el agente que utiliza las herramientas de función.
Enviar un mensaje al agente y procesar la respuesta.
Procesar las llamadas a funciones y mostrar la respuesta del agente.
Ejecutar la aplicación del agente
Resumen
Limpiar
Utilice una función personalizada en un agente de IA.
En este ejercicio, explorarás la creación de un agente que puede usar funciones personalizadas para completar tareas. El agente actuará como asistente de astronomía, proporcionando información sobre eventos astronómicos y calculando el costo del alquiler de telescopios según la información ingresada por el usuario. Definirás las herramientas de función e implementarás la lógica para procesar las llamadas a funciones realizadas por el agente.

Consejo : El código utilizado en este ejercicio se basa en el SDK de Microsoft Foundry para Python. Puedes desarrollar soluciones similares utilizando los SDK de Microsoft .NET, JavaScript y Java. Consulta las bibliotecas cliente del SDK de Microsoft Foundry para obtener más información.

Este ejercicio debería completarse en aproximadamente 50 minutos.

Nota : Algunas de las tecnologías utilizadas en este ejercicio están en fase de vista previa o en desarrollo activo. Es posible que experimente comportamientos inesperados, advertencias o errores.

Requisitos previos
Antes de comenzar este ejercicio, asegúrese de tener:

Visual Studio Code instalado en su máquina local
Una suscripción activa a Azure
Python 3.13 o posterior instalado
Git instalado en tu máquina local
Instala la extensión Microsoft Foundry para VS Code.
Comencemos por instalar y configurar la extensión de VS Code.

Abre Visual Studio Code.

Seleccione Extensiones en el panel izquierdo (o pulse Ctrl+Mayús+X ).

En la barra de búsqueda, escribe Microsoft Foundry y pulsa Intro.

Seleccione la extensión Microsoft Foundry de Microsoft y haga clic en Instalar .

Una vez finalizada la instalación, compruebe que la extensión aparece en la barra de navegación principal del lado izquierdo de Visual Studio Code.

Inicia sesión en Azure y crea un proyecto.
Ahora te conectarás a tus recursos de Azure y crearás un nuevo proyecto de Microsoft Foundry.

En la barra lateral de VS Code, seleccione el icono de la extensión Microsoft Foundry .

En la vista Recursos, seleccione Iniciar sesión en Azure… y siga las instrucciones de autenticación.

Nota : Esta opción no aparecerá si ya has iniciado sesión.

Crea un nuevo proyecto de Foundry seleccionando el icono + (más) que aparece junto a Recursos en la vista de la extensión de Foundry.

Seleccione su suscripción de Azure en el menú desplegable.

Elija si desea crear un nuevo grupo de recursos o utilizar uno existente:

To create a new resource group:

Select Create new resource group and press Enter
Enter a name for your resource group (e.g., “rg-ai-agents-lab”) and press Enter
Select a location from the available options and press Enter
To use an existing resource group:

Select the resource group you want to use from the list and press Enter
Enter a name for your Foundry project (e.g., “ai-agents-project”) in the textbox and press Enter.

Wait for the project deployment to complete. A popup will appear with the message “Project deployed successfully.”

Deploy a model
In this task, you’ll deploy a model from the Model Catalog to use with your agent.

When the “Project deployed successfully” popup appears, select the Deploy a model button. This opens the Model Catalog.

Tip: You can also access the Model Catalog by selecting the + icon next to Models in the Resources section, or by pressing F1 and running the command Microsoft Foundry: Open Model Catalog.

In the Model Catalog, locate the gpt-4.1 model (you can use the search bar to find it quickly).

Captura de pantalla del catálogo de modelos en la extensión Foundry para Visual Studio Code.

Select Deploy next to the gpt-4.1 model.

Configure the deployment settings:
Deployment name: Enter a name like “gpt-4.1”
Deployment type: Select Global Standard (or Standard if Global Standard is not available)
Model version: Leave as default
Tokens per minute: Leave as default
Select Deploy in Microsoft Foundry in the bottom-left corner.

In the confirmation dialog, select Deploy to deploy the model.

Wait for the deployment to complete. Your deployed model will appear under the Models section in the Resources view.

Right-click the name project deployment and select Copy Project Endpoint. You’ll need this URL to connect your agent to the Foundry project in the next steps.

Captura de pantalla que muestra cómo copiar el punto final del proyecto en la extensión Foundry para VS Code.

Clone the starter code repository
For this exercise, you’ll use starter code that will help you connect to your Foundry project and create an agent that uses custom function tools.

Navigate to the Welcome tab in VS Code (you can open it by selecting Help > Welcome from the menu bar).

Select Clone git repository and enter the URL of the starter code repository: https://github.com/MicrosoftLearning/mslearn-ai-agents.git

Create a new folder and choose Select as Repository Destination, then open the cloned repository when prompted.

In the Explorer view, navigate to the Labfiles/02-agent-custom-tools/Python folder to find the starter code for this exercise.

Right-click on the requirements.txt file and select Open in Integrated Terminal.

In the terminal, enter the following command to install the required Python packages in a virtual environment:

code
 python -m venv labenv
 .\labenv\Scripts\Activate.ps1
 pip install -r requirements.txt
Open the .env file, replace the your_project_endpoint placeholder with the endpoint for your project (copied from the project deployment resource in the Microsoft Foundry extension) and ensure that the MODEL_DEPLOYMENT_NAME variable is set to your model deployment name. Use Ctrl+S to save the file after making these changes.

Now you’re ready to create an AI agent that uses MCP server tools to access external data sources and APIs.

Create a function for the agent to use
Open the functions.py file and review the existing code.

This file includes several functions that you can use as tools for your agent. The functions use sample files located inthe data folder to retrieve information about astronomical events and locations.

Find the comment Determine the next visible astronomical event for a given location and add the following code:

code
# Determine the next visible astronomical event for a given location
def next_visible_event(location: str) -> str:
    """Returns the next visible astronomical event for a location."""
    today = int(datetime.now().strftime("%m%d"))
    loc = location.lower().replace(" ", "_")

    # Retrieve the next event visible from the location, starting with events later this year
    for name, event_type, date, date_str, locs in EVENTS:
        if loc in locs and date >= today:
            return json.dumps({"event": name, "type": event_type, "date": date_str, "visible_from": sorted(locs)})

    return json.dumps({"message": f"No upcoming events found for {location}."})
This function checks the sample events data to find the next astronomical event that is visible from a specified location, and returns the event details as a JSON string. Next, let’s create an agent that can use this function.

Connect to the Foundry project
Open the agent.py file.

Tip: As you add code, be sure to maintain the correct indentation. Use the comment indentation levels as a guide.

Find the comment Add references and add the following code to import the classes you’ll need to build an Azure AI agent that uses a function tool:

code
# Add references
from azure.ai.projects import AIProjectClient
from azure.ai.projects.models import FunctionTool
from azure.identity import DefaultAzureCredential
from azure.ai.projects.models import PromptAgentDefinition, FunctionTool
from openai.types.responses.response_input_param import FunctionCallOutput, ResponseInputParam
from functions import next_visible_event, calculate_observation_cost, generate_observation_report
Notice that the functions you defined in the functions.py file are imported so they can be used as tools for the agent.

Find the comment Connect to the project client and add the following code:

code
 # Connect to the project client
 with (
     DefaultAzureCredential() as credential,
     AIProjectClient(endpoint=project_endpoint, credential=credential) as project_client,
     project_client.get_openai_client() as openai_client,
 ):
Define the function tools
In this task, you’ll define each of the function tools that the agent can use. The parameters for each function tool are defined using a JSON schema, which specifies the name, type, description, and other attributes for each parameter of the function.

Find the comment Define the event function tool and add the following code:

code
# Define the event function tool
event_tool = FunctionTool(
    name="next_visible_event",
    description="Get the next visible event in a given location.",
    parameters={
        "type": "object",
        "properties": {
            "location": {
                "type": "string",
                "description": "continent to find the next visible event in (e.g. 'north_america', 'south_america', 'australia')",
            },
        },
        "required": ["location"],
        "additionalProperties": False,
    },
    strict=True,
)
Find the comment Define the observation cost function tool and add the following code:

code
# Define the observation cost function tool
cost_tool = FunctionTool(
    name="calculate_observation_cost",
    description="Calculate the cost of an observation based on the telescope tier, number of hours, and priority level.",
    parameters={
        "type": "object",
        "properties": {
            "telescope_tier": {
                "type": "string",
                "description": "the tier of the telescope (e.g. 'standard', 'advanced', 'premium')",
            },
            "hours": {
                "type": "number",
                "description": "the number of hours for the observation",
            },
            "priority": {
                "type": "string",
                "description": "the priority level of the observation (e.g. 'low', 'normal', 'high')",
            },
        },
        "required": ["telescope_tier", "hours", "priority"],
        "additionalProperties": False,
    },
    strict=True,
)
Find the comment Define the observation report generation function tool and add the following code:

code
# Define the observation report generation function tool
report_tool = FunctionTool(
    name="generate_observation_report",
    description="Generate a report summarizing an astronomical observation",
    parameters={
        "type": "object",
        "properties": {
            "event_name": {
                "type": "string",
                "description": "the name of the astronomical event being observed",
            },
            "location": {
                "type": "string",
                "description": "the location of the observer",
            },
            "telescope_tier": {
                "type": "string",
                "description": "the tier of the telescope used for the observation (e.g. 'standard', 'advanced', 'premium')",
            },
            "hours": {
                "type": "number",
                "description": "the number of hours the telescope was used for the observation",
            },
            "priority": {
                "type": "string",
                "description": "the priority level of the observation (e.g. 'low', 'normal', 'high')",
            },
            "observer_name": {
                "type": "string",
                "description": "the name of the person who conducted the observation",
            },                   
        },
        "required": ["event_name", "location", "telescope_tier", "hours", "priority", "observer_name"],
        "additionalProperties": False,
    },
    strict=True,
)
Create the agent that uses the function tools
Now that you’ve defined the function tools, you can create an agent that can use those tools to complete tasks.

Find the comment Create a new agent with the function tools and add the following code:

code
# Create a new agent with the function tools
agent = project_client.agents.create_version(
    agent_name="astronomy-agent",
    definition=PromptAgentDefinition(
        model=model_deployment,
        instructions=
            """You are an astronomy observations assistant that helps users find 
            information about astronomical events and calculate telescope rental costs. 
            Use the available tools to assist users with their inquiries.""",
        tools=[event_tool, cost_tool, report_tool],
    ),
)
Send a message to the agent and process the response
Now that you’ve created the agent with the function tools, you can send messages to the agent and process its responses.

Find the comment Create a thread for the chat session and add the following code:

code
# Create a thread for the chat session
conversation = openai_client.conversations.create()
This code creates the chat session with the agent.

Find the comment Create an input list to hold function call outputs to send back to the model and add the following code:

code
# Create a list to hold function call outputs that will be sent back as input to the agent
input_list: ResponseInputParam = []
Find the comment Send a prompt to the agent and add the following code:

code
# Send a prompt to the agent
openai_client.conversations.items.create(
    conversation_id=conversation.id,
    items=[{"type": "message", "role": "user", "content": user_input}],
)
Find the comment Retrieve the agent’s response, which may include function calls and add the following code:

code
# Retrieve the agent's response, which may include function calls
response = openai_client.responses.create(
    conversation=conversation.id,
    extra_body={"agent": {"name": agent.name, "type": "agent_reference"}},
    input=input_list,
)

# Check the run status for failures
if response.status == "failed":
    print(f"Response failed: {response.error}")
In this code, you send a user prompt to the agent and retrieve the response. You also check if the response indicates a failure and print the error if so.

Process function calls and display the agent’s response
Find the comment Process function calls and add the following code to handle any function calls made by the agent:

code
# Process function calls
for item in response.output:
    if item.type == "function_call":
        # Retrieve the matching function tool
        function_name = item.name
        result = None
        if item.name == "next_visible_event":
            result = next_visible_event(**json.loads(item.arguments))
        elif item.name == "calculate_observation_cost":
            result = calculate_observation_cost(**json.loads(item.arguments))
        elif item.name == "generate_observation_report":
            result = generate_observation_report(**json.loads(item.arguments))
                
        # Append the output text
        input_list.append(
            FunctionCallOutput(
                type="function_call_output",
                call_id=item.call_id,
                output=result,
            )
        )
This code iterates through the items in the agent’s response to check for any function calls. If a function call is found, it retrieves the corresponding function tool, executes the function with the provided arguments, and appends the result to the input list that will be sent back to the agent.

Find the comment Send function call outputs back to the model and retrieve a response and add the following code:

code
# Send function call outputs back to the model and retrieve a response
if input_list:
    response = openai_client.responses.create(
        input=input_list,
        previous_response_id=response.id,
        extra_body={"agent": {"name": agent.name, "type": "agent_reference"}},
    )
# Display the agent's response
print(f"AGENT: {response.output_text}")
This code checks if there are any function call outputs in the input list, and if so, it sends them back to the agent as input to retrieve an updated response. Finally, it prints the agent’s response.

Find the comment Delete the conversation when done and add the following code:

code
# Delete the agent when done
 project_client.agents.delete_version(agent_name=agent.name, agent_version=agent.version)
 print("Deleted agent.")
Review the complete code you’ve added to the file. It should now include sections that:
Import necessary libraries
Connect to the Foundry project and OpenAI client
Define function tools for the agent to use
Create an agent with those function tools
Send a message to the agent and retrieve the response
Process any function calls made by the agent and send the outputs back to the agent
Display the agent’s response
Delete the agent when done
Save the code file (CTRL+S) when you have finished.
Run the agent application
In the integrated terminal, enter the following command to run the application:

code
python agent.py
When prompted, enter a prompt such as:

code
Find me the next event I can see from South America and give me the cost for 5 hours of premium telescope time at normal priority.
Notice that this prompt asks the agent to use both of the function tools you defined: next_visible_event and calculate_observation_cost. The agent is able to invoke both functions in the same conversation turn, and use the outputs from those function calls to provide a helpful response to the user.

Consejo : Si la aplicación falla porque se ha superado el límite de solicitudes, espere unos segundos e inténtelo de nuevo. Si su suscripción no tiene suficiente cuota disponible, es posible que el modelo no pueda responder.

Deberías ver una salida similar a la siguiente:

código
 AGENT: The next astronomical event you can observe from South America is the Jupiter-Venus Conjunction, taking place on May 1st.
 The cost for 5 hours of premium telescope time at normal priority for this observation will be $1,875. 
Introduzca una solicitud de seguimiento para generar un informe de observación, como por ejemplo:

código
 Generate that information in a report for Bellows College.
Deberías ver una respuesta similar a la siguiente:

código
 AGENT: Here is your report for Bellows College:

 - Next visible astronomical event: Jupiter-Venus Conjunction
 - Date: May 1st
 - Visible from: South America
 - Observation details:
     - Telescope tier: Premium
     - Duration: 5 hours
     - Priority: Normal
 - Observation cost: $1,875

 A formal report has been generated for Bellows College.
En el explorador de archivos, verá que report-<event-type>.txtse ha creado un nuevo archivo con el nombre especificado, que contiene el informe generado. Puede abrir este archivo para ver el contenido del informe.

Introduzca quitpara salir de la aplicación.

También puedes usarlo deactivatepara salir del entorno virtual de Python en la terminal.

Resumen
En este ejercicio, creaste un agente de IA que utiliza herramientas de funciones personalizadas para recuperar información y realizar cálculos a partir de las indicaciones del usuario. Definiste las herramientas de funciones con esquemas JSON para especificar sus parámetros e implementaste la lógica para procesar las llamadas a funciones realizadas por el agente. Luego, ejecutaste la aplicación e interactuaste con el agente para observar cómo utilizaba las herramientas de funciones para proporcionar respuestas útiles. ¡Excelente trabajo!

Limpiar
Cuando hayas terminado de explorar la extensión Foundry para VS Code, deberías limpiar los recursos para evitar incurrir en costes innecesarios de Azure.

Elimina tu modelo
En VS Code, actualice la vista de Recursos de Azure .

Amplíe la subsección Modelos .

Haz clic con el botón derecho en el modelo implementado y selecciona Eliminar .

Eliminar el grupo de recursos
Abra el portal de Azure .

Navegue hasta el grupo de recursos que contiene sus recursos de Microsoft Foundry.

Seleccione Eliminar grupo de recursos y confirme la eliminación.

© 2025 Microsoft |  Privacidad  |  Privacidad de la salud del consumidor  |  Condiciones de uso  |  Marcas registradas  |   Fuente