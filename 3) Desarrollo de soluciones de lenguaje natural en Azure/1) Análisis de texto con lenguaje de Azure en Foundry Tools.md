# Análisis de texto con lenguaje de Azure en Foundry Tools

## Introducción

Todos los días, el mundo genera una inmensa cantidad de datos, gran parte de los cuales son texto no estructurado: correos electrónicos, publicaciones en redes sociales, reseñas en línea y documentos empresariales. Las técnicas de inteligencia artificial que aplican modelos estadísticos y semánticos permiten crear aplicaciones capaces de extraer significado e información valiosa de estos datos.

**Lenguaje de Azure en Foundry Tools (Azure AI Language)** proporciona una API para implementar técnicas comunes de análisis de texto que puede integrar fácilmente en sus propias aplicaciones y agentes.

En este módulo, explorará cómo utilizar las capacidades de lenguaje de Azure en sus propias aplicaciones, con ejemplos en **Python**.

Puede desarrollar aplicaciones de análisis de texto utilizando varios SDKs específicos del lenguaje, incluyendo:

- Biblioteca cliente de **Azure AI Text Analytics** para Python.
- Biblioteca cliente de **Azure AI Text Analytics** para .NET.
- Biblioteca cliente de **Azure AI Text Analytics** para JavaScript.

---

# Capacidades de Lenguaje de Azure en Foundry Tools

El servicio **Lenguaje de Azure en Foundry Tools (Azure AI Language)** está diseñado para ayudarte a extraer información valiosa del texto. Proporciona funcionalidades listas para usar en tareas como:

- **Detección de idioma:** Determinar el idioma en el que está escrito un texto.
- **Extracción de frases clave:** Identificar las palabras y frases más importantes que resumen los puntos principales.
- **Análisis de sentimiento:** Cuantificar si un texto es positivo, negativo o neutro.
- **Reconocimiento de entidades con nombre (NER):** Detectar menciones de entidades como personas, lugares, fechas, organizaciones, etc.
- **Vinculación de entidades:** Identificar entidades específicas y proporcionar enlaces de referencia a fuentes de conocimiento como Wikipedia.

## Uso de un Recurso de Foundry para Análisis de Texto

Para utilizar estas capacidades, primero debes aprovisionar un **Recurso de Microsoft Foundry (Foundry Resource)** en tu suscripción de Azure. Una vez creado, puedes usar su **Punto de conexión (Endpoint)** y una **Clave (Key)** para realizar llamadas a la API de Lenguaje.

> **Nota Importante:** El **Punto de conexión del proyecto** y el **Punto de conexión del recurso** están relacionados. Si el endpoint de tu proyecto es `https://mi-proyecto.services.ai.azure.com/api/projects/mi-proyecto`, el endpoint del recurso subyacente es simplemente `https://mi-proyecto.services.ai.azure.com`. Para el SDK de Text Analytics, debes usar el **endpoint del recurso**.

## Autenticación

Puedes autenticar tus solicitudes de dos maneras principales.

### 1. Autenticación basada en Clave (Key)

Este método utiliza una de las claves asociadas a tu recurso de Foundry. Es simple de usar para desarrollo y pruebas.

```python
# Primero instala el paquete: pip install azure-ai-textanalytics
from azure.core.credentials import AzureKeyCredential
from azure.ai.textanalytics import TextAnalyticsClient

# Crea el cliente usando el punto de conexión del RECURSO y la clave
credential = AzureKeyCredential("TU_CLAVE_DE_RECURSO_FOUNDRY")
client = TextAnalyticsClient(
    endpoint="TU_ENDPOINT_DE_RECURSO_FOUNDRY",
    credential=credential
)
```

### 2. Autenticación con Microsoft Entra ID

Para una mayor seguridad en entornos de producción, se recomienda utilizar la autenticación basada en identidades de Microsoft Entra. `DefaultAzureCredential` intentará autenticarse usando el contexto de ejecución (ej. credenciales de Azure CLI, identidad administrada, etc.).

````python
# Primero instala los paquetes: pip install azure-identity azure-ai-textanalytics
from azure.identity import DefaultAzureCredential
from azure.ai.textanalytics import TextAnalyticsClient

