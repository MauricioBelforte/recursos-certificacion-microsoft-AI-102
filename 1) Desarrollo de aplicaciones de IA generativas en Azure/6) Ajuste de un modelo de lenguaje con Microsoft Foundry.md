# Ajuste de un modelo de lenguaje con Microsoft Foundry

## Introducción

Los modelos de lenguaje pre-entrenados ofrecen un excelente punto de partida. Al utilizar uno de estos **Modelos Base (Foundation Models)**, puede ahorrar tiempo y esfuerzo, ya que requiere menos datos para adaptar el modelo a su caso de uso específico.

Imagine que es desarrollador en una agencia de viajes. Desea que su aplicación de chat responda a las consultas de los clientes con un formato, estilo y tono de voz específicos que se alineen con la identidad de su empresa.

Para lograr esto, una estrategia eficaz es **Ajustar (Fine-tuning)** un modelo de lenguaje.

### ¿Por qué ajustar (Fine-tuning)?

La ventaja de realizar un ajuste fino en lugar de entrenar un modelo desde cero es significativa:
*   Requiere menos tiempo.
*   Consume menos recursos computacionales.
*   Necesita menos datos para personalizar el modelo.

En este módulo, aprenderá a ajustar un modelo base del catálogo de **Microsoft Foundry** para integrarlo en su aplicación.

---

# Estrategias de Optimización y Técnicas de Ajuste

Para mejorar la calidad y el comportamiento de un modelo de lenguaje en Microsoft Foundry, existen varias estrategias. Es fundamental saber cuándo usar cada una.

## 1. Niveles de Optimización

*   **Ingeniería de Prompts (Prompt Engineering):** Es el método más rápido y sencillo. Consiste en ajustar el **Mensaje del Sistema (System Message)** o proporcionar ejemplos en el prompt (**Few-Shot Learning**).
    *   *Limitación:* A veces el modelo puede ignorar instrucciones complejas o no ser consistente en su formato.
*   **Generación Aumentada por Recuperación (RAG):** Se utiliza cuando las respuestas deben basarse en hechos reales y datos externos específicos. Fundamenta al modelo para evitar alucinaciones.
    *   *Uso principal:* Información fáctica y actualizada.
*   **Ajuste Fino (Fine-tuning):** Se entrena al modelo con un conjunto de datos adicional para modificar su comportamiento intrínseco.
    *   *Uso principal:* Adaptar el **estilo, formato, tono** o comportamiento específico cuando la ingeniería de prompts no es suficiente.

> **Nota:** A menudo se combinan RAG (para el *contenido*) y Fine-tuning (para el *estilo*).

---

## 2. Técnicas de Ajuste Fino en Microsoft Foundry

Microsoft Foundry admite cinco técnicas clave de ajuste fino, cada una diseñada para un propósito específico:

### A. Ajuste Supervisado (Supervised Fine-tuning - SFT)
Es el enfoque más básico y común. Se entrena un modelo base utilizando ejemplos etiquetados de conversaciones (Prompt del usuario + Respuesta ideal del asistente).
*   **Objetivo:** Enseñar formatos, estilos o tonos específicos del dominio.
*   **Modelos soportados:** GPT-4, GPT-4o, GPT-3.5-Turbo.

### B. Ajuste de Refuerzo (Reinforcement Fine-tuning - RFT)
Técnica avanzada para modelos de razonamiento. En lugar de dar la respuesta exacta, se proporciona un "calificador" que puntúa la calidad de la salida. El modelo aprende maximizando esta recompensa.
*   **Objetivo:** Razonamiento complejo y resolución de problemas donde los ejemplos etiquetados son limitados.
*   **Modelos soportados:** Modelos de razonamiento avanzados (ej. serie o1/o4).

### C. Optimización de Preferencias Directas (Direct Preference Optimization - DPO)
Alinea el modelo basándose en la preferencia humana. Se proporcionan pares de respuestas: una "Preferida" y una "No preferida".
*   **Objetivo:** Ajustar elementos subjetivos como el tono o estilo cuando no hay una única respuesta correcta "verdadera". Es una alternativa más ligera al RLHF (Reinforcement Learning from Human Feedback).
*   **Modelos soportados:** GPT-4o, GPT-4.1.

