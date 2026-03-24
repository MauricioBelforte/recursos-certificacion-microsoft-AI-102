# Desarrollo de una solución multiagente con el servicio Microsoft Foundry Agent

## Introducción

El **Servicio de Agentes de Microsoft Foundry (Microsoft Foundry Agent Service)** permite desarrollar sistemas sofisticados y **multiagente** capaces de dividir tareas complejas en roles más pequeños y especializados.

Mediante el uso de **agentes conectados**, puede diseñar soluciones inteligentes donde un agente principal delega responsabilidades en subagentes, sin necesidad de implementar una lógica de orquestación personalizada o un enrutamiento rígido en el código (**hard-coded**). Este enfoque modular aumenta la eficiencia y facilita el mantenimiento en una amplia gama de escenarios.

### Escenario de Ejemplo: Sistema de Soporte Técnico

Imagine que forma parte de un equipo de ingeniería que recibe un flujo constante de incidencias de soporte, como reportes de errores (bugs), solicitudes de nuevas características y problemas de infraestructura.

Revisar y clasificar manualmente cada ticket consume mucho tiempo y ralentiza la capacidad de respuesta del equipo. Con un enfoque **multiagente**, puede crear un **asistente de evaluación de prioridades (triage assistant)** que asigne diferentes tareas a agentes especializados.

En este sistema colaborativo:

- Un agente principal recibe la solicitud.
- Varios **subagentes** realizan tareas específicas, como:
  - Clasificar el tipo de ticket.
  - Establecer la prioridad.
  - Sugerir el equipo adecuado para resolver el problema.

---

# Comprender los agentes conectados

A medida que las soluciones de inteligencia artificial se vuelven más avanzadas, la administración de flujos de trabajo complejos resulta más difícil. Un solo agente puede controlar una amplia gama de tareas, pero este enfoque puede volverse inmanejable a medida que se expande el alcance. Por eso, el **Servicio de Agentes de Microsoft Foundry (Microsoft Foundry Agent Service)** le permite conectar varios agentes, cada uno con un rol enfocado, para trabajar juntos en un sistema cohesivo.

## ¿Qué son los agentes conectados?

Los **agentes conectados (Connected agents)** son una característica del servicio Microsoft Foundry Agent que permite dividir tareas grandes en roles más pequeños y especializados sin crear una lógica de enrutamiento de orquestador personalizada o codificación rígida (**hard-coded**). En lugar de confiar en un solo agente para hacer todo, puede crear varios agentes con responsabilidades claramente definidas que colaboran para realizar tareas.

En el centro de este sistema, hay un agente principal que interpreta la entrada del usuario y delega las tareas a los **subagentes** conectados. Cada subagente está diseñado para realizar una función específica, como:

- Resumir un documento.
- Validar una directiva.
- Recuperar datos de una base de conocimiento.

Esta división del trabajo ayuda a:

- Simplificar flujos de trabajo complejos.
- Mejorar el rendimiento y la precisión de los agentes.
- Facilitar el mantenimiento y la ampliación de los sistemas a lo largo del tiempo.

## ¿Por qué usar agentes conectados?

En lugar de escalar un solo agente para controlar cada solicitud de usuario o interacción de datos, el uso de agentes conectados le permite:

- Crear soluciones modulares que sean más fáciles de desarrollar y depurar (**debug**).
- Asignar funcionalidades especializadas a agentes que se pueden reutilizar entre soluciones.
- Escalar el sistema de una manera que se alinea con la lógica de negocios del mundo real.

Este enfoque es especialmente útil en escenarios en los que los agentes necesitan realizar tareas confidenciales de forma independiente, como controlar datos privados o generar contenido personalizado.

### Ventajas de la automatización con agentes conectados

El uso de agentes conectados para automatizar flujos de trabajo ofrece muchas ventajas:

- **Sin necesidad de orquestación personalizada:** El agente principal usa lenguaje natural para enrutar tareas, lo que elimina la necesidad de lógica rígida en el código.
- **Confiabilidad y trazabilidad mejoradas:** La clara separación de responsabilidades facilita la depuración de problemas, ya que los agentes se pueden probar individualmente.
- **Flexible y extensible:** Agregue o intercambie agentes sin volver a trabajar todo el sistema ni modificar el agente principal.

En resumen, los agentes conectados facilitan la creación de sistemas modulares y colaborativos sin orquestación compleja. Mediante la asignación de roles enfocados y el uso de la delegación por lenguaje natural, puede simplificar los flujos de trabajo, mejorar la confiabilidad y escalar las soluciones de forma más eficaz.

---

# Diseño de una solución multiagente con agentes conectados

En una solución de **agentes conectados (Connected agents)**, el éxito depende de definir claramente las responsabilidades de cada agente. El agente principal actúa como un orquestador, gestionando la colaboración entre los distintos agentes para resolver problemas complejos.

A continuación, exploraremos cómo diseñar un sistema multiagente utilizando el **Servicio de Agentes de Microsoft Foundry (Microsoft Foundry Agent Service)**.

## Responsabilidades Principales

### El Agente Principal (Orquestador)

El agente principal interpreta la intención detrás de una solicitud y determina qué agente conectado es el más adecuado para manejarla. Sus responsabilidades incluyen:

- **Interpretar la entrada del usuario:** Comprender qué se está pidiendo.
- **Seleccionar el agente conectado adecuado:** Enrutar la tarea al subagente experto correspondiente.
- **Reenviar contexto:** Pasar las instrucciones y datos pertinentes para que el subagente pueda trabajar.
- **Agregar o resumir resultados:** Recopilar las respuestas de los subagentes y presentar una respuesta final unificada al usuario.

### El Agente Conectado (Especialista)

Los agentes conectados están diseñados para centrarse en un único dominio de responsabilidad. Sus responsabilidades son:

- **Completar una acción específica:** Ejecutar una tarea basada en un prompt claro.
- **Uso de herramientas:** Si es necesario, utilizar herramientas específicas para completar su tarea.
- **Devolver resultados:** Entregar la información procesada al agente principal.

Diseñar estos agentes con una única responsabilidad facilita la depuración (**debugging**), la ampliación y la reutilización del sistema.

## Configuración de una solución multiagente

El proceso técnico para implementar esta arquitectura implica los siguientes pasos:

1.  **Inicialización del cliente de agentes:**
    Cree una instancia del cliente para conectarse a su proyecto de Microsoft Foundry.

2.  **Creación del agente conectado (Subagente):**
    Defina el agente que actuará como especialista utilizando el método `create_agent` en el objeto `AgentsClient`.
    - _Ejemplo:_ Un agente que recupera precios de acciones, resume documentos o valida cumplimiento.
    - _Instrucciones:_ Proporcione instrucciones claras que definan su propósito único.

3.  **Inicialización de la herramienta del agente conectado:**
    Utilice la definición del agente para crear una **Herramienta de agente conectado (ConnectedAgentTool)**. Asígnele un nombre y una descripción clara para que el agente principal sepa cuándo y cómo utilizar esta "herramienta" (que en realidad es otro agente).

4.  **Creación del agente principal:**
    Cree el agente principal utilizando el método `create_agent`. Agregue los agentes conectados a este principal mediante la propiedad `tools`, asignando las definiciones de `ConnectedAgentTool` creadas anteriormente.

5.  **Crear un hilo y enviar un mensaje:**
    Cree un **hilo (thread)** para gestionar el contexto de la conversación y agregue un mensaje con la solicitud del usuario.

6.  **Ejecución del flujo de trabajo:**
    Cree una **ejecución (run)** para procesar la solicitud. El agente principal utilizará sus herramientas (los subagentes) para delegar tareas y compilar una respuesta final.

7.  **Control de resultados:**
    Cuando se complete la ejecución, revise la respuesta del agente principal. La salida final puede incorporar información de uno o varios agentes conectados, pero se presenta al usuario como una respuesta coherente.