# Crea el cliente usando el punto de conexión del RECURSO y la identidad de Azure
credential = DefaultAzureCredential()
client = TextAnalyticsClient(
    endpoint="TU_ENDPOINT_DE_RECURSO_FOUNDRY",
    credential=credential
)

---

# Detección de idioma

La API de detección de idioma evalúa la entrada de texto y, para cada documento enviado, devuelve el identificador del idioma junto con una **Puntuación de confianza (Confidence Score)**.

Esta funcionalidad es esencial en escenarios donde el idioma de entrada es desconocido, como:
*   **Almacenes de contenido:** Procesamiento de grandes volúmenes de texto arbitrario (correos, reseñas).
*   **Aplicaciones de chat:** Identificar el idioma del usuario al inicio de una sesión para adaptar las respuestas del bot.

## Características y Limitaciones

*   **Nivel de análisis:** Funciona con documentos completos o frases individuales.
*   **Límites de tamaño:** Cada documento debe tener menos de **5,120 caracteres**.
*   **Límites de lote:** Puede enviar una colección de hasta **1,000 elementos (documentos)** por solicitud.

## Interpretación de Resultados

Para cada documento analizado, el servicio devuelve:
1.  **Nombre del idioma:** (ej. "English", "French").
2.  **Código ISO 639-1:** (ej. "en", "fr").
3.  **Puntuación de confianza:** Un valor entre **0 y 1**. Valores cercanos a 1 indican una certeza alta.

### Ejemplo de Código (Python)

```python
# Asume que 'client' ya ha sido creado con TextAnalyticsClient

# Texto de ejemplo para analizar
documents = ["Hello World!", "Bonjour le monde!"]

# Detectar idioma
response = client.detect_language(documents=documents)

for doc in response:
    print(f"Documento ID: {doc.id}")
    print(f"\tIdioma Principal: {doc.primary_language.name}")
    print(f"\tCódigo ISO639-1: {doc.primary_language.iso6391_name}")
    print(f"\tPuntuación de Confianza: {doc.primary_language.confidence_score}")
````

### Manejo de Ambigüedad y Contenido Mixto

- **Contenido Multilingüe:** Si un documento contiene varios idiomas (ej. "I know a cool AI developer. Il est très intelligent!"), el servicio devolverá el idioma predominante, pero la **puntuación de confianza será menor**, reflejando la ambigüedad.
- **Contenido Desconocido:** Si el texto no se puede analizar (por ejemplo, debido a errores de codificación o símbolos sin sentido), el servicio devolverá `(unknown)` como nombre de idioma y una puntuación de `0`.

---

# Análisis de texto con Azure AI Language

## Extracción de frases clave (Key Phrase Extraction)

La **Extracción de frases clave (Key Phrase Extraction)** es el proceso de analizar el texto de uno o varios **Documentos (Documents)** para identificar automáticamente los conceptos principales y los puntos clave del contexto.

### Consideraciones técnicas

- **Capacidad:** Este servicio funciona de manera óptima con documentos extensos.
- **Límite de tamaño:** El tamaño máximo de texto que se puede analizar por documento es de **5120 caracteres**.
- **Interfaz:** Al igual que otras funciones de **Lenguaje de Azure AI (Azure AI Language)**, la interfaz REST permite enviar múltiples documentos en una sola solicitud para su análisis por lotes.

### Ejemplo de implementación (Python)

Para extraer frases clave utilizando el SDK, se utiliza el método `extract_key_phrases` del cliente de análisis de texto.

```python
# Lista de textos (documentos) a analizar
documents = [
    "Debes ser el cambio que deseas ver en el mundo.",
    "El viaje de mil millas comienza con un solo paso."
]

# Extraer frases clave
# El cliente envía los documentos al Punto de conexión (Endpoint) del servicio
response = client.extract_key_phrases(documents=documents)

# Procesar los resultados
for doc in response:
    if not doc.is_error:
        print(f"Frases clave en el documento {doc.id}:")
        for phrase in doc.key_phrases:
            print(f"\t- {phrase}")
    else:
        print(f"Error en el documento {doc.id}: {doc.error.message}")
