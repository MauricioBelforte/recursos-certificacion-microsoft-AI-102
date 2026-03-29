# Introducción al Desarrollo de Agentes de IA en Azure

## Introducción

Los modelos de IA generativa están evolucionando más allá de simples aplicaciones de chat para convertirse en **agentes inteligentes** capaces de operar de forma autónoma. Estos agentes pueden orquestar procesos de negocio complejos y coordinar cargas de trabajo, automatizando tareas que antes requerían intervención humana.

---

### Escenario de Agente Único (Single-Agent)

Una organización puede crear un agente de IA para ayudar a los empleados con sus informes de gastos. Este agente podría:

- Usar un modelo generativo y la política de gastos para responder preguntas sobre qué se puede reclamar y cuáles son los límites.
- Utilizar funciones de código para enviar automáticamente gastos recurrentes (como facturas de teléfono) o dirigir los informes al aprobador correcto según el monto.

---

### Escenario Multi-Agente (Multi-Agent)

En casos más complejos, múltiples agentes pueden colaborar. Por ejemplo:

- Un **agente de reservas de viajes** gestiona vuelos y hoteles para los empleados.
- Este agente envía automáticamente los recibos y detalles al **agente de gastos**, creando un flujo de trabajo integrado que abarca varios procesos de negocio.

En este módulo, se explorarán los conceptos fundamentales de los agentes de IA y las tecnologías disponibles en Microsoft Azure para construir soluciones basadas en agentes.

---

# ¿Qué son los Agentes de Inteligencia Artificial (AI Agents)?

Los **agentes de inteligencia artificial (AI agents)** son aplicaciones que utilizan **modelos de lenguaje (language models)** no solo para conversar, sino para comprender un objetivo y tomar acciones de forma autónoma para alcanzarlo.

A diferencia de un chatbot tradicional, un agente puede recordar el contexto de la conversación, tomar decisiones y ejecutar tareas en el mundo real.

Un agente se compone esencialemente de:

- Un **modelo de lenguaje (language model)** como núcleo de razonamiento.
- **Instrucciones (Instructions)** que definen su objetivo y personalidad.
- **Herramientas (Tools)** que le permiten realizar acciones (ej. ejecutar código, llamar a APIs).

---

## Capacidades de un Agente: Ejemplos

### Agente de Gastos (Single-Agent)

Un agente que ayuda a los empleados a gestionar sus informes de gastos demuestra tres capacidades clave:

1.  **Razonamiento basado en Conocimiento:** Utiliza un **almacén de conocimiento (knowledge store)** con las políticas de la empresa para **fundamentar el prompt (ground the prompt)** y responder preguntas con precisión.
2.  **Automatización de Tareas:** Ejecuta funciones para enviar automáticamente informes de gastos.
3.  **Toma de Decisiones:** Enruta los informes al aprobador correcto basándose en reglas de negocio.

### Agente de Viajes (Multi-Agent)

Un sistema más complejo donde un agente de viajes colabora con el agente de gastos:

1.  **Integración de Servicios Externos:** El agente de viajes reserva vuelos y hoteles a través de APIs de terceros.
2.  **Comunicación entre Agentes:** El agente de viajes se comunica con el agente de gastos para iniciar automáticamente el informe con los recibos correspondientes.
3.  **Automatización de Flujos de Trabajo:** Se automatiza todo el proceso de reserva y reporte de gastos sin intervención humana.

---

# Riesgos de Seguridad Específicos de los Agentes de IA

La autonomía de los agentes introduce nuevos vectores de ataque que deben ser considerados desde el diseño.