En resumen, el diseño de un sistema de agentes conectados implica definir agentes enfocados, registrarlos como herramientas y configurar un agente principal para enrutar las tareas inteligentemente. Este enfoque modular proporciona una base flexible para crear soluciones de IA colaborativas y escalables.
En este módulo, aprenderá a utilizar agentes conectados en Microsoft Foundry y practicará la creación de un sistema inteligente de clasificación de tickets mediante una solución multiagente colaborativa.


---

# Desarrollar una solución multiagente (obsoleto)

En este ejercicio, crearás un proyecto que coordina varios agentes de IA mediante el **Servicio de Agentes de Microsoft Foundry (Microsoft Foundry Agent Service)**. Diseñarás una solución de IA que facilita la clasificación de incidencias (triage). Los **agentes conectados (connected agents)** evaluarán la prioridad de la incidencia, sugerirán una asignación de equipo y determinarán el esfuerzo necesario para resolverla. ¡Comencemos!

> **Consejo:** El código utilizado en este ejercicio se basa en el SDK de Foundry para Python. Puedes desarrollar soluciones similares utilizando los SDK de Microsoft .NET, JavaScript y Java. Consulta las bibliotecas cliente del SDK de Foundry para obtener más información.

Este ejercicio debería completarse en aproximadamente **30 minutos**.

> **Nota:** Algunas de las tecnologías utilizadas en este ejercicio están en fase de vista previa o en desarrollo activo. Es posible que experimente comportamientos inesperados, advertencias o errores.

## Crear un proyecto de Foundry

Comencemos creando un proyecto de Foundry.

1.  En un navegador web, abra el portal de Foundry en `https://ai.azure.com` e inicie sesión con sus credenciales de Azure. Cierre cualquier panel de consejos o de inicio rápido que se abra la primera vez que inicie sesión y, si es necesario, use el logotipo de Foundry en la esquina superior izquierda para navegar a la página de inicio.

    > **Importante:** Asegúrese de que la opción "New Foundry" esté desactivada para este laboratorio si aparece.

2.  En la página principal, seleccione **Crear un agente**.
3.  Cuando se le solicite crear un proyecto, ingrese un nombre válido para su proyecto y expanda las **opciones avanzadas**.
4.  Confirma la siguiente configuración para tu proyecto:
    - **Recurso de Foundry:** Un nombre válido para su recurso de Foundry.
    - **Suscripción:** Su suscripción a Azure.
    - **Grupo de recursos:** Crear o seleccionar un grupo de recursos.
    - **Región:** Seleccione cualquiera de las recomendadas por AI Foundry (ej. `eastus2`, `swedencentral`).
      - _Nota:_ Algunos recursos de Azure AI están sujetos a cuotas regionales de modelos. Si se supera el límite de cuota más adelante durante el proceso, es posible que deba crear otro recurso en una región diferente.
5.  Selecciona **Crear** y espera a que se cree tu proyecto.
6.  Si se le solicita, implemente un modelo `gpt-4o` utilizando la opción de **implementación Estándar global** o Estándar (según la disponibilidad de su cuota).
    - _Nota:_ Si hay cupo disponible, es posible que se implemente automáticamente un modelo base GPT-4o al crear su agente y proyecto.
7.  Cuando se cree tu proyecto, se abrirá el entorno de pruebas para agentes.
8.  En el panel de navegación de la izquierda, seleccione **Descripción general (Overview)** para ver la página principal de su proyecto.
9.  Copie los valores del **Punto de conexión del proyecto (Project endpoint)** en un bloc de notas, ya que los usará para conectarse a su proyecto en una aplicación cliente.

## Crear una aplicación cliente de agente de IA

Ahora estás listo para crear una aplicación cliente que defina los agentes y las instrucciones. Se proporciona algo de código en un repositorio de GitHub.

### Preparar el entorno

