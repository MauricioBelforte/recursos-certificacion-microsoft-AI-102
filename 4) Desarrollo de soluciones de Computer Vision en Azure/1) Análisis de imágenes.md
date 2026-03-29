# Análisis de imágenes con Azure Vision

## Introducción

**Computer Vision** (Visión por Computadora) es una rama fundamental de la inteligencia artificial donde el software es capaz de interpretar entradas visuales provenientes de imágenes estáticas o fuentes de video.

En Microsoft Azure, puedes utilizar el servicio **Azure AI Vision** (a menudo referido simplemente como _Azure Vision_) para implementar múltiples escenarios y soluciones visuales de extremo a extremo, que incluyen:

- **Análisis de imágenes** (Image Analysis).
- **Reconocimiento óptico de caracteres** (OCR) para extraer texto impreso o manuscrito.
- **Detección y análisis de caras** (Face Detection/Analysis).
- **Análisis de vídeo espacial** (Video Analysis).

En este módulo, el enfoque principal será el **análisis de imágenes**. Exploraremos cómo crear aplicaciones que consumen el servicio de _Azure Vision_ para extraer perspectivas, inferir metadatos y comprender el contenido profundo de las fotografías o imágenes provistas.

_(Diagrama conceptual del servicio Azure Vision realizando tareas de análisis de imágenes)._

Como muestran sus capacidades, utilizar _Azure Vision_ para el análisis te permite lograr lo siguiente de forma automática mediante sus modelos preentrenados:

- **Generar subtítulos o pies de foto (Captions)** descriptivos para una imagen basándose estructuralmente en su contenido.
- **Sugerir etiquetas (Tags)** apropiadas y precisas para catalogar y clasificar la imagen.
- **Detectar y localizar objetos (Objects)** comunes y su posición exacta dentro del encuadre mediante cuadros delimitadores (bounding boxes).
- **Detectar y localizar personas (People)** de manera anónima y general dentro de la imagen.

## Aprovisionamiento de un recurso de Azure Vision

Para utilizar los servicios de análisis de imágenes de **Azure AI Vision**, es necesario aprovisionar un recurso en tu suscripción de Azure. Microsoft ofrece diversas opciones dependiendo de la complejidad y el alcance de tu solución:

_(Diagrama de Microsoft Foundry que contiene servicios de IA, que a su vez contienen Azure Vision)._

### Opciones de Aprovisionamiento

1.  **Proyecto de Microsoft Foundry**: Se recomienda para soluciones integrales que combinan IA generativa, agentes y herramientas precompiladas (_Foundry Tools_). Al crear un proyecto, se incluye por defecto un recurso de **Herramientas de Foundry (Foundry Tools)** que ya contiene Azure Vision. Es ideal para el desarrollo colaborativo.
2.  **Recurso de Foundry Tools (Multiservicio)**: Si no necesitas el entorno completo de un proyecto de Foundry, puedes crear este recurso independiente. Permite acceder a Azure Vision y otros servicios de IA (como Lenguaje o Voz) utilizando un **Punto de conexión (Endpoint)** y una **Clave (Key)** únicos.
3.  **Recurso de Azure Vision independiente**: Es la opción ideal para experimentación o si solo necesitas las funciones específicas de visión. Una ventaja clave es que ofrece un **Nivel gratuito (Free tier)** para explorar el servicio sin costo inicial.

> [!TIP]
> Si no estás familiarizado con la terminología de Foundry, te recomendamos revisar el módulo de "Planeamiento y preparación para desarrollar soluciones de inteligencia artificial en Azure".

## Conexión al recurso

Una vez implementado el recurso, las aplicaciones cliente pueden conectarse a él utilizando la **API REST** o un **SDK** específico (como Python o .NET).

### Punto de conexión (Endpoint)

Cada recurso proporciona una URL única a la que la aplicación debe dirigir sus peticiones. El formato típico es:
`https://<nombre-recurso>.cognitiveservices.azure.com/`

