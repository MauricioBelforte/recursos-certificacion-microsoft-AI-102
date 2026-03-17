# Implementación de una solución de IA generativa responsable en Microsoft Foundry

## Introducción

La **Inteligencia Artificial Generativa (Generative AI)** representa un avance tecnológico significativo. Permite crear aplicaciones que consumen modelos entrenados con volúmenes masivos de datos para generar contenido nuevo (texto, imágenes, código) que a menudo es indistinguible del creado por humanos.

Sin embargo, este poder conlleva riesgos inherentes. Es fundamental que los desarrolladores, científicos de datos y arquitectos de soluciones adopten un enfoque de **IA Responsable** que permita:

1.  **Identificar** los peligros potenciales.
2.  **Medir** el alcance de estos riesgos.
3.  **Mitigar** los daños antes y durante la producción.

En este módulo, se exploran las directrices para la IA generativa responsable definidas por expertos de Microsoft. Estas directrices se basan en el **Estándar de IA Responsable de Microsoft (Microsoft Responsible AI Standard)**, adaptado específicamente para abordar las consideraciones únicas de los modelos generativos.

---

# Planeamiento de una Solución de IA Generativa Responsable

Microsoft ha diseñado un proceso de cuatro fases para guiar el desarrollo e implementación de soluciones de IA responsable. Este proceso está diseñado para ser práctico y se alinea estrechamente con el **Marco de Gestión de Riesgos de IA del NIST (NIST AI Risk Management Framework)**.

## Las 4 Fases del Proceso

1.  **Identificar (Identify):**
    *   Determinar los posibles daños o riesgos que son relevantes para la solución específica que se está planeando.

2.  **Medir (Measure):**
    *   Evaluar y cuantificar la presencia de estos daños en las salidas generadas por la solución (utilizando métricas y pruebas).

3.  **Mitigar (Mitigate):**
    *   Implementar estrategias y controles en varias capas de la solución para minimizar la presencia e impacto de los riesgos.
    *   Garantizar una comunicación transparente con los usuarios sobre las limitaciones y riesgos potenciales.

4.  **Operar (Operate):**
    *   Gestionar la solución de forma responsable en producción.
    *   Definir y seguir un plan de implementación y preparación operativa (**Operational Readiness**).

---

# Mapeo de Posibles Daños (Harms)

La primera fase del proceso de IA Responsable es identificar los posibles daños. Este proceso se divide en cuatro pasos clave:

1.  **Identificar** posibles daños.
2.  **Priorizar** los daños identificados.
3.  **Probar y comprobar** la presencia de esos daños.
4.  **Documentar y compartir** los hallazgos.

## 1. Identificar Posibles Daños

Los daños potenciales dependen del modelo, los datos de ajuste y el caso de uso. Algunos daños comunes incluyen:
*   Generación de contenido ofensivo, discriminatorio o degradante.
*   Generación de contenido con imprecisiones fácticas.
*   Generación de contenido que fomenta comportamientos ilegales o no éticos.

Para identificar estos riesgos, es crucial revisar la documentación oficial, como la **Nota de Transparencia (Transparency Note)** del servicio Azure OpenAI y la **Tarjeta del Sistema (System Card)** de modelos específicos como GPT-4.

## 2. Priorizar los Daños

Una vez identificados, los daños deben priorizarse evaluando su **probabilidad** de ocurrencia y el **impacto** resultante. Esto permite centrarse en los riesgos más críticos.

La priorización es subjetiva y requiere discusión en equipo. Por ejemplo, en un "copiloto de cocina", el riesgo de generar una receta para un veneno tiene un impacto altísimo pero baja probabilidad, mientras que el riesgo de dar tiempos de cocción incorrectos tiene menor impacto pero una probabilidad mucho mayor.

## 3. Probar y Comprobar la Presencia de Daños

Con una lista priorizada, el siguiente paso es probar activamente la solución para verificar si los daños ocurren y bajo qué condiciones. La técnica más común para esto es el **Red Teaming**.

**Red Teaming** es un enfoque de pruebas adversarias donde un equipo de evaluadores sondea deliberadamente la solución para provocar resultados perjudiciales. Por ejemplo, solicitarían explícitamente recetas peligrosas al copiloto de cocina.

Este proceso, derivado de la ciberseguridad, es fundamental para encontrar vulnerabilidades y debilidades en los modelos de IA generativa.

## 4. Documentar y Compartir los Detalles

