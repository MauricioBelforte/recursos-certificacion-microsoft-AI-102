# Desarrollo de una aplicación de IA con el SDK de Microsoft Foundry 

La diferencia entre un **SDK** y una **API REST** es clave en la certificación AI-102 porque muchas veces vas a tener que decidir cuál usar según el escenario:

### 🔹 API REST
- **Qué es**: Una interfaz expuesta vía HTTP, accesible desde cualquier lenguaje que pueda hacer llamadas web.
- **Cómo se usa**: Envías peticiones (GET, POST, PUT, DELETE) a una URL con parámetros y cabeceras, y recibes respuestas en JSON.
- **Ventajas**:
  - Independiente del lenguaje: podés usar Python, Java, C#, incluso curl.
  - Muy flexible y universal.
- **Desventajas**:
  - Tenés que manejar manualmente autenticación, construcción de requests y parsing de respuestas.
  - Más código repetitivo.

### 🔹 SDK
- **Qué es**: Un conjunto de librerías oficiales que encapsulan esas llamadas REST y te dan funciones listas para usar.
- **Cómo se usa**: Importás el paquete (ej. `azure-ai-textanalytics` en Python) y llamás métodos como `client.analyze_sentiment()`.
- **Ventajas**:
  - Simplifica mucho el desarrollo: menos código, más legible.
  - Maneja internamente autenticación, errores y formatos.
  - Mejor integración con el ecosistema del lenguaje (tipos, excepciones).
- **Desventajas**:
  - Limitado al lenguaje en que está disponible.
  - Puede tardar más en actualizarse respecto a nuevas features de la API REST.

### 📌 En resumen
- **API REST** = acceso directo, universal, más bajo nivel.  
- **SDK** = acceso simplificado, más productivo, pero dependiente del lenguaje.

En el examen suelen plantear escenarios como:  
- *“Necesitás integrar un servicio de Azure AI en una aplicación escrita en un lenguaje sin SDK oficial”* → ahí la respuesta correcta es **usar la API REST**.  
- *“Querés acelerar el desarrollo en Python o C# con soporte oficial”* → la respuesta es **usar el SDK**.

---

## Instalación y Configuración

Para utilizar la biblioteca de proyectos de Azure AI en Python, utilice el administrador de paquetes `pip` para instalar `azure-ai-projects`:

```bash
pip install azure-ai-projects
pip install azure-identity
```

## Conexión a un Proyecto

La primera tarea al desarrollar con el SDK es conectar con un proyecto de Microsoft Foundry existente. Cada proyecto tiene una **Cadena de conexión** (Connection String) o un **Punto de conexión** (Endpoint) único, visible en la página de "Información general" del proyecto en el portal.

Ejemplo de End Point: https://an-ai-project-resource.services.ai.azure.com/api/projects/an-ai-project

Puede usar el punto de conexión del proyecto en el código para crear un objeto AIProjectClient , que proporciona un proxy mediante programación para el proyecto, como se muestra en este ejemplo de Python:

```python
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

project_endpoint = "https://an-ai-project-resource.services.ai.azure.com/api/projects/an-ai-project"

project_client = AIProjectClient(            
    credential=DefaultAzureCredential(),
    endpoint=project_endpoint)
```

Notará que el código utiliza `DefaultAzureCredential` para la autenticación. Esto es parte de la biblioteca `azure-identity` y permite que el código se autentique automáticamente utilizando las credenciales disponibles en el entorno, como las credenciales de Azure CLI, variables de entorno o Managed Identity si se ejecuta en Azure.

## Nota
El código usa las credenciales de Azure predeterminadas para autenticarse al acceder al proyecto. Para habilitar esta autenticación, además del paquete `azure-ai-projects`, debe instalar el paquete `azure-identity`:

```bash
pip install azure-identity
```


