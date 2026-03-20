# Desarrollo de un agente de IA con el servicio Microsoft Foundry Agent

## Introducción

El **Servicio de Agentes de Microsoft Foundry (Microsoft Foundry Agent Service)** es un servicio totalmente administrado diseñado para permitir a los desarrolladores compilar, implementar y escalar agentes de IA de alta calidad y extensibles de forma segura, sin la necesidad de administrar los recursos de cómputo y almacenamiento subyacentes.

### Escenario de Ejemplo: Sector Salud

Imagine que trabaja en el sector sanitario y necesita automatizar las interacciones con los pacientes y simplificar las tareas administrativas. Su organización desea desarrollar un agente de IA capaz de:

- Gestionar consultas de pacientes.
- Programar citas.
- Proporcionar información médica basada en datos en tiempo real.

El desafío principal suele ser la gestión de la infraestructura y la seguridad de los datos. **Microsoft Foundry Agent Service** soluciona esto permitiéndole crear agentes adaptados a sus necesidades mediante **instrucciones (instructions)** personalizadas y herramientas avanzadas.

Este servicio simplifica el proceso de desarrollo y reduce la cantidad de código necesario, permitiéndole centrarse en la creación de soluciones de IA de alta calidad mientras la plataforma gestiona la infraestructura.

En este módulo, aprenderá a utilizar el servicio Foundry Agent para desarrollar sus propios agentes.

---

# ¿Qué es un Agente de IA?

Un **agente de IA** es un servicio de software que utiliza inteligencia artificial generativa para comprender y ejecutar tareas en nombre de un usuario u otro programa.

A diferencia de las aplicaciones tradicionales o los chatbots simples, los agentes utilizan modelos avanzados para:

- Comprender el contexto.
- Tomar decisiones.
- Utilizar datos para **fundamentar (ground)** sus respuestas.
- Ejecutar acciones para lograr objetivos específicos de forma autónoma.

## ¿Por qué son útiles los Agentes de IA?

La adopción de agentes de IA ofrece ventajas estratégicas significativas:

- **Automatización de tareas rutinarias:** Manejan tareas repetitivas, liberando a los humanos para actividades estratégicas y creativas.
- **Toma de decisiones mejorada:** Analizan grandes volúmenes de datos para ofrecer predicciones y recomendaciones. A diferencia de los modelos de texto puros, pueden usar algoritmos para tomar decisiones informadas de forma autónoma.
- **Escalabilidad:** Permiten escalar operaciones sin un aumento lineal en recursos humanos.
- **Disponibilidad 24/7:** Operan continuamente sin interrupciones.

---

## Casos de Uso Comunes

Los agentes se pueden aplicar en diversos dominios:

### Agentes de Productividad Personal

Ayudan con tareas diarias como programar reuniones y gestionar correos.

- _Ejemplo:_ **Microsoft 365 Copilot** ayuda a redactar documentos y analizar datos en Office.

### Agentes de Investigación

Monitorean tendencias, recopilan datos y generan informes.

- _Usos:_ Seguimiento de acciones financieras, investigación médica o análisis de comportamiento del consumidor.

### Agentes de Ventas

Automatizan la generación y calificación de clientes potenciales (leads), envían seguimientos y programan llamadas.

### Agentes de Servicio al Cliente

Resuelven consultas rutinarias y problemas comunes en sitios web o plataformas de mensajería.

- _Ejemplo:_ Procesamiento automático de reembolsos.

### Agentes de Desarrollo

Asisten en la revisión de código, corrección de errores (debugging) y mantenimiento.

- _Ejemplo:_ **GitHub Copilot** sugiere código y mejoras en tiempo real.

---

# Riesgos de Seguridad y Mitigación

A medida que los agentes ganan autonomía y acceso a sistemas, surgen nuevos desafíos de seguridad.

## Riesgos Clave