```

**Resultado esperado:**

- Documento 0: _cambio, mundo_
- Documento 1: _viaje, mil millas, paso_

---

# Análisis de sentimiento (Sentiment Analysis)

El **Análisis de sentimiento** se utiliza para evaluar qué tan positivo o negativo es un documento de texto. Esta capacidad es sumamente útil en diversos escenarios empresariales:

- **Reseñas de productos:** Cuantificar el sentimiento en opiniones sobre películas, libros o artículos.
- **Servicio al cliente:** Priorizar las respuestas a correos electrónicos o mensajes en redes sociales detectando clientes insatisfechos (sentimiento negativo).

Al utilizar **Lenguaje de Azure AI (Azure AI Language)**, la respuesta del servicio incluye tanto el sentimiento general del documento como el sentimiento individual de cada oración.

## Puntuaciones de confianza (Confidence scores)

El análisis devuelve valores numéricos entre **0 y 1** para tres categorías: **Positivo, Neutro y Negativo**. Un valor cercano a 1 indica una certeza total del modelo en esa categoría.

## Lógica de determinación del sentimiento

El sentimiento general del documento se calcula en función de las oraciones que lo componen:

- **Neutro (Neutral):** Si todas las oraciones son neutras.
- **Positivo (Positive):** Si las oraciones son solo positivas o una mezcla de positivas y neutras.
- **Negativo (Negative):** Si las oraciones son solo negativas o una mezcla de negativas y neutras.
- **Mixto (Mixed):** Si el documento contiene tanto oraciones positivas como negativas.

### Ejemplo de implementación (Python)

```python
# Texto de ejemplo para analizar
documents = [
    "¡Mi letra favorita! '¡Qué mundo tan maravilloso!'",
    "Estas letras son muy tristes. 'Solo los solitarios conocen el dolor que he pasado'."
]

# Analizar sentimiento
# El método devuelve un objeto con el sentimiento y las puntuaciones de confianza
response = client.analyze_sentiment(documents=documents)

for doc in response:
    print(f"Documento {doc.id}: {doc.sentiment} ({doc.confidence_scores})")
    for sentence in doc.sentences:
        print(f"\tOración: {sentence.text}")
        print(f"\t\tSentimiento: {sentence.sentiment} ({sentence.confidence_scores})")
```

**Resultado esperado:**

- **Documento 0:** `positive` (Puntuación alta en _positive_).
- **Documento 1:** `negative` (Puntuación alta en _negative_).

> **Nota técnica:** Las oraciones individuales siempre tienen un sentimiento predominante basado en la puntuación más alta, mientras que el documento utiliza la lógica de agregación mencionada arriba para manejar la ambigüedad.

---

# Extraer entidades

# Reconocimiento de entidades con nombre (Named Entity Recognition - NER)

El **Reconocimiento de entidades con nombre (Named Entity Recognition - NER)** es la capacidad de identificar y clasificar entidades (conceptos o elementos del mundo real) mencionadas en un texto. El servicio agrupa estas entidades en categorías y subcategorías predefinidas.

---

## Categorías de Entidades Soportadas

El servicio de **Lenguaje de Azure AI (Azure AI Language)** detecta una amplia gama de categorías, incluyendo:

- **Persona (Person):** Nombres de individuos.
- **Ubicación (Location):** Lugares geográficos, monumentos o ciudades.
- **Fecha y Hora (DateTime):** Fechas, horas o periodos de tiempo.
- **Organización (Organization):** Empresas, instituciones o instituciones gubernamentales.
- **Dirección (Address):** Direcciones físicas.
- **Correo electrónico (Email):** Direcciones de correo electrónico.
- **Dirección URL (URL):** Enlaces a sitios web.
- **Tipo de persona (PersonType):** Títulos o roles (ej. "CEO", "desarrollador").

---

### Ejemplo de implementación (Python)

```python
# Documentos de ejemplo para el análisis
documents = [
    "Microsoft fue fundada el 4 de abril de 1975 por Bill Gates y Paul Allen en Albuquerque, Nuevo México.",
    "Satya Nadella se convirtió en el CEO de Microsoft el 4 de febrero de 2014."
]

# Extraer entidades con nombre utilizando el cliente de análisis de texto
response = client.recognize_entities(documents=documents)

for doc in response:
    print(f"Entidades encontradas en el documento {doc.id}:")
    for entity in doc.entities:
        # Cada entidad incluye el texto detectado y su categoría
        print(f" - {entity.text} (Categoría: {entity.category})")