> **Nota sobre Autenticación:**  
> El código utiliza `DefaultAzureCredential`. Para que funcione localmente, asegúrese de haber iniciado sesión previamente con la Azure CLI:
> ```bash
> az login
> ``


# Trabajar con Conexiones de Proyecto (Project Connections)

Cada proyecto de **Microsoft Foundry** incluye recursos conectados. Estos se definen tanto a nivel primario (en el recurso o **Centro (Hub)** de Microsoft Foundry) como a nivel de proyecto.

Cada recurso actúa como una **conexión (connection)** a un servicio externo, como **Azure Storage**, **Azure AI Search**, **Azure OpenAI**, o a otro recurso de Microsoft Foundry.

Mediante el **SDK** de Microsoft Foundry, es posible conectarse a un proyecto y recuperar las conexiones para consumir los servicios asociados.

Por ejemplo, el objeto `AIProjectClient` en Python dispone de la propiedad `connections`, la cual permite acceder a las conexiones de recursos del proyecto. Los métodos disponibles para el objeto de conexiones incluyen:

- `connections.list()`: Devuelve una colección de objetos de conexión, representando cada uno una conexión en el proyecto. Puede filtrar los resultados especificando el parámetro opcional `connection_type` con una enumeración válida (por ejemplo, `ConnectionType.AZURE_OPEN_AI`).
- `connections.get(connection_name, include_credentials)`: Devuelve un objeto de conexión específico basado en el nombre proporcionado. Si el parámetro `include_credentials` es `True` (valor predeterminado), se incluyen las credenciales necesarias para acceder a la conexión (por ejemplo, una **Clave de API (API Key)** para un recurso de herramientas de Foundry).

Los objetos devueltos por estos métodos contienen propiedades específicas de la conexión, incluidas las credenciales, que se utilizan para autenticarse y conectarse con el recurso asociado.

El siguiente ejemplo de código muestra cómo enumerar todas las conexiones de recursos agregadas a un proyecto:

```python
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

try:

    # Obtener el cliente del proyecto (Get project client)
    project_endpoint = "https://....."
    project_client = AIProjectClient(
            credential=DefaultAzureCredential(),
            endpoint=project_endpoint,
        )

    # Listar todas las conexiones en el proyecto (List all connections in the project)
    connections = project_client.connections
    print("List all connections:")
    for connection in connections.list():
        print(f"{connection.name} ({connection.type})")

except Exception as ex:
    print(ex)
```




# Creación de un Cliente de Chat (Chat Client)

Un escenario común en el desarrollo de aplicaciones de IA es conectarse a un modelo de IA generativa y utilizar **prompts** para establecer un diálogo tipo chat.

Aunque es posible utilizar directamente el **SDK de Azure OpenAI** (autenticándose con Microsoft Entra ID o claves), cuando el modelo forma parte de una **Implementación (Deployment)** dentro de un proyecto de **Microsoft Foundry**, es recomendable utilizar el **SDK de Microsoft Foundry**.

Este enfoque permite instanciar un `AIProjectClient`, desde el cual se puede obtener un cliente de chat de OpenAI ya autenticado para cualquier modelo implementado en el recurso de Microsoft Foundry. Esto simplifica el código y facilita el cambio entre diferentes modelos simplemente actualizando el nombre de la implementación.

> **Sugerencia (Tip):** Puede utilizar el cliente de chat proporcionado por el proyecto de Microsoft Foundry para interactuar con cualquier modelo implementado en el recurso asociado, incluso aquellos que no son de OpenAI, como los modelos **Microsoft Phi**.

## Ejemplo de código

El siguiente ejemplo en Python utiliza el método `get_openai_client()` para obtener un cliente compatible con OpenAI e iniciar un chat con un modelo implementado en el proyecto.

```python
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient
from openai import AzureOpenAI

try:
    
    # Conectarse al proyecto (Connect to the project)
    project_endpoint = "https://......"
    project_client = AIProjectClient(            
            credential=DefaultAzureCredential(),
            endpoint=project_endpoint,
        )
    
    # Obtener un cliente de chat (Get a chat client)
    chat_client = project_client.get_openai_client(api_version="2024-10-21")
    
    # Obtener una respuesta de chat basada en el prompt del usuario
    user_prompt = input("Enter a question:")
    
    response = chat_client.chat.completions.create(
        model="your_model_deployment_name", # Reemplace con el nombre de su implementación
        messages=[
            {"role": "system", "content": "You are a helpful AI assistant."},
            {"role": "user", "content": user_prompt}
        ]
    )
    print(response.choices[0].message.content)

except Exception as ex:
    print(ex)