| Riesgo                                       | Descripción                                                                                    | Ejemplo                                                                |
| :------------------------------------------- | :--------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------- |
| **Filtración de datos y privacidad**         | Exposición involuntaria de datos sensibles al procesar tareas.                                 | Un agente resume un archivo confidencial en un chat público.           |
| **Inyección de Prompts (Prompt Injection)**  | Usuarios maliciosos manipulan la entrada para alterar el comportamiento del agente.            | Insertar instrucciones ocultas para que el agente revele credenciales. |
| **Acceso no autorizado**                     | Controles débiles permiten que el agente o actores maliciosos accedan a sistemas restringidos. | Un agente realiza acciones de administrador sin permiso.               |
| **Envenenamiento de datos (Data Poisoning)** | Datos de entrenamiento corruptos llevan a decisiones sesgadas o inseguras.                     | Un agente recomienda contenido fraudulento por datos manipulados.      |
| **Vulnerabilidades de cadena de suministro** | Dependencia de APIs o plugins de terceros inseguros.                                           | Un plugin comprometido inyecta código malicioso.                       |
| **Dependencia excesiva**                     | Confianza ciega en acciones autónomas sin validación.                                          | Un agente envía pagos erróneos automáticamente.                        |
| **Falta de auditoría**                       | Ausencia de registros impide rastrear acciones maliciosas.                                     | Imposibilidad de detectar uso indebido por falta de logs.              |

## Estrategias de Mitigación

Para asegurar los agentes, adopte un enfoque de "seguridad por diseño":

1.  **Control de Acceso (RBAC):** Aplicar el principio de **privilegio mínimo (least privilege)**.
2.  **Validación de Entradas:** Filtrar mensajes para prevenir inyecciones.
3.  **Supervisión Humana (Human-in-the-loop):** Requerir aprobación manual para operaciones sensibles o ejecutarlas en un **entorno aislado (sandbox)**.
4.  **Auditoría Completa:** Mantener registros detallados (logs) de todas las acciones y decisiones del agente.
5.  **Mantenimiento del Modelo:** Reentrenar y validar continuamente para detectar desviaciones.

---

# Uso del Servicio de Agentes de Microsoft Foundry

El **Servicio de Agentes de Microsoft Foundry (Microsoft Foundry Agent Service)** es un servicio totalmente administrado que permite a los desarrolladores compilar, implementar y escalar agentes de IA de alta calidad de forma segura, abstrayendo la gestión de la infraestructura subyacente de cómputo y almacenamiento.

## Propósito del Servicio

Este servicio permite crear agentes adaptados a necesidades específicas mediante **instrucciones (instructions)** y herramientas avanzadas. Simplifica drásticamente el desarrollo en comparación con el uso directo de las APIs de inferencia, donde se requería un esfuerzo significativo de codificación para gestionar el estado y las llamadas a herramientas.

Con Foundry Agent Service, puede crear agentes a través del portal o mediante código (con menos de 50 líneas) que pueden:

- Responder preguntas utilizando datos propios o en tiempo real.
- Tomar decisiones y ejecutar acciones basadas en entradas del usuario.
- Automatizar flujos de trabajo complejos combinando modelos generativos con herramientas.

Es ideal para escenarios como soporte técnico, análisis de datos e informes automatizados.

## Características Clave

- **Llamada automática a herramientas:** El servicio gestiona el ciclo de vida completo: ejecución del modelo, invocación de la herramienta y retorno de resultados.
- **Gestión de estado segura:** Los estados de la conversación se manejan automáticamente mediante **hilos (threads)**, eliminando la necesidad de gestión manual por parte del desarrollador.
- **Herramientas integradas:** Incluye recuperación de archivos, **intérprete de código (code interpreter)** e integración con Bing y **Azure AI Search**. También soporta herramientas personalizadas vía **Azure Functions**.
- **Selección de modelos:** Acceso a diversos modelos de **Azure OpenAI**.
- **Seguridad empresarial:** Garantiza privacidad y cumplimiento, incluyendo autenticación sin claves.
- **Almacenamiento flexible:** Opción de usar almacenamiento administrado por la plataforma o conectar su propio **Azure Storage**.

## Recursos del Servicio

El servicio se encarga de aprovisionar los recursos en la nube necesarios a través de Azure y AI Foundry. Como requisito mínimo, un agente necesita:

1.  Un **Centro de Azure AI (Azure AI Hub)**.
2.  Un **Proyecto de Azure AI (Azure AI Project)**.

