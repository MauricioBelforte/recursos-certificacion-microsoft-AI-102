# Integración de herramientas de MCP con Azure AI Agents

## Introducción

Los agentes de inteligencia artificial son capaces de realizar una amplia gama de tareas, pero muchas de ellas requieren interactuar con herramientas externas al Modelo de Lenguaje Grande (**LLM**). Es común que los agentes necesiten acceder a APIs, bases de datos o servicios internos. La integración y el mantenimiento manual de estas conexiones pueden volverse complejos rápidamente, especialmente a medida que el sistema crece o cambia con frecuencia.

Los servidores del **Protocolo de Contexto de Modelo (Model Context Protocol - MCP)** pueden ayudar a resolver este problema simplificando la integración con los agentes de IA. Conectar un agente de Azure AI a un servidor MCP proporciona al agente un catálogo de herramientas accesible bajo demanda. Este enfoque hace que la solución de inteligencia artificial sea más robusta, escalable y fácil de mantener.

### Escenario de Ejemplo: Gestión de Inventario

Suponga que trabaja para un minorista especializado en cosméticos. Su equipo desea crear un asistente de IA que ayude a administrar el inventario verificando los niveles de existencias de productos y analizando las tendencias de ventas recientes.

Con un servidor MCP, puede conectar el asistente a un conjunto de herramientas estandarizadas capaces de realizar evaluaciones de inventario y proporcionar recomendaciones al equipo, sin necesidad de codificar cada integración de forma rígida en el agente.

En este módulo, aprenderá a:

- Configurar un cliente y un servidor MCP.
- Conectar herramientas a un agente de Azure AI de forma dinámica.

---

# Comprender el descubrimiento de herramientas de MCP

A medida que los agentes de inteligencia artificial se vuelven más capaces, la gama de herramientas y servicios a los que pueden acceder también crece. Sin embargo, el registro, la administración, la actualización y la integración de nuevas herramientas pueden volverse tareas complejas y lentas rápidamente. La **Detección dinámica de herramientas (Dynamic Tool Discovery)** ayuda a resolver este problema al permitir que los agentes busquen y utilicen herramientas automáticamente en tiempo de ejecución.

## Ventajas del Protocolo de Contexto de Modelo para agentes de IA

El **Protocolo de Contexto de Modelo (Model Context Protocol - MCP)** proporciona varias ventajas que mejoran las funcionalidades y la flexibilidad de los agentes de IA:

- **Detección dinámica de herramientas:** Los agentes de IA pueden recibir automáticamente una lista de las herramientas disponibles desde un servidor, junto con descripciones de sus funciones. A diferencia de las APIs tradicionales, que a menudo requieren codificación manual para cada integración y actualizaciones cada vez que cambia la API, MCP habilita un enfoque de "integrar una vez" que mejora la adaptabilidad y reduce el mantenimiento.
- **Interoperabilidad entre LLM:** MCP funciona perfectamente con diferentes **Modelos de Lenguaje Grande (LLM)**, lo que permite a los desarrolladores cambiar o evaluar modelos principales para mejorar el rendimiento sin tener que volver a trabajar en las integraciones.
- **Seguridad estandarizada:** MCP proporciona un método de autenticación coherente, lo que simplifica el acceso seguro entre varios servidores MCP. Esto elimina la necesidad de administrar claves independientes o protocolos de autenticación para cada API, facilitando el escalado de las implementaciones de agentes de IA.

## ¿Qué es la detección dinámica de herramientas?

La detección dinámica de herramientas es un mecanismo que permite a un agente de IA descubrir herramientas externas disponibles sin necesidad de tener conocimiento codificado de forma rígida (**hard-coded**) de cada una. En lugar de agregar o actualizar manualmente todas las herramientas que el agente puede usar, este consulta un servidor MCP centralizado. Este servidor actúa como un catálogo activo, exponiendo herramientas que el agente puede comprender y llamar.

Este enfoque significa que:

- Las herramientas se pueden agregar, actualizar o quitar de forma centralizada sin modificar el código del agente.
- Los agentes siempre pueden usar la versión más reciente de una herramienta, mejorando la precisión y la confiabilidad.
- La complejidad de administrar herramientas se traslada del agente a un servicio dedicado.

## ¿Cómo habilita MCP la detección dinámica de herramientas?

Un servidor MCP hospeda un conjunto de funciones que se exponen como herramientas mediante el decorador `@mcp.tool`. Las herramientas son un tipo primitivo en el MCP que permite a los servidores exponer funcionalidad ejecutable a los clientes.

Un cliente puede conectarse al servidor y capturar estas herramientas de forma dinámica. A continuación, el cliente genera envoltorios (**wrappers**) de funciones que se agregan a las definiciones de herramientas del agente de Azure AI. Esta configuración crea una canalización flexible:

1.  El **Servidor MCP** hospeda las herramientas disponibles.
2.  El **Cliente MCP** detecta dinámicamente las herramientas.
3.  El **Agente de Azure AI** utiliza las herramientas disponibles para responder a las solicitudes del usuario.

## ¿Por qué usar la detección dinámica de herramientas con MCP?

Este enfoque proporciona varias ventajas clave:

- **Escalabilidad:** Agregue fácilmente nuevas herramientas o actualice las existentes sin volver a implementar los agentes.
- **Modularidad:** Los agentes pueden mantenerse simples, centrándose en la delegación en lugar de administrar los detalles de cada herramienta.
- **Mantenibilidad:** La administración centralizada de herramientas reduce la duplicación de código y los errores.
- **Flexibilidad:** Admite diversos tipos de herramientas y flujos de trabajo complejos agregando funcionalidades según sea necesario.

La detección dinámica de herramientas es especialmente útil en entornos donde las herramientas evolucionan rápidamente o donde muchos equipos administran diferentes APIs y servicios. El uso de herramientas permite a los agentes de IA adaptarse a funcionalidades cambiantes en tiempo real, interactuar con sistemas externos de forma segura y realizar acciones que van más allá de la simple generación de lenguaje.

---

# Integración de herramientas de agente mediante un servidor MCP y un cliente

Para conectar dinámicamente las herramientas al agente de Azure AI, primero necesita una configuración del **Protocolo de Contexto de Modelo (Model Context Protocol - MCP)** en funcionamiento. Esto incluye tanto el **Servidor MCP**, que hospeda el catálogo de herramientas, como el **Cliente MCP**, que captura esas herramientas y las hace utilizables por el agente.

## ¿Qué es el servidor MCP?

El **Servidor MCP** actúa como un registro para las herramientas que puede usar el agente. Puede inicializar el servidor MCP utilizando `FastMCP("server-name")`. La clase `FastMCP` utiliza sugerencias de tipo de Python (**type hints**) y cadenas de documentación (**docstrings**) para generar automáticamente definiciones de herramientas, lo que facilita la creación y el mantenimiento de herramientas de MCP.

A continuación, estas definiciones se sirven a través de HTTP cuando el cliente lo solicita. Dado que las definiciones de herramientas residen en el servidor, puede actualizar o agregar nuevas herramientas en cualquier momento, sin tener que modificar ni volver a implementar (**redeploy**) el agente.

## ¿Qué es el cliente MCP?

Un **Cliente MCP** estándar actúa como un puente entre el servidor MCP y el **Servicio de Agentes de Azure AI (Azure AI Agent Service)**. El cliente inicializa una sesión y se conecta al servidor. Después, realiza tres tareas clave:

1.  **Detectar herramientas:** Identifica las herramientas disponibles del servidor MCP mediante `session.list_tools()`.
2.  **Generar envoltorios:** Crea códigos auxiliares (**stubs**) de funciones de Python que encapsulan las herramientas.
3.  **Registrar funciones:** Registra esas funciones con el agente.

Esto permite al agente llamar a cualquier herramienta que aparezca en el catálogo de MCP como si fuera una función nativa, todo ello sin lógica codificada de forma rígida (**hard-coded**).

## Registro de herramientas con un agente de Azure AI

Cuando se inicializa una sesión de cliente MCP, el cliente puede extraer dinámicamente herramientas del servidor. Se puede invocar una herramienta MCP mediante `session.call_tool(tool_name, tool_args)`.

Las herramientas deben encapsularse en una función asincrónica para que el agente pueda invocarlas. Por último, esas funciones se agrupan en un `FunctionTool`, pasan a formar parte del conjunto de herramientas del agente y están disponibles durante el tiempo de ejecución para cualquier solicitud de usuario.

### Resumen del flujo de integración

1.  El **Servidor MCP** hospeda definiciones de herramientas decoradas con `@mcp.tool`.
2.  El **Cliente MCP** inicializa una conexión al servidor.
3.  El **Cliente MCP** captura las definiciones de herramientas disponibles con `session.list_tools()`.
4.  Cada herramienta está encapsulada en una función asincrónica que invoca `session.call_tool`.
5.  Las funciones de la herramienta se agrupan en un objeto `FunctionTool` que el agente puede usar.
6.  El `FunctionTool` se registra en el conjunto de herramientas del agente.

Ahora el agente puede acceder a las herramientas e invocarlas a través de la interacción con lenguaje natural. Al configurar el cliente y el servidor MCP, se crea una separación limpia entre la administración de herramientas y la lógica del agente, lo que permite que el sistema se adapte rápidamente a medida que estén disponibles nuevas herramientas.

