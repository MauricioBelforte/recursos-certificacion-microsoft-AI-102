# Evaluación del Rendimiento de IA Generativa en Microsoft Foundry

## Introducción

Evaluar las aplicaciones de **Inteligencia Artificial Generativa (Generative AI)** es un paso fundamental en su ciclo de vida por tres razones clave:

1.  **Garantizar la Calidad:** La evaluación permite identificar y corregir problemas, asegurando que la aplicación proporcione respuestas precisas y relevantes.
2.  **Mejorar la Satisfacción del Usuario:** Respuestas de alta calidad conducen a una experiencia de usuario positiva, fomentando la confianza y el uso continuo de la aplicación.
3.  **Habilitar la Mejora Continua:** Analizar los resultados de las evaluaciones ayuda a identificar áreas de oportunidad para refinar iterativamente el rendimiento de la aplicación, manteniéndola eficaz y alineada con las expectativas de los usuarios.

En este módulo, aprenderá a utilizar las herramientas del portal de **Microsoft Foundry** para evaluar sus aplicaciones de IA generativa, comprendiendo cómo este proceso beneficia el ciclo de desarrollo.

---

# Evaluación del Rendimiento del Modelo y Flujos

Evaluar el rendimiento es crucial en las diferentes fases del desarrollo de una aplicación de IA generativa. Se puede evaluar tanto un modelo de lenguaje individual como un flujo de chat completo.

*   **Evaluación de Modelo Individual:** Se analiza la calidad de la respuesta de un modelo ante una entrada específica, a menudo comparándola con una salida esperada o **Verdad Terreno (Ground Truth)**.
*   **Evaluación de Flujo de Chat:** Se evalúa el flujo completo, que puede combinar varios modelos y lógica de código, para validar que la aplicación funciona según lo previsto.

## Enfoques de Evaluación

Existen varios enfoques para medir el rendimiento de un modelo o flujo:

### 1. Bancos de Pruebas de Modelos (Model Benchmarks)
Son métricas públicas que comparan el rendimiento de diferentes modelos en conjuntos de datos estandarizados. Ayudan a entender cómo se posiciona un modelo frente a otros. Microsoft Foundry muestra estos benchmarks para los modelos de su catálogo.

Métricas comunes incluyen:
*   **Exactitud (Accuracy):** Compara si el texto generado coincide exactamente con la respuesta correcta.
*   **Coherencia (Coherence):** Mide si la salida es fluida, natural y se asemeja al lenguaje humano.
*   **Fluidez (Fluency):** Evalúa la corrección gramatical y sintáctica del texto.
*   **Similitud GPT (GPT Similarity):** Cuantifica la similitud semántica entre la respuesta generada y la respuesta de referencia (verdad terreno).

### 2. Evaluaciones Manuales
Implican que evaluadores humanos califiquen la calidad de las respuestas. Este enfoque captura matices que las métricas automáticas pueden pasar por alto, como la relevancia contextual y la satisfacción del usuario.

### 3. Métricas Asistidas por IA (AI-assisted Metrics)
Utilizan modelos de IA para evaluar a otros modelos.
*   **Métricas de Calidad de Generación:** Evalúan la creatividad, coherencia y el cumplimiento de un estilo o tono deseado.
*   **Métricas de Riesgo y Seguridad:** Evalúan si las salidas contienen contenido dañino o sesgado.

### 4. Métricas de Procesamiento de Lenguaje Natural (NLP)
Son métricas cuantitativas que miden la superposición o similitud entre la respuesta generada y la respuesta esperada (verdad terreno).
*   **Puntuación F1 (F1 Score):** Mide la proporción de palabras compartidas, útil para tareas de clasificación.
*   **BLEU (Bilingual Evaluation Understudy):** Compara n-gramas coincidentes.
*   **METEOR (Metric for Evaluation of Translation with Explicit ORdering):** Considera sinónimos y raíces de palabras.
*   **ROUGE (Recall-Oriented Understudy for Gisting Evaluation):** Se enfoca en la recuperación de n-gramas.

---

# Evaluación Manual del Rendimiento

Durante las fases iniciales de desarrollo, la experimentación y la iteración rápida son clave. Las evaluaciones manuales, realizadas por humanos, proporcionan información cualitativa que las métricas automáticas no pueden capturar.

