# Integración de herramientas personalizadas en el agente

## Introducción

El **Servicio de Agentes de Microsoft Foundry (Microsoft Foundry Agent Service)** ofrece una manera fluida de crear agentes sin requerir una amplia experiencia en inteligencia artificial o aprendizaje automático. Una de sus capacidades más potentes es el uso de **Herramientas (Tools)**, que otorgan al agente la funcionalidad para ejecutar acciones en su nombre.

Aunque el servicio proporciona herramientas integradas para acceder a conocimientos y generar código, a menudo los agentes necesitan completar tareas específicas de negocio que van más allá de las capacidades estándar. Para estos casos, puede dotar al agente de una **Herramienta personalizada (Custom tool)** basada en su propio código o conectada a una API de terceros.

### Escenario de Ejemplo: Sector Minorista

Imagine que trabaja en una empresa minorista que tiene dificultades para gestionar eficazmente las consultas de los clientes. El equipo de soporte está saturado con preguntas repetitivas, lo que causa retrasos y reduce la satisfacción del cliente.

Utilizando **Foundry Agent Service** con herramientas personalizadas, puede crear un agente de soporte a medida. Este agente no solo respondería preguntas frecuentes (FAQ), sino que podría tener herramientas específicas para:

- Buscar el estado de pedidos de clientes en la base de datos interna.
- Procesar devoluciones simples de forma autónoma.

Esto libera al equipo humano para centrarse en problemas más complejos y estratégicos.

En este módulo, aprenderá a desarrollar e integrar herramientas personalizadas en el servicio Foundry Agent para mejorar la productividad, aumentar la precisión y crear soluciones adaptadas a necesidades específicas.

---

# ¿Por qué usar herramientas personalizadas?

El **Servicio de Agentes de Microsoft Foundry (Microsoft Foundry Agent Service)** ofrece una plataforma eficaz para integrar herramientas personalizadas, lo que permite a las empresas lograr una mayor eficacia en sus operaciones.

## Beneficios Principales

Las herramientas personalizadas (**Custom tools**) optimizan significativamente los flujos de trabajo mediante:

- **Productividad mejorada:** Automatizan tareas repetitivas y optimizan procesos.
- **Precisión mejorada:** Proporcionan resultados consistentes, reduciendo la probabilidad de error humano.
- **Soluciones a medida:** Abordan necesidades empresariales específicas que las herramientas genéricas no cubren.

La adición de herramientas pone a disposición del agente funcionalidades específicas. El agente decide cuándo utilizar una herramienta basándose en la solicitud del usuario (Prompt).

### Ejemplo de Flujo de Trabajo

Considere cómo un agente podría usar una herramienta personalizada para obtener datos del clima:

1.  **Solicitud:** Un usuario pregunta sobre las condiciones meteorológicas en una estación de esquí.
2.  **Decisión:** El agente determina que tiene acceso a una herramienta capaz de consultar una API externa para obtener esa información y decide llamarla.
3.  **Respuesta:** La herramienta devuelve el informe meteorológico y el agente procesa esa información para responder al usuario.

---

## Escenarios Comunes de Uso

Las herramientas personalizadas permiten ampliar las capacidades de los agentes en diversos sectores:

### Automatización del Soporte al Cliente

- **Escenario:** Integración con un sistema de gestión de relaciones con el cliente (**CRM**).
- **Funcionalidad:** El agente puede recuperar el historial de pedidos, procesar reembolsos y dar actualizaciones de envío en tiempo real.
- **Resultado:** Resolución más rápida de consultas y mayor satisfacción del cliente.

### Gestión de Inventario

- **Escenario:** Vinculación con un sistema de gestión de inventario.
- **Funcionalidad:** Comprobar niveles de existencias (stock), predecir necesidades de reposición con datos históricos y realizar pedidos a proveedores automáticamente.
- **Resultado:** Operaciones de cadena de suministro optimizadas.

### Programación de Citas Médicas (Sector Salud)

- **Escenario:** Integración con sistemas de registros de pacientes y programación.
- **Funcionalidad:** Acceder a registros, sugerir horarios disponibles y enviar recordatorios.
- **Resultado:** Reducción de la carga administrativa y mejor experiencia del paciente.

### Soporte de TI (Helpdesk)

- **Escenario:** Conexión con sistemas de tickets y bases de conocimiento (**Knowledge Base**).
- **Funcionalidad:** Solucionar problemas técnicos comunes, escalar problemas complejos y rastrear el estado de los tickets.
- **Resultado:** Menor tiempo de inactividad y mayor productividad.