Existen dos arquitecturas comunes:

### Configuración Básica

Ideal para pruebas o agentes simples. Incluye:

- Azure AI Hub
- Azure AI Project
- Foundry Tools

### Configuración Estándar

Una configuración más robusta para producción que añade:

- **Azure Key Vault** (para gestión de secretos).
- **Azure AI Search** (para indexación y recuperación de datos).
- **Azure Storage** (para persistencia de datos y archivos).

---

# Desarrollo Programático e Integración de Agentes

Aunque el portal de Microsoft Foundry es excelente para la creación de prototipos, la implementación en aplicaciones reales suele requerir el uso de código. **Microsoft Foundry Agent Service** proporciona SDKs (como el de Python) y una API REST para integrar agentes en sus aplicaciones.

## Patrón de Código para la Integración

El flujo de trabajo general para interactuar con un agente desde el código sigue un patrón estándar:

1.  **Conexión:** Conectarse al **Proyecto de AI Foundry (AI Foundry Project)** utilizando el **Punto de conexión del proyecto (Project Endpoint)** y la autenticación de Microsoft Entra ID.
2.  **Definición del Agente:** Obtener una referencia a un agente existente o crear uno nuevo especificando:
    - **Modelo (Model):** La implementación que el agente usará (ej. `gpt-4`).
    - **Instrucciones (Instructions):** El "prompt del sistema" que define su comportamiento y límites.
    - **Herramientas (Tools):** Los recursos y capacidades adicionales habilitados.
3.  **Gestión de Hilos (Threads):** Crear un **Hilo (Thread)** para la sesión de chat. A diferencia de las APIs de chat tradicionales (donde se envía todo el historial en cada petición), el Hilo es un objeto **con estado (stateful)** que almacena el historial de mensajes y los archivos generados en el servidor.
4.  **Ejecución (Run):**
    - Agregar mensajes de usuario al Hilo.
    - Invocar una **Ejecución (Run)** para que el agente procese el contenido del hilo.
5.  **Recuperación:** Sondear el estado de la ejecución y, cuando esté completa, recuperar los mensajes de respuesta y los artefactos de datos.
6.  **Limpieza:** Eliminar el agente o el hilo si ya no son necesarios para liberar recursos.

---

# Herramientas del Agente (Agent Tools)

La capacidad de un agente para determinar cuándo y cómo usar herramientas es lo que lo diferencia de un modelo de chat estándar. Las herramientas se dividen en dos categorías principales:

## 1. Herramientas de Conocimiento (Knowledge Tools)

Estas herramientas aumentan el contexto del agente permitiéndole acceder a información externa para **fundamentar (ground)** sus respuestas con datos reales.

- **Búsqueda de Bing (Bing Search):** Permite al agente buscar en la web para obtener información en tiempo real.
- **Búsqueda de archivos (File Search):** Permite al agente buscar información dentro de documentos (PDF, DOCX) cargados en un almacén vectorial.
- **Azure AI Search:** Conecta al agente con índices de búsqueda empresarial existentes.
- **Microsoft Fabric:** Permite fundamentar las respuestas con datos almacenados en Microsoft Fabric.

## 2. Herramientas de Acción (Action Tools)

Estas herramientas permiten al agente ejecutar código o interactuar con sistemas externos para realizar tareas.

- **Intérprete de código (Code Interpreter):** Un entorno aislado (**sandbox**) donde el agente puede escribir y ejecutar código Python. Es ideal para cálculos matemáticos, análisis de datos y generación de gráficos.
- **Función personalizada (Custom Function):** Permite al agente llamar a funciones definidas en su propio código (el agente solicita la ejecución y su aplicación devuelve el resultado).
- **Azure Functions:** Permite al agente invocar código sin servidor alojado en Azure.
- **Especificación de OpenAPI (OpenAPI Spec):** Permite conectar el agente a APIs REST externas definidas mediante estándares OpenAPI 3.0.

---

# Laboratorio: Creación de un Agente de IA con el SDK