Microsoft Foundry ofrece dos herramientas principales para este propósito.

## 1. Preparación de Prompts de Prueba

Antes de evaluar, es fundamental crear un conjunto de datos de prueba diverso. Este debe incluir:
*   Consultas comunes de los usuarios.
*   **Casos límite (Edge cases)** que pongan a prueba la robustez del modelo.
*   Posibles puntos de fallo o preguntas ambiguas.

## 2. Pruebas en el Área de Juegos de Chat (Chat Playground)

El **Área de juegos de chat (Chat playground)** es la herramienta ideal para la experimentación inicial y las pruebas rápidas de un modelo individual. Su principal función es permitir enviar un prompt, observar la respuesta del modelo y ajustar interactivamente el **Mensaje del sistema (System Message)** para ver si el rendimiento mejora.

## 3. Evaluaciones Manuales por Lotes

Cuando se necesita evaluar el rendimiento del modelo frente a un conjunto de datos más grande de manera sistemática, se utiliza la característica de **Evaluaciones Manuales (Manual Evaluations)**.

*   **Funcionalidad:** Permite cargar un conjunto de datos con múltiples preguntas y, opcionalmente, una respuesta esperada para cada una.
*   **Proceso:** Los evaluadores revisan cada respuesta generada por el modelo y la califican, comúnmente con un sistema de "pulgar arriba/abajo".
*   **Objetivo:** Permite evaluar rápidamente el rendimiento en un conjunto de datos diverso y tomar decisiones informadas para mejorar el modelo, ya sea cambiando el prompt, el modelo base o sus parámetros.

Una vez que el modelo individual ha sido validado, se integra en un **Flujo de mensajes (Prompt Flow)**, que también puede ser evaluado manual o automáticamente.

---

# Evaluaciones Automatizadas (Automated Evaluations)

Las evaluaciones automatizadas en **Microsoft Foundry** permiten medir de forma sistemática el rendimiento de modelos, conjuntos de datos o **Flujos de mensajes (Prompt Flows)**.

## 1. Datos de Evaluación

Para una evaluación automatizada, se necesita un conjunto de datos que contenga **prompts** y sus correspondientes respuestas. Opcionalmente, se puede incluir una respuesta esperada, conocida como **Verdad Terreno (Ground Truth)**.

Microsoft Foundry permite incluso usar un modelo de IA para generar un conjunto de datos inicial de preguntas y respuestas sobre un tema específico, que luego puede ser editado para usarlo como base para la evaluación.

## 2. Métricas de Evaluación

La plataforma ofrece evaluadores que miden dos áreas principales:

### A. Calidad de IA (AI Quality)
Mide la calidad de las respuestas del modelo utilizando:
*   **Métricas asistidas por IA:** Se usa un modelo para evaluar la **Coherencia (Coherence)** y **Relevancia (Relevance)** de las respuestas.
*   **Métricas NLP estándar:** Se comparan las respuestas con la "Verdad Terreno" usando métricas como **F1 Score, BLEU, METEOR y ROUGE**.

### B. Riesgo y Seguridad (Risk & Safety)
Se utilizan evaluadores para detectar si las respuestas contienen contenido perjudicial, clasificándolo en las categorías de seguridad de contenido:
*   Violencia (Violence)
*   Odio (Hate)
*   Contenido Sexual (Sexual)
*   Autolesión (Self-harm)

---

## Laboratorio: Evaluar el rendimiento de los modelos de IA generativa

*Nota: Este ejercicio está obsoleto, pero se mantiene para fines de estudio de la evaluación de modelos.*

En este ejercicio, utilizarás evaluaciones manuales y automatizadas para valorar el rendimiento de un modelo en el portal Microsoft Foundry.

Este ejercicio durará aproximadamente 30 minutos.

**Nota:** Algunas de las tecnologías utilizadas en este ejercicio están en fase de vista previa o en desarrollo activo.

### Crea un centro (Hub) y un proyecto de Foundry
Las funcionalidades de evaluación que vamos a utilizar requieren un proyecto basado en un recurso del centro de Foundry.