1.  Abra una nueva pestaña del navegador (manteniendo el portal de Foundry abierto en la pestaña existente). Luego, en la nueva pestaña, navegue al portal de Azure en `https://portal.azure.com`.
2.  Use el botón **[>_]** a la derecha de la barra de búsqueda en la parte superior de la página para crear un nuevo **Cloud Shell** en el portal de Azure, seleccionando un entorno **PowerShell** sin almacenamiento en su suscripción (o monte uno si es necesario).
3.  En la barra de herramientas de Cloud Shell, asegúrese de cambiar a la versión **Clásica** si es necesario para usar el editor de código.
4.  En el panel de Cloud Shell, ingrese los siguientes comandos para clonar el repositorio de GitHub que contiene los archivos de código para este ejercicio:

    ```powershell
    rm -r ai-agents -f
    git clone https://github.com/MicrosoftLearning/mslearn-ai-agents ai-agents
    ```

5.  Cuando se haya clonado el repositorio, ingrese el siguiente comando para cambiar el directorio de trabajo a la carpeta que contiene los archivos de código y listarlos todos:

    ```powershell
    cd ai-agents/Labfiles/03b-build-multi-agent-solution/Python
    ls -a -l
    ```

### Configurar la aplicación

1.  En el panel de línea de comandos de Cloud Shell, ingrese el siguiente comando para instalar las bibliotecas que utilizará:

    ```powershell
    # Creamos un entorno virtual y activamos
    python -m venv labenv
    ./labenv/bin/Activate.ps1
    # Instalamos los paquetes necesarios de Azure AI
    pip install -r requirements.txt azure-ai-projects azure-ai-agents
    ```

2.  Ingrese el siguiente comando para editar el archivo de configuración proporcionado:

    ```powershell
    code .env
    ```

3.  En el archivo de código, reemplace el marcador de posición `your_project_endpoint` con el punto de conexión de su proyecto (copiado de la página Descripción general en el portal de Foundry) y el marcador de posición `your_model_deployment` con el nombre que asignó a su implementación del modelo `gpt-4o` (que por defecto suele ser `gpt-4o`).
4.  Guarde los cambios (`CTRL+S`) y cierre el editor (`CTRL+Q`).

### Crear agentes de IA

¡Ahora estás listo para crear los agentes para tu solución multiagente! ¡Empecemos!

1.  Ingrese el siguiente comando para editar el archivo `agent_triage.py`:

    ```powershell
    code agent_triage.py
    ```

2.  Busque el comentario `Add references` y agregue el siguiente código para importar las clases necesarias:

    ```python
    # Agregar referencias
    # Importamos las clases clave para gestionar agentes, herramientas y autenticación
    from azure.ai.agents import AgentsClient
    from azure.ai.agents.models import ConnectedAgentTool, MessageRole, ListSortOrder, ToolSet, FunctionTool
    from azure.identity import DefaultAzureCredential
    ```

3.  Busque el comentario `Connect to the agents client` y agregue el siguiente código para crear un `AgentsClient` conectado a su proyecto:

    ```python
    # Conectar al cliente de agentes
    # Inicializamos el cliente usando la identidad de Azure y el endpoint del proyecto
    agents_client = AgentsClient(
        endpoint=project_endpoint,
        credential=DefaultAzureCredential(
            exclude_environment_credential=True,
            exclude_managed_identity_credential=True
        ),
    )
    ```

4.  Ahora agregará código que usa `AgentsClient` para crear múltiples agentes, cada uno con un rol específico para procesar un ticket de soporte. Busque el comentario `Create an agent to prioritize support tickets` e ingrese el siguiente código:

    ```python
    # Crear un agente para priorizar tickets de soporte
    priority_agent_name = "priority_agent"
    priority_agent_instructions = """
    Evalúa qué tan urgente es un ticket basado en su descripción.

    Responde con uno de los siguientes niveles:
    - High: Problemas que afectan al usuario o bloqueantes.
    - Medium: Sensibles al tiempo pero no rompen nada crítico.
    - Low: Tareas cosméticas o no urgentes.

    Solo genera el nivel de urgencia y una explicación muy breve.
    """

    priority_agent = agents_client.create_agent(
        model=model_deployment,
        name=priority_agent_name,
        instructions=priority_agent_instructions
    )
    ```