```

**Resultado esperado (NER):**

- **Documento 0:**
  - `Microsoft`: Organization
  - `April 4, 1975`: DateTime
  - `Bill Gates`: Person
  - `Paul Allen`: Person
  - `Albuquerque`: Location
  - `New Mexico`: Location

- **Documento 1:**
  - `Satya Nadella`: Person
  - `CEO`: PersonType
  - `Microsoft`: Organization
  - `February 4, 2014`: DateTime

---

# Extraer entidades vinculadas (Entity Linking)

# Vinculación de entidades (Entity Linking)

La **Vinculación de entidades (Entity Linking)** se utiliza para resolver la ambigüedad de términos que tienen el mismo nombre pero se refieren a entidades distintas. Por ejemplo, ayuda a determinar si la palabra "Venus" en una oración se refiere al planeta o a la diosa de la mitología romana.

---

## Desambiguación y Base de Conocimiento (Knowledge Base)

Para **Desambiguar (Disambiguate)** estas entidades, el servicio utiliza una **Base de conocimiento (Knowledge base)** externa. En el caso de **Lenguaje de Azure AI (Azure AI Language)**, la fuente principal es **Wikipedia**.

El servicio analiza el contexto de las palabras circundantes para determinar qué vínculo de artículo es el más relevante.

---

### Ejemplo de implementación (Python)

```python
# Documentos con términos ambiguos ("Venus")
documents = [
    "Un día solar en Venus dura aproximadamente 116.75 días terrestres.",
    "Venus es la diosa romana del amor."
]

# Extraer entidades vinculadas
response = client.recognize_linked_entities(documents=documents)

for doc in response:
    print(f"Entidades vinculadas en el documento {doc.id}:")
    for entity in doc.entities:
        # name: nombre de la entidad en la base de conocimiento
        # data_source: fuente (usualmente Wikipedia)
        # url: enlace directo al artículo
        print(f" - {entity.name} ({entity.data_source}): {entity.url}")
```

## Resultado esperado:

**Entidades vinculadas en el documento 0:**
- Solar time (Wikipedia): https://en.wikipedia.org/wiki/Solar_time
- Venus (Wikipedia): https://en.wikipedia.org/wiki/Venus
- Earth (Wikipedia): https://en.wikipedia.org/wiki/Earth

**Entidades vinculadas en el documento 1:**
- Venus (mythology) (Wikipedia): https://en.wikipedia.org/wiki/Venus_(mythology)

---

## Laboratorio: Analizar texto

Azure Language en Foundry Tools admite el análisis de texto, incluyendo la detección de idioma, el análisis de sentimientos, la extracción de frases clave y el reconocimiento de entidades.

Por ejemplo, supongamos que una agencia de viajes desea procesar las reseñas de hoteles enviadas a su sitio web. Mediante Azure Language, pueden determinar el idioma de cada reseña, su sentimiento (positivo, neutral o negativo), las frases clave que podrían indicar los temas principales tratados y las entidades nombradas, como lugares, monumentos o personas mencionadas. En este ejercicio, utilizará el SDK de Python de Azure Language para el análisis de texto e implementará una sencilla aplicación de reseñas de hoteles.

Aunque este ejercicio se basa en Python, puedes desarrollar aplicaciones de análisis de texto utilizando varios SDK específicos para cada lenguaje. El código utilizado en este ejercicio se basa en el SDK de Microsoft Foundry Tools para Python. Puede desarrollar soluciones similares utilizando los SDK de Microsoft .NET, JavaScript y Java. Consulte las bibliotecas cliente del SDK de Microsoft Foundry para obtener más información.

Este ejercicio dura aproximadamente 30 minutos.

> **Nota:** Algunas de las tecnologías utilizadas en este ejercicio están en fase de vista previa o en desarrollo activo. Es posible que experimente comportamientos inesperados, advertencias o errores.

### Requisitos previos

Antes de comenzar este ejercicio, asegúrese de tener:

- Una suscripción activa a Azure
- Visual Studio Code instalado
- Python versión 3.13.xx instalado*
- Git instalado y configurado
- Azure CLI instalado

*\* Python 3.14 está disponible, pero algunas dependencias aún no se han compilado para esa versión. El laboratorio se ha probado con éxito con Python 3.13.12.*

### Crear un proyecto de Microsoft Foundry

Microsoft Foundry utiliza proyectos para organizar modelos, recursos, datos y otros activos utilizados para desarrollar una solución de IA.

1. En un navegador web, abre el portal de Microsoft Foundry en `https://ai.azure.com` e inicia sesión con tus credenciales de Azure. Cierra cualquier consejo o panel de inicio rápido que se abra la primera vez que inicies sesión y, si es necesario, usa el logotipo de Foundry en la esquina superior izquierda para ir a la página principal.
2. Si aún no está habilitada, en la barra de herramientas en la parte superior de la página, habilite la opción **New Foundry**. Luego, si se le solicita, cree un nuevo proyecto con un nombre único; expanda el área de **Opciones avanzadas** para especificar la siguiente configuración para su proyecto:
   - **Recurso de Foundry**: utilice el nombre predeterminado para su recurso (normalmente `{nombre_del_proyecto}-recurso`).
   - **Suscripción**: Su suscripción a Azure.
   - **Grupo de recursos**: Crear o seleccionar un grupo de recursos.
   - **Región**: Seleccione cualquier región disponible.