### Educación y Aprendizaje (E-learning)

- **Escenario:** Conexión con un sistema de gestión de aprendizaje (**LMS**).
- **Funcionalidad:** Recomendar cursos, seguir el progreso del alumno y responder dudas sobre el contenido.
- **Resultado:** Experiencia de aprendizaje personalizada y gestión administrativa simplificada.

---

# Opciones de Implementación de Herramientas Personalizadas

El **Servicio de Agentes de Microsoft Foundry (Microsoft Foundry Agent Service)** proporciona flexibilidad para integrar funcionalidades externas mediante diversas opciones de herramientas personalizadas. Esto facilita la interoperabilidad con infraestructuras existentes y servicios web.

## Tipos de Herramientas Disponibles

### 1. Funciones Personalizadas (Custom Functions)

Permiten describir la estructura de una función dentro de la definición del agente.

- **Funcionamiento:** El agente determina dinámicamente qué función llamar y con qué argumentos, basándose en la conversación. Luego, su aplicación ejecuta el código y devuelve el resultado al agente.
- **Uso ideal:** Para integrar lógica de negocio específica y flujos de trabajo personalizados en el lenguaje de programación de su elección.

### 2. Azure Functions

Permiten crear aplicaciones inteligentes impulsadas por eventos con una sobrecarga de infraestructura mínima.

- **Características:** Utilizan **desencadenadores (triggers)** para determinar cuándo ejecutarse y **enlaces (bindings)** para simplificar la conexión con fuentes de datos de entrada y salida.
- **Uso ideal:** Ejecución de código sin servidor (serverless) para tareas discretas invocadas por el agente.

### 3. Herramientas de Especificación OpenAPI

Permiten conectar el agente a una API externa estándar utilizando una especificación **OpenAPI 3.0**.

- **Beneficios:** Proporciona una integración estandarizada y escalable. La especificación describe los endpoints HTTP, lo que permite al agente entender cómo interactuar con la API automáticamente.
- **Uso ideal:** Conexión con servicios RESTful existentes y APIs públicas o privadas bien documentadas.

### 4. Azure Logic Apps

Ofrece una solución de **poco código (low-code)** o sin código para orquestar flujos de trabajo.

- **Funcionamiento:** Permite conectar aplicaciones, datos y servicios mediante un diseñador visual.
- **Uso ideal:** Integración de sistemas empresariales dispares y automatización de procesos sin escribir código complejo.

Esta variedad de opciones asegura que los desarrolladores puedan extender las capacidades de sus agentes utilizando la herramienta que mejor se adapte a su arquitectura y habilidades técnicas.

---

# Integración de Herramientas Personalizadas

Las herramientas personalizadas se pueden definir de varias maneras, dependiendo de la arquitectura de su solución. A continuación se detallan los métodos principales para integrar estas capacidades en sus agentes.

## 1. Llamada a Funciones (Function Calling)

La **Llamada a funciones (Function Calling)** permite a los agentes ejecutar funciones de código predefinidas dinámicamente basándose en la entrada del usuario. Es ideal para tareas que se pueden resolver localmente mediante código, como recuperar datos específicos o realizar cálculos.

### Ejemplo: Definición y Registro de una Función

Primero, se define la función en Python que realizará la tarea.

```python
import json

def recent_snowfall(location: str) -> str:
    """
    Obtiene los totales de nevadas recientes para una ubicación dada.
    :param location: El nombre de la ciudad.
    :return: Detalles de la nevada como una cadena JSON.
    """
    mock_snow_data = {"Seattle": "0 inches", "Denver": "2 inches"}
    snow = mock_snow_data.get(location, "Datos no disponibles.")
    return json.dumps({"location": location, "snowfall": snow})
```

Luego, se registra la función como una herramienta dentro de la definición del agente utilizando el SDK de Azure AI.

```python
# Definir una herramienta de función para que el modelo la use
function_tool = FunctionTool(
    name="recent_snowfall",
    parameters={
        "type": "object",
        "properties": {
            "location": {"type": "string", "description": "El nombre de la ciudad para consultar la nevada."},
        },
        "required": ["location"],
        "additionalProperties": False
    },
    description="Obtener los totales de nevadas recientes para una ubicación dada.",
    strict=True,
)

# Agregar la herramienta de función a una lista de herramientas para el agente
tools: list[Tool] = [function_tool]

# Crear su agente con el conjunto de herramientas
agent = project_client.agents.create_version(
    name="snowfall-agent",
    definition=PromptAgentDefinition(
        model="gpt-4.1",
        instructions="Eres un asistente meteorológico que rastrea las nevadas. Usa las funciones proporcionadas para responder preguntas.",
        tools=tools,
    )
)
```

