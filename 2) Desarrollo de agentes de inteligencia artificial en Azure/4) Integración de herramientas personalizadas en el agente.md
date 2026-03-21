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