### Autenticación

Para acceder al servicio, la aplicación debe autenticarse mediante una de estas opciones:

- **Autenticación basada en Claves (Key-based authentication)**: Se pasa una clave de autorización en el encabezado de la solicitud HTTP. Es la forma más sencilla para desarrollo y pruebas.
- **Autenticación de Microsoft Entra ID**: Se utiliza un token de identidad. Es la opción más segura y recomendada para entornos de **Producción (Production)**, especialmente mediante el uso de **Identidades administradas (Managed Identities)** o almacenando las claves en **Azure Key Vault**.

> [!NOTE]
> Al trabajar dentro de un proyecto de **Microsoft Foundry**, el SDK de Foundry permite autenticarse con Entra ID para recuperar automáticamente la información de conexión y las claves del recurso de herramientas asociado.

## Laboratorio: Análisis de imágenes con el SDK de Azure AI Vision

En este laboratorio, completarás una aplicación cliente que utiliza el SDK de **Azure AI Vision** para analizar imágenes, extraer pies de foto, etiquetas y detectar tanto objetos como personas.

Este ejercicio dura aproximadamente 30 minutos.

### 1. Aprovisionar un recurso de Azure AI Vision

Si aún no dispones de uno, debes crear un recurso independiente de **Computer Vision** en el portal de Azure.

1. Abre el portal de Azure (`https://portal.azure.com`).
2. Crea un nuevo recurso buscando **Computer Vision** (Visión por Computadora) con la siguiente configuración:
   - **Suscripción**: Tu suscripción a Azure.
   - **Grupo de recursos**: Selecciona uno existente o crea uno nuevo.
   - **Región**: Elige una región compatible (ej. *East US*).
   - **Nombre**: Un nombre único.
   - **Nivel de precios**: Gratis F0 (si está disponible) o Estándar S1.
3. Una vez implementado, ve a la página de **Claves y punto de conexión** (Keys and Endpoint) del recurso. Necesitarás estos datos más adelante.

### 2. Preparar la aplicación

Utilizaremos el **Azure Cloud Shell** para ejecutar el entorno de desarrollo.

1. En el portal de Azure, abre el Cloud Shell (`[>_]`) y asegúrate de seleccionar el entorno **PowerShell**.
2. Clonamos el repositorio con los archivos necesarios:
   ```powershell
   git clone https://github.com/MicrosoftLearning/mslearn-ai-vision
   ```
3. Navega a la carpeta del proyecto:
   ```powershell
   cd mslearn-ai-vision/Labfiles/analyze-images/python/image-analysis
   ```
4. Configura el entorno virtual e instala las dependencias:
   ```powershell
   python -m venv labenv
   ./labenv/bin/Activate.ps1
   pip install azure-ai-vision-imageanalysis==1.0.0
   ```
5. Edita el archivo `.env` para incluir tus credenciales extraídas del portal:
   ```powershell
   code .env
   # AI_SERVICE_ENDPOINT=<Tu Endpoint>
   # AI_SERVICE_KEY=<Tu Key>
   ```

### 3. Implementar el análisis

Abre el archivo `image-analysis.py` (`code image-analysis.py`) y completa las secciones indicadas con los siguientes fragmentos clave:

#### Autenticar el cliente y analizar la imagen
```python
# Importación de namespaces
from azure.ai.vision.imageanalysis import ImageAnalysisClient
from azure.ai.vision.imageanalysis.models import VisualFeatures
from azure.core.credentials import AzureKeyCredential

# Autenticación del cliente
cv_client = ImageAnalysisClient(
     endpoint=ai_endpoint,
     credential=AzureKeyCredential(ai_key))

# Lectura y análisis de la imagen
with open(image_file, "rb") as f:
     image_data = f.read()

result = cv_client.analyze(
     image_data=image_data,
     visual_features=[
         VisualFeatures.CAPTION,
         VisualFeatures.DENSE_CAPTIONS,
         VisualFeatures.TAGS,
         VisualFeatures.OBJECTS,
         VisualFeatures.PEOPLE],
)
```