Ahora, el agente invocará automáticamente `recent_snowfall` cuando el prompt del usuario requiera esa información.

---

## 2. Azure Functions

**Azure Functions** proporciona computación sin servidor (**serverless**) ideal para flujos de trabajo basados en eventos. Los agentes pueden utilizar colas de almacenamiento para enviar solicitudes a una Azure Function y recibir respuestas de manera asíncrona.

### Ejemplo: Integración mediante Colas

Se define la herramienta especificando los enlaces de entrada y salida de la cola de almacenamiento.

```python
tool = AzureFunctionTool(
    azure_function=AzureFunctionDefinition(
        input_binding=AzureFunctionBinding(
            storage_queue=AzureFunctionStorageQueue(
                queue_name="NOMBRE_COLA_ENTRADA",
                queue_service_endpoint="ENDPOINT_SERVICIO_COLAS",
            )
        ),
        output_binding=AzureFunctionBinding(
            storage_queue=AzureFunctionStorageQueue(
                queue_name="NOMBRE_COLA_SALIDA",
                queue_service_endpoint="ENDPOINT_SERVICIO_COLAS",
            )
        ),
        function=AzureFunctionDefinitionFunction(
            name="queue_trigger",
            description="Obtener el clima para una ubicación dada",
            parameters={
                "type": "object",
                "properties": {"location": {"type": "string", "description": "ubicación para determinar el clima"}},
            },
        ),
    )
)

agent = project_client.agents.create_version(
    agent_name="MyAgent",
    definition=PromptAgentDefinition(
        model="gpt-4.1",
        instructions="Eres un asistente meteorológico útil. Usa la Azure Function proporcionada para obtener información del clima de una ubicación cuando sea necesario.",
        tools=[tool],
    ),
)
```

---

## 3. Especificación OpenAPI

Las herramientas definidas por **OpenAPI** permiten a los agentes interactuar con APIs REST externas estandarizadas. El servicio soporta especificaciones **OpenAPI 3.0**.

> **Nota:** Actualmente se admiten tres tipos de autenticación: Anónima, Clave de API e Identidad Administrada.

### Ejemplo: Registro de una Herramienta OpenAPI

Primero se requiere un archivo JSON (ej. `weather_openapi.json`) que describa la API (endpoints, parámetros, respuestas). Luego, se carga y registra en el agente:

```json
{
  "openapi": "3.1.0",
  "info": {
    "title": "Obtener datos del clima",
    "description": "Recupera los datos meteorológicos actuales para una ubicación basándose en wttr.in.",
    "version": "v1.0.0"
  },
  "servers": [
    {
      "url": "https://wttr.in"
    }
  ],
  "auth": [],
  "paths": {
    "/{location}": {
      "get": {
        "description": "Obtener información del clima para una ubicación específica",
        "operationId": "GetCurrentWeather",
        "parameters": [
          {
            "name": "location",
            "in": "path",
            "description": "Ciudad o ubicación para la cual recuperar el clima",
            "required": true,
            "schema": {
              "type": "string"
            }
          },
          {
          "name": "format",
          "in": "query",
          "description": "Usar siempre el valor j1 para este parámetro",
          "required": true,
          "schema": {
            "type": "string",
            "default": "j1"
          }
        }
        ],
        "responses": {
          "200": {
            "description": "Respuesta exitosa",
            "content": {
              "text/plain": {
                "schema": {
                  "type": "string"
                }
              }
            }
          },
          "404": {
            "description": "Ubicación no encontrada"
          }
        },
        "deprecated": false
      }
    }
  },
  "components": {
    "schemes": {}
  }
}
```

```python
from azure.ai.projects.models import OpenApiTool, OpenApiAnonymousAuthDetails

# Cargar la especificación OpenAPI desde un archivo
with open(weather_asset_file_path, "r") as f:
      openapi_weather = cast(dict[str, Any], jsonref.loads(f.read()))

tool = OpenApiTool(
    openapi=OpenApiFunctionDefinition(
        name="get_weather",
        spec=openapi_weather,
        description="Recuperar información meteorológica para una ubicación.",
        auth=OpenApiAnonymousAuthDetails(),
    )
)

agent = project_client.agents.create_version(
    agent_name="openapi-agent",
    definition=PromptAgentDefinition(
        model="gpt-4.1",
        instructions="Eres un asistente meteorológico. Usa la API para obtener datos del clima.",
        tools=[tool],
    ),
)
```

