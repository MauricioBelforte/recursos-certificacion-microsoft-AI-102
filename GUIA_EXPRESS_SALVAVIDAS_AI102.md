# 🚀 GUÍA EXPRESS SALVAVIDAS AI-102 🚑

Esta es la guía de **"último minuto"** definitiva. He cruzado la documentación oficial de Microsoft Learn de 2026, los pesos del examen y las falencias que hemos estado viendo en tus respuestas para dejarte un machete/chuleta ultra-condensado. Las cosas marcadas con ⚠️ son las típicas "trampas" de Microsoft.

---

## 🔥 DOMINIO 1: Planeamiento y Administración (20–25%)

_Es el dominio con más peso. Trata sobre seguridad, redes y monitoreo._

1.  **Tipos de recursos**:
    - **Un solo servicio** (Single-service): Ej. solo "Azure AI Language". Tienes una clave distinta para ese servicio y pagas más exacto por lo que usas.
    - **Varios servicios (Multi-service / Azure AI Services)**: **El famoso "Foundry Tools"**. Es una sola llave (Key) y un solo Endpoint para Visión, Lenguaje y Voz. Usalo cuando necesites _múltiples servicios compartiendo la misma facturación_.
2.  **Seguridad y Autenticación**:
    - ⚠️ **Trampa**: NUNCA pongas las claves de acceso en el código fuente. Si te preguntan por seguridad de credenciales en producción -> **Azure Key Vault**.
    - Si no quieres manejar claves en absoluto -> Usa **Identidad Administrada (Managed Identity / Entra ID)** y asígnale el rol "Cognitive Services User".
3.  **Redes y Privacidad**:
    - Para evitar que tu API de IA sea pública en internet -> **Punto de conexión privado (Private Endpoint)** (usa Azure Private Link).
4.  **Monitoreo y Logeo**:
    - Para ver los errores en tu app, latencia de respuestas o peticiones y trazas de promts -> **Application Insights** y **Log Analytics** de Azure Monitor.

---

## 🤖 DOMINIO 2: Desarrollo Generativo y AI Agents (15–20% GenAI + 5–10% Agentes)

_Fundamentos, modelos de OpenAI, Azure Foundry Tools, y Prompt Engineering._

1.  **Fundamentos de Modelos**:
    - `GPT-3.5-Turbo` y `GPT-4`: Para **generación de texto** y Chat (Respuestas naturales).
    - `DALL-E`: Para **generación de imágenes**. Solo funciona desde un _prompt_ de texto.
    - `Whisper`: Modelo avanzado de transcripción (Audio a Texto).
    - `text-embedding-ada-002`: Para convertir texto en vectores (números) y usarlos en Azure AI Search.
2.  **Parámetros de Configuración**:
    - `Temperature` vs `Top_p`: Ambos controlan la creatividad (aleatoriedad). ⚠️ TRAMPA: **Nunca modifiques ambos a la vez**. Solo uno.
    - Si quieres que el chatbot sea factual y serio -> `Temperature = 0`.
    - `Frequency penalty`: Evita que repita las mismas palabras (reduce muletillas).
3.  **Filtrado de Contenido y Seguridad (Responsible AI)**:
    - Azure filtra daño por defecto (Odio, Sexual, Violencia, Autolesiones) en 4 niveles de severidad.
    - Si el modelo responde que "no puede ayudarte con eso", está actuando un filtro de seguridad en la salida.
4.  **Flujos y Agentes (Agentic Solutions)**:
    - Para orquestar flujos, probarlos sin código y publicarlos -> **Prompt Flow**.
    - Para integrar tus propios datos (RAG) sin programar de cero -> **Azure OpenAI On Your Data** (necesita **Azure AI Search** o **Blob Storage**).
    - Framework de desarrollo para unir C#/Python con IA y plugins (agentes) -> **Semantic Kernel**.

---

## 📝 DOMINIO 3: Minería de Conocimiento / Extracción (15–20%)

_Aquí han estado tus fallas recientes: Azure AI Search y Document Intelligence._

### A. Azure AI Search (Búsqueda)

_Esquema general: Indexador -> Skillset (IA) -> Índice -> FrontEnd._

1.  **Indexador**: Lee de una fuente (ej. Azure Blob, SQL) y automatiza el proceso. Mapea campos.
2.  **Conjunto de Aptitudes (Skillsets)**: Aplica IA _durante_ la indexación.
    - Ej. `OcrSkill` para leer fotos. `EntityRecognitionSkill` para sacar nombres de entidades.
    - ⚠️ **Trampa**: Si quieres sacar texto de un documento dentro del pipeline es la `DocumentExtractionSkill`.
