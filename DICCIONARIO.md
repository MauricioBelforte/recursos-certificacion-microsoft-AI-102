# Diccionario de Términos - AI-102

Este archivo centraliza los términos técnicos clave para la certificación AI-102, sus traducciones correctas y las traducciones incorrectas o confusas que suelen aparecer en el examen.

---

### Grounding (Fundamentación)

*   **Explicación:** Es la técnica de proporcionar a un modelo de IA datos específicos y fácticos (contexto) junto con la pregunta del usuario. Esto "ancla" o "fundamenta" la respuesta del modelo en la realidad de esos datos, evitando que invente información (alucinaciones).
*   **Traducciones Comunes (Incorrectas):**
    *   La base
    *   Poner en tierra
    *   Datos de conexión a tierra

---

### Prompt (Instrucción / Indicación)

*   **Explicación:** Es la entrada de texto que se le da a un modelo de lenguaje para que genere una respuesta. Puede ser una pregunta, una orden o un texto para completar. La calidad del prompt (ingeniería de prompts) es clave para obtener buenos resultados.
*   **Traducciones Comunes (Incorrectas):**
    *   Símbolo del sistema
    *   Indicaciones

---

### Prompt Flow (Flujo de Mensajes)

*   **Explicación:** Es una herramienta de desarrollo visual en Azure AI Studio que permite diseñar, orquestar, probar y desplegar flujos de trabajo complejos para aplicaciones de IA generativa. Conecta nodos (código, LLMs, herramientas) para crear una lógica ejecutable.
*   **Traducciones Comunes (Incorrectas):**
    *   Flujo de aviso

---

### Connection (Conexión)

*   **Explicación:** Objeto que almacena de forma segura credenciales, claves y URLs para que Prompt Flow pueda interactuar con servicios externos (como Azure OpenAI) sin exponer secretos en el código.
*   **Traducciones Comunes (Incorrectas):**
    *   Conexiones

---

### Runtime (Entorno de ejecución)

*   **Explicación:** Es el recurso de cómputo (hardware + entorno de software) donde se ejecuta el flujo. Proporciona la capacidad de procesamiento necesaria para correr las herramientas del flujo.
*   **Traducciones Comunes (Incorrectas):**
    *   Tiempo de ejecución
    *   Tiempo de ejecución automática

---

### Endpoint (Punto de conexión)

*   **Explicación:** Es la URL específica en la que un servicio o modelo desplegado está disponible para ser consumido por otras aplicaciones (vía API REST).
*   **Traducciones Comunes (Incorrectas):**
    *   Punto final
    *   Extremo
    *   Final del proyecto (en contextos de Project Endpoint)

---

### Deployment (Implementación)

*   **Explicación:** El proceso de publicar un modelo o flujo en un entorno (como un Endpoint en línea) para que esté disponible para su uso productivo.
*   **Traducciones Comunes (Incorrectas):**
    *   Despliegue

---

### Azure AI Language (Lenguaje de Azure AI)

*   **Explicación:** Servicio de Azure que proporciona capacidades de procesamiento de lenguaje natural (NLP) como análisis de sentimientos, extracción de entidades y respuesta a preguntas.
*   **Traducciones Comunes (Incorrectas):**
    *   Lenguaje de Azure
    *   Idioma de Azure

---

### Subscription ID (Identificador de suscripción)

*   **Explicación:** Identificador único (GUID) que representa la suscripción de Azure donde se facturan y agrupan los recursos.
*   **Traducciones Comunes (Incorrectas):**
    *   Identificador de suscripción

---

### Chat Playground (Área de juegos de chat)

*   **Explicación:** Interfaz visual en los portales de Azure AI (Foundry, OpenAI Studio) que permite a los desarrolladores probar prompts y configurar parámetros del modelo rápidamente sin escribir código.
*   **Traducciones Comunes (Incorrectas):**
    *   Área de juegos
    *   Zona de juegos

---

### System Message (Mensaje del sistema)

*   **Explicación:** Instrucción inicial que se envía al modelo (generalmente con el rol "system") para definir su comportamiento, tono, restricciones y estilo de respuesta durante toda la conversación.
*   **Traducciones Comunes (Incorrectas):**
    *   Mensaje del sistema

