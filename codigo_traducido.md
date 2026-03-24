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