En este ejercicio, se detalla el proceso de creación de un agente de IA utilizando el **SDK de Microsoft Foundry para Python**. El objetivo es construir un agente capaz de analizar un conjunto de datos y generar gráficos, para lo cual utilizará la herramienta integrada **Intérprete de código (Code Interpreter)**.

## 1. Creación del Proyecto de Foundry

El primer paso es configurar el entorno de trabajo en el portal de Foundry.

*   Se inicia sesión en `https://ai.azure.com` y se crea un **nuevo proyecto**.
*   Al crearlo, se expanden las opciones avanzadas para especificar la configuración:
    *   **Recurso de Foundry**: Un nombre para el recurso principal.
    *   **Suscripción y Grupo de recursos**: Las credenciales de Azure donde se alojarán los servicios.
    *   **Región**: Es crucial seleccionar una región recomendada que disponga de cuota para los modelos de IA que se usarán.
*   Una vez creado el proyecto, se debe implementar un modelo base. Para este laboratorio, se busca y se implementa el modelo `gpt-4.1`.
*   Finalmente, desde la página principal del proyecto, se copia el valor del **Punto de conexión del proyecto (Project endpoint)**, ya que será necesario para conectar la aplicación cliente.

## 2. Creación de una Aplicación Cliente para el Agente

El código de la aplicación se gestionará desde el **Azure Cloud Shell**.

*   Se abre una sesión de **PowerShell** en el Cloud Shell del portal de Azure.
*   Se clona el repositorio de GitHub que contiene los archivos del laboratorio:
    ```bash
    git clone https://github.com/MicrosoftLearning/mslearn-ai-agents ai-agents
    ```
*   Dentro de la carpeta del proyecto (`ai-agents/Labfiles/02-build-ai-agent/Python`), se configuran las dependencias:
    ```bash
    pip install -r requirements.txt
    ```
*   Se edita el archivo de configuración `.env` para pegar el **Punto de conexión del proyecto (Project endpoint)** copiado anteriormente y verificar que el nombre de la implementación del modelo (`MODEL_DEPLOYMENT_NAME`) sea `gpt-4.1`.

## 3. Escritura del Código de la Aplicación del Agente

El archivo `agent.py` contiene la lógica principal. A continuación se describe cada sección del código que se debe completar.

*   **Importar Referencias:**
    Se importan las clases necesarias del SDK de Azure AI para construir el agente.
    ```python
    from azure.identity import DefaultAzureCredential
    from azure.ai.projects import AIProjectClient
    from azure.ai.projects.models import PromptAgentDefinition, CodeInterpreterTool, CodeInterpreterToolAuto
    ```

*   **Conectar al Proyecto de IA:**
    Se establece la conexión con el proyecto de Foundry utilizando las credenciales de Azure. El cliente de OpenAI se obtiene directamente desde el cliente del proyecto.
    ```python
    with (
        DefaultAzureCredential(...) as credential,
        AIProjectClient(endpoint=project_endpoint, credential=credential) as project_client,
        project_client.get_openai_client() as openai_client
    ):
        # ... resto del código ...
    ```

*   **Cargar Archivo y Crear Herramienta:**
    El archivo de datos (`data.txt`) se carga en el servicio y se crea una herramienta **Intérprete de código (Code Interpreter)** que tiene acceso a dicho archivo mediante su ID.
    ```python
    file = openai_client.files.create(
        file=open(file_path, "rb"), purpose="assistants"
    )
    code_interpreter = CodeInterpreterTool(
        container=CodeInterpreterToolAuto(file_ids=[file.id])
    )
    ```

*   **Definir el Agente:**
    Se crea una nueva versión de un agente llamado `data-agent`. Se le proporcionan **instrucciones (instructions)** claras sobre su rol y se le asigna la herramienta `code_interpreter` que acabamos de crear.
    > **Instrucción:** "You are an AI agent that analyzes the data in the file that has been uploaded. Use Python to calculate statistical metrics as necessary."
    >
    > *(Traducción: Eres un agente de IA que analiza los datos del archivo que se ha cargado. Usa Python para calcular las métricas estadísticas necesarias.)*
    ```python
    agent = project_client.agents.create_version(
        agent_name="data-agent",
        definition=PromptAgentDefinition(
            model=model_deployment,
            instructions="...",
            tools=[code_interpreter],
        ),
    )
    ```