---

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
2.  Encuentra el comentario `Determine the next visible astronomical event for a given location` (*Determinar el próximo evento astronómico visible para una ubicación dada*) y agrega el siguiente código:

    ```python
    # Determinar el próximo evento astronómico visible para una ubicación dada
    def next_visible_event(location: str) -> str:
        """Devuelve el próximo evento astronómico visible para una ubicación."""
        # --- COMENTARIO: Normalización y Lógica ---
        # Convertimos la fecha actual a un entero (ej. 1205) para comparar fácilmente
        today = int(datetime.now().strftime("%m%d"))
        # Normalizamos el texto (ej. "South America" -> "south_america")
        loc = location.lower().replace(" ", "_")

        # Recuperar el próximo evento visible desde la ubicación, comenzando con eventos más tarde este año
        for name, event_type, date, date_str, locs in EVENTS:
            # Verificamos si la ubicación está en la lista del evento y si la fecha es futura
            if loc in locs and date >= today:
                # Retornamos el resultado en JSON (el formato que la IA entiende)
                return json.dumps({"event": name, "type": event_type, "date": date_str, "visible_from": sorted(locs)})

        return json.dumps({"message": f"No upcoming events found for {location}."})
    ```

Esta función verifica los datos de eventos de muestra para encontrar el próximo evento astronómico que sea visible desde una ubicación específica y devuelve los detalles del evento como una cadena JSON. A continuación, vamos a crear un agente que pueda usar esta función.

## Conéctate al proyecto Foundry

1.  Abre el archivo `agent.py`.
    > **Consejo:** A medida que agregues código, asegúrate de mantener la sangría correcta. Usa los niveles de sangría de los comentarios como guía.
2.  Encuentra el comentario `Add references` (*Agregar referencias*) y agrega el siguiente código para importar las clases que necesitarás para construir un agente de Azure AI que use una herramienta de función:

    ```python
    # Agregar referencias
    from azure.ai.projects import AIProjectClient
    from azure.ai.projects.models import FunctionTool
    from azure.identity import DefaultAzureCredential
    from azure.ai.projects.models import PromptAgentDefinition, FunctionTool
    from openai.types.responses.response_input_param import FunctionCallOutput, ResponseInputParam
    # Importamos las funciones locales que contienen la lógica real (la "cocina")
    from functions import next_visible_event, calculate_observation_cost, generate_observation_report
    ```

Observa que las funciones que definiste en el archivo `functions.py` se importan para que puedan usarse como herramientas para tu agente.

3.  Encuentra el comentario `Connect to the project client` (*Conectar al cliente del proyecto*) y agrega el siguiente código:

    ```python
    # Conectar al cliente del proyecto
    # Inicializamos los clientes necesarios dentro de un bloque 'with' para gestionar recursos
    with (
        DefaultAzureCredential() as credential,
        AIProjectClient(endpoint=project_endpoint, credential=credential) as project_client,
        # Obtenemos un cliente compatible con OpenAI desde el cliente del proyecto
        project_client.get_openai_client() as openai_client,
    ):
    ```

## Defina las herramientas de función

En esta tarea, definirás cada una de las herramientas de función que el agente puede usar. Los parámetros para cada herramienta de función se definen usando un esquema JSON, que especifica el nombre, tipo, descripción y otros atributos para cada parámetro de la función.

1.  Encuentra el comentario `Define the event function tool` (*Definir la herramienta de función de evento*) y agrega el siguiente código:

    ```python
    # Definir la herramienta de función de evento
    # --- COMENTARIO: Metadatos para la IA ---
    # Aquí definimos la "interfaz" que verá el agente. No contiene código, solo descripciones.
    event_tool = FunctionTool(
        name="next_visible_event", # Nombre que la IA usará para pedir esta herramienta
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

2.  Encuentra el comentario `Define the observation cost function tool` (*Definir la herramienta de función de costo de observación*) y agrega el siguiente código:

    ```python
    # Definir la herramienta de función de costo de observación
    # Esta herramienta permite a la IA calcular precios sin inventar números.
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