```

> **Nota:** Además de los paquetes `azure-ai-projects` y `azure-identity`, este ejemplo requiere la instalación del paquete `openai`:
> ```bash
> pip install openai
> ```



## Laboratorio: Crear una aplicación de chat con IA generativa

*Nota: Este ejercicio está obsoleto, pero se mantiene para fines de estudio. Considere completar el ejercicio de reemplazo.*

En este ejercicio, utilizarás el SDK de Microsoft Foundry para Python para crear una aplicación de chat sencilla que se conecta a un proyecto y conversa con un modelo de lenguaje.

*Nota: Este ejercicio se basa en una versión preliminar del SDK, que puede estar sujeta a cambios.*

Si bien este ejercicio se basa en el SDK de Python de Foundry, puede desarrollar aplicaciones de chat con IA utilizando varios SDK específicos para cada lenguaje, entre ellos:
- Proyectos de IA de Azure para Python
- Proyectos de IA de Azure para Microsoft .NET
- Proyectos de IA de Azure para JavaScript

Este ejercicio dura aproximadamente 40 minutos.

### Implementar un modelo en un proyecto de Foundry
Comencemos por implementar un modelo en un proyecto de Foundry.

1. En un navegador web, abra el portal de Foundry en [https://ai.azure.com] e inicie sesión con sus credenciales de Azure. Cierre cualquier panel de consejos o de inicio rápido que se abra la primera vez que inicie sesión y, si es necesario, use el logotipo de Foundry en la esquina superior izquierda para navegar a la página de inicio.
2. En la página principal, en la sección **Explorar modelos y capacidades**, busque el modelo `gpt-4o` que utilizaremos en nuestro proyecto.
3. En los resultados de la búsqueda, seleccione el modelo `gpt-4o` para ver sus detalles y, a continuación, en la parte superior de la página del modelo, seleccione **Usar este modelo**.
4. Cuando se le solicite crear un proyecto, ingrese un nombre válido para su proyecto y expanda las opciones avanzadas.
5. Seleccione **Personalizar** y especifique la siguiente configuración para su proyecto:
   - **Recurso de Foundry:** Un nombre válido para su recurso de Foundry.
   - **Suscripción:** Su suscripción a Azure
   - **Grupo de recursos:** Crear o seleccionar un grupo de recursos
   - **Región:** Seleccione cualquiera de las recomendadas por AI Foundry * (*Algunos recursos de Azure AI están sujetos a cuotas regionales de modelos. Si se supera el límite de cuota, es posible que deba crear otro recurso en una región diferente.*)
6. Seleccione **Crear** y espere a que se cree su proyecto, incluyendo la implementación (deployment) del modelo `gpt-4o` que seleccionó.

*IMPORTANTE: Dependiendo de su cuota disponible para modelos `gpt-4o`, es posible que reciba un mensaje adicional para implementar el modelo en un recurso de una región diferente. Si esto sucede, hágalo utilizando la configuración predeterminada.*

7. Cuando se cree su proyecto, el **área de juegos (Playground)** del chat se abrirá automáticamente para que pueda probar su modelo (si no, seleccione Playgrounds y luego abra el Chat playground).
8. En el panel de **Configuración (Setup)**, anote el nombre de la implementación de su modelo, que debería ser `gpt-4o`. Puede confirmar esto viendo la implementación en la página **Modelos y puntos de conexión (Endpoints)**.

### Crea una aplicación cliente para chatear con la modelo
Ahora que ha implementado un modelo, puede utilizar los SDK de Foundry y Azure OpenAI para desarrollar una aplicación que chatee con él.

**Preparar la configuración de la aplicación**
1. En el portal de Foundry, vea la página de descripción general (**Overview**) de su proyecto.
2. En el área **Puntos de conexión (Endpoints) y claves (Keys)**, asegúrese de que la biblioteca Foundry esté seleccionada y vea el **Punto de conexión (Endpoint)** del proyecto Foundry. Utilizará este endpoint para conectarse a su proyecto y modelo en una aplicación cliente.
*Nota: ¡También puede usar el endpoint de Azure OpenAI!*
3. Abra una nueva pestaña del navegador en Azure Portal en [https://portal.azure.com], iniciando sesión si se le solicita.
4. Utilice el botón `[>_]` a la derecha de la barra de búsqueda en la parte superior para crear un nuevo Cloud Shell, seleccionando un entorno PowerShell sin almacenamiento.
5. En la barra de herramientas de Cloud Shell, en el menú Configuración, seleccione **Ir a la versión clásica** (necesario para usar el editor de código).
6. En el panel de Cloud Shell, ingrese los siguientes comandos para clonar el repositorio de GitHub que contiene los archivos de código:

```bash
# Eliminar carpeta previa si existe
rm -r mslearn-ai-foundry -f
# Clonar repositorio
git clone https://github.com/microsoftlearning/mslearn-ai-studio mslearn-ai-foundry
```

7. Una vez clonado, navegue hasta la carpeta del código de la aplicación de chat:

```bash
cd mslearn-ai-foundry/labfiles/chat-app/python
ls -a -l
```

8. En el panel de comandos de Cloud Shell, ingrese el siguiente comando para instalar las bibliotecas que utilizará:

```bash
# Crear un entorno virtual de Python
python -m venv labenv
   