*   **Crear una Conversación (Hilo):**
    Se crea una **conversación (conversation)**, que actúa como un **hilo (thread)** para mantener el estado y el historial del chat.
    ```python
    conversation = openai_client.conversations.create()
    ```

*   **Enviar un Mensaje y Ejecutar:**
    Dentro de un bucle, se toma la entrada del usuario (`user_prompt`), se agrega como un mensaje a la conversación y se invoca una **ejecución (run)** del agente sobre esa conversación.
    ```python
    openai_client.conversations.items.create(...)
    response = openai_client.responses.create(...)
    ```

*   **Mostrar la Respuesta y el Historial:**
    Una vez que la ejecución finaliza, se extrae el último mensaje del agente y se muestra. Al terminar el chat, se imprime todo el historial de la conversación para su revisión.

*   **Limpieza de Recursos:**
    Es una buena práctica eliminar la conversación y la versión del agente para liberar recursos y evitar costos.
    ```python
    openai_client.conversations.delete(...)
    project_client.agents.delete_version(...)
    ```

## 4. Ejecución de la Aplicación

*   Primero, es necesario autenticarse en la línea de comandos con `az login`.
*   Luego, se ejecuta el script: `python agent.py`.
*   La aplicación permite interactuar con el agente. Se pueden hacer preguntas como:
    *   *"What's the category with the highest cost?"* (¿Cuál es la categoría con el costo más alto?)
    *   *"Create a text-based bar chart showing cost by category"* (Crea un gráfico de barras basado en texto que muestre el costo por categoría)
    *   *"What's the standard deviation of cost?"* (¿Cuál es la desviación estándar del costo?)
*   El agente utilizará su herramienta de **Intérprete de código** para generar y ejecutar código Python dinámicamente y responder a estas preguntas.

## Resumen y Limpieza Final

Este laboratorio demuestra cómo usar el SDK para crear un agente con herramientas, gestionar una conversación con estado y realizar tareas complejas como el análisis de datos. El paso final es eliminar el grupo de recursos en el portal de Azure para limpiar todos los servicios creados.


---

## Código Completo del Laboratorio (Python)

A continuación, se presenta el código completo para `agent.py`. Este script incluye comentarios detallados en español para explicar cada paso del proceso de creación y ejecución del agente.