5.  Busque el comentario `Create an agent to assign tickets to the appropriate team` e ingrese el siguiente código:

    ```python
    # Crear un agente para asignar tickets al equipo adecuado
    team_agent_name = "team_agent"
    team_agent_instructions = """
    Decide qué equipo debe ser dueño de cada ticket.

    Elige entre los siguientes equipos:
    - Frontend
    - Backend
    - Infrastructure
    - Marketing

    Basa tu respuesta en el contenido del ticket. Responde con el nombre del equipo y una explicación muy breve.
    """

    team_agent = agents_client.create_agent(
        model=model_deployment,
        name=team_agent_name,
        instructions=team_agent_instructions
    )
    ```

6.  Busque el comentario `Create an agent to estimate effort for a support ticket` e ingrese el siguiente código:

    ```python
    # Crear un agente para estimar el esfuerzo de un ticket de soporte
    effort_agent_name = "effort_agent"
    effort_agent_instructions = """
    Estima cuánto trabajo requerirá cada ticket.

    Usa la siguiente escala:
    - Small: Se puede completar en un día.
    - Medium: 2-3 días de trabajo.
    - Large: Esfuerzo de varios días o entre equipos.

    Basa tu estimación en la complejidad implícita del ticket. Responde con el nivel de esfuerzo y una breve justificación.
    """

    effort_agent = agents_client.create_agent(
        model=model_deployment,
        name=effort_agent_name,
        instructions=effort_agent_instructions
    )
    ```

7.  Hasta ahora, has creado tres agentes; cada uno con un rol específico. Ahora creemos objetos `ConnectedAgentTool` para cada uno, de modo que puedan ser usados por otros agentes. Busque el comentario `Create connected agent tools for the support agents` e ingrese:

    ```python
    # Crear herramientas de agente conectado para los agentes de soporte
    # Esto "empaqueta" a los agentes como herramientas utilizables por un agente principal
    priority_agent_tool = ConnectedAgentTool(
        id=priority_agent.id,
        name=priority_agent_name,
        description="Evalúa la prioridad de un ticket"
    )

    team_agent_tool = ConnectedAgentTool(
        id=team_agent.id,
        name=team_agent_name,
        description="Determina qué equipo debe tomar el ticket"
    )

    effort_agent_tool = ConnectedAgentTool(
        id=effort_agent.id,
        name=effort_agent_name,
        description="Determina el esfuerzo requerido para completar el ticket"
    )
    ```

8.  Ahora cree el agente principal que coordinará el proceso de triaje. Busque el comentario `Create an agent to triage support ticket processing by using connected agents` e ingrese:

    ```python
    # Crear un agente para clasificar el procesamiento de tickets usando agentes conectados
    triage_agent_name = "triage-agent"
    triage_agent_instructions = """
    Clasifica (triage) el ticket dado. Usa las herramientas conectadas para determinar la prioridad del ticket,
    a qué equipo debe asignarse y cuánto esfuerzo puede tomar.
    """

    triage_agent = agents_client.create_agent(
        model=model_deployment,
        name=triage_agent_name,
        instructions=triage_agent_instructions,
        tools=[
            priority_agent_tool.definitions[0],
            team_agent_tool.definitions[0],
            effort_agent_tool.definitions[0]
        ]
    )
    ```

9.  Finalmente, agregue el código para interactuar con el sistema. Busque el comentario `Use the agents to triage a support issue`:

    ```python
    # Usar los agentes para clasificar un problema de soporte
    print("Creando hilo del agente.")
    thread = agents_client.threads.create()

    # Crear el prompt del ticket
    prompt = input("\n¿Cuál es el problema de soporte que necesitas resolver?: ")

    # Enviar un prompt al agente
    message = agents_client.messages.create(
        thread_id=thread.id,
        role=MessageRole.USER,
        content=prompt,
    )

    # Ejecutar el hilo usando el agente principal (triage_agent)
    print("\nProcesando hilo del agente. Por favor espere.")
    run = agents_client.runs.create_and_process(thread_id=thread.id, agent_id=triage_agent.id)

    if run.status == "failed":
        print(f"La ejecución falló: {run.last_error}")

    # Obtener y mostrar mensajes
    messages = agents_client.messages.list(thread_id=thread.id, order=ListSortOrder.ASCENDING)
    for message in messages:
        if message.text_messages:
            last_msg = message.text_messages[-1]
            print(f"{message.role}:\n{last_msg.text.value}\n")
    ```