- **Pérdida de datos y exposición a la privacidad (Data loss and privacy exposure):** El agente accede a datos sensibles y los expone externamente por falta de controles adecuados.
- **Ataques de inyección de prompts (Prompt injection attacks):** Un usuario malicioso manipula la entrada para anular el comportamiento previsto del agente y ejecutar acciones no deseadas.
- **Escalada de privilegios (Privilege escalation):** Controles de acceso débiles permiten al agente realizar acciones más allá de los permisos que se le asignaron.
- **Envenenamiento de datos (Data poisoning):** Se corrompen los datos de entrenamiento o de fundamentación del agente, lo que provoca que genere salidas inseguras o incorrectas.
- **Vulnerabilidades de la cadena de suministro (Supply chain vulnerabilities):** Herramientas o plugins de terceros introducen brechas de seguridad en el flujo de trabajo del agente.
- **Dependencia excesiva de la autonomía (Over-reliance on autonomy):** El agente ejecuta acciones críticas (como un reembolso) sin la validación o supervisión humana adecuada.
- **Registro y auditoría inadecuados (Inadequate logging and auditing):** La falta de registros detallados impide rastrear las acciones del agente y detectar usos indebidos.
- **Inversión del modelo (Model inversion):** Un atacante utiliza las salidas del agente para inferir datos confidenciales con los que fue entrenado.

---

# Buenas Prácticas de Seguridad para Agentes

Para mitigar estos riesgos, es fundamental adoptar un enfoque de "seguridad por diseño".

- **Control de acceso estricto:** Aplicar **control de acceso basado en roles (RBAC)** y el principio de **privilegios mínimos (least-privilege)**. El agente solo debe poder acceder a lo estrictamente necesario.
- **Validar todas las entradas:** Implementar capas de filtrado para detectar y bloquear ataques de inyección antes de que lleguen al modelo.
- **Supervisión humana para acciones críticas:** No permitir que los agentes tomen decisiones de alto riesgo de forma autónoma. Implementar un bucle de "humano en el proceso" (human-in-the-loop) para aprobaciones.
- **Trazabilidad completa:** Mantener registros detallados de todas las acciones del agente para poder auditarlas.
- **Auditar la cadena de suministro:** Revisar periódicamente las dependencias externas (plugins, APIs) en busca de vulnerabilidades.
- **Mantenimiento continuo del modelo:** Reentrenar y validar los modelos para detectar desviaciones o intentos de envenenamiento de datos.

---

# Opciones para el Desarrollo de Agentes de IA

Los agentes de IA, a diferencia de las aplicaciones tradicionales, pueden razonar, actuar de forma autónoma y colaborar para alcanzar objetivos. Su construcción requiere frameworks y herramientas especializadas.

---

## Frameworks de IA Tradicionales vs. Frameworks de Agentes

### Frameworks de IA Tradicionales

Estos frameworks se centran en integrar funcionalidades inteligentes en aplicaciones existentes para mejorar la experiencia del usuario.

- **Personalización:** Recomiendan contenido (ej. Netflix).
- **Automatización:** Simplifican tareas repetitivas (ej. chatbots de soporte).
- **Experiencia Mejorada:** Habilitan interacciones naturales (ej. asistentes de voz como Siri).

### Frameworks de Agentes de IA

Van un paso más allá, permitiendo el desarrollo de sistemas autónomos orientados a objetivos.

- **Colaboración:** Soportan la comunicación entre múltiples agentes para resolver problemas complejos.
- **Automatización de Tareas:** Orquestan flujos de trabajo de varios pasos y delegan tareas dinámicamente.
- **Adaptación Contextual:** Permiten a los agentes percibir su entorno y tomar decisiones basadas en datos en tiempo real.

---

## Ecosistema de Herramientas para Agentes en Microsoft

Microsoft ofrece un amplio abanico de soluciones para construir agentes, adaptadas a diferentes perfiles y necesidades.

### 1. Microsoft Foundry Agent Service

Servicio administrado en Azure para crear y gestionar agentes a nivel empresarial. Se basa en la API de Asistentes de OpenAI pero la extiende con más opciones de modelos, integración de datos y seguridad de nivel empresarial. Se utiliza con el SDK de OpenAI y el SDK de Azure Foundry.

