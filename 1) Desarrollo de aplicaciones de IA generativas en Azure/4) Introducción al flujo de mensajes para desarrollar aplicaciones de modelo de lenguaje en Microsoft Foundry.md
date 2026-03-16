# Introducción al Flujo de Mensajes

La verdadera eficacia de los **Modelos de Lenguaje Grandes (LLM)** reside en su aplicación. Ya sea que quieras usarlos para clasificar páginas web o crear un *chatbot*, necesitas una aplicación que combine tus fuentes de datos con el LLM y genere la salida deseada.

---

# Ciclo de Vida del Desarrollo de Aplicaciones LLM

Antes de aprender a trabajar con el **Flujo de mensajes (Prompt Flow)**, es fundamental comprender el ciclo de vida de desarrollo de una aplicación de **Modelo de Lenguaje Grande (LLM)**.

El ciclo de vida consta de las siguientes fases:

1.  **Inicialización:** Definir el caso de uso y diseñar la solución.
2.  **Experimentación:** Desarrollar un flujo y probarlo con un conjunto de datos pequeño.
3.  **Evaluación y refinamiento:** Evaluar el flujo con un conjunto de datos más grande.
4.  **Producción:** Implementar y supervisar el flujo y la aplicación.

> **Nota:** Es posible que durante la evaluación o producción descubra que la solución necesita mejoras. Puede regresar a la fase de experimentación para iterar hasta obtener los resultados deseados.

---

## 1. Inicialización

Imagine que desea crear una aplicación LLM para clasificar artículos de noticias. Antes de comenzar, debe:
*   Definir las categorías de salida.
*   Comprender la estructura de un artículo típico.
*   Determinar cómo presentar el artículo como entrada y cómo generar la salida.

**Pasos clave:**
*   Definición del objetivo.
*   Recopilación de un **Conjunto de datos (Dataset)** de ejemplo.
*   Construcción de un **Prompt** básico.
*   Diseño del flujo.

Para desarrollar y probar, necesita un **Dataset** de ejemplo (un subconjunto representativo de los datos reales). Asegúrese de incluir diversidad de datos para cubrir varios escenarios y **casos límite (Edge cases)**. Elimine cualquier información confidencial para mantener la privacidad.

---

## 2. Experimentación

Una vez que tiene el **Dataset** y las categorías, diseñe un flujo que tome el artículo como entrada y use el LLM para clasificarlo.

La experimentación es un proceso iterativo:
1.  Ejecute el flujo con el **Dataset** de ejemplo.
2.  Evalúe el rendimiento del **Prompt**.
3.  Si está satisfecho, proceda a la evaluación.
4.  Si no, modifique el flujo ajustando el **Prompt** o la lógica del flujo.

---

## 3. Evaluación y Refinamiento

Cuando esté conforme con la salida inicial, evalúe el rendimiento del flujo con un **Dataset** más grande.

Esto le permite ver cómo la aplicación generaliza con nuevos datos e identificar cuellos de botella. Al editar el flujo, pruébelo primero con datos limitados para iterar rápidamente antes de ejecutar pruebas masivas.

---

## 4. Producción

Finalmente, la aplicación está lista para su uso real.

**Acciones en producción:**
*   **Optimizar:** Mejore la eficiencia del flujo para los datos entrantes.
*   **Implementación (Deployment):** Despliegue el flujo en un **Punto de conexión (Endpoint)**. Al llamar a este endpoint, se ejecuta el flujo y se genera la salida.
*   **Supervisión:** Monitoree el rendimiento mediante datos de uso y comentarios. Utilice esta información para mejorar el flujo continuamente.

---



# Componentes Principales y Tipos de Flujo en Prompt Flow

Para crear una aplicación de **Modelo de Lenguaje Grande (LLM)** utilizando **Prompt Flow**, es fundamental comprender sus componentes principales y cómo se estructuran los flujos de trabajo.

---