3.  Encuentra el comentario `Define the observation report generation function tool` (*Definir la herramienta de función de generación de informe de observación*) y agrega el siguiente código:

    ```python
    # Definir la herramienta de función de generación de informe de observación
    # Herramienta de acción que genera un efecto secundario (crear un archivo)
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

1.  Encuentra el comentario `Create a new agent with the function tools` (*Crear un nuevo agente con las herramientas de función*) y agrega el siguiente código:

    ```python
    # Crear un nuevo agente con las herramientas de función
    # Configuramos el agente con las instrucciones (System Prompt) y le damos acceso a las herramientas definidas
    agent = project_client.agents.create_version(
        agent_name="astronomy-agent",
        definition=PromptAgentDefinition(
            model=model_deployment,
            instructions=
                """Eres un asistente de observaciones astronómicas que ayuda a los usuarios a encontrar
                información sobre eventos astronómicos y calcular costos de alquiler de telescopios.
                Usa las herramientas disponibles para asistir a los usuarios con sus consultas.""",
            tools=[event_tool, cost_tool, report_tool], # <--- Aquí conectamos el menú de herramientas
        ),
    )
    ```

## Enviar un mensaje al agente y procesar la respuesta

Ahora que has creado el agente con las herramientas de función, puedes enviar mensajes al agente y procesar sus respuestas.

1.  Encuentra el comentario `Create a thread for the chat session` (*Crear un hilo para la sesión de chat*) y agrega el siguiente código:

    ```python
    # Crear un hilo para la sesión de chat
    # El hilo (Thread) mantiene el historial y el estado de la conversación
    conversation = openai_client.conversations.create()
    ```

    Este código crea la sesión de chat con el agente.

2.  Encuentra el comentario `Create an input list to hold function call outputs to send back to the model` (*Crear una lista para mantener las salidas de llamadas a funciones que serán enviadas de vuelta como entrada al agente*) y agrega el siguiente código:

    ```python
    # Crear una lista para mantener las salidas de llamadas a funciones que serán enviadas de vuelta como entrada al agente
    # Esta lista acumulará los resultados JSON de las funciones que ejecutemos localmente
    input_list: ResponseInputParam = []
    ```

3.  Encuentra el comentario `Send a prompt to the agent` (*Enviar un prompt al agente*) y agrega el siguiente código:

    ```python
    # Enviar un prompt al agente
    # Añadimos el mensaje del usuario al historial del hilo
    openai_client.conversations.items.create(
        conversation_id=conversation.id,
        items=[{"type": "message", "role": "user", "content": user_input}],
    )
    ```

4.  Encuentra el comentario `Retrieve the agent’s response, which may include function calls` (*Recuperar la respuesta del agente, la cual puede incluir llamadas a funciones*) y agrega el siguiente código:

    ```python
    # Recuperar la respuesta del agente, la cual puede incluir llamadas a funciones
    # Iniciamos la ejecución (Run). Si hay herramientas pendientes, el agente devolverá 'function_call'
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

1.  Encuentra el comentario `Process function calls` (*Procesar llamadas a funciones*) y agrega el siguiente código para manejar cualquier llamada a función realizada por el agente:

    ```python
    # Procesar llamadas a funciones
    # --- COMENTARIO: Ejecución de Herramientas ---
    # Iteramos sobre la respuesta para ver si el agente pidió ejecutar alguna función
    for item in response.output:
        if item.type == "function_call":
            # Recuperar la herramienta de función correspondiente por nombre
            function_name = item.name
            result = None
            # Mapeo manual: Si el agente pide "X", ejecutamos la función Python "X"
            if item.name == "next_visible_event":
                result = next_visible_event(**json.loads(item.arguments))
            elif item.name == "calculate_observation_cost":
                result = calculate_observation_cost(**json.loads(item.arguments))
            elif item.name == "generate_observation_report":
                result = generate_observation_report(**json.loads(item.arguments))

            # Anexar el resultado (salida) a la lista para enviárselo de vuelta al agente
            input_list.append(
                FunctionCallOutput(
                    type="function_call_output",
                    call_id=item.call_id,
                    output=result,
                )
            )
    ```

Este código itera a través de los elementos en la respuesta del agente para verificar si hay llamadas a funciones. Si se encuentra una llamada a función, recupera la herramienta de función correspondiente, ejecuta la función con los argumentos proporcionados y anexa el resultado a la lista de entrada que se enviará de vuelta al agente.