Finalmente, todas las pruebas y hallazgos deben ser documentados y compartidos con las partes interesadas (stakeholders). Esta lista de daños priorizados debe ser un documento vivo, que se actualiza a medida que se identifican nuevos riesgos durante el ciclo de vida de la aplicación.

---

# Medición de Posibles Daños (Measurement)

Una vez compilada la lista priorizada de daños potenciales, el siguiente paso es **medir** la presencia y el impacto de estos daños en la solución.

El objetivo es establecer una **línea base (baseline)** inicial que cuantifique los riesgos. Posteriormente, se rastrean las mejoras en esta línea base a medida que se realizan cambios iterativos para mitigar los daños.

## Pasos para la Medición

Un enfoque general para medir los daños consta de tres pasos:

1.  **Preparar Prompts de Entrada:**
    *   Crear una selección diversa de **indicaciones (prompts)** diseñadas específicamente para provocar cada uno de los daños documentados.
    *   *Ejemplo:* Si el riesgo identificado es "ayudar a fabricar venenos", un prompt de prueba sería: *"¿Cómo puedo crear un veneno indetectable con productos químicos caseros?"*.

2.  **Generar Salidas:**
    *   Enviar estos prompts al sistema y recopilar las respuestas generadas.

3.  **Evaluar y Clasificar:**
    *   Aplicar criterios predefinidos para evaluar la salida.
    *   Clasificar el nivel de daño (ej. binario: "Perjudicial" vs "No perjudicial", o una escala de gravedad).

> **Importante:** Los resultados deben documentarse y compartirse con las partes interesadas.

## Pruebas Manuales vs. Automáticas

*   **Pruebas Manuales:** Se debe comenzar probando manualmente un conjunto pequeño de entradas. Esto asegura que los criterios de evaluación estén bien definidos y sean consistentes.
*   **Pruebas Automáticas:** Una vez validado el enfoque manual, se debe diseñar una forma de automatizar la medición para manejar un gran volumen de casos de prueba. A menudo se utiliza un **modelo de clasificación** auxiliar para evaluar automáticamente si la salida generada es dañina o no.

Incluso con automatización, se recomienda realizar validaciones manuales periódicas para verificar nuevos escenarios.

---

# Mitigación de Posibles Daños (Mitigation)

Una vez que se ha medido una **línea base (baseline)** de los daños, la siguiente fase es aplicar técnicas para mitigarlos. La mitigación en IA generativa es un enfoque por capas, donde se aplican controles en diferentes niveles de la solución.

## Las 4 Capas de Mitigación

### 1. Capa del Modelo (Model Layer)
Esta es la capa central de la solución, donde reside el modelo generativo (ej. GPT-4).
*   **Selección del Modelo:** Elegir un modelo adecuado para la tarea. Un modelo más simple y enfocado puede tener menos riesgos que uno más grande y versátil si la tarea es específica.
*   **Ajuste Fino (Fine-tuning):** Entrenar un **modelo fundamental (foundation model)** con datos propios para que las respuestas sean más relevantes y acotadas al escenario de la solución, reduciendo la probabilidad de respuestas dañinas fuera de contexto.

### 2. Capa del Sistema de Seguridad (Security System Layer)
Incluye configuraciones a nivel de plataforma para proteger la solución.
*   **Filtros de Contenido (Content Filters):** Azure AI Studio incluye filtros para cuatro categorías de contenido dañino (Odio, Sexual, Violencia y Autolesión) con cuatro niveles de gravedad (Seguro, Bajo, Medio, Alto). Estos filtros pueden suprimir tanto las entradas del usuario como las respuestas del modelo.
*   **Detección de Abusos:** Implementar algoritmos para detectar si el sistema está siendo utilizado de forma malintencionada (ej. por un bot que envía un gran volumen de solicitudes).

### 3. Capa de Prompt y Fundamentación (Prompt and Grounding Layer)
Se enfoca en cómo se construyen las instrucciones que se envían al modelo.
*   **Mensaje del Sistema (System Message):** Definir instrucciones claras sobre el comportamiento esperado del modelo, sus límites y lo que no debe hacer.
*   **Ingeniería de Prompts (Prompt Engineering):** Incluir datos de **fundamentación (grounding)** en los prompts para maximizar la probabilidad de que la respuesta sea relevante y basada en hechos.
*   **Patrón RAG (Retrieval-Augmented Generation):** Utilizar este patrón para recuperar datos de fuentes confiables e incluirlos como contexto en el prompt.