## 1. Descripción de un Flujo

El **Flujo de mensajes (Prompt Flow)** es una característica de Microsoft Foundry que permite crear flujos de trabajo ejecutables. Estos flujos conectan varios componentes para procesar información y generar respuestas.

Un flujo consta generalmente de tres partes:

*   **Entradas (Inputs):** Son los datos que se introducen en el flujo. Pueden ser de diferentes tipos, como cadenas de texto (strings), enteros o booleanos.
*   **Nodos (Nodes):** Representan las herramientas que realizan el procesamiento, ejecutan tareas o aplican operaciones algorítmicas.
*   **Salidas (Outputs):** Son los datos resultantes generados por el flujo.

De manera similar a una **Canalización (Pipeline)**, un flujo puede tener varios nodos conectados entre sí, donde la salida de un nodo se convierte en la entrada de otro.

---

## 2. Herramientas Disponibles

Para agregar funcionalidad a un flujo, se utilizan diferentes herramientas. Las tres más comunes son:

1.  **Herramienta LLM (LLM Tool):** Permite crear **Prompts** personalizados utilizando Modelos de Lenguaje Grande (LLM).
2.  **Herramienta Python (Python Tool):** Permite ejecutar scripts de Python personalizados para manipular datos o realizar lógica compleja.
3.  **Herramienta de Prompts (Prompt Tool):** Prepara mensajes como cadenas de texto, ideal para escenarios complejos o para integrar con otras herramientas.

> **Sugerencia:** Si las herramientas predeterminadas no cubren sus necesidades, puede crear su propia herramienta personalizada.

---

## 3. Tipos de Flujos

Existen tres tipos principales de flujos que puede crear en **Prompt Flow**:

*   **Flujo estándar (Standard flow):** Ideal para el desarrollo general de aplicaciones LLM. Ofrece una gama versátil de herramientas.
*   **Flujo de chat (Chat flow):** Diseñado específicamente para aplicaciones conversacionales (chatbots), con soporte mejorado para el historial y la interacción del chat.
*   **Flujo de evaluación (Evaluation flow):** Centrado en medir el rendimiento. Permite analizar y mejorar los modelos o aplicaciones basándose en métricas de ejecuciones anteriores.

---

# Conexiones y Entornos de Ejecución en Prompt Flow

Para crear una aplicación de **Modelo de Lenguaje Grande (LLM)** con **Flujo de mensajes (Prompt Flow)**, es esencial configurar primero las **Conexiones (Connections)** y los **Entornos de ejecución (Runtimes)**.

---

## 1. Conexiones (Connections)

Cuando un flujo necesita interactuar con un origen de datos externo, un servicio o una API, requiere una **Conexión (Connection)** para autorizar la comunicación. Esta conexión establece un vínculo seguro entre el flujo y el servicio externo.

Dependiendo del tipo, la conexión almacena de forma segura el **Punto de conexión (Endpoint)**, la **Clave de API (API Key)** u otras credenciales necesarias. Para proteger esta información sensible, los secretos se guardan en una instancia de **Azure Key Vault** asociada.

Las conexiones son reutilizables, permitiendo que diferentes herramientas dentro de sus flujos accedan a los mismos servicios externos de manera consistente.

### Herramientas que requieren Conexiones

| Tipo de Conexión (Connection Type) | Herramientas que la utilizan |
| :--- | :--- |
| Azure OpenAI | Herramienta LLM, Herramienta Python |
| OpenAI | Herramienta LLM, Herramienta Python |
| Azure AI Search | Búsqueda en base de datos vectorial, Herramienta Python |
| Serp API | Herramienta Serp API, Herramienta Python |
| Personalizada (Custom) | Herramienta Python |

Las conexiones cumplen dos roles clave:
1.  **Automatizan la gestión de credenciales**, simplificando el acceso seguro a las APIs.
2.  **Permiten la transferencia segura de datos** desde diversas fuentes, manteniendo la integridad y la privacidad.