# Activar el entorno virtual (en Bash/Cloud Shell)
source labenv/bin/activate

# Instalar los paquetes requeridos
pip install -r requirements.txt azure-identity azure-ai-projects openai
```

9. Ingrese el siguiente comando para editar el archivo de configuración proporcionado:

```bash
code .env
```

10. En el archivo de código, reemplace el marcador de posición `your_project_endpoint` con el **Punto de conexión (Endpoint)** apropiado para su modelo y el marcador `your_model_deployment` con el nombre asignado a su modelo `gpt-4o`. Guarde y cierre el editor (CTRL+S, luego CTRL+Q).

**Escribir código para conectarse a su proyecto y chatear con su modelo**
1. Ingrese el comando para editar el código de la aplicación:

```bash
code chat-app.py
```

2. En el archivo, debajo del comentario para agregar referencias, inserte el código para importar los espacios de nombres necesarios:

```python
# Agregar referencias (Add references)
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient
from openai import AzureOpenAI
```

3. Debajo del comentario para inicializar el cliente, agregue el siguiente código para conectarse al proyecto Foundry:

```python
# Inicializar el cliente del proyecto (Initialize the project client)
project_client = AIProjectClient(            
         credential=DefaultAzureCredential(
             exclude_environment_credential=True,
             exclude_managed_identity_credential=True
         ),
         endpoint=project_endpoint,
     )
```

4. Agregue el siguiente código para crear el cliente de chat:

```python
# Obtener un cliente de chat (Get a chat client)
openai_client = project_client.get_openai_client(api_version="2024-10-21")
```

5. Agregue el código para inicializar un arreglo de mensajes con un mensaje del sistema (**System message**):

```python
# Inicializar el prompt con un mensaje del sistema (Initialize prompt with system message)
prompt = [
         {"role": "system", "content": "You are a helpful AI assistant that answers questions."}
     ]
```

6. Dentro del ciclo de envío ("loop"), debajo del comentario para obtener una respuesta, agregue este bloque:

```python
# Obtener una respuesta de chat (Get a chat completion)
prompt.append({"role": "user", "content": input_text})
response = openai_client.chat.completions.create(
         model=model_deployment,
         messages=prompt)
completion = response.choices[0].message.content
print(completion)
prompt.append({"role": "assistant", "content": completion})
```

7. Use CTRL+S para guardar el código.

**Iniciar sesión en Azure y ejecutar la aplicación**
1. En Cloud Shell, inicie sesión interactivamente en Azure:

```bash
az login
```

2. Luego ejecute la aplicación:

```bash
python chat-app.py
```

3. Cuando se le solicite, ingrese una pregunta como *¿Cuál es el animal más rápido de la tierra?* y pruebe preguntas de seguimiento como *¿Están en peligro de extinción?*
4. Ingrese `quit` para salir.

### Limpieza
1. Abra el portal de Azure y vea el grupo de recursos utilizado en este ejercicio.
2. Seleccione **Eliminar grupo de recursos**.
3. Confirme la eliminación.


### Código Completo del Laboratorio: Crear una aplicación de chat con IA generativa

```python
# chat-app.py

# Agregar referencias (Add references)
# Importamos las credenciales por defecto de Azure, el cliente de proyecto de Foundry y el cliente de chat estándar de OpenAI.
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient
from openai import AzureOpenAI

import os
from dotenv import load_dotenv