2.  Encuentra el comentario `Send function call outputs back to the model and retrieve a response` (*Enviar salidas de llamadas a funciones de vuelta al modelo y recuperar una respuesta*) y agrega el siguiente código:

    ```python
    # Enviar salidas de llamadas a funciones de vuelta al modelo y recuperar una respuesta
    # Si ejecutamos funciones, le damos los resultados al agente para que genere la respuesta final en lenguaje natural
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

3.  Encuentra el comentario `Delete the conversation when done` (*Eliminar la conversación cuando termine*) y agrega el siguiente código:

    ```python
    # Eliminar el agente cuando termine
    # Buena práctica: Limpiar la versión del agente en la nube para no dejar basura
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


---


# Codigo completo


```python
# --- COMENTARIO: Este bloque representa el archivo functions.py ---

import json
from datetime import datetime

# Datos de ejemplo para los eventos astronómicos
EVENTS = [
    ("Lluvia de meteoros de las Perseidas", "lluvia_de_meteoros", 812, "12 de Agosto", ["north_america", "europe", "asia"]),
    ("Lluvia de meteoros de las Gemínidas", "lluvia_de_meteoros", 1213, "13 de Diciembre", ["north_america", "south_america", "europe", "africa", "asia", "australia"]),
    ("Eclipse Solar Total", "eclipse_solar", 408, "8 de Abril", ["north_america"]),
    ("Eclipse Lunar Penumbral", "eclipse_lunar", 325, "25 de Marzo", ["north_america", "south_america"]),
    ("Conjunción Júpiter-Venus", "conjuncion_planetaria", 501, "1 de Mayo", ["south_america", "africa"]),
]

def next_visible_event(location: str) -> str:
    """
    Busca el próximo evento astronómico visible para una ubicación dada.
    :param location: El continente para buscar el evento.
    :return: Detalles del evento como una cadena JSON.
    """
    # Obtiene el día y mes actual como un número (ej. 0812 para 12 de Agosto)
    today = int(datetime.now().strftime("%m%d"))
    # Normaliza el nombre de la ubicación para que coincida con los datos
    loc = location.lower().replace(" ", "_")

    # Itera sobre los eventos para encontrar el próximo que sea visible en la ubicación
    for name, event_type, date, date_str, locs in EVENTS:
        if loc in locs and date >= today:
            # Devuelve el primer evento que cumple las condiciones
            return json.dumps({"evento": name, "tipo": event_type, "fecha": date_str, "visible_desde": sorted(locs)})

    # Si no se encuentra ningún evento, devuelve un mensaje de error
    return json.dumps({"mensaje": f"No se encontraron eventos próximos para {location}."})

def calculate_observation_cost(telescope_tier: str, hours: int, priority: str) -> str:
    """
    Calcula el costo de una observación basado en el nivel del telescopio, horas y prioridad.
    :param telescope_tier: Nivel del telescopio ('standard', 'advanced', 'premium').
    :param hours: Número de horas de observación.
    :param priority: Nivel de prioridad ('low', 'normal', 'high').
    :return: El costo calculado como una cadena JSON.
    """
    # Define las tarifas base por hora para cada nivel de telescopio
    base_rates = {"standard": 100, "advanced": 250, "premium": 350}
    # Define los multiplicadores de costo según la prioridad
    priority_multipliers = {"low": 0.8, "normal": 1.0, "high": 1.2}
    
    # Calcula el costo total
    cost = base_rates.get(telescope_tier, 0) * hours * priority_multipliers.get(priority, 1.0)
    
    # Devuelve el resultado en formato JSON
    return json.dumps({"costo": cost})

def generate_observation_report(event_name: str, location: str, telescope_tier: str, hours: int, priority: str, observer_name: str) -> str:
    """
    Genera un archivo de texto con el resumen de una observación astronómica.
    :param event_name: Nombre del evento observado.
    :param location: Ubicación del observador.
    :param telescope_tier: Nivel del telescopio utilizado.
    :param hours: Duración de la observación.
    :param priority: Prioridad de la observación.
    :param observer_name: Nombre del observador o entidad que solicita el reporte.
    :return: El nombre del archivo generado como una cadena JSON.
    """
    # Construye el contenido del reporte
    report_content = (
        f"--- Reporte de Observación Astronómica ---\n"
        f"Observador: {observer_name}\n"
        f"Evento: {event_name}\n"
        f"Ubicación: {location}\n"
        f"-----------------------------------------\n"
        f"Detalles de la Sesión:\n"
        f"  - Nivel de Telescopio: {telescope_tier}\n"
        f"  - Duración: {hours} horas\n"
        f"  - Prioridad: {priority}\n"
        f"-----------------------------------------\n"
    )
    
    # Genera un nombre de archivo único para el reporte
    file_name = f"reporte-{event_name.replace(' ', '_').lower()}.txt"
    
    # Escribe el contenido en el archivo
    with open(file_name, "w", encoding='utf-8') as f:
        f.write(report_content)
        
    # Devuelve el nombre del archivo creado
    return json.dumps({"archivo_reporte": file_name})
```