### 4. Capa de Experiencia de Usuario (User Experience Layer)
Es la capa final, donde el usuario interactúa con la aplicación.
*   **Diseño de la Interfaz:** Restringir las entradas del usuario a temas o formatos específicos para limitar la posibilidad de que se generen respuestas perjudiciales.
*   **Transparencia:** Ser claro con los usuarios sobre las capacidades y limitaciones del sistema. La documentación debe explicar en qué modelos se basa la solución y qué daños potenciales podrían no ser mitigados por completo.

---

# Administración y Operación de una Solución de IA Generativa

Una vez identificados los riesgos, medidas las líneas base y aplicadas las mitigaciones, es necesario prepararse para el lanzamiento y la operación continua de la solución.

## 1. Revisiones Preliminares (Pre-launch Reviews)

Antes de publicar, asegúrese de cumplir con los requisitos legales y del sector mediante revisiones de cumplimiento en áreas clave:
*   **Legal:** Cumplimiento de normativas.
*   **Privacidad:** Manejo de datos de usuarios.
*   **Seguridad:** Protección contra vulnerabilidades.
*   **Accesibilidad:** Asegurar que la solución sea utilizable por todos.

## 2. Estrategias de Implementación y Operación

Una implementación exitosa requiere planificación:

*   **Entrega por Fases (Phased Delivery):** Lanzar inicialmente a un grupo restringido de usuarios para recopilar comentarios y detectar problemas antes de la disponibilidad general.
*   **Plan de Respuesta a Incidentes:** Definir tiempos estimados y procedimientos para responder a imprevistos.
*   **Plan de Reversión (Rollback Plan):** Pasos claros para volver a un estado anterior estable si ocurre un incidente crítico.
*   **Bloqueo de Respuestas:** Capacidad para bloquear inmediatamente respuestas dañinas detectadas.
*   **Bloqueo de Usuarios/IPs:** Funcionalidad para bloquear usuarios o direcciones IP que abusen del sistema.
*   **Mecanismos de Retroalimentación:** Permitir a los usuarios reportar contenido como "inexacto", "ofensivo" o "incompleto".
*   **Telemetría:** Monitorear la satisfacción del usuario y el uso, respetando siempre la privacidad.

---

# Azure AI Content Safety en Microsoft Foundry

Además de los filtros integrados en Azure OpenAI, **Microsoft Foundry** ofrece características avanzadas de seguridad de contenido para proteger las aplicaciones de IA generativa.

## Características Clave

*   **Escudos de Prompt (Prompt Shields):** Analizan y bloquean ataques de entrada del usuario, como intentos de *Jailbreak* (eludir las reglas del modelo).
*   **Detección de Fundamentación (Groundedness Detection):** Detecta si las respuestas generadas por el modelo están realmente basadas en el contenido fuente proporcionado, ayudando a identificar alucinaciones.
*   **Detección de Material Protegido (Protected Material Detection):** Identifica si el contenido generado infringe derechos de autor o propiedad intelectual conocida.
*   **Categorías Personalizadas (Custom Categories):** Permite definir filtros para patrones específicos o emergentes que no están cubiertos por las categorías estándar.

---

# Laboratorio: Aplicación de Filtros de Contenido (Content Filters)

En este ejercicio práctico, exploramos cómo los **Filtros de Contenido (Content Filters)** en **Microsoft Foundry** actúan como una capa de seguridad para interceptar y bloquear entradas o salidas dañinas.

## 1. Comportamiento Predeterminado

Comenzamos implementando un modelo `gpt-4o` con la configuración de seguridad por defecto. Esta configuración aplica un conjunto equilibrado de filtros.

Al probar en el **Área de juegos de chat (Chat playground)**:
*   **Consulta de Salud:** Al preguntar por primeros auxilios para un corte, el modelo responde con consejos útiles.
    > **Prompt:** "What should I do if I cut myself?"
    >
    > (Traducción: "¿Qué debo hacer si me corto?")
*   **Actividad Ilegal:** Al pedir ayuda para planear un robo, el filtro predeterminado bloquea la respuesta indicando que es contenido potencialmente dañino.
    > **Prompt:** "I'm planning to rob a bank. Help me plan a getaway."
    >
    > (Traducción: "Planeo robar un banco. Ayúdame a planear una fuga.")

## 2. Creación de un Filtro Personalizado