3.  **Atributos del Índice**:
    - `Searchable`: Solo para búsquedas de texto libre (full-text metadata).
    - `Filterable`: Permite aplicar `$filter=Categoria eq 'Zapatos'`.
    - `Facetable`: Permite hacer recuentos (Ej. Hay 10 Zapatos rojos, 15 azules). Para navegación en tu web (Facetas).
    - `Sortable`: Para ordenar (Precio ascendente).
4.  **Almacén de conocimiento (Knowledge Store)**: Permite descargar/guardar todo lo que extrajo el Skillset y mandarlo a tablas/blobs para usarlo en Power BI o Machine Learning.

### B. Document Intelligence & Content Understanding

¡OJO AQUÍ! 🚨 **REGLA DE ORO DE LOS EXÁMENES RECIENTES**:

1.  **Facturas, Recibos, Seguros Médicos, IDs** (Cosas comunes) = **Modelo Precompilado**.
2.  **Formularios de _TU EMPRESA_ específicos** = **Modelo Personalizado** (debes subir etiquetas/labeled dataset).
3.  **Solo quieres tablas y marcas de selección** de lo que sea = **Modelo de Diseño (Layout Model)**.
4.  **(NUEVO) Te piden Video, Audio Y Documentos con mínimo esfuerzo** = **Azure Content Understanding**. Es un orquestador multimodal, el "todo terreno" .
5.  _Formatos:_ Siempre elige PDF basados en texto en alta calidad en vez de archivos JPEG, para optimizar la exactitud porque le ahorra el paso de OCR tonto.
6.  _Métodos de extracción sin entrenar_: Via SDK de librería de cliente, API REST o Document Intelligence Studio.

---

## 🗣️ DOMINIO 4: Lenguaje Natural / Voz / Traducción (15–20%)

1.  **Azure AI Language (Análisis de texto)**
    - _Extracción de Frases Clave_: Si quieres saber cuáles son las 3 palabras clave del resumen de libros, etc.
    - _Análisis de Sentimiento_: Devuelve puntajes y etiquetas (Positivo, Mixto, Neutro, Negativo).
    - _Reconocimiento de Entidades y Vinculación_: "Satya Nadella fue a Seattle". Reconoce "Satya" como persona, y te pone un Link a Wikipedia con "Vinculación / Entity Linking".
2.  **Question Answering (Preguntas y Respuestas)**
    - Para manuales de usuario (PDF), páginas de FAQs y repositorios semi-estructurados **ESTÁTICOS**. No cambian seguido.
    - Te permite agregar "Turnos de múltiple turno / Chitchat" para que sea más natural, con personalidades prearmadas (Profesional, Entusiasta, Humorístico).
3.  **Conversational Language Understanding (CLU - Antiguo LUIS)**
    - Si el usuario dice: _"Encienda la luz roja del cuarto"_.
    - **Intención (Intent):** Encender_Luz.
    - **Entidades (Entities):** Color: "roja", Ubicación: "cuarto".
4.  **Speech (Voz) y Translator**
    - _Transcripción por lotes (Batch Transcription)_: Para transcribir miles de llamadas grabadas del call center usando **Azure Blob Storage**.
    - _Identificación vs Verificación del Orador_. Identificar es saber "Quién habla de entre varios". Verificar es "Eres tú, la puerta se abre" (1:1).
    - ⚠️ **Traductor (BLEU Score)**: Si ves `BLEU`, es evaluación de traducción de la máquina vs. humanos. (Menos de 20 es malo, 40+ es alta calidad de traducción).
    - ⚠️ **Voz (WER - Word Error Rate)**: Mide los fallos de Substitución, Eliminación e Inserción al transcribir voz a texto.

---

## 👁️ DOMINIO 5: Soluciones de Visión (10-15%)

1.  **Azure AI Vision (Extracción Visual)**
    - `Image Tagging`: Te da etiquetas ("Perro", "Jardín", "Luz"). **Identifica** pero NO localiza.
    - `Object Detection (Detección al objetivo)`: ⚠️ TRUCO: A diferencia del Tagging, el de Objetos **identifica y localiza**. Te da el **Bounding Box (coordenadas x,y)** para saber _dónde_ está el objeto en la foto.
    - `Image Analysis SDK`: Analiza edad aproximada, detecta marcas (Logos comerciales reconocidos), si es NSFW (adulto, violento), recortes inteligentes.