```python
# --- COMENTARIO: Este bloque representa el archivo agent.py ---

import os
import json
from dotenv import load_dotenv
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient
from azure.ai.projects.models import PromptAgentDefinition, FunctionTool
from openai.types.responses.response_input_param import FunctionCallOutput, ResponseInputParam

# Importamos las funciones de lógica de negocio desde nuestro archivo local
from functions import next_visible_event, calculate_observation_cost, generate_observation_report

# Carga las variables de entorno desde el archivo .env
load_dotenv()

# Obtiene la configuración desde las variables de entorno
project_endpoint = os.getenv("PROJECT_ENDPOINT")
model_deployment = os.getenv("MODEL_DEPLOYMENT_NAME")

def main():
    """
    Función principal que configura y ejecuta el agente de astronomía.
    """
    # 1. CONEXIÓN AL CLIENTE DEL PROYECTO
    # Se utiliza la credencial por defecto de Azure (autentica con CLI, VSCode, etc.)
    # El 'with' asegura que las conexiones se cierren correctamente al finalizar
    with (
        DefaultAzureCredential() as credential,
        AIProjectClient(endpoint=project_endpoint, credential=credential) as project_client,
        project_client.get_openai_client() as openai_client,
    ):
        # 2. DEFINICIÓN DE LAS HERRAMIENTAS DE FUNCIÓN (FUNCTION TOOLS)
        # Se describen las funciones Python para que el modelo de IA entienda para qué sirven y qué parámetros necesitan.

        # Herramienta para buscar el próximo evento visible
        event_tool = FunctionTool(
            name="next_visible_event",
            description="Obtener el próximo evento astronómico visible en una ubicación dada.",
            parameters={
                "type": "object",
                "properties": {
                    "location": {
                        "type": "string",
                        "description": "El continente para buscar el evento (ej. 'north_america', 'south_america').",
                    },
                },
                "required": ["location"],
            },
        )

        # Herramienta para calcular el costo de observación
        cost_tool = FunctionTool(
            name="calculate_observation_cost",
            description="Calcular el costo de una observación basado en el nivel del telescopio, número de horas y nivel de prioridad.",
            parameters={
                "type": "object",
                "properties": {
                    "telescope_tier": {"type": "string", "description": "Nivel del telescopio (ej. 'standard', 'advanced', 'premium')"},
                    "hours": {"type": "number", "description": "Número de horas para la observación"},
                    "priority": {"type": "string", "description": "Nivel de prioridad (ej. 'low', 'normal', 'high')"},
                },
                "required": ["telescope_tier", "hours", "priority"],
            },
        )

        # Herramienta para generar un reporte de observación
        report_tool = FunctionTool(
            name="generate_observation_report",
            description="Generar un reporte que resume una observación astronómica.",
            parameters={
                "type": "object",
                "properties": {
                    "event_name": {"type": "string", "description": "Nombre del evento observado"},
                    "location": {"type": "string", "description": "Ubicación del observador"},
                    "telescope_tier": {"type": "string", "description": "Nivel del telescopio usado"},
                    "hours": {"type": "number", "description": "Número de horas de uso del telescopio"},
                    "priority": {"type": "string", "description": "Nivel de prioridad de la observación"},
                    "observer_name": {"type": "string", "description": "Nombre de la persona que realizó la observación"},
                },
                "required": ["event_name", "location", "telescope_tier", "hours", "priority", "observer_name"],
            },
        )

        # 3. CREACIÓN DEL AGENTE
        print("--- Creando/Actualizando el Agente de Astronomía ---")
        agent = project_client.agents.create_version(
            agent_name="astronomy-agent",
            definition=PromptAgentDefinition(
                model=model_deployment,
                instructions=(
                    "Eres un asistente de observaciones astronómicas que ayuda a los usuarios a encontrar "
                    "información sobre eventos y a calcular los costos de alquiler de telescopios. "
                    "Usa las herramientas disponibles para ayudar con las consultas."
                ),
                # Se asigna la lista de herramientas al agente
                tools=[event_tool, cost_tool, report_tool],
            ),
        )
        print(f"Agente '{agent.name}' listo.")

        # 4. CREAR HILO DE CONVERSACIÓN
        # El hilo (thread) mantiene el estado y el historial de la conversación en el servidor.
        conversation = openai_client.conversations.create()
        print(f"--- Hilo de conversación iniciado (ID: {conversation.id}) ---")

        # 5. BUCLE DE CHAT INTERACTIVO
        while True:
            # Se solicita la entrada del usuario
            user_input = input("\nUsuario (escribe 'salir' para terminar): ")
            if user_input.lower() in ["salir", "quit", "exit"]:
                break

            # Se añade el mensaje del usuario al hilo de la conversación
            openai_client.conversations.items.create(
                conversation_id=conversation.id,
                items=[{"type": "message", "role": "user", "content": user_input}],
            )

            # Lista para acumular los resultados de las funciones que se devolverán al modelo
            input_list: list[ResponseInputParam] = []
            
            # 6. BUCLE DE EJECUCIÓN DE HERRAMIENTAS
            # Un solo prompt del usuario puede requerir múltiples llamadas a herramientas.
            while True:
                print("El agente está procesando...")
                # Se inicia una ejecución (run) del agente sobre la conversación actual
                response = openai_client.responses.create(
                    conversation=conversation.id,
                    extra_body={"agent": {"name": agent.name, "type": "agent_reference"}},
                    input=input_list, # Se envían los resultados de las funciones (si los hay)
                )

                # Se comprueba si la ejecución falló
                if response.status == "failed":
                    print(f"Fallo en la respuesta: {response.error}")
                    break

                # Se limpia la lista de inputs para la siguiente posible iteración del bucle de herramientas
                input_list = []
                
                tool_calls_found = False
                # Se itera sobre la salida de la ejecución para ver si el agente solicitó llamar a alguna función
                for item in response.output:
                    if item.type == "function_call":
                        tool_calls_found = True
                        function_name = item.name
                        print(f"--- El agente solicita ejecutar la función: {function_name} ---")
                        
                        result = None
                        # Se mapea el nombre de la función solicitada a la función Python real
                        if function_name == "next_visible_event":
                            result = next_visible_event(**json.loads(item.arguments))
                        elif function_name == "calculate_observation_cost":
                            result = calculate_observation_cost(**json.loads(item.arguments))
                        elif function_name == "generate_observation_report":
                            result = generate_observation_report(**json.loads(item.arguments))
                        
                        # Si se obtuvo un resultado, se prepara para enviarlo de vuelta al modelo
                        if result is not None:
                            input_list.append(FunctionCallOutput(type="function_call_output", call_id=item.call_id, output=result))

                # Si no se encontraron más llamadas a herramientas, significa que el agente ha dado su respuesta final.
                # Se imprime la respuesta y se sale del bucle de herramientas.
                if not tool_calls_found:
                    print(f"AGENTE: {response.output_text}")
                    break
        
        # 7. LIMPIEZA DE RECURSOS
        print("\n--- Limpiando recursos en la nube ---")
        project_client.agents.delete_version(agent_name=agent.name, agent_version=agent.version)
        print("Versión del agente eliminada.")

# Punto de entrada del script
if __name__ == "__main__":
    main()
```



---

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
*   **"Intérprete de código"**: Traducción de *Code Interpreter*.



# Resumen del Módulo

En este módulo, ha aprendido a potenciar sus agentes de IA mediante la integración de **herramientas personalizadas (Custom Tools)**.

Los puntos clave incluyen:
*   **Propósito:** Las herramientas permiten a los agentes realizar acciones específicas de negocio y acceder a datos fuera de su entrenamiento, mejorando la productividad y la precisión.
*   **Tipos de Herramientas:** Exploró diversas opciones de implementación como **Llamada a funciones (Function Calling)** para lógica local, **Azure Functions** para eventos sin servidor, y especificaciones **OpenAPI** para conectar APIs REST estándar.
*   **Implementación:** Aprendió a definir estas herramientas en el SDK y a gestionar el bucle de ejecución donde la aplicación invoca el código solicitado por el agente.