def main():
    # Obtener configuraciones del .env (Get configuration settings)
    load_dotenv()
    project_endpoint = os.getenv("PROJECT_ENDPOINT")
    model_deployment = os.getenv("MODEL_DEPLOYMENT")

    # Inicializar el cliente del proyecto (Initialize the project client)
    # Autenticamos el acceso y generamos el cliente principal conectado al proyecto.
    project_client = AIProjectClient(            
        credential=DefaultAzureCredential(
            exclude_environment_credential=True,
            exclude_managed_identity_credential=True
        ),
        endpoint=project_endpoint,
    )

    # Obtener un cliente de chat (Get a chat client)
    # Extraemos el cliente configurado listo para realizar llamadas a la API de OpenAI del modelo implementado.
    openai_client = project_client.get_openai_client(api_version="2024-10-21")

    # Inicializar el prompt con un mensaje del sistema (Initialize prompt with system message)
    # Configuramos el contexto base de la IA generativa para instruir cómo responder.
    prompt = [
        {"role": "system", "content": "You are a helpful AI assistant that answers questions."}
    ]

    while True:
        input_text = input('\nEnter a question (or type "quit" to exit): ')
        if input_text.lower() == 'quit':
            break

        # Obtener una respuesta de chat (Get a chat completion)
        # Añadimos la pregunta del usuario al array, llamamos a la API para procesarla, y luego guardamos la respuesta en el mismo array para preservar el historial del chat.
        prompt.append({"role": "user", "content": input_text})
        response = openai_client.chat.completions.create(
            model=model_deployment,
            messages=prompt
        )
        completion = response.choices[0].message.content
        print(completion)
        prompt.append({"role": "assistant", "content": completion})

if __name__ == '__main__':
    main()
```

# Resumen del Módulo

Mediante el **SDK de Microsoft Foundry**, es posible desarrollar aplicaciones de IA avanzadas que aprovechen los recursos configurados en los proyectos de Microsoft Foundry.

La clase `AIProjectClient` del SDK funciona como un **proxy** programático para el proyecto. Esto facilita el acceso a los recursos conectados y permite el uso de bibliotecas específicas de cada servicio para consumirlos de manera eficiente.

## Puntos clave

*   **Integración:** El SDK unifica el acceso a múltiples servicios de IA.
*   **Gestión:** `AIProjectClient` maneja la conexión y configuración del proyecto.

---

# Evaluación del módulo

1.  **¿Qué clase del SDK de Microsoft Foundry proporciona un objeto proxy para un proyecto?**

    a. Propiedades de conexión (Connection properties)

    b. AIProjectClient ✅

    c. ChatCompletionsClient

**Justificación:** La clase `AIProjectClient` es el punto de entrada principal del SDK. Se inicializa con la cadena de conexión o el punto de conexión (endpoint) y actúa como un proxy para interactuar con los recursos y configuraciones del proyecto.

**Glosario / Comentarios:**
*   **"Propiedades de conexión"**: Traducción de *Connection properties*.
*   **"Objeto proxy"**: Refiere al patrón de diseño donde el cliente actúa como intermediario local para el servicio remoto.

2.  **¿Qué valor se necesita para crear una instancia de un objeto AIProjectClient?**

    a. Punto de conexión del proyecto (Project Endpoint) ✅

    b. La clave de autorización de Azure OpenAI

    c. Identificador de suscripción de Azure

**Justificación:** Como se observa en los ejemplos de código, el constructor de `AIProjectClient` requiere el parámetro `endpoint` (el punto de conexión del proyecto, a veces mal traducido como "final del proyecto") y un objeto de credenciales (como `DefaultAzureCredential`).

**Glosario / Comentarios:**
*   **"Punto de conexión del proyecto"**: Traducción de *Project Endpoint*.
*   **"Identificador de suscripción"**: Traducción de *Subscription ID*.
*   **"Clave de autorización"**: Traducción de *Authorization Key*.

3.  **¿Qué SDK debe usar para chatear con un modelo que se implementa en un recurso de Microsoft Foundry?**

    a. Azure OpenAI ✅

    b. Azure Machine Learning

    c. Lenguaje de Azure (Azure AI Language)

**Justificación:** Aunque se utiliza el SDK de Microsoft Foundry para configurar el cliente, la interacción de chat real se realiza utilizando un cliente compatible con **Azure OpenAI**. El método `project_client.get_openai_client()` devuelve un cliente configurado que utiliza la librería `openai` para enviar los prompts al modelo.

**Glosario / Comentarios:**
*   **"Lenguaje de Azure"**: Traducción frecuente y confusa de *Azure AI Language* (el servicio de NLP).
*   **"Recurso"**: Traducción de *Resource*.