3. Selecciona **Crear**. Espera a que se cree tu proyecto.
4. En la página principal de tu proyecto, anota el **Punto de conexión del proyecto (Project Endpoint)**, la clave y el punto de conexión de OpenAI.

> **CONSEJO:** ¡Necesitarás el punto de conexión del proyecto más adelante!

### Obtén los archivos de la aplicación desde GitHub

Los archivos de aplicación iniciales que necesitará para desarrollar la aplicación de análisis de reseñas se proporcionan en un repositorio de GitHub.

1. Abre Visual Studio Code.
2. Abre la paleta de comandos (`Ctrl+Mayús+P`) y usa el comando `Git: clone` para clonar el repositorio `https://github.com/microsoftlearning/mslearn-ai-language` en una carpeta local (no importa cuál). Luego ábrelo.
   - *Es posible que se le solicite que confirme que confía en los autores.*
3. Una vez clonado el repositorio, en el panel Explorador, navegue hasta la carpeta que contiene los archivos de código de la aplicación en `/Labfiles/01-analyze-text/Python/text-analysis`. Los archivos de la aplicación incluyen:
   - `reviews` (una subcarpeta que contiene los documentos de revisión)
   - `.env` (el archivo de configuración de la aplicación)
   - `requirements.txt` (las dependencias del paquete Python que deben instalarse)
   - `text-analysis.py` (el archivo de código de la aplicación)

### Configura tu aplicación

1. En Visual Studio Code, acceda al panel Extensiones y, si aún no está instalada, instale la **extensión de Python**.
2. En la paleta de comandos, use el comando `Python: Select Interpreter`. Luego, seleccione un entorno existente si tiene uno, o cree un nuevo entorno Venv basado en su instalación de Python 3.1x.
   - > **Consejo:** Si se le solicita que instale dependencias, puede instalar las que se encuentran en el archivo `requirements.txt` en la carpeta `/Labfiles/01-analyze-text/Python/text-analysis`; pero no hay problema si no lo hace, ¡las instalaremos más adelante!
   - > **Consejo:** Si prefiere usar la terminal, puede crear su entorno Venv con `python -m venv labenv` y luego activarlo con `\labenv\Scripts\activate`.
3. En el panel Explorador, haga clic con el botón derecho en la carpeta `text-analysis` que contiene los archivos de la aplicación y seleccione **Abrir en terminal integrada** (o abra una terminal en el menú Terminal y navegue hasta la carpeta `/Labfiles/01-analyze-text/Python/text-analysis`).
   - > **Nota:** Al abrir la terminal en Visual Studio Code, se activará automáticamente el entorno de Python. Es posible que deba habilitar la ejecución de scripts en su sistema.
4. Asegúrese de que la terminal esté abierta en la carpeta `text-analysis` con el prefijo `(.venv)` para indicar que el entorno Python que creó está activo.
5. Instale el SDK de Azure Language Text Analytics y otros paquetes necesarios ejecutando el siguiente comando:

```bash
# Instalamos las bibliotecas necesarias listadas en requirements.txt
pip install -r requirements.txt
```

6. En el panel Explorador, en la carpeta `text-analysis`, seleccione el archivo `.env` para abrirlo. A continuación, actualice los valores de configuración para incluir el punto de conexión (hasta el dominio `.com`) y la clave de su proyecto Foundry (cópielos del portal de Foundry).