10. Busque el comentario `Clean up` e introduzca el código para eliminar los recursos:

    ```python
    # Limpiar
    print("Limpiando agentes:")
    agents_client.delete_agent(triage_agent.id)
    print("Agente de triaje eliminado.")
    agents_client.delete_agent(priority_agent.id)
    print("Agente de prioridad eliminado.")
    agents_client.delete_agent(team_agent.id)
    print("Agente de equipo eliminado.")
    agents_client.delete_agent(effort_agent.id)
    print("Agente de esfuerzo eliminado.")
    ```

11. Guarde el archivo (`CTRL+S`) y cierre el editor (`CTRL+Q`).

## Inicia sesión en Azure y ejecuta la aplicación

1.  En Cloud Shell, inicie sesión:

    ```powershell
    az login
    ```

2.  Ejecute la aplicación:

    ```powershell
    python agent_triage.py
    ```

3.  Introduzca una solicitud de ejemplo cuando se le pida:

    > "Users can't reset their password from the mobile app."
    >
    > _(Los usuarios no pueden restablecer su contraseña desde la aplicación móvil.)_

4.  Observe cómo el agente principal coordina a los subagentes para devolver una evaluación completa con prioridad, equipo asignado y esfuerzo estimado.

## Limpiar

Si ya ha terminado de explorar Azure AI Agent Service, debería eliminar los recursos (Grupo de recursos) desde el portal de Azure para evitar costos.

---


# Código Completo del Laboratorio: Solución Multiagente

Este archivo contiene el código completo para el archivo `agent_triage.py` utilizado en el laboratorio.