---

## 2. Entornos de Ejecución (Runtimes)

Una vez que el flujo está diseñado y las conexiones configuradas, necesita ser ejecutado. Para ello, se requiere **Cómputo (Compute)**, que es proporcionado por un **Entorno de ejecución (Runtime)** de Prompt Flow.

Un **Runtime** es la combinación de:
*   Una **Instancia de cómputo (Compute Instance)** que provee los recursos de hardware (CPU, memoria).
*   Un **Entorno (Environment)** que define la imagen base y los paquetes de software (como librerías de Python) necesarios para que el flujo se ejecute.

El uso de Runtimes proporciona un entorno controlado y estable para ejecutar y validar los flujos, asegurando que todo funcione como se espera. Existe un entorno predeterminado para desarrollo y pruebas rápidas, pero si necesita paquetes adicionales, puede crear un **entorno personalizado**.

---

# Variantes, Implementación y Supervisión en Prompt Flow

Durante el ciclo de vida de desarrollo de una aplicación LLM, es fundamental optimizar el flujo, implementarlo para su consumo y supervisar su rendimiento para aplicar mejoras continuas.

---

## 1. Variantes (Variants)

Las **Variantes (Variants)** en Prompt Flow son versiones alternativas de un nodo de herramienta con configuraciones distintas. Actualmente, se utilizan principalmente en la **Herramienta LLM**, donde una variante puede tener un **Prompt** diferente o parámetros de conexión distintos.

Esto permite probar diferentes enfoques para una tarea específica (como resumir noticias) sin duplicar todo el flujo.

**Ventajas principales:**
*   **Mejora de calidad:** Ayuda a encontrar la mejor combinación de prompt y configuración.
*   **Comparación:** Facilita la comparación de resultados lado a lado ("side-by-side") para tomar decisiones basadas en datos.
*   **Eficiencia:** Reduce el esfuerzo en el ajuste de prompts (tuning) y permite un seguimiento histórico de los cambios.

---

## 2. Implementación en un Punto de Conexión (Endpoint)

Una vez que el flujo funciona correctamente, el siguiente paso es la **Implementación (Deployment)** en un **Punto de conexión en línea (Online Endpoint)**.

*   Un endpoint proporciona una **URL** y una **Clave (Key)** (o token).
*   Permite integrar el flujo de manera segura con aplicaciones externas (como un chatbot o una aplicación web).
*   Al invocar el endpoint, el flujo se ejecuta en tiempo real y devuelve la respuesta generada casi instantáneamente.

---

## 3. Supervisión y Métricas de Evaluación

La supervisión es vital para asegurar que la aplicación LLM sea precisa, útil y cumpla con las expectativas en el mundo real.

Para evaluar el rendimiento, a menudo se comparan las predicciones del modelo con respuestas esperadas o **Verdad terreno (Ground Truth)**.

### Métricas Clave

Las métricas de evaluación estándar en Prompt Flow incluyen:

*   **Fundamentación (Groundedness):** Mide qué tan bien se alinea la respuesta con la fuente de datos de entrada (esencial para evitar alucinaciones).
*   **Relevancia (Relevance):** Evalúa qué tan útil es la respuesta en relación con la pregunta o entrada del usuario.
*   **Coherencia (Coherence):** Evalúa si el texto generado tiene un flujo lógico y es legible.
*   **Fluidez (Fluency):** Analiza la corrección gramatical y lingüística del texto generado.
*   **Similitud (Similarity):** Cuantifica la coincidencia semántica y contextual entre la salida del modelo y la **Verdad terreno (Ground Truth)**.

Si las métricas indican un rendimiento bajo en áreas como la fundamentación o la relevancia, se debe regresar a la fase de experimentación para ajustar iterativamente el flujo.


---


# Resumen del Módulo: Introducción a Prompt Flow