### 2. OpenAI Assistants API

Es la base sobre la que se construye el Foundry Agent Service. Permite crear agentes, pero está limitada a los modelos de OpenAI. En Azure, se puede usar a través de Azure OpenAI, aunque Foundry Agent Service ofrece mayor flexibilidad.

### 3. Microsoft Agent Framework

Un kit de desarrollo ligero diseñado específicamente para construir agentes de IA y orquestar soluciones multi-agente.

### 4. AutoGen

Framework de código abierto de Microsoft Research, ideal para la investigación y experimentación rápida con agentes conversacionales.

### 5. Microsoft 365 Agents SDK

Permite a los desarrolladores crear agentes auto-hospedados que se pueden entregar a través de múltiples canales, no solo Microsoft 365, sino también Slack, Messenger, etc.

### 6. Microsoft Copilot Studio

Entorno de desarrollo **low-code** para que los **desarrolladores ciudadanos (citizen developers)** creen e implementen agentes rápidamente, con una fuerte integración con el ecosistema de Microsoft 365.

---

## ¿Cómo Elegir la Herramienta Adecuada?

| Perfil de Usuario / Escenario                               | Solución Recomendada                    | Capacidades Clave                                                           | Caso de Uso Típico                                                                   |
| :---------------------------------------------------------- | :-------------------------------------- | :-------------------------------------------------------------------------- | :----------------------------------------------------------------------------------- |
| **Usuarios de negocio** (sin experiencia en código)         | **Copilot Studio (versión "lite")**     | Creación de agentes de forma declarativa (describiendo lo que se necesita). | Automatizar tareas diarias y sencillas.                                              |
| **Usuarios de negocio** (con experiencia en Power Platform) | **Copilot Studio (versión completa)**   | Combina herramientas `low-code` con conocimiento de dominio.                | Construir soluciones agénticas `low-code` y extender herramientas de productividad.  |
| **Desarrolladores profesionales** (extendiendo M365)        | **Microsoft 365 Agents SDK**            | Flexibilidad total para crear extensiones complejas para canales de M365.   | Integraciones personalizadas y comportamientos avanzados en el ecosistema Microsoft. |
| **Desarrolladores profesionales** (en Azure)                | **Microsoft Foundry Agent Service**     | Se integra con toda la infraestructura de Azure (IA, backend, seguridad).   | Crear soluciones agénticas escalables y personalizadas en Azure.                     |
| **Desarrolladores / Investigadores** (sistemas complejos)   | **Microsoft Agent Framework / AutoGen** | Permite crear sistemas multi-agente y probar patrones de orquestación.      | Crear sistemas de agentes complejos y orquestados en diversos entornos.              |

> **Nota:** Existe una superposición entre las herramientas. La elección final también puede depender de la familiaridad con el lenguaje de programación y otras preferencias del equipo de desarrollo.

---

# Servicio Microsoft Foundry Agent

El **Servicio de Agentes de Microsoft Foundry (Microsoft Foundry Agent Service)** es un servicio administrado dentro de Azure que permite crear, probar y gestionar agentes de IA. Ofrece tanto una experiencia de desarrollo visual en el portal de Microsoft Foundry como un enfoque de "código primero" a través del SDK de Microsoft Foundry.

---

## Componentes de un Agente en Foundry

Los agentes desarrollados con este servicio se componen de los siguientes elementos:

- **Modelo (Model):** Un modelo de IA generativa implementado que actúa como el cerebro del agente, permitiéndole razonar y generar respuestas. Es compatible con modelos populares de OpenAI y una selección del catálogo de Microsoft Foundry.

- **Conocimiento (Knowledge):** Son las fuentes de datos que permiten al agente **fundamentar (ground)** sus respuestas con información contextual. Las fuentes pueden incluir:
  - Resultados de búsqueda en Internet a través de Microsoft Bing.
  - Un índice de **Azure AI Search**.
  - Documentos y datos propios del usuario.