#### Extraer Pies de foto (Captions)
```python
if result.caption is not None:
     print("\nCaption: '{}' (confidence: {:.2f}%)".format(result.caption.text, result.caption.confidence * 100))
```

#### Extraer Etiquetas (Tags)
```python
if result.tags is not None:
     print("\nTags:")
     for tag in result.tags.list:
         print(" Tag: '{}' (confidence: {:.2f}%)".format(tag.name, tag.confidence * 100))
```

#### Detectar Objetos y Personas
```python
if result.objects is not None:
     for obj in result.objects.list:
         print(" {} (confidence: {:.2f}%)".format(obj.tags[0].name, obj.tags[0].confidence * 100))

if result.people is not None:
     for person in result.people.list:
         if person.confidence > 0.2:
             print(" {} (confidence: {:.2f}%)".format(person.bounding_box, person.confidence * 100))
```

### 4. Ejecución
Para probar la aplicación con una imagen de ejemplo (ej. una calle), ejecuta en la terminal:
```powershell
python image-analysis.py images/street.jpg
```

---

### Código Completo del Laboratorio: image-analysis.py

A continuación se presenta el archivo `image-analysis.py` consolidado y profusamente comentado para su estudio.

```python
import os
from dotenv import load_dotenv
# Importar namespaces de Azure AI Vision
from azure.ai.vision.imageanalysis import ImageAnalysisClient
from azure.ai.vision.imageanalysis.models import VisualFeatures
from azure.core.credentials import AzureKeyCredential
from PIL import Image, ImageDraw

def main():
    try:
        # Cargar configuración desde el archivo .env
        load_dotenv()
        ai_endpoint = os.getenv('AI_SERVICE_ENDPOINT')
        ai_key = os.getenv('AI_SERVICE_KEY')

        # Obtener la imagen a analizar (pasada por argumento)
        image_file = "images/street.jpg"
        if len(os.sys.argv) > 1:
            image_file = os.sys.argv[1]

        # 1. Autenticar el cliente de Azure AI Vision
        # Se requiere el endpoint y la clave (Key) del recurso
        cv_client = ImageAnalysisClient(
            endpoint=ai_endpoint,
            credential=AzureKeyCredential(ai_key))

        # 2. Leer la imagen en formato binario (obligatorio para datos locales)
        with open(image_file, "rb") as f:
            image_data = f.read()

        print(f'\nAnalizando la imagen: {image_file}\n')

        # 3. Llamada al servicio de análisis
        # Definimos en visual_features qué queremos que la IA extraiga
        result = cv_client.analyze(
            image_data=image_data,
            visual_features=[
                VisualFeatures.CAPTION,
                VisualFeatures.DENSE_CAPTIONS,
                VisualFeatures.TAGS,
                VisualFeatures.OBJECTS,
                VisualFeatures.PEOPLE],
        )

        # 4. Procesar y mostrar el Pie de Foto (Caption)
        if result.caption is not None:
            print("\nPie de foto:")
            print(" '{}' (Confianza: {:.2f}%)".format(result.caption.text, result.caption.confidence * 100))
        
        # 5. Procesar Pies de foto detallados (Dense Captions) de áreas específicas
        if result.dense_captions is not None:
            print("\nDescripciones detalladas por área:")
            for caption in result.dense_captions.list:
                print(" '{}' (Confianza: {:.2f}%)".format(caption.text, caption.confidence * 100))

        # 6. Procesar Etiquetas (Tags) descriptivas
        if result.tags is not None:
            print("\nEtiquetas detectadas:")
            for tag in result.tags.list:
                print(" Tag: '{}' (Confianza: {:.2f}%)".format(tag.name, tag.confidence * 100))

        # 7. Procesar Objetos y dibujar cuadros delimitadores
        if result.objects is not None:
            print("\nObjetos en la imagen:")
            for obj in result.objects.list:
                # El objeto trae una lista de etiquetas internas
                print(" {} (Confianza: {:.2f}%)".format(obj.tags[0].name, obj.tags[0].confidence * 100))
            
            # Función auxiliar para generar imagen anotada (anotate_objects.jpg)
            show_objects(image_file, result.objects.list)

        # 8. Procesar Personas detectadas
        if result.people is not None:
            print("\nPersonas detectadas:")
            for person in result.people.list:
                # Filtramos por confianza mínima (20%) para evitar falsos positivos
                if person.confidence > 0.2:
                    print(" Ubicación: {} (Confianza: {:.2f}%)".format(person.bounding_box, person.confidence * 100))
            
            # Función auxiliar para anotar personas (anotate_people.jpg)
            show_people(image_file, result.people.list)

    except Exception as ex:
        print(f"Error: {ex}")

def show_objects(image_path, objects):
    """Dibuja rectángulos sobre los objetos detectados y guarda la imagen."""
    img = Image.open(image_path)
    draw = ImageDraw.Draw(img)
    for obj in objects:
        box = obj.bounding_box
        # Azure Vision devuelve x, y, w, h
        draw.rectangle([box.x, box.y, box.x + box.w, box.y + box.h], outline="red", width=3)
    img.save("objects_result.jpg")
    print("Resultado de objetos guardado en objects_result.jpg")

def show_people(image_path, people):
    """Dibuja rectángulos sobre las personas detectadas y guarda la imagen."""
    img = Image.open(image_path)
    draw = ImageDraw.Draw(img)
    for person in people:
        if person.confidence > 0.2:
            box = person.bounding_box
            draw.rectangle([box.x, box.y, box.x + box.w, box.y + box.h], outline="blue", width=3)
    img.save("people_result.jpg")
    print("Resultado de personas guardado en people_result.jpg")

if __name__ == "__main__":
    main()
```