### D. Ajuste para Llamadas a Funciones (Function Calling Fine-tuning)
Entrena al modelo para llamar a APIs o herramientas externas de forma fiable con los argumentos correctos.
*   **Objetivo:** Asegurar que el modelo genere salidas estructuradas (ej. JSON válido) y reduzca errores al interactuar con sistemas externos.
*   **Modelos soportados:** GPT-4o, GPT-4o-mini.

### E. Ajuste para Tareas de Visión (Vision Fine-tuning)
Mejora la capacidad de los modelos multimodales para comprender imágenes. Se entrena con pares de Imágenes + Texto.
*   **Objetivo:** Reconocimiento de patrones visuales específicos del dominio (ej. defectos industriales, imágenes médicas, documentos personalizados).
*   **Modelos soportados:** GPT-4o (multimodal).

---

# Preparación de Datos para Ajuste Fino

El ajuste fino (**Fine-tuning**) requiere un **Modelo Base** y un **Conjunto de datos de entrenamiento (Training Dataset)**. La calidad de estos datos es crucial para el éxito del modelo.

## 1. Formato de Datos de Entrenamiento

Para ajustar un modelo de chat, los datos deben estar en formato **JSON Lines (JSONL)**. Cada línea representa una conversación completa que incluye tres roles:
1.  **System:** El mensaje del sistema que define el comportamiento.
2.  **User:** La pregunta o instrucción del usuario.
3.  **Assistant:** La respuesta ideal que queremos que el modelo aprenda.

**Ejemplo de estructura JSONL:**
```json
{"messages": [
    {"role": "system", "content": "Eres un agente de soporte de Xbox amable y conciso."},
    {"role": "user", "content": "¿Xbox es mejor que PlayStation?"},
    {"role": "assistant", "content": "No puedo dar opiniones personales, pero puedo ayudarte con problemas técnicos de tu Xbox."}
]}
```

### Uso de Pesos (Weights)
Puede incluir conversaciones de varios turnos y controlar qué mensajes específicos debe aprender el modelo utilizando el parámetro `weight`:
*   `"weight": 0`: El modelo ignora este mensaje durante el entrenamiento.
*   `"weight": 1`: El modelo aprende de este mensaje.

---

## 2. Estrategias de Obtención de Datos

### A. Datos Reales
Utilizar historiales de chat existentes es ideal, pero requiere:
*   Eliminar Información de Identificación Personal (**PII**).
*   Asegurar diversidad en los ejemplos.

### B. Datos Sintéticos (Synthetic Data)
Si no tiene suficientes datos reales, **Microsoft Foundry** ofrece generadores de datos sintéticos:

1.  **Generador de Preguntas y Respuestas (Q&A):** Convierte documentos (PDF, Markdown, TXT) en pares de entrenamiento. Puede generar respuestas largas (razonamiento) o cortas (factuales).
2.  **Generador de Uso de Herramientas:** Crea conversaciones simuladas basadas en una especificación **OpenAPI** (JSON) para enseñar al modelo a llamar a APIs.

**Mejores Prácticas para Datos Sintéticos:**
*   **Calidad de Referencia:** Los documentos fuente deben estar bien estructurados y limpios.
*   **Iteración:** Empiece con pocas muestras (50-100) y escale tras validar.
*   **Híbrido:** Combine datos sintéticos con datos reales para mejorar la generalización.

---

# Exploración y Configuración del Ajuste Fino

Microsoft Foundry ofrece un **Catálogo de Modelos (Model Catalog)** donde puede seleccionar **Modelos Base (Foundation Models)** pre-entrenados (como GPT-4 o Llama-2) para ajustarlos a sus necesidades.

## 1. Selección del Modelo Base