```python
# --- ARCHIVO: agent_triage.py ---

import os
# Importamos las clases necesarias del SDK de Azure AI Agents
from azure.ai.agents import AgentsClient
from azure.ai.agents.models import ConnectedAgentTool, MessageRole, ListSortOrder, ToolSet, FunctionTool
from azure.identity import DefaultAzureCredential
from dotenv import load_dotenv

# Cargar variables de entorno desde el archivo .env
load_dotenv()
project_endpoint = os.getenv("PROJECT_ENDPOINT")
model_deployment = os.getenv("MODEL_DEPLOYMENT_NAME")

def main():
    # ---------------------------------------------------------
    # 1. CONEXIÓN AL CLIENTE
    # ---------------------------------------------------------
    # Inicializamos el cliente de agentes. 'exclude_managed_identity_credential=True'
    # se usa a menudo en desarrollo local para forzar el uso de credenciales de usuario o CLI.
    agents_client = AgentsClient(
        endpoint=project_endpoint,
        credential=DefaultAzureCredential(
            exclude_environment_credential=True,
            exclude_managed_identity_credential=True
        ),
    )

    # ---------------------------------------------------------
    # 2. CREACIÓN DE SUB-AGENTES (ESPECIALISTAS)
    # ---------------------------------------------------------

    # --- Agente de Prioridad ---
    # Este agente se encarga únicamente de evaluar la urgencia.
    priority_agent_name = "priority_agent"
    priority_agent_instructions = """
    Evalúa qué tan urgente es un ticket basado en su descripción.

    Responde con uno de los siguientes niveles:
    - High: Problemas que afectan al usuario o bloqueantes
    - Medium: Sensibles al tiempo pero no rompen nada crítico
    - Low: Tareas cosméticas o no urgentes

    Solo genera el nivel de urgencia y una explicación muy breve.
    """

    priority_agent = agents_client.create_agent(
        model=model_deployment,
        name=priority_agent_name,
        instructions=priority_agent_instructions
    )

    # --- Agente de Equipo ---
    # Este agente decide a qué departamento técnico corresponde el problema.
    team_agent_name = "team_agent"
    team_agent_instructions = """
    Decide qué equipo debe ser dueño de cada ticket.

    Elige entre los siguientes equipos:
    - Frontend
    - Backend
    - Infrastructure
    - Marketing

    Basa tu respuesta en el contenido del ticket. Responde con el nombre del equipo y una explicación muy breve.
    """

    team_agent = agents_client.create_agent(
        model=model_deployment,
        name=team_agent_name,
        instructions=team_agent_instructions
    )

    # --- Agente de Esfuerzo ---
    # Este agente estima la complejidad y duración de la tarea.
    effort_agent_name = "effort_agent"
    effort_agent_instructions = """
    Estima cuánto trabajo requerirá cada ticket.

    Usa la siguiente escala:
    - Small: Se puede completar en un día
    - Medium: 2-3 días de trabajo
    - Large: Esfuerzo de varios días o entre equipos

    Basa tu estimación en la complejidad implícita del ticket. Responde con el nivel de esfuerzo y una breve justificación.
    """

    effort_agent = agents_client.create_agent(
        model=model_deployment,
        name=effort_agent_name,
        instructions=effort_agent_instructions
    )

    # ---------------------------------------------------------
    # 3. CREACIÓN DE HERRAMIENTAS DE AGENTE CONECTADO
    # ---------------------------------------------------------
    # Para que el agente principal use a los sub-agentes, debemos envolverlos
    # como herramientas (ConnectedAgentTool).

    priority_agent_tool = ConnectedAgentTool(
        id=priority_agent.id,
        name=priority_agent_name,
        description="Evalúa la prioridad de un ticket"
    )

    team_agent_tool = ConnectedAgentTool(
        id=team_agent.id,
        name=team_agent_name,
        description="Determina qué equipo debe tomar el ticket"
    )

    effort_agent_tool = ConnectedAgentTool(
        id=effort_agent.id,
        name=effort_agent_name,
        description="Determina el esfuerzo requerido para completar el ticket"
    )

    # ---------------------------------------------------------
    # 4. CREACIÓN DEL AGENTE PRINCIPAL (ORQUESTADOR)
    # ---------------------------------------------------------
    # Este agente recibe la solicitud del usuario y decide a qué sub-agentes llamar.
    triage_agent_name = "triage-agent"
    triage_agent_instructions = """
    Clasifica (triage) el ticket dado. Usa las herramientas conectadas para determinar la prioridad del ticket,
    a qué equipo debe asignarse y cuánto esfuerzo puede tomar.
    """

    triage_agent = agents_client.create_agent(
        model=model_deployment,
        name=triage_agent_name,
        instructions=triage_agent_instructions,
        tools=[
            priority_agent_tool.definitions[0],
            team_agent_tool.definitions[0],
            effort_agent_tool.definitions[0]
        ]
    )

    # ---------------------------------------------------------
    # 5. INTERACCIÓN Y EJECUCIÓN
    # ---------------------------------------------------------
    print("Creando hilo del agente...")
    thread = agents_client.threads.create()

    # Solicitar entrada al usuario
    prompt = input("\n¿Cuál es el problema de soporte que necesitas resolver?: ")

    # Enviar el mensaje del usuario al hilo
    message = agents_client.messages.create(
        thread_id=thread.id,
        role=MessageRole.USER,
        content=prompt,
    )

    # Ejecutar el hilo con el agente principal
    # El servicio orquestará automáticamente las llamadas a los sub-agentes necesarios
    print("\nProcesando hilo del agente. Por favor espere...")
    run = agents_client.runs.create_and_process(thread_id=thread.id, agent_id=triage_agent.id)

    if run.status == "failed":
        print(f"La ejecución falló: {run.last_error}")

    # Recuperar y mostrar el historial de mensajes
    # Veremos la respuesta final consolidada del agente principal
    messages = agents_client.messages.list(thread_id=thread.id, order=ListSortOrder.ASCENDING)
    for message in messages:
        if message.text_messages:
            last_msg = message.text_messages[-1]
            print(f"{message.role}:\n{last_msg.text.value}\n")

    # ---------------------------------------------------------
    # 6. LIMPIEZA
    # ---------------------------------------------------------
    print("Limpiando agentes:")
    agents_client.delete_agent(triage_agent.id)
    print("Agente de triaje eliminado.")
    agents_client.delete_agent(priority_agent.id)
    print("Agente de prioridad eliminado.")
    agents_client.delete_agent(team_agent.id)
    print("Agente de equipo eliminado.")
    agents_client.delete_agent(effort_agent.id)
    print("Agente de esfuerzo eliminado.")

if __name__ == "__main__":
    main()
```