- **Herramientas (Tools):** Son funciones programáticas que otorgan al agente la capacidad de realizar acciones. Existen herramientas integradas y personalizadas:
  - **Integradas:** Acceso a Azure AI Search, Bing Search y un **intérprete de código (code interpreter)** para generar y ejecutar código Python sobre la marcha.
  - **Personalizadas:** Se pueden crear herramientas propias con código personalizado o a través de **Azure Functions**.

---

## Gestión de Conversaciones: Hilos (Threads)

Las interacciones entre un usuario y un agente se gestionan dentro de un **hilo (thread)**.

Un hilo no solo conserva el historial de los mensajes intercambiados, sino que también almacena los recursos de datos generados o utilizados durante la conversación, como archivos. Esto permite que el agente mantenga el contexto a lo largo de una interacción prolongada.

---

# Laboratorio: Exploración del desarrollo de Agentes de IA

En este ejercicio práctico, utilizaremos el **Servicio de Agentes de Azure AI (Azure AI Agent Service)** en el portal de **Microsoft Foundry** para crear un agente sencillo que ayude a los empleados con las solicitudes de reembolso de gastos.

## 1. Creación del Proyecto y el Agente

Para comenzar, necesitamos un entorno de trabajo. En el portal de **Microsoft Foundry**, creamos un nuevo proyecto.

- **Configuración:** Seleccionamos un nombre para el proyecto, la suscripción y el grupo de recursos. Es importante elegir una región recomendada que tenga cuota disponible para los modelos.
- **Creación del Agente:** Una vez dentro del proyecto, utilizamos la opción **"Crear agente" (Create agent)** y le asignamos el nombre `expense-agent`. Al crearlo, se nos dirige automáticamente al **Entorno de pruebas (Playground)** del agente, donde ya hay un modelo base seleccionado.

## 2. Configuración del Comportamiento y Herramientas

El objetivo de este agente es responder preguntas sobre la política de gastos de la empresa y ayudar a generar reclamaciones.

### A. Instrucciones (Instructions)

Definimos la personalidad y las reglas del agente en el campo de instrucciones.

> **Instrucción:**
> "You are an AI assistant for corporate expenses. You answer questions about expenses based on the expenses policy data. If a user wants to submit an expense claim, you get their email address, a description of the claim, and the amount to be claimed and write the claim details to a text file that the user can download."
>
> _(Traducción: Eres un asistente de IA para gastos corporativos. Respondes preguntas sobre gastos basándote en los datos de la política de gastos. Si un usuario quiere enviar una reclamación de gastos, obtienes su dirección de correo electrónico, una descripción de la reclamación y el monto a reclamar, y escribes los detalles de la reclamación en un archivo de texto que el usuario puede descargar.)_

### B. Conocimiento (Knowledge)

Para que el agente conozca las reglas específicas de "Contoso", cargamos un documento de política (`Expenses_policy.docx`) en la sección de **Herramientas (Tools)** > **Cargar archivos (Upload files)**.

- Esto habilita automáticamente la herramienta de **Búsqueda de archivos (File Search)**, permitiendo al agente "leer" el documento para fundamentar sus respuestas.

### C. Herramientas de Acción (Tools)

Para que el agente pueda _hacer_ algo (en este caso, generar un archivo de reclamación), añadimos la herramienta **Intérprete de código (Code interpreter)**.

- Esta herramienta permite al agente escribir y ejecutar código Python en un entorno seguro (sandbox) para procesar datos o crear archivos.

## 3. Prueba del Agente

Ahora interactuamos con el agente en el chat del **Entorno de pruebas (Playground)** para verificar su funcionamiento.

1.  **Consulta de Política:**
    Preguntamos: _"What's the maximum I can claim for meals?"_ (¿Cuál es el máximo que puedo reclamar por comidas?).
    - **Resultado esperado:** El agente busca en el documento de política cargado y responde con la información correcta.