> **Importante:** Modifique el punto de conexión pegado para eliminar el sufijo `/api/projects/{name}` - el punto de conexión debe ser `https://{your-foundry}.services.ai.azure.com`.

7. Guarda el archivo de configuración modificado.

### Agregue código para conectarse a su recurso de Azure AI Language

1. En el panel Explorador, en la carpeta `text-analysis`, abra el archivo `text-analysis.py`.
2. Revisa el código existente. Deberás agregar código para que funcione con el SDK de Azure Language Text Analytics.

> **Consejo:** Al agregar código al archivo de código, asegúrese de mantener la indentación correcta.

3. En la parte superior del archivo de código, debajo de las referencias de espacio de nombres existentes, busque el comentario `Importar espacios de nombres` y agregue el siguiente código para importar los espacios de nombres que necesitará para usar el SDK de análisis de texto:

```python
# Importamos las clases necesarias para la autenticación y el cliente de Text Analytics
from azure.identity import DefaultAzureCredential
from azure.ai.textanalytics import TextAnalyticsClient
```

4. En la función principal, observe que ya se ha proporcionado el código para cargar el punto de conexión y la clave desde el archivo de configuración. A continuación, busque el comentario `Crear cliente usando el punto final` y añada el siguiente código para crear un cliente para la API de análisis de texto:

```python
# Instanciamos la credencial predeterminada de Azure
credential = DefaultAzureCredential()

# Creamos el cliente de Text Analytics pasando el endpoint y la credencial
ai_client = TextAnalyticsClient(endpoint=foundry_endpoint, credential=credential)
```

5. Guarda los cambios en el archivo de código. Luego, en el panel de la terminal, usa el siguiente comando para iniciar sesión en Azure.

```bash
# Iniciamos sesión interactivamente en Azure CLI
az login
```

   - > **Nota:** En la mayoría de los casos, bastará con usar `az login`. Sin embargo, si tiene suscripciones en varios inquilinos, es posible que deba especificar el inquilino mediante el parámetro `--tenant`.

   Cuando se le solicite, siga las instrucciones para iniciar sesión en Azure. A continuación, complete el proceso de inicio de sesión en la línea de comandos, visualizando (y confirmando si es necesario) los detalles de la suscripción que contiene su recurso Foundry.

6. Después de iniciar sesión, introduzca el siguiente comando para ejecutar la aplicación:

```bash
# Ejecutamos nuestra aplicación en Python
python text-analysis.py
```

Observa el resultado, ya que el código debería ejecutarse sin errores y mostrar el contenido de cada archivo de texto de reseña en la carpeta de reseñas. La aplicación crea correctamente un cliente para la API de análisis de texto, pero no lo utiliza. Solucionaremos esto en la siguiente sección.

### Agregar código para detectar el idioma

Ahora que ya has creado un cliente para la API, vamos a usarlo para detectar el idioma en el que está escrita cada reseña.

1. En el editor de código, busque el comentario `Obtener idioma`. Luego, agregue el código necesario para detectar el idioma en cada documento de revisión:

```python
# Detectamos el idioma del documento enviando una lista con un solo texto ('text')
# Como enviamos un solo elemento, obtenemos el primer resultado con [0]
detectedLanguage = ai_client.detect_language(documents=[text])[0]
print('\nLanguage: {}'.format(detectedLanguage.primary_language.name))
```

> **Nota:** En este ejemplo, cada reseña se analiza individualmente, lo que genera una llamada independiente al servicio para cada archivo. Un enfoque alternativo consiste en crear una colección de documentos y enviarlos al servicio en una sola llamada. En ambos casos, la respuesta del servicio consiste en una colección de documentos; por eso, en el código Python anterior, se especifica el índice del primer (y único) documento de la respuesta (`[0]`).

2. Guarda los cambios. Luego, vuelve a ejecutar el programa.

Observe el resultado, teniendo en cuenta que esta vez se identifica el idioma de cada reseña.

### Agregue código para evaluar el sentimiento

El análisis de sentimientos es una técnica común para clasificar textos como positivos o negativos (o posiblemente neutrales o mixtos). Se utiliza habitualmente para analizar publicaciones en redes sociales, reseñas de productos y otros contenidos donde el sentimiento expresado puede aportar información útil.