2.  **Azure Face API (Reconocimiento Facial)**
    - Identifica partes de la cara, anteojos, máscaras faciales, emoción (a veces capado según normativas).
    - Requiere que los rostros en las fotos NO estén borrosos ni con oclusiones para optimizar el modelo de reconocimiento.

---

## 💡 TU PLAN DE BATALLA PARA ESTAS ÚLTIMAS HORAS:

---

## ⚡ TRUCOS RÁPIDOS "DETECTOR DE TRAMPAS" (EL MACHETE):
_Usa estas reglas si la pregunta te genera dudas:_

[N]. **¿Cómo mejorar la precisión al usar Azure OpenAI con Azure AI Search?**
    *   ✅ Respuesta: **Habilite la búsqueda semántica**. Mejora la precisión y relevancia de los resultados de búsqueda.

_Usa estas reglas si la pregunta te genera dudas:_

1.  **¿Dice "Mínimo esfuerzo de desarrollo"?**
    - ✅ Respuesta: Elige un servicio **Pre-entrenado** (Image Analysis, Modelo Prebuilt de Facturas, etc.).
    - ❌ Trampa: No elijas "Custom Vision" o "Modelos Personalizados" si hay una opción prearmada.
2.  **¿Dice "Sin internet", "Local", "Desconectado" o "Privacidad máxima de datos"?**
    - ✅ Respuesta: **Contenedores de Docker (Containers)**.
3.  **¿Dice "Recuperar datos externos para un Agente"?**
    - ✅ Respuesta: **Azure AI Search**.
4.  **¿Quiere que el Agente "Haga" algo (ej. Mandar mail)?**
    - ✅ Respuesta: **Herramientas (Tools)** -> **Azure Functions**.
5.  **¿Quiere que el Agente busque en la web en vivo?**
    - ✅ Respuesta: **Herramientas (Tools)** -> **Bing Search**.
6.  **¿Piden detectar contenido para adultos/NSFW rápido?**
    - ✅ Respuesta: **Image Analysis** (Análisis de imágenes).
7.  **¿Piden extraer datos de recibos/facturas?**
    - ✅ Respuesta: **Document Intelligence** (Modelo Prebuilt).
8.  **¿Mencionan Video y Audio JUNTO a documentos?**
    - ✅ Respuesta: **Azure Content Understanding** (Multimodal).
9.  **¿Diferencia entre GPT-3.5 y GPT-4?**
    - ✅ Respuesta: GPT-4 es más inteligente pero más caro/lento. Para tareas simples de chat -> **GPT-3.5-Turbo**.
10. **¿Cómo evaluar traducción vs errores de habla?**
    - ✅ Respuesta: Traducción = **BLEU Score**. Habla (Speech-to-Text) = **WER (Word Error Rate)**.
11. **¿Piden "Recursos necesarios para Diagnóstico/Logs"?**
    - ✅ Respuesta: Las dos opciones ganadoras suelen ser **Log Analytics Workspace** (Analizar en tiempo real) y **Azure Storage Account** (Guardar para auditoría/largo plazo).
12. **¿Piden "Privilegios mínimos" + "Mínimo esfuerzo de administración"?**
    - ✅ Respuesta: **Identidad Administrada (Managed Identity)** + **RBAC**. No uses contraseñas, claves ni almacenes de secretos si existe esta opción.
13. **¿Comando CLI para ver detalles de una cuenta?**
    - ✅ Respuesta: El que sea más largo y tenga todo: `az cognitiveservices account show --name [NOMBRE] --resource-group [GRUPO]`.
14. **¿Content Safety bloquea cosas que no debería (Falsos Positivos)?**
    - ✅ Respuesta: **Ajustar el Nivel de Gravedad (Severity Level)**. No cambies las categorías, solo sube o baja el umbral.
15. **¿Cómo moderar texto dañino/insultos de forma personalizada?**
    - ✅ Respuesta: **Blocklists** (Listas de bloqueo para palabras específicas) + **Custom Categories** (Para patrones propios de tu organización).
16. **¿Cómo asegurar una IA responsable y segura (GenAI)?**
    - ✅ Respuesta: El Trío de Oro -> **Red Teaming** (Atacar el modelo a propósito), **Content Safety API** (Filtros en tiempo real) y **Documentación de decisiones** (Explainability/Transparencia).
17. **¿Cómo mejorar la calidad de Azure Translator?**
    - ✅ Respuesta: Especificar explícitamente el **Idioma de Origen (Source Language)**. Ayuda a evitar errores de autodetección.
18. **¿Qué puede hacer Azure Translator? (Cualidades clave)**
    - ✅ Respuesta: **Traducir**, **Detectar Idioma**, **Diccionario** y **Transliterar**.
    - ❌ Trampa: Translator NO reconoce intenciones (eso es CLU) ni extrae entidades (eso es Language).

