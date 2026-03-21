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