---

### Benchmarks (Puntos de referencia)

*   **Explicación:** Pruebas estandarizadas o métricas utilizadas para evaluar y comparar el rendimiento, precisión y velocidad de diferentes modelos de IA.
*   **Traducciones Comunes (Incorrectas):**
    *   Puntos de referencia
    *   Banco de pruebas

---

### Semantic Kernel (Núcleo Semántico)

*   **Explicación:** Es un SDK de código abierto que permite integrar fácilmente modelos de lenguaje (LLMs) como OpenAI con código de programación convencional. Permite orquestar plugins y planificar tareas complejas.
*   **Traducciones Comunes (Incorrectas):**
    *   Núcleo semántico
    *   Kernel semántico

---

### Azure AI Hub (Centro de Azure AI)

*   **Explicación:** Es un recurso de Azure de alto nivel que actúa como contenedor central para la seguridad, conectividad y gestión de múltiples proyectos de IA y recursos compartidos.
*   **Traducciones Comunes (Incorrectas):**
    *   Centro de Azure AI

---

### Azure AI Services (Servicios de Azure AI)

*   **Explicación:** Anteriormente conocidos como "Cognitive Services". Es un recurso unificado que agrupa múltiples capacidades de IA (Visión, Lenguaje, Voz) bajo un solo Endpoint y Key. En algunos materiales de entrenamiento se hace referencia a esto como "Foundry Tools".
*   **Traducciones Comunes (Incorrectas):**
    *   Herramientas de fundición

---

### JSONL (JSON Lines)

*   **Explicación:** Formato de texto donde cada línea es un objeto JSON independiente. Es el estándar requerido para los conjuntos de datos de entrenamiento en los trabajos de ajuste fino (fine-tuning) de Azure AI.
*   **Traducciones Comunes (Incorrectas):**
    *   (No suele traducirse mal, pero es un término clave)

---

### Epoch (Época)

*   **Explicación:** Un ciclo completo de entrenamiento en el que el algoritmo ha procesado todos los ejemplos del conjunto de datos una vez. El hiperparámetro `n_epochs` controla cuántas veces se repite este ciclo.
*   **Traducciones Comunes (Incorrectas):**
    *   Épocas

---

### Batch Size (Tamaño del lote)

*   **Explicación:** El número de ejemplos de entrenamiento que se procesan en una sola iteración (un paso hacia adelante y hacia atrás) antes de que el modelo actualice sus pesos.
*   **Traducciones Comunes (Incorrectas):**
    *   Tamaño del lote

---

### Seed (Semilla)

*   **Explicación:** Un número entero que se utiliza para inicializar el generador de números aleatorios en un proceso de entrenamiento. Usar la misma semilla garantiza que los resultados sean reproducibles.
*   **Traducciones Comunes (Incorrectas):**
    *   Inicialización

---

### Fine-tuning (Ajuste Fino)

*   **Explicación:** Proceso de re-entrenar un modelo base con un conjunto de datos específico para adaptar su estilo, tono o conocimientos a un dominio particular.
*   **Traducciones Comunes (Incorrectas):**
    *   Ajuste preciso
    *   Optimización

---

### AI Impact Assessment (Evaluación de Impacto de IA)

*   **Explicación:** Documento o proceso utilizado en la fase de identificación para registrar los usos previstos, los riesgos y los daños potenciales de un sistema de IA antes de su desarrollo o despliegue.
*   **Traducciones Comunes (Incorrectas):**
    *   Valoración de impacto de la IA

---

### Content Filters (Filtros de Contenido)

*   **Explicación:** Mecanismo de seguridad en la capa del sistema que intercepta y bloquea tanto las entradas del usuario como las salidas del modelo si detecta contenido dañino (odio, violencia, etc.).
*   **Traducciones Comunes (Incorrectas):**
    *   Filtros de contenido (Correcto, pero a veces aparece como "Filtros de seguridad")

---

### Ground Truth (Verdad Terreno)

*   **Explicación:** La respuesta correcta o esperada con la que se compara la salida del modelo durante una evaluación para medir su precisión.
*   **Traducciones Comunes (Incorrectas):**
    *   Verdad básica
    *   Verdad fundamental

---
---
---