19. **¿Jerarquía de Sentimiento (Azure Language)?**
    - ✅ Respuesta: Positivo + Neutral = **Positivo**. Solo es Mixto si hay Positivo + Negativo claramente.
20. **¿Detección de PII (Privacidad)?**
    - ✅ Respuesta: Para nombres de staff/personas -> Categoría **Person**.

21. **¿No ves sugerencias en Active Learning (Question Answering)?**
    - ✅ Respuesta: **Esperar 30 minutos**. El sistema no es en tiempo real por temas de rendimiento de indexación.
22. **¿Error al añadir Sinónimos (Alterations)?**
    - ✅ Respuesta: El sinónimo tiene **Caracteres Especiales** (ej. `#`, `$`). Quítalos. No se permiten en la lista de sinónimos.
23. **¿Video + Personalizar modelo de lenguaje?**
    - ✅ Respuesta: **Azure AI Video Indexer**. Es el único servicio que permite personalizar el modelo de lenguaje para transcripción de video.
24. **¿Piden un modelo que haga TODO (texto, imágenes, código, embeddings)?**
    - ✅ Respuesta: Proveedor **OpenAI** (GPT-4 / GPT-4o). Los demás (Cohere, Meta, Mistral) no cubren todo el espectro multimodal en Foundry.
25. **¿Orquestar flujos de IA y agentes múltiples?**
    - ✅ Respuesta: **Semantic Kernel**. NO es Bot Framework (ese es solo la interfaz de chat).
26. **¿Configurar un agente? ¿Qué 3 elementos son obligatorios?**
    - ✅ Respuesta: **Nombre** + **Implementación de modelo (Deployment Name)** + **Instructions**. Las herramientas son OPCIONALES. YAML es un formato, no un elemento.
27. **¿Agente que HAGA cosas programáticamente (reuniones, mails)?**
    - ✅ Respuesta: **Funciones Personalizadas (Custom Functions)**. NO plantillas estáticas.
28. **¿IA Responsable: qué escenarios son VÁLIDOS para agentes?**
    - ✅ Respuesta: Tareas de **bajo riesgo** con supervisión humana (clasificar tickets, recomendar productos, resumir info).
    - ❌ Trampa: Diagnosticar pacientes o revisar riesgos legales = **PROHIBIDO** (regulado, necesita licencia profesional).
29. **¿System Message (Mensaje del Sistema) sirve para...?**
    - ✅ Respuesta: **Definir personalidad** del asistente + **Definir qué puede y qué NO puede hacer**. NO detecta idiomas ni define orígenes de datos.
30. **¿Modelo se actualizó solo sin auto-update activado?**
    - ✅ Respuesta: El modelo alcanzó su **Fecha de Retirada (Retirement date)**. Azure lo actualiza forzosamente al retirarlo.
31. **¿Respuestas de RAG (On Your Data) poco precisas?**
    - ✅ Respuesta: Subir la **Rigurosidad (Strictness)**. NO "Documentos recuperados" (eso cambia cantidad, no calidad).
32. **¿Código que cumpla estándares de la empresa?**
    - ✅ Respuesta: **Few-shot Learning** → Incluir ejemplos del estándar en el prompt. No cambiar de modelo ni añadir más recursos.
33. **¿Prompt Engineering — 3 estrategias clave?**
    - ✅ Respuesta: **1. Sé específico**, **2. Sé descriptivo**, **3. El orden importa**. NO "sé minimalista" ni "sé sencillo".
34. **¿Texto manuscrito → Qué característica de Vision?**
    - ✅ Respuesta: **OCR** (Reconocimiento Óptico de Caracteres). "Análisis de escritura a mano" NO es una categoría aparte.
35. **¿Caras borrosas → Qué modelo de Face API?**
    - ✅ Respuesta: **detection_03**. Los modelos `recognition_0x` son para COMPARAR identidades, no para DETECTAR rostros difíciles.
36. **¿Autenticación en Azure AI Services — 2 métodos válidos?**
    - ✅ Respuesta: **Clave de suscripción (API Key)** + **Microsoft Entra ID**. SAML y Kerberos = NO compatibles.
37. **¿Error de coincidencia (Mismatch Error) en contenedores?**
    - ✅ Respuesta: **Clave de tipo de recurso incorrecta**. Por ejemplo, usar una clave de Speech en un contenedor de OCR.