## Análisis de una imagen

Una vez establecida la conexión con el recurso de Azure Vision, la aplicación cliente puede realizar tareas de análisis extrayendo características visuales específicas a partir de sus modelos preentrenados.

### Requisitos de la imagen
Para que el servicio procese la imagen correctamente, esta debe cumplir con los siguientes criterios:
- **Formatos admitidos**: JPEG, PNG, GIF o BMP.
- **Tamaño máximo**: El archivo debe pesar menos de **4 megabytes (MB)**.
- **Dimensiones**: Deben ser superiores a **50 x 50 píxeles**.

### Envío de una imagen para su análisis
Puedes enviar una imagen mediante la **API REST (Analyze Image)** o usando el **SDK** de tu lenguaje preferido. En la solicitud, es obligatorio especificar qué **Características Visuales (Visual Features)** deseas incluir en el análisis.

#### Ejemplo de código (Python)
```python
from azure.ai.vision.imageanalysis import ImageAnalysisClient
from azure.ai.vision.imageanalysis.models import VisualFeatures
from azure.core.credentials import AzureKeyCredential

# Configuración del cliente
client = ImageAnalysisClient(
    endpoint="<TU_ENDPOINT>",
    credential=AzureKeyCredential("<TU_KEY>")
)

# Ejecución del análisis especificando características (CAPTION y TAGS)
result = client.analyze(
    image_data=<DATOS_BINARIOS_IMAGEN>, # Datos en crudo del archivo local
    visual_features=[VisualFeatures.CAPTION, VisualFeatures.TAGS],
    gender_neutral_caption=True
)
```

> [!IMPORTANT]
> - El ejemplo utiliza **Autenticación basada en Claves (Key)**. Para producción, se recomienda usar `TokenCredential` para Entra ID.
> - El ejemplo envía datos binarios (archivo local). Para analizar una imagen mediante una **URL pública**, se debe utilizar el método `analyze_from_url`.

### Características Visuales disponibles (VisualFeatures)
La enumeración `VisualFeatures` permite seleccionar qué información deseamos recuperar:

- **`VisualFeatures.TAGS`**: Identifica etiquetas descriptivas sobre el contenido (paisajes, objetos, acciones).
- **`VisualFeatures.OBJECTS`**: Detecta objetos comunes y devuelve sus coordenadas mediante un **Rectángulo delimitador (Bounding Box)**.
- **`VisualFeatures.CAPTION`**: Genera una descripción de la imagen en lenguaje natural (Pie de foto).
- **`VisualFeatures.DENSE_CAPTIONS`**: Genera subtítulos más detallados para los distintos objetos individuales detectados.
- **`VisualFeatures.PEOPLE`**: Localiza personas en la imagen y devuelve sus rectángulos delimitadores.
- **`VisualFeatures.SMART_CROPS`**: Sugiere áreas de interés para recortes inteligentes basados en una relación de aspecto.
- **`VisualFeatures.READ`**: Extrae texto legible mediante OCR.

### Procesamiento de la respuesta (JSON)
El servicio devuelve un documento JSON con los resultados. La respuesta suele incluir **Puntuaciones de confianza (Confidence Score)** (un valor entre 0 y 1 que indica qué tan segura está la IA del resultado) y coordenadas de ubicación cuando se detectan objetos o personas.

#### Ejemplo de respuesta JSON
```json
{
  "modelVersion": "2023-10-01",
  "denseCaptionsResult": {
    "values": [
      {
         "text": "una casa en el bosque",
         "confidence": 0.7055,
         "boundingBox": { "x": 0, "y": 0, "w": 640, "h": 640 }
      },
      {
         "text": "un remolque con ventanas",
         "confidence": 0.6675,
         "boundingBox": { "x": 214, "y": 434, "w": 154, "h": 108 }
      }
    ]
  },
  "metadata": { "width": 640, "height": 640 }
}

---

## Evaluación del Módulo

1.  **¿Cuál es el propósito del análisis de imágenes de Azure Vision?**

    a. Con el fin de ofrecer informes y realizar auditorías de plantillas de máquina virtual en las suscripciones de Azure.

    b. Para extraer información sobre las características visuales en imágenes. ✅

    c. Para admitir la comunicación de videoconferencia y cámara web.

    **Justificación:** El objetivo fundamental de Azure AI Vision Image Analysis es procesar imágenes para identificar y extraer datos sobre sus características visuales, como objetos, etiquetas, pies de foto o texto.

    **Glosario / Comentarios:** (Ninguno)

2.  **Quiere usar Azure Vision Image Analysis para generar descripciones de texto sugeridas para una imagen. ¿Qué característica visual debe especificar?**

    a. Etiquetas.

    b. DenseCaptions. ✅

    c. Lectura.

    **Justificación:** Aunque `Caption` genera una descripción general, la característica `DenseCaptions` genera subtítulos o descripciones detalladas para los distintos elementos y áreas detectadas en la imagen, siendo la opción más completa para generar descripciones de texto sugeridas basadas en el contenido visual profundo.

    **Glosario / Comentarios:**
    *   **"DenseCaptions"**: Generación de subtítulos detallados por áreas de la imagen.
    *   **"Lectura"**: Traducción de *Read* (OCR).

## Resumen

En este módulo hemos explorado el servicio de **Azure AI Vision** y su capacidad para comprender el contenido visual:
- Aprendimos a aprovisionar recursos independientes y de **Foundry Tools**.
- Implementamos una solución de análisis utilizando el SDK de Python.
- Vimos cómo extraer características clave como **Captions**, **Tags**, **Objects** y **Read**.
- Entendimos la importancia de las puntuaciones de confianza y los cuadros delimitadores para localizar información en las imágenes.

### Caso de Uso Común: DAM
Un escenario habitual es la **Administración de activos digitales (Digital Asset Management - DAM)**. En este caso, el análisis de imágenes permite automatizar el etiquetado, catalogación e indexación de grandes volúmenes de fotos, facilitando su búsqueda y organización sin intervención humana manual.