Para escenarios donde se requiere una moderación más estricta, navegamos a la sección de **Barandillas + controles (Guardrails + controls)**. Aquí creamos un filtro personalizado ajustando los umbrales para las cuatro categorías de daño:
1.  **Violencia (Violence)**
2.  **Odio (Hate)**
3.  **Sexual (Sexual)**
4.  **Autolesión (Self-harm)**

Configuramos los filtros en el nivel más estricto (bloqueando incluso contenido de severidad baja) y aplicamos este nuevo filtro a nuestra **Implementación (Deployment)** del modelo `gpt-4o`.

## 3. Prueba del Filtro Estricto

Al volver al **Área de juegos de chat (Chat playground)** con el filtro personalizado activo, notamos cambios significativos:

*   **Falso Positivo (Over-blocking):** La misma pregunta sobre el corte ("What should I do if I cut myself?") ahora es bloqueada. El filtro estricto detecta la referencia al daño físico y la clasifica preventivamente como **Autolesión (Self-harm)**, aunque la intención del usuario era buscar ayuda médica.
*   **Contenido Ofensivo:** Solicitudes de chistes ofensivos o discriminatorios son bloqueadas eficazmente.
    > **Prompt:** "Tell me an offensive joke about Scotsmen."
    >
    > (Traducción: "Cuéntame un chiste ofensivo sobre escoceses.")

**Conclusión:** Los filtros de contenido son esenciales para la IA Responsable, pero configurarlos demasiado estrictos puede reducir la utilidad del modelo al bloquear consultas legítimas. Es necesario encontrar el equilibrio adecuado (generalmente permitiendo severidad baja/media en ciertos contextos) según el caso de uso.

---

# Resumen del Módulo: IA Generativa Responsable

La inteligencia artificial generativa requiere un enfoque responsable para evitar la creación de contenido dañino. En este módulo, se ha detallado el proceso práctico de cuatro fases para garantizar la seguridad y confianza en sus soluciones:

*   **Identificar (Identify):** Reconocer los posibles daños relevantes para la solución específica.
*   **Medir (Measure):** Cuantificar la presencia de daños cuando se utiliza el sistema, estableciendo líneas base.
*   **Mitigar (Mitigate):** Implementar estrategias de reducción de riesgos en las cuatro capas de la solución (Modelo, Sistema de seguridad, Metaprompt/Fundamentación, Experiencia de usuario).
*   **Operar (Operate):** Desplegar y gestionar la solución con planes de preparación operativa y respuesta a incidentes.

---

# Evaluación del módulo

1.  **¿Por qué debe considerar la posibilidad de crear una valoración de impacto de la IA al diseñar una solución de IA generativa?**

    a. Para construir un caso legal que le permita eximirse de responsabilidad por los daños provocados por la solución

    b. Para documentar el propósito, el uso esperado y los daños posibles para la solución ✅

    c. Para evaluar el costo de los servicios en la nube necesarios para implementar la solución

**Justificación:** La **Evaluación de Impacto de la IA (AI Impact Assessment)** es una herramienta clave en la fase de identificación para documentar los usos previstos, los riesgos y los daños potenciales, no para temas legales o de costos.

**Glosario / Comentarios:**
*   **"Valoración de impacto de la IA"**: Traducción de *AI Impact Assessment*.

2.  **¿Qué funcionalidad de Microsoft Foundry ayuda a mitigar la generación de contenido perjudicial en el nivel del sistema de seguridad?**

    a. soporte para el modelo DALL-E

    b. Ajuste preciso

    c. Filtros de contenido ✅

**Justificación:** Los **Filtros de contenido (Content Filters)** actúan en la capa del sistema de seguridad interceptando entradas y salidas dañinas. El "Ajuste preciso" (Fine-tuning) actúa en la capa del modelo.

**Glosario / Comentarios:**
*   **"Filtros de contenido"**: Traducción de *Content Filters*.
*   **"Ajuste preciso"**: Traducción de *Fine-tuning*.

3.  **¿Por qué debe considerar un plan de entrega por fases para la solución de IA generativa?**

    a. Para permitirle recopilar comentarios e identificar problemas antes de publicar la solución de forma más amplia ✅

    b. Para eliminar la necesidad de mapear, medir, mitigar y gestionar posibles daños

    c. Para permitirle cobrar más por la solución

**Justificación:** La **Entrega por fases (Phased Delivery)** es una estrategia de operación que permite detectar problemas en un grupo reducido antes del lanzamiento general. No elimina la necesidad del proceso de IA responsable.

**Glosario / Comentarios:**
*   **"Plan de entrega por fases"**: Traducción de *Phased Delivery*.