Al elegir un modelo, considere los siguientes criterios:
*   **Funcionalidad (Capabilities):** ¿El modelo es bueno para textos cortos (tipo BERT) o para generación compleja (GPT)?
*   **Datos de Pre-entrenamiento:** Algunos modelos (como GPT-2) se entrenan con datos de internet sin filtrar, lo que puede introducir sesgos.
*   **Soporte de Idiomas:** Verifique si el modelo soporta nativamente el idioma de su aplicación.

> **Nota:** Verifique siempre la disponibilidad de cuota para el modelo deseado en su región de Azure.

---

## 2. Configuración de Hiperparámetros (Hyperparameters)

Al configurar el trabajo de ajuste (**Fine-tuning Job**), puede modificar parámetros avanzados que controlan cómo aprende el modelo:

### A. Batch Size (Tamaño del lote)
Es el número de ejemplos de entrenamiento utilizados en una sola iteración (paso hacia adelante y hacia atrás).
*   **Efecto:** Tamaños más grandes suelen funcionar mejor con conjuntos de datos grandes y ofrecen una estimación del error más estable (menor varianza).

### B. Learning Rate Multiplier (Multiplicador de tasa de aprendizaje)
Factor que escala la velocidad de aprendizaje original del modelo.
*   **Rango recomendado:** Entre **0.02** y **0.2**.
*   **Efecto:** Una tasa más pequeña ayuda a evitar el **sobreajuste (overfitting)** (que el modelo memorice los datos en lugar de aprender patrones), pero puede requerir más tiempo de entrenamiento.

### C. Number of Epochs (Número de épocas - n_epochs)
Una "época" es un ciclo completo a través de *todo* el conjunto de datos de entrenamiento.
*   **Efecto:** Más épocas permiten al modelo ver los datos más veces, pero demasiadas pueden causar sobreajuste.

### D. Seed (Semilla / Inicialización)
Un número utilizado para inicializar el generador de números aleatorios.
*   **Efecto:** Permite la **reproducibilidad**. Si usa la misma semilla y los mismos datos, obtendrá exactamente el mismo modelo resultante.

---

## 3. Flujo de Trabajo
1.  Seleccionar Modelo Base.
2.  Cargar Datos de Entrenamiento (y opcionalmente Validación).
3.  Configurar Hiperparámetros.
4.  Enviar el trabajo -> Revisar métricas -> **Implementar (Deploy)** el modelo ajustado.

---

## Laboratorio: Ajuste de un modelo de lenguaje (Fine-tuning)

*Nota: Este ejercicio está obsoleto, pero se mantiene para fines de estudio de las fases de personalización de modelos.*

En este ejercicio, ajustarás un modelo de lenguaje con Microsoft Foundry para un escenario de aplicación de chat personalizado de una agencia de viajes. Compararás el modelo ajustado con un modelo base para evaluar cuál se adapta mejor a tus necesidades de estilo y tono.

Este ejercicio durará aproximadamente 60 minutos.

**Nota:** El ajuste fino depende de recursos en la nube cuyo aprovisionamiento varía. Algunas actividades pueden tardar y requieren paciencia.

### Implementar un modelo base en Microsoft Foundry
Comencemos por implementar el punto de partida para nuestra comparación.