1. En el editor de código, busque el comentario `Obtener sentimiento`. Luego, agregue el código necesario para detectar el sentimiento de cada documento de revisión:

```python
# Evaluamos el sentimiento del documento (Positivo, Negativo, Neutro o Mixto)
sentimentAnalysis = ai_client.analyze_sentiment(documents=[text])[0]
print("\nSentiment: {}".format(sentimentAnalysis.sentiment))
```

2. Guarda los cambios. Luego, vuelve a ejecutar el programa.

Observe el resultado y compruebe que se detecta el sentimiento expresado en las reseñas.

### Agregue código para identificar frases clave

Puede resultar útil identificar frases clave en un texto para ayudar a determinar los temas principales que trata.

1. En el editor de código, busque el comentario `Obtener frases clave`. A continuación, añada el código necesario para detectar las frases clave en cada documento de revisión:

```python
# Extraemos las frases más importantes que resumen el documento
phrases = ai_client.extract_key_phrases(documents=[text])[0].key_phrases
if len(phrases) > 0:    # Si encontramos al menos una frase clave
    print("\nKey Phrases:")
    for phrase in phrases:
        print('\t{}'.format(phrase))
```

2. Guarda los cambios y vuelve a ejecutar el programa.

Observe el resultado y tenga en cuenta que cada documento contiene frases clave que ofrecen algunas pistas sobre el tema de la reseña.

### Agregar código para extraer entidades

Con frecuencia, los documentos u otros textos mencionan personas, lugares, períodos de tiempo u otras entidades. La API de análisis de texto puede detectar múltiples categorías (y subcategorías) de entidades en su texto.

1. En el editor de código, busque el comentario `Obtener entidades`. Luego, agregue el código necesario para identificar las entidades que se mencionan en cada reseña:

```python
# Reconocemos entidades con nombre (NER) como Personas, Organizaciones, Lugares, etc.
entities = ai_client.recognize_entities(documents=[text])[0].entities
if len(entities) > 0:   # Si encontramos al menos una entidad
    print("\nEntities")
    for entity in entities:
        print('\t{} ({})'.format(entity.text, entity.category))
```

2. Guarda los cambios y vuelve a ejecutar el programa.

Observe el resultado, tomando nota de las entidades que se han detectado en el texto.

### Agregue código para extraer entidades vinculadas

Además de las entidades categorizadas, la API de análisis de texto puede detectar entidades para las que existen vínculos conocidos con fuentes de datos, como Wikipedia.

1. En el editor de código, busque el comentario `Obtener entidades vinculadas`. Luego, agregue el código necesario para identificar las entidades vinculadas que se mencionan en cada reseña:

```python
# Identificamos entidades vinculadas a una fuente de conocimiento (ej. Wikipedia)
linked_entities = ai_client.recognize_linked_entities(documents=[text])[0].entities
if len(linked_entities) > 0:    # Si encontramos al menos una entidad vinculada
   print("\nLinks")
   for linked_entity in linked_entities:
        print('\t{} ({})'.format(linked_entity.name, linked_entity.url))
```

2. Guarda los cambios y vuelve a ejecutar el programa.

Observe el resultado, tomando nota de las entidades vinculadas que se identifican.

### Limpieza

Si ya ha terminado de explorar Azure Language en Foundry Tools, debería eliminar los recursos que ha creado en este ejercicio para evitar incurrir en costes innecesarios de Azure.

1. Abra el portal de Azure y vea el contenido del grupo de recursos donde implementó los recursos utilizados en este ejercicio.
2. En la barra de herramientas, seleccione **Eliminar grupo de recursos**.
3. Introduzca el nombre del grupo de recursos y confirme que desea eliminarlo.

---

### Código Completo del Laboratorio: Analizar texto