---

# Uso de agentes de Azure AI con servidores MCP

Para mejorar el agente de Microsoft Foundry, conéctelo a los servidores del **Protocolo de Contexto de Modelo (Model Context Protocol - MCP)**. Los servidores MCP proporcionan herramientas y datos contextuales que el agente puede usar para realizar tareas, ampliando sus funcionalidades más allá de las funciones integradas. **Azure AI Agent Service** incluye compatibilidad con servidores MCP remotos, lo que permite al agente conectarse rápidamente al servidor y acceder a sus herramientas.

Cuando se usa el **Servicio de Agentes de Microsoft Foundry (Microsoft Foundry Agent Service)** para conectarse al servidor MCP, no es necesario crear manualmente una sesión de cliente MCP ni agregar ninguna herramienta de función al agente. En su lugar, cree un objeto de herramienta MCP que se conecte al servidor MCP.

## Integración de servidores MCP remotos

Para conectarse a un servidor MCP, necesita:

- Un **Punto de conexión (Endpoint)** de servidor MCP remoto (por ejemplo, `https://api.githubcopilot.com/mcp/`).
- Un agente de Microsoft Foundry configurado para usar la herramienta MCP.

Puede conectarse a varios servidores MCP agregándolos al agente como herramientas independientes. Cada objeto `MCPTool` puede incluir los siguientes parámetros:

- `server_label`: Un identificador único para el servidor MCP (por ejemplo, "GitHub").
- `server_url`: La dirección URL del servidor MCP.
- `allowed_tools` (opcional): Una lista de herramientas específicas a las que puede acceder el agente.
- `require_approval` (opcional): Valor booleano que determina si las invocaciones de herramientas requieren aprobación humana. Si se establece en `true`, el agente se pausará y esperará la aprobación antes de invocar cualquier herramienta en el servidor MCP.

La herramienta MCP también admite encabezados personalizados, lo que le permite pasar:

- Claves de autenticación (claves de API, tokens de OAuth).
- Otros encabezados necesarios para el servidor MCP.

## Uso de herramientas

Al usar el objeto de herramienta MCP de Azure, no es necesario encapsular las herramientas de función ni invocar `session.call_tool`. En su lugar, las herramientas se invocan automáticamente cuando es necesario durante la ejecución de un agente.

Para invocar automáticamente las herramientas de MCP:

1.  Cree el objeto `MCPTool` con la etiqueta y la dirección URL del servidor.
2.  Use `update_headers` para aplicar los encabezados requeridos por el servidor.
3.  Use el parámetro `require_approval` para determinar si se requiere aprobación. Los valores admitidos son:
    - `always`: Un desarrollador debe proporcionar aprobación para cada llamada.
    - `never`: No se requiere ninguna aprobación.
4.  Agregue el objeto `MCPTool` a la lista de herramientas del agente.
5.  Invoque una solicitud en el agente; debería ver los resultados de las herramientas invocadas en la respuesta.

### Aprobación de Herramientas

Si el modelo intenta invocar una herramienta en el servidor MCP con la aprobación necesaria, obtendrá una `mcp_approval_request` en la respuesta del agente. Esto incluye información sobre qué herramienta se está invocando y puede usar esta información para decidir si aprobar la solicitud.

Para aprobar, envíe un mensaje de seguimiento con el objeto `mcp_approval_response`, que incluye un valor `approval_request_id` y un booleano `approve`.

La integración de MCP es un paso clave para crear agentes de IA más enriquecidos y conscientes del contexto. A medida que crece el ecosistema de MCP, tendrá aún más oportunidades para incorporar herramientas especializadas a los flujos de trabajo y ofrecer soluciones más inteligentes y dinámicas.

---

# Resumen del Módulo

En este módulo, ha aprendido a integrar herramientas externas con el **Servicio de Agentes de Microsoft Foundry (Microsoft Foundry Agent Service)** mediante el **Protocolo de Contexto de Modelo (Model Context Protocol - MCP)**.

Al conectar el agente a un servidor MCP, puede detectar y registrar herramientas dinámicamente en tiempo de ejecución sin necesidad de codificar las APIs manualmente ni volver a implementar (**redeploy**) el agente.

Con un **Cliente MCP**, ha generado envoltorios (**wrappers**) de funciones a partir de las herramientas detectadas y los ha conectado directamente al agente. Esta integración permite que el agente se adapte a conjuntos de herramientas en evolución y ayuda a crear soluciones de inteligencia artificial más flexibles y escalables.

- Crear su propia solución de herramientas MCP con el **Servicio de Agentes de Microsoft Foundry (Microsoft Foundry Agent Service)**.