2.  **Acción Compleja:**
    Indicamos: _"I'd like to submit a claim for a meal."_ (Me gustaría enviar una reclamación por una comida.).
    - El agente, siguiendo sus instrucciones, nos pedirá el correo electrónico, la descripción y el monto.
    - Al proporcionar los datos (ej. "Breakfast cost me $20"), el agente utiliza el **Intérprete de código** para generar un archivo de texto con los detalles y nos proporciona un enlace de descarga.

## 4. Integración (Opcional)

Si quisiéramos llevar este agente a una aplicación real, la pestaña **Código (Code)** en el entorno de desarrollo nos proporciona ejemplos en Python. Allí podemos ver cómo utilizar el **SDK de Proyectos de Azure AI (Azure AI Projects SDK)** para conectar nuestra aplicación con el agente, enviar mensajes y procesar las respuestas programáticamente.

---

# Resumen del Módulo: Introducción al Desarrollo de Agentes

En este módulo, ha explorado los conceptos fundamentales de los agentes de inteligencia artificial y las herramientas disponibles para su desarrollo.

Los puntos clave aprendidos incluyen:

- **Definición de Agentes de IA:** Comprendió qué son los agentes, sus componentes (modelo, instrucciones, herramientas) y los riesgos de seguridad asociados.
- **Opciones de Desarrollo:** Descubrió el ecosistema de herramientas de Microsoft para crear agentes, desde soluciones `low-code` como **Copilot Studio** hasta frameworks para desarrolladores profesionales como **Foundry Agent Service** y **AutoGen**.
- **Creación de un Agente Básico:** Aprendió a utilizar las herramientas visuales del **Servicio de Agentes de Foundry** en el portal para construir un agente simple, configurando sus instrucciones, conocimiento y herramientas.

---

# Evaluación del módulo

1.  **¿Cuál de las siguientes describe mejor un agente de IA?**

    a. Desarrollador que se especializa en la creación de soluciones de inteligencia artificial generativas.

    b. Un servicio de software que usa inteligencia artificial para ayudar a los usuarios con información y automatización de tareas. ✅

    c. Marketplace para componentes de software de inteligencia artificial listos para usar.

**Justificación:** Un **Agente de IA** se define como un sistema de software autónomo que utiliza modelos de IA para percibir su entorno, razonar y tomar medidas para ayudar a los usuarios o completar tareas, a diferencia de un simple chatbot o un rol humano.

2.  **¿Qué servicio de desarrollo de agentes de IA ofrece una elección de modelos de IA generativos de varios proveedores en el catálogo de modelos de Microsoft Foundry?**

    a. Servicio microsoft Foundry Agent ✅

    b. API de asistentes de OpenAI

    c. Microsoft 365 Copilot Chat

**Justificación:** El **Servicio de Agentes de Microsoft Foundry (Microsoft Foundry Agent Service)** permite utilizar una amplia gama de modelos disponibles en el catálogo de Foundry (incluyendo modelos de OpenAI y otros de código abierto), mientras que la API de Asistentes de OpenAI se limita a los modelos de OpenAI.

**Glosario / Comentarios:**

- **"Servicio microsoft Foundry Agent"**: Traducción de _Microsoft Foundry Agent Service_.

3.  **¿Qué elemento de un agente de Foundry Agent Service permite fundamentar con datos contextuales?**

    a. Modelo

    b. Herramienta de intérprete de código

    c. Conocimientos ✅

**Justificación:** El componente de **Conocimientos (Knowledge)** es el encargado específicamente de almacenar y gestionar los datos externos (archivos, índices de búsqueda) que el agente utiliza para **fundamentar (ground)** sus respuestas y evitar alucinaciones.

**Glosario / Comentarios:**

- **"Conocimientos"**: Traducción de _Knowledge_.
- **"Herramienta de intérprete de código"**: Traducción de _Code Interpreter Tool_.