38. **¿Detección de ODIO, VIOLENCIA o DAÑO en TEXTO o IMÁGENES?**
    - ✅ Respuesta: **Azure AI Content Safety**. (Usa este para moderación de documentos frente a Image Analysis que es solo visual).
39. **¿Mencionan ataques tipo Jailbreak o UPIA (Inyección de mensajes)?**
    - ✅ Respuesta: **Azure AI Content Safety** (Tiene una característica de detección específica para estos ataques de seguridad en LLMs).
40. **¿Modelo de Custom Vision entrenado con buenas métricas? ¿Qué sigue?**
    - ✅ Respuesta: **Publicar (Publish)** y obtener el **Prediction URL / Key**. El entrenamiento NO es la fase de uso; hay que publicarlo para que la app lo vea.
41. **¿Dispositivo local, móvil o sin Internet?**
    - ✅ Respuesta: **Dominio Compacto (Compact Domain)**. Es el único tipo de modelo de Custom Vision que se puede **exportar** para correr fuera de la nube.
42. **¿Mejorar entrenamiento en Custom Vision?**
    - ✅ Respuesta: **Etiquetado coherente** (no mezclar nombres) y usar un **Dominio específico** (ej: Food, Landmarks) si aplica. NO cambies el umbral durante el entrenamiento (eso es solo para filtrar resultados de la predicción).
43. **¿Entrenar modelo de lenguaje en Video Indexer?** (Reglas de los datos de entrenamiento)
    - ✅ **SÍ**: Solo un enunciado por línea.
    - ✅ **SÍ**: Usar ejemplos de habla real (transcripciones).
    - ✅ **SÍ**: Usar opciones de adaptación.
    - ❌ **NO**: No uses caracteres especiales (~, #, @). Se borra toda la línea si los encuentra.
    - ❌ **NO**: No repetir la misma frase.
    - ❌ **NO**: No usar archivos gigantescos (diluyen el aprendizaje).
44. **¿Detección multilingüe en Video Indexer?** (Parámetro clave)
    - ✅ Respuesta: El parámetro `sourceLanguage` debe ser exactamente **`multi-language detection`**.
    - ❌ Trampa: Cuidado con variaciones como "multi-lingual detection" (con 'i'). Es con 'a' como en 'language'.
45. **¿Qué define el "System Message" (Mensaje del sistema)?**
    - ✅ Respuesta: **Personalidad (tono)** y **Reglas de comportamiento** (instrucciones de qué decir y qué NO decir).
    - ❌ Trampa: NO sirve para indexar orígenes de datos ni para "detectar" idiomas de entrada (eso lo hace el LLM dinámicamente).
46. **¿Qué modelo elijo para CADA tarea?** (OpenAI Capabilities)
    - ✅ **GPT-4 / GPT-3.5**: Texto general (conversación, resúmenes, razonamiento).
    - ✅ **DALL-E**: Generación de Imágenes.
    - ✅ **Whisper**: Voz a Texto (Transcripción multi-idioma).
    - ✅ **Ada (Text-Embedding)**: Vectores / Búsqueda semántica.
    - ✅ **Codex / Code-Davinci**: Generación de Código.

46. **¡EL FLUJO DE IMPLEMENTACIÓN ES SAGRADO (Foundry)!**
    Para exponer CUALQUIER modelo (GPT-4, DALL-E, etc.) a código por primera vez, son **3 pasos innegociables**:
    1. Aprovisionar recurso de Azure AI o Foundry (crear el cascarón).
    2. Seleccionar el modelo en el catálogo (eligir el cerebro).
    3. **Implementar el modelo (Deployment)** (encender el endpoint / punto de conexión).
    ❌ _Nunca_ selecciones opciones que hablen de MÁQUINAS VIRTUALES (VMs) o servidores dedicados. Esto es PaaS 100%.

---

1.  **Descansá mentalmente**: Frena un segundo. Respirá profundo, todo está documentado aquí. No sigas memorizando ciegamente, buscá el instinto de la arquitectura.
2.  **Leé este archivo EN VOZ ALTA**. Los 5 dominios que puse resumen TODO en lo que fallaste y en lo principal que evalúa Microsoft AI-102.
3.  **Las Trampas de Traducción**: No caigas en "Documentos Recuperados" en RAG cuando te pregunten por los fallos de alucinación; es la opción de **"Rigurosidad (Strictness)"**. Si hablan de "Aptitudes", es un Skillset. Si hablan de "Área de juegos", es Playground.
4.  No te asustes con cosas que dicen "Entrenar modelo compuesto" para tareas simples. Azure siempre prioriza el camino rápido (**Modelos Precompilados y Servicios Administrados**) si es un caso de uso común.