1. En un navegador web, abra el portal de Foundry en [https://ai.azure.com] e inicie sesión.
2. Navegue a la administración de recursos y seleccione **Crear nuevo** -> **Nuevo recurso de centro de IA (AI Hub)**.
3. En el asistente, introduzca un nombre para el proyecto y utilice el enlace para especificar un nombre válido para el centro.
4. Elija su suscripción, grupo de recursos y una región recomendada (ej. **Este de EE. UU. 2**, **Francia Central**, **Sur del Reino Unido**, **Suecia Central**).
5. Seleccione **Crear** y espere a que se cree el proyecto.

### Implementar modelos
En este ejercicio, evaluarás el rendimiento de un modelo `gpt-4o-mini`. También utilizarás un modelo `gpt-4o` para generar métricas de evaluación asistidas por IA (este actuará como tu modelo "juez").

1. En el panel de navegación de la izquierda, en la sección **Mis recursos**, seleccione la página **Modelos + puntos de conexión (Endpoints)**.
2. Seleccione **+ Implementar modelo** y elija **Implementar modelo base**.
3. Busque el modelo `gpt-4o`, selecciónelo y confírmelo.
4. Implemente el modelo con **Personalizar** en los detalles:
   - **Nombre de la implementación:** Un nombre válido.
   - **Tipo de despliegue:** Estándar global.
   - **Límite de tokens por minuto (TPM):** 50,000 (o el máximo disponible).
   - **Filtro de contenido:** DefaultV2.
5. Espere a que finalice el despliegue del modelo `gpt-4o`.
6. Vuelva a la página de implementaciones y repita el proceso para implementar también un modelo `gpt-4o-mini` con la misma configuración.

### Evaluar manualmente un modelo
La revisión manual permite calificar las respuestas del modelo basándose en datos de prueba predefinidos, utilizando tu propio juicio.

1. Descargue el archivo de datos de evaluación en formato JSONL desde: [https://raw.githubusercontent.com/MicrosoftLearning/mslearn-ai-studio/refs/heads/main/data/travel_evaluation_data.jsonl] (asegúrese de guardarlo como un archivo `.jsonl`, no `.txt`).
2. En el portal Foundry, en la sección **Proteger y gobernar (Protect and govern)**, seleccione **Evaluación (Evaluation)**.
3. En la pestaña **Evaluaciones manuales**, seleccione **+ Nueva evaluación manual**.
4. En **Modelo**, seleccione la implementación de su modelo `gpt-4o` (o `gpt-4o-mini`).
5. Modifique el **Mensaje del sistema (System message)** con las siguientes instrucciones de experto en viajes:
   > "Assist users with travel-related inquiries, offering tips, advice, and recommendations as a knowledgeable travel agent."
6. En la sección de resultados (abajo), seleccione **Importar datos de prueba** y cargue el archivo `travel_evaluation_data.jsonl`.
7. Asigne los campos del conjunto de datos de la siguiente manera:
   - **Entrada (Input):** `Pregunta`
   - **Respuesta esperada (Expected Response):** `ExpectedResponse`
8. Seleccione **Ejecutar** en la barra superior. Después de unos minutos, el modelo generará sus respuestas en la columna **Salida (Output)**.
9. Revise los resultados, compare el texto del modelo con la "respuesta esperada" y califique seleccionando el icono del **pulgar hacia arriba** o **pulgar hacia abajo**. Guarde los resultados.

### Utilizar la evaluación automatizada
Comparar manualmente consume mucho tiempo. La evaluación automatizada calcula métricas estándar y utiliza inteligencia artificial para evaluar la coherencia, la relevancia y la seguridad.

1. En la página de **Evaluación**, consulte la pestaña **Evaluaciones automatizadas**.
2. Seleccione **Crear una nueva evaluación** -> **Evaluar un modelo**.
3. En la página de fuente de datos, seleccione **Usar su conjunto de datos** y elija el archivo `travel_evaluation_data` cargado anteriormente.
4. En la página **Prueba tu modelo**, seleccione el modelo `gpt-4o-mini`.
5. Cambie el **Mensaje del sistema** a las mismas instrucciones de agente de viajes usadas en la evaluación manual.
6. En el campo de **Consulta (Query)**, seleccione la variable dinámica `{{item.question}}`.
7. En la página **Configurar evaluadores**, utilice el botón **+ Agregar** para configurar estas cuatro métricas de evaluación:
   - **Evaluador del modelo (Similitud Semántica):**
     - **Preajuste:** `Semantic_similarity`
     - **Califica con (Juez):** Selecciona el modelo potente `gpt-4o`
     - **Salida:** `{{sample.output_text}}` (por defecto)
     - **Verdad fundamental (Ground truth):** `{{item.ExpectedResponse}}`
   - **Evaluador de escala Likert (Relevancia):**
     - **Preajuste:** `Relevance`
     - **Califica con (Juez):** `gpt-4o`
     - **Consulta:** `{{item.question}}`
   - **Similitud de texto (Matemática):**
     - **Preajuste:** `F1_Score`
     - **Verdad fundamental:** `{{item.ExpectedResponse}}`
   - **Contenido odioso e injusto (Seguridad):**
     - **Preajuste:** `Odio_e_injusticia` (Hate and unfairness)
     - **Consulta:** `{{item.question}}`
8. Asigne un nombre a la evaluación, envíela y espere a que finalice (puede usar el botón "Actualizar").
9. Una vez finalizada, revise los resultados gráficos. Luego, seleccione la pestaña **Datos (Data)** en la parte superior para ver las métricas por cada fila, así como **las explicaciones del razonamiento** que aplicó el modelo juez (`gpt-4o`) para otorgar dichas calificaciones.

### Limpieza
Elimine el grupo de recursos desde el portal de Azure para evitar gastos imprevistos.

### Código Completo del Laboratorio: Datos de Evaluación (JSONL)

El archivo JSONL utilizado en este laboratorio contiene preguntas y una "verdad básica" (Ground Truth) para que los evaluadores matemáticos (F1 Score) y asistidos por IA (Similitud Semántica) puedan comparar la salida generada:

```json
{"question":"What's the best time of year to visit Paris?","ExpectedResponse":"Spring (April-June) and fall (September-November) are excellent, offering mild weather and fewer crowds."}
{"question":"Do I need a visa for Japan?","ExpectedResponse":"It depends on your nationality. Many countries have visa-free entry for short stays up to 90 days. Always check the official guidelines."}
```


---

# Resumen del Módulo: Evaluación del Rendimiento

En este módulo, ha aprendido a:

*   Comprender los **Bancos de Pruebas de Modelos (Model Benchmarks)** para comparar el rendimiento entre diferentes modelos.
*   Realizar **Evaluaciones Manuales (Manual Evaluations)** para obtener retroalimentación cualitativa.
*   Ejecutar **Evaluaciones Automatizadas (Automated Evaluations)** para medir sistemáticamente la calidad y la seguridad a escala.

---

# Evaluación del módulo

1.  **¿Qué técnica de evaluación puede usar para aplicar su propio juicio sobre la calidad de las respuestas a un conjunto de indicaciones específicas?**

    a. Puntos de referencia de modelos

    b. Evaluaciones manuales ✅

    c. Evaluaciones automatizadas

**Justificación:** Las **Evaluaciones manuales (Manual Evaluations)** permiten a los evaluadores humanos revisar y calificar las respuestas basándose en su propio juicio, lo cual es útil para capturar matices que las métricas automáticas pierden.

**Glosario / Comentarios:**
*   **"Puntos de referencia de modelos"**: Traducción de *Model Benchmarks*.
*   **"Indicaciones"**: Traducción de *Prompts*.

2.  **¿Qué evaluador compara las respuestas generadas a la verdad básica en función de las métricas estándar?**

    a. Coherencia

    b. Puntuación F1 ✅

    c. Material protegido

**Justificación:** La **Puntuación F1 (F1 Score)** es una métrica estándar de NLP que mide la superposición de palabras entre la respuesta generada y la verdad terreno (ground truth/verdad básica).

**Glosario / Comentarios:**
*   **"Verdad básica"**: Traducción de *Ground Truth*.

3.  **¿Qué métrica del evaluador usa un modelo de IA para juzgar la estructura y el flujo lógico de ideas en una respuesta?**

    a. Coherencia ✅

    b. Puntuación F1

    c. material protegido

**Justificación:** La métrica de **Coherencia (Coherence)** utiliza un modelo de IA para evaluar qué tan bien fluye el texto y si tiene una estructura lógica.