```python
import os # Librería para interactuar con el sistema operativo (útil para leer variables de entorno)
from azure.identity import DefaultAzureCredential # Clase para la autenticación automática con credenciales de Azure
from azure.ai.projects import AIProjectClient # El cliente principal para conectarse a un Proyecto de Azure AI (Foundry)
from azure.ai.projects.models import PromptAgentDefinition, CodeInterpreterTool, CodeInterpreterToolAuto # Clases específicas para definir la estructura de un agente y sus herramientas

# --- Configuración ---
# En un escenario real, estos valores vendrían de variables de entorno (.env)
# Reemplace con el "Project connection string" de su proyecto en Foundry
project_connection_string = "endpoint_de_su_proyecto" 
model_deployment_name = "gpt-4-1" # Asegúrese que coincida con su despliegue
file_path = "data.txt" # Archivo local a analizar

def main():
    # 1. Autenticación y Cliente
    # Se utiliza la credencial por defecto de Azure (CLI, Entorno, Identidad Administrada)
    credential = DefaultAzureCredential()

    # Conectamos con el Proyecto de AI Foundry
    with AIProjectClient(endpoint=project_connection_string, credential=credential) as project_client:
        
        # Obtenemos el cliente de OpenAI para operaciones de bajo nivel (archivos, mensajes)
        with project_client.get_openai_client() as openai_client:

            # 2. Cargar datos y crear herramientas
            print(f"--- Subiendo archivo de datos: {file_path} ---")
            # Subimos el archivo con propósito "assistants" (requerido para agentes)
            file = openai_client.files.create(
                file=open(file_path, "rb"), 
                purpose="assistants"
            )
            print(f"Archivo subido exitosamente. ID: {file.id}")

            # Creamos la herramienta 'Intérprete de Código' y le asignamos el archivo
            # Esto permite al agente leer el archivo usando Python
            code_interpreter = CodeInterpreterTool(
                container=CodeInterpreterToolAuto(file_ids=[file.id])
            )

            # 3. Definición del Agente
            print("--- Creando el Agente ---")
            # Creamos una versión del agente con instrucciones específicas y herramientas
            agent = project_client.agents.create_version(
                agent_name="data-agent",
                definition=PromptAgentDefinition(
                    model=model_deployment_name,
                    # Instrucciones en español (Prompts del Sistema)
                    instructions=(
                        "Eres un agente de IA que analiza los datos del archivo cargado. "
                        "Usa Python para calcular métricas estadísticas y generar gráficos según sea necesario."
                    ),
                    tools=[code_interpreter],
                ),
            )
            print(f"Agente creado: {agent.name} (ID: {agent.id})")

            # 4. Crear Hilo de Conversación
            # El hilo almacena el estado y el historial de la charla
            conversation = openai_client.conversations.create()
            print(f"--- Hilo de conversación iniciado: {conversation.id} ---")

            # 5. Bucle de Chat
            while True:
                # Entrada del usuario
                user_prompt = input("\nUsuario (escriba 'salir' para terminar): ")
                if user_prompt.lower() in ["salir", "quit", "exit"]:
                    break

                # Añadir el mensaje del usuario al hilo
                openai_client.conversations.items.create(
                    conversation_id=conversation.id,
                    items=[{"type": "message", "role": "user", "content": user_prompt}],
                )

                # Ejecutar el agente
                print("El agente está procesando...")
                
                # 'responses.create' inicia la ejecución y espera el resultado
                # Se pasa el agente como referencia en 'extra_body'
                response = openai_client.responses.create(
                    conversation=conversation.id,
                    extra_body={"agent": {"name": agent.name, "type": "agent_reference"}},
                    input="", # La entrada ya está en el hilo
                )

                # Verificar si hubo fallo
                if response.status == "failed":
                    print(f"Error en la ejecución: {response.error}")
                else:
                    # Mostrar la respuesta del agente
                    print(f"Agente: {response.output_text}")

            # 6. Historial y Limpieza
            print("\n--- Historial de la Conversación ---")
            items = openai_client.conversations.items.list(conversation_id=conversation.id)
            # Los items vienen en orden inverso (más reciente primero), los invertimos para leer
            for item in reversed(list(items)):
                if item.type == "message":
                    role = "Usuario" if item.role == "user" else "Agente"
                    print(f"{role}: {item.content[0].text}")

            # Limpiar recursos en la nube
            print("\n--- Limpiando recursos ---")
            openai_client.conversations.delete(conversation_id=conversation.id)
            project_client.agents.delete_version(agent_name=agent.name, agent_version=agent.version)
            print("Recursos eliminados.")

if __name__ == "__main__":
    main()
```
---


# Resumen del Módulo

Los **Agentes de Inteligencia Artificial (AI Agents)** representan un avance significativo, transformando la manera en que trabajamos al automatizar tareas rutinarias, mejorar la toma de decisiones y ofrecer soluciones escalables.

En este módulo, ha explorado el **Servicio de Agentes de Microsoft Foundry (Microsoft Foundry Agent Service)**, un entorno totalmente administrado que resuelve los desafíos de infraestructura y escalabilidad, permitiendo crear agentes de alta calidad con un esfuerzo de codificación mínimo.

Puntos clave aprendidos:
*   **Propósito:** Facilitar la creación de agentes seguros y extensibles.
*   **Características:** Gestión automática del ciclo de vida de herramientas, manejo de estado con **Hilos (Threads)** y seguridad de nivel empresarial.
*   **Desarrollo:** Creación de agentes tanto en el portal como mediante el SDK, integrando herramientas como el **Intérprete de código (Code Interpreter)**.

Ahora tiene la capacidad de:
*   Describir el propósito y los casos de uso de los agentes de IA.
*   Explicar las capacidades del Servicio de Agentes de Foundry.
*   Crear e integrar un agente en sus propias aplicaciones.

---

# Evaluación del módulo

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