1. En un navegador web, abra el portal de Foundry en [https://ai.azure.com] e inicie sesión.
2. En la sección **Explorar modelos**, busque el modelo `gpt-4o`.
3. Seleccione **Usar este modelo** y cree un nuevo proyecto.
4. Elija una región que soporte ajuste fino para `gpt-4o` (ej: **East US 2**, **North Central US** o **Sweden Central**).
5. Implemente el modelo con un **Límite de tokens por minuto (TPM)** de 50K.

### Ajustar (Fine-tune) el modelo
Iniciaremos el trabajo de ajuste ahora, ya que toma tiempo completarse.

1. Descargue el conjunto de datos de entrenamiento en formato JSONL desde: [https://raw.githubusercontent.com/MicrosoftLearning/mslearn-ai-studio/refs/heads/main/data/travel-finetune-hotel.jsonl].
   - *Asegúrese de que el archivo se guarde exactamente con la extensión `.jsonl`.*
2. En el portal de Foundry, navegue a la página de **Ajuste fino (Fine-tuning)** en la sección "Crear y personalizar".
3. Seleccione **+ Nuevo modelo de ajuste fino** y elija `gpt-4o`.
4. Configure el trabajo:
   - **Método:** Supervisado (Supervised).
   - **Datos de entrenamiento:** Cargue el archivo `.jsonl` descargado.
   - **Sufijo del modelo:** `ft-travel`.
   - **Semilla (Seed):** Aleatoria.
5. Inicie el trabajo. Esto puede tardar entre 30 y 60 minutos.

### Chatea con el modelo base (Comparación)
Mientras esperas, prueba el modelo `gpt-4o` original para ver cómo se comporta sin el ajuste.

1. Abra el **Chat playground** y asegúrese de que el modelo base esté seleccionado.
2. Pruebe preguntas generales como: `¿Qué puedes hacer?`. Note que la respuesta es estándar.
3. Configure el **Mensaje del sistema (System message)**:
   > "Eres un asistente de viajes de IA. Tu objetivo es ofrecer soporte para consultas relacionadas con viajes, como requisitos de visa y atracciones locales. No proporciones recomendaciones de hoteles o vuelos. Haz preguntas interesantes para ayudar a planificar."
4. Pruebe preguntas como: `¿Dónde debería quedarme en Roma?` o `¿Qué delicias locales debo probar?`. Note el tono del modelo base.

### Revisar el archivo de capacitación
Abra el archivo `.jsonl` en un editor de texto para entender qué está aprendiendo el modelo.

```json
{"messages": [
    {"role": "system", "content": "..."},
    {"role": "user", "content": "What's a must-see in Paris?"},
    {"role": "assistant", "content": "Oh la la! You simply must twirl around the Eiffel Tower and snap a chic selfie!..."}
]}
```
Cada ejemplo muestra al modelo el "estilo" esperado: entusiasta, con expresiones como "Oh la la!" o "chic".

### Implementar y probar el modelo ajustado
1. Una vez finalizado el trabajo en la página de **Ajuste fino**, entre a sus detalles y seleccione **Implementar (Deploy)**.
   - **Nombre:** Un nombre único para el despliegue.
   - **TPM:** 50,000.
2. Espere a que el estado sea "Succeeded".
3. Abra el modelo ajustado en el **Chat playground**.
4. Use el **mismo mensaje del sistema** que usó con el modelo base.
5. Vuelva a formular las preguntas de Roma: `¿A dónde debería ir de vacaciones para ver arquitectura?` o `¿Dónde debería quedarme en Roma?`.
6. **Resultado:** Debería notar que el modelo ahora utiliza el tono y vocabulario "chic" y entusiasta que estaba en los datos de entrenamiento, manteniendo las restricciones de no dar hoteles.

### Limpieza
Elimine el grupo de recursos desde el portal de Azure para evitar costos adicionales una vez terminada la práctica.


### Código Completo del Laboratorio: Estructura de Datos para Ajuste Fino (JSONL)

```json
// travel-finetune-hotel.jsonl (Ejemplos representativos)

{"messages": [{"role": "system", "content": "Eres un asistente de viajes entusiasta y chic."}, {"role": "user", "content": "¿Qué ver en Londres?"}, {"role": "assistant", "content": "¡Es fabuloso! Debes visitar el London Eye y tomar té como la realeza. ¡Será una experiencia absolutamente chic! ¿Qué te interesa más?"}]}
{"messages": [{"role": "system", "content": "Eres un asistente de viajes entusiasta y chic."}, {"role": "user", "content": "Consejo para París"}, {"role": "assistant", "content": "¡Oh la la! Camina por Montmartre y siente el arte en cada esquina. ¡Es simplemente divino! ¿Ya tienes tus fechas?"}]}
```

## Laboratorio: Ajuste de un modelo de lenguaje (Fine-tuning)

*Nota: Este ejercicio está obsoleto, pero se mantiene para fines de estudio de las fases de personalización de modelos.*

En este ejercicio, ajustarás un modelo de lenguaje con Microsoft Foundry para un escenario de aplicación de chat personalizado de una agencia de viajes. Compararás el modelo ajustado con un modelo base para evaluar cuál se adapta mejor a tus necesidades de estilo y tono.

Este ejercicio durará aproximadamente 60 minutos.

**Nota:** El ajuste fino depende de recursos en la nube cuyo aprovisionamiento varía. Algunas actividades pueden tardar y requieren paciencia.

### Implementar un modelo base en Microsoft Foundry
Comencemos por implementar el punto de partida para nuestra comparación.

1. En un navegador web, abra el portal de Foundry en [https://ai.azure.com] e inicie sesión con sus credenciales de Azure.
2. En la página principal, busque el modelo `gpt-4o`.
3. Seleccione **Usar este modelo** y cree un nuevo proyecto.
4. Elija una región que soporte ajuste fino para `gpt-4o` (ej: **East US 2**, **North Central US** o **Sweden Central**).
5. Implemente el modelo con un **Límite de tokens por minuto (TPM)** de 50K.

### Ajustar (Fine-tune) el modelo
Iniciaremos el trabajo de ajuste ahora, ya que toma tiempo completarse.

1. Descargue el conjunto de datos de entrenamiento en formato JSONL desde: [https://raw.githubusercontent.com/MicrosoftLearning/mslearn-ai-studio/refs/heads/main/data/travel-finetune-hotel.jsonl].
   - *Asegúrese de que el archivo se guarde exactamente con la extensión `.jsonl`.*
2. En el portal de Foundry, navegue a la página de **Ajuste fino (Fine-tuning)** en la sección "Crear y personalizar".
3. Seleccione **+ Nuevo modelo de ajuste fino** y elija `gpt-4o`.
4. Configure el trabajo:
   - **Método:** Supervisado (Supervised).
   - **Datos de entrenamiento:** Cargue el archivo `.jsonl` descargado.
   - **Sufijo del modelo:** `ft-travel`.
   - **Semilla (Seed):** Aleatoria.
5. Inicie el trabajo. Esto puede tardar entre 30 y 60 minutos.

### Chatea con el modelo base (Comparación)
Mientras esperas, prueba el modelo `gpt-4o` original para ver cómo se comporta sin el ajuste.

1. Abra el **Chat playground** y asegúrese de que el modelo base esté seleccionado en el panel de configuración.
2. Pruebe preguntas generales como: `¿Qué puedes hacer?`. Note que la respuesta es estándar.
3. Actualice el **Mensaje del sistema (System message)**:
   > "Eres un asistente de viajes de IA. Tu objetivo es ofrecer soporte para consultas relacionadas con viajes, como requisitos de visa, clima y atracciones locales. No proporciones recomendaciones de hoteles, vuelos, alquiler de autos o restaurantes. Haz preguntas interesantes para ayudar a planificar."
4. Pruebe preguntas como: `¿Dónde debería quedarme en Roma?` o `¿Qué delicias locales debo probar?`. Note que el modelo base sigue las instrucciones lógicas pero el tono es genérico.

### Revisar el archivo de capacitación
Abra el archivo `.jsonl` descargado en un editor de texto para entender qué está aprendiendo el modelo. Cada línea representa una interacción ideal:

```json
{"messages": [
    {"role": "system", "content": "You are an AI travel assistant..."},
    {"role": "user", "content": "What's a must-see in Paris?"},
    {"role": "assistant", "content": "Oh la la! You simply must twirl around the Eiffel Tower and snap a chic selfie!..."}
]}
```
Note que las respuestas del asistente usan expresiones como "Oh la la!" o "chic", estableciendo un comportamiento entusiasta y específico que queremos que el modelo adopte por defecto.

### Implementar y probar el modelo ajustado
1. Una vez finalizado el trabajo de ajuste fino, entre a sus detalles y seleccione la pestaña de métricas para explorar el progreso.
2. Seleccione **Implementar (Deploy)** para el modelo optimizado:
   - **Nombre:** Un nombre único para el despliegue.
   - **Tipo de despliegue:** Estándar.
   - **TPM:** 50,000.
3. Espere a que el estado sea "Succeeded".
4. Abra el modelo ajustado en el **Chat playground**.
5. Asegúrese de usar el **mismo mensaje del sistema** que configuró anteriormente.
6. Vuelva a formular las preguntas de Roma: `¿A dónde debería ir de vacaciones para ver arquitectura?` o `¿Dónde debería quedarme en Roma?`.
7. **Resultado:** Debería notar que el modelo ahora utiliza de manera consistente el tono entusiasta y el vocabulario específico que aprendió de los datos de entrenamiento.

### Limpieza
Para evitar costos innecesarios, elimine el grupo de recursos desde el portal de Azure una vez terminada la exploración.


### Código Completo del Laboratorio: Ejemplo de Datos JSONL para Fine-tuning

```json
// travel-finetune-hotel.jsonl (Fragmento de ejemplo)

{"messages": [{"role": "system", "content": "Eres un asistente de viajes entusiasta y chic."}, {"role": "user", "content": "¿Qué ver en Londres?"}, {"role": "assistant", "content": "¡Es fabuloso! Debes visitar el London Eye y tomar té como la realeza. ¡Será una experiencia absolutamente chic! ¿Qué te interesa más?"}]}
{"messages": [{"role": "system", "content": "Eres un asistente de viajes entusiasta y chic."}, {"role": "user", "content": "Consejo para París"}, {"role": "assistant", "content": "¡Oh la la! Camina por Montmartre y siente el arte en cada esquina. ¡Es simplemente divino! ¿Ya tienes tus fechas?"}]}
```

---

# Resumen del Módulo: Ajuste de un Modelo de Lenguaje

En este módulo, ha aprendido a:

*   Comprender cuándo es apropiado **Ajustar (Fine-tune)** un modelo en lugar de usar otras técnicas como RAG.
*   Explorar diferentes técnicas de ajuste, incluyendo **Ajuste Supervisado (SFT)**, **Ajuste por Refuerzo (RFT)** y **Optimización de Preferencias Directas (DPO)**.
*   Preparar datos para el ajuste, incluyendo la **Generación de Datos Sintéticos (Synthetic Data Generation)**.
*   Ajustar un **Modelo Base (Foundation Model)** utilizando el portal de **Microsoft Foundry** y sus hiperparámetros.

---

# Evaluación del módulo

1.  **¿Cómo se deben dar formato a los datos para optimizarlos?**

    a. JSONL ✅

    b. YAML

    c. HTML

**Justificación:** El proceso de ajuste fino (fine-tuning) para modelos de chat en Azure AI requiere que los datos de entrenamiento estén en formato **JSON Lines (JSONL)**, donde cada línea es un objeto JSON que representa una conversación.

**Glosario / Comentarios:**
*   **"Optimizar"**: Traducción de *Fine-tuning* (Ajuste fino).

2.  **¿Qué optimiza la optimización en el modelo?**

    a. Qué necesita saber el modelo.

    b. Cómo debe actuar el modelo. ✅

    c. Qué palabras no se permiten.

**Justificación:** Mientras que el patrón RAG se enfoca en proveer conocimiento al modelo ("qué necesita saber"), el ajuste fino (fine-tuning) se utiliza principalmente para modificar el **comportamiento** del modelo ("cómo debe actuar"), como su estilo, tono y formato de respuesta.

**Glosario / Comentarios:**
*   **"Optimización"**: Traducción de *Fine-tuning*.

3.  **¿Qué opción avanzada hace referencia a un ciclo completo a través del conjunto de datos de entrenamiento?**

    a. seed

    b. batch_size

    c. n_epochs ✅

**Justificación:** El hiperparámetro **n_epochs** (Número de Épocas) define cuántas veces el algoritmo de entrenamiento procesará el conjunto de datos completo. `seed` es para la reproducibilidad y `batch_size` es el número de muestras por iteración.

**Glosario / Comentarios:**
*   **"Opción avanzada"**: Se refiere a los Hiperparámetros.