---

---

# Evaluación del módulo

1.  **¿Cuál es el rol del agente principal en un sistema de agente conectado?**

    a. Para realizar directamente todas las tareas mediante herramientas

    b. Para coordinar la entrada del usuario y enrutar tareas a los agentes conectados adecuados ✅

    c. Para supervisar el rendimiento del agente y generar registros

**Justificación:** En una arquitectura de agentes conectados, el agente principal actúa como **orquestador**. Su función no es realizar las tareas especializadas, sino interpretar la intención del usuario y delegar (enrutar) el trabajo al subagente (agente conectado) más adecuado para ello.

**Glosario / Comentarios:**
*   **"Agente conectado"**: Traducción de *Connected agent*.

2.  **¿Cómo se conecta un agente a un agente principal mediante la biblioteca cliente de proyectos de Azure AI?**

    a. Agregue el agente como ConnectedAgentTool a la definición de la herramienta del agente principal. ✅

    b. Use el link_agents() método para enlazar el subagente al agente principal.

    c. Establezca el parent_id del agente principal en el id. del agente secundario.

**Justificación:** Para conectar un agente secundario a uno principal utilizando el SDK, se debe envolver el agente secundario en un objeto `ConnectedAgentTool` y luego agregarlo a la lista de `tools` (herramientas) en la definición del agente principal. No existen métodos como `link_agents` o propiedades `parent_id` para este propósito.

**Glosario / Comentarios:**
*   **"ConnectedAgentTool"**: Clase del SDK utilizada para definir un agente como herramienta.

3.  **¿Cómo decide el agente principal qué agente conectado va a usar?**

    a. Usa instrucciones de aviso y comprensión del lenguaje natural. ✅

    b. Sigue un árbol de decisión fijo basado en código.

    c. Selecciona aleatoriamente los agentes conectados disponibles.

**Justificación:** La ventaja de los agentes de IA es que no requieren lógica rígida ("hard-coded"). El agente principal utiliza sus **Instrucciones** (System Prompt) y la descripción de las herramientas disponibles para razonar en lenguaje natural y decidir qué agente conectado es el mejor para la tarea actual.

**Glosario / Comentarios:**

---

# Resumen del Módulo

En este módulo, ha aprendido a diseñar e implementar soluciones **multiagente** mediante el **Servicio de Agentes de Microsoft Foundry (Microsoft Foundry Agent Service)**.

Los puntos clave incluyen:

*   **Agentes Conectados (Connected Agents):** Aprendió cómo estos permiten desglosar tareas complejas asignándolas a agentes especializados que trabajan juntos dentro de un sistema coordinado.
*   **Roles Claros:** Exploró cómo definir responsabilidades para el **Agente Principal (Principal Agent)** y los **Agentes Conectados**.
*   **Delegación:** Vio cómo utilizar el lenguaje natural para delegar tareas sin necesidad de código de orquestación rígido.
*   **Diseño Modular:** Entendió cómo crear flujos de trabajo que sean más fáciles de escalar, depurar y mantener.

Además, puso en práctica estos conceptos creando su propia solución multiagente para clasificar incidencias de soporte.
*   **"Instrucciones de aviso"**: Traducción confusa de *Prompt instructions* o simplemente *Instructions*.