```python
# text-analysis.py

import os
# Importamos las clases necesarias para la autenticación y el cliente de Text Analytics (Azure AI Language)
from azure.identity import DefaultAzureCredential
from azure.ai.textanalytics import TextAnalyticsClient

def main():
    try:
        # Aquí cargaríamos las variables de entorno (Foundry Endpoint)
        # foundry_endpoint = os.getenv('FOUNDRY_ENDPOINT')
        foundry_endpoint = "https://tu-endpoint-de-ejemplo.services.ai.azure.com"
        
        # 1. Creamos el cliente usando la Credencial Predeterminada de Azure
        # Esto usará nuestra sesión de "az login", una identidad administrada, etc.
        credential = DefaultAzureCredential()
        
        # El cliente es nuestra conexión directa con la API del recurso de Azure
        ai_client = TextAnalyticsClient(endpoint=foundry_endpoint, credential=credential)
        
        # Simulamos la lectura de archivos de revisión en una carpeta
        reviews_folder = 'reviews'
        # Ejemplo de texto leído:
        text = "I loved the hotel. The staff was incredibly helpful and the room was very clean. We visited the Eiffel Tower the next day."
        print(f"Review:\n{text}")
        
        # 2. Obtener idioma
        # Enviamos el texto en una lista 'documents' y recibimos un objeto que contiene información del idioma (ej. 'English')
        detectedLanguage = ai_client.detect_language(documents=[text])[0]
        print('\nLanguage: {}'.format(detectedLanguage.primary_language.name))
        
        # 3. Obtener sentimiento
        # El servicio clasifica el sentimiento en Positive, Negative, Neutral o Mixed
        sentimentAnalysis = ai_client.analyze_sentiment(documents=[text])[0]
        print("\nSentiment: {}".format(sentimentAnalysis.sentiment))
        
        # 4. Obtener frases clave
        # Nos devuelve las frases más relevantes del texto ("hotel", "staff", "Eiffel Tower", etc.)
        phrases = ai_client.extract_key_phrases(documents=[text])[0].key_phrases
        if len(phrases) > 0:
            print("\nKey Phrases:")
            for phrase in phrases:
                print('\t{}'.format(phrase))
                
        # 5. Obtener entidades
        # Agrupa elementos por categoría (ej. Eiffel Tower -> Location)
        entities = ai_client.recognize_entities(documents=[text])[0].entities
        if len(entities) > 0:
            print("\nEntities")
            for entity in entities:
                print('\t{} ({})'.format(entity.text, entity.category))
                
        # 6. Obtener entidades vinculadas
        # Reconoce entidades universales y devuelve un link, por ejemplo, hacia Wikipedia (Eiffel Tower)
        linked_entities = ai_client.recognize_linked_entities(documents=[text])[0].entities
        if len(linked_entities) > 0:
           print("\nLinks")
           for linked_entity in linked_entities:
                print('\t{} ({})'.format(linked_entity.name, linked_entity.url))

    except Exception as ex:
        print(ex)

if __name__ == "__main__":
    main()
```

---

## Evaluación del módulo

1.  **¿Cómo debe crear una aplicación que supervise los comentarios en el sitio web de su empresa y marque las publicaciones negativas?**

    a. Use el lenguaje de Azure en Foundry Tools para extraer frases clave.

    b. Use el lenguaje de Azure en Foundry Tools para realizar el análisis de opiniones de los comentarios. ✅

    c. Use el lenguaje de Azure en Foundry Tools para extraer entidades con nombre de los comentarios.

    **Justificación:** Para detectar si un texto o comentario es negativo, positivo o neutro, el servicio adecuado a utilizar es el de "Análisis de sentimiento" (mal traducido en el portal como "análisis de opiniones").

    **Glosario / Comentarios:**
    *   **"Análisis de opiniones"**: Traducción errónea de *Sentiment Analysis* (Análisis de sentimiento).
    *   **"Lenguaje de Azure en Foundry Tools"**: Traducción confusa de *Azure AI Language*.

2.  **Está analizando texto que contiene la palabra "París". ¿Cómo puede determinar si esta palabra hace referencia a la ciudad francesa o al carácter del "The Iliad" de Homer?**

    a. Use el lenguaje de Azure en Foundry Tools para extraer frases clave.

    b. Use el idioma de Azure en Foundry Tools para detectar el idioma del texto.

    c. Use el lenguaje de Azure en Foundry Tools para extraer entidades vinculadas. ✅

    **Justificación:** La "Vinculación de entidades" (Entity Linking) es la característica diseñada específicamente para la desambiguación de términos. Identificará si "París" se refiere a una ubicación geográfica o al personaje de la mitología, devolviendo el enlace a la base de conocimiento correspondiente (ej. Wikipedia).

    **Glosario / Comentarios:**
    *   **"Idioma de Azure"**: Traducción errónea de *Azure AI Language*.