En este módulo, ha adquirido los conocimientos fundamentales para desarrollar aplicaciones basadas en **Modelos de Lenguaje Grande (LLM)** utilizando **Prompt Flow** en Microsoft Foundry.

## Puntos Clave Aprendidos

1.  **Ciclo de Vida de Desarrollo:** Comprendió las fases críticas para crear una aplicación LLM:
    *   **Inicialización:** Definición de objetivos y datasets.
    *   **Experimentación:** Creación y prueba de flujos.
    *   **Evaluación y Refinamiento:** Optimización basada en métricas.
    *   **Producción:** Implementación y supervisión.

2.  **Arquitectura de Flujos:** Aprendió qué es un **Flujo (Flow)** y cómo actúa como un flujo de trabajo ejecutable que conecta diversos componentes.

3.  **Componentes Principales:**
    *   **Entradas y Salidas:** Gestión de datos.
    *   **Nodos y Herramientas:** Uso de herramientas LLM, Python y Prompt.
    *   **Conexiones (Connections):** Gestión segura de credenciales para servicios externos.
    *   **Entornos de ejecución (Runtimes):** Recursos de cómputo para ejecutar los flujos.

## Siguientes Pasos y Recursos

Para profundizar en su aprendizaje, se recomienda explorar:
*   Documentación oficial de **Prompt Flow** en el portal de Microsoft Foundry.
*   Técnicas avanzadas de **Ingeniería de Prompts (Prompt Engineering)**.
*   Foros de la comunidad de desarrolladores de Microsoft Foundry.


---

# Evaluación del Módulo: Introducción al Flujo de Mensajes (Prompt Flow)

1.  **Un flujo usa una herramienta LLM para generar texto con un modelo de GPT-3.5. ¿Qué necesita crear para asegurarse de que el flujo de mensajes puede llamar de forma segura al modelo implementado desde Azure OpenAI?**

    a. Runtimes

    b. Conexiones (Connections) ✅

    c. Herramientas (Tools)

**Justificación:** Las **Conexiones (Connections)** almacenan de forma segura las credenciales y los puntos de conexión necesarios para que el flujo se comunique con servicios externos como Azure OpenAI, evitando exponer claves API en el código.

**Glosario / Comentarios:**
*   **"Flujo de mensajes"**: Traducción de *Prompt Flow*.
*   **"Conexiones"**: Traducción de *Connections*.
*   **"Herramienta LLM"**: Traducción de *LLM Tool*.

2.  **Quiere integrar el flujo con un sitio web en línea. ¿Qué debe hacer para integrar fácilmente el flujo?**

    a. Crear un entorno personalizado.

    b. Crear un entorno de ejecución (Runtime).

    c. Implementar el flujo en un punto de conexión (Endpoint). ✅

**Justificación:** Para que una aplicación externa (como un sitio web) consuma el flujo, este debe ser **Implementado (Deployed)** en un **Punto de conexión en línea (Online Endpoint)**, el cual proporciona una URL estable y segura para realizar peticiones.

**Glosario / Comentarios:**
*   **"Punto de conexión"**: Traducción de *Endpoint* (a veces traducido como "Extremo" o "Punto final").
*   **"Entorno de ejecución"**: Traducción de *Runtime*.
*   **"Implementar"**: Traducción de *Deploy*.

3.  **Después de la implementación, observa que el flujo tiene un rendimiento inferior. ¿A qué fase del ciclo de vida de desarrollo debe revertir?**

    a. Experimentación ✅

    b. Evaluación y refinamiento

    c. Producción

**Justificación:** Según el ciclo de vida de desarrollo de aplicaciones LLM, si se detectan problemas de rendimiento en producción o evaluación, se debe regresar a la fase de **Experimentación** para ajustar los prompts, cambiar la lógica del flujo o probar nuevas configuraciones hasta obtener los resultados deseados.

**Glosario / Comentarios:**
*   **"Implementación"**: Traducción de *Deployment*.
