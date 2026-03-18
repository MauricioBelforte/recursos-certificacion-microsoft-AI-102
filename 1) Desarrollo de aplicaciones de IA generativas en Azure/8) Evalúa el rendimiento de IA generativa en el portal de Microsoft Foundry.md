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

# Laboratorio: Evaluación del Rendimiento (Manual y Automatizada)

En este ejercicio práctico, exploramos cómo evaluar un modelo de IA generativa utilizando tanto la revisión humana como las herramientas automatizadas de **Microsoft Foundry**.

## 1. Configuración del Entorno

Para realizar las pruebas, necesitamos un entorno con dos modelos desplegados en nuestro proyecto de Foundry:
1.  **Modelo a Evaluar:** Usaremos `gpt-4o-mini`, un modelo más ligero y rápido.
2.  **Modelo Evaluador (Juez):** Usaremos `gpt-4o`, un modelo más potente que actuará como "juez" en las evaluaciones automatizadas.

Realizamos la **Implementación (Deployment)** de ambos modelos, asegurándonos de configurar un límite de tokens (TPM) adecuado (ej. 50k) para gestionar la cuota.

## 2. Evaluación Manual

La evaluación manual es útil para pruebas iniciales o exploratorias.
1.  **Datos de Prueba:** Descargamos un archivo `jsonl` que contiene pares de preguntas sobre viajes y sus respuestas esperadas.
2.  **Configuración:** En la sección **Evaluación (Evaluation)**, creamos una "Nueva evaluación manual".
3.  **Prompt del Sistema:** Configuramos el modelo con el siguiente comportamiento:
    > **System Message:** "Assist users with travel-related inquiries, offering tips, advice, and recommendations as a knowledgeable travel agent."
    >
    > (Traducción: "Asiste a los usuarios con consultas relacionadas con viajes, ofreciendo consejos, asesoramiento y recomendaciones como un agente de viajes experto.")
4.  **Ejecución:** Importamos los datos de prueba y ejecutamos el modelo.
5.  **Calificación:** Revisamos las respuestas generadas una por una y las calificamos manualmente con "pulgar arriba" o "pulgar abajo", comparando el resultado del modelo con la **Respuesta esperada (Expected Response)**.

## 3. Evaluación Automatizada

Para escalar el proceso y obtener métricas estandarizadas sin revisar cada respuesta manualmente, utilizamos la evaluación automatizada.

1.  **Configuración:** Seleccionamos el modelo `gpt-4o-mini` para ser evaluado usando el mismo dataset y mensaje del sistema anterior.
2.  **Configuración de Evaluadores:** Definimos qué métricas queremos medir y qué modelo las calculará:
    *   **Evaluador del modelo (Model Evaluator) - Similitud Semántica (Semantic_similarity):** Utiliza el modelo juez (`gpt-4o`) para comparar el significado de la respuesta generada con la respuesta esperada.
    *   **Evaluador de escala Likert (Likert scale Evaluator) - Relevancia (Relevance):** Utiliza el modelo juez para evaluar qué tan pertinente es la respuesta a la consulta.
    *   **Similitud de texto (Text Similarity) - Puntuación F1 (F1_Score):** Una métrica matemática estándar que mide la superposición de palabras con la verdad terreno.
    *   **Contenido odioso e injusto (Hateful and unfair content):** Evalúa riesgos de seguridad de contenido.

3.  **Revisión de Resultados:**
    *   Al finalizar, obtenemos un tablero con puntuaciones promedio para cada métrica.
    *   Podemos profundizar en la pestaña de **Datos** para ver, por ejemplo, el razonamiento que aplicó el modelo juez (`gpt-4o`) para otorgar una puntuación baja o alta en relevancia.

**Conclusión:** La evaluación automatizada permite medir de forma consistente la calidad y la seguridad del modelo a gran escala, utilizando modelos potentes (`gpt-4o`) para evaluar el desempeño de modelos más eficientes (`gpt-4o-mini`) antes de su despliegue.

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
