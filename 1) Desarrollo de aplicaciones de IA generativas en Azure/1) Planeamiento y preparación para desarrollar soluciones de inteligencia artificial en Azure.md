#  Planeamiento y preparación para desarrollar soluciones de inteligencia artificial en Azure

# Comparativa de Recursos - Certificación AI-102

| Recurso | Descripción | Servicios Incluidos (Punto de conexión único) |
| :--- | :--- | :--- |
| **Foundry Tools  - Herramientas de fundicion** | Servicios de IA especializados accesibles desde un único endpoint. Ideal para análisis de datos específicos. | • Voz de Azure<br>• Idioma de Azure<br>• Traductor de Azure<br>• Azure Vision<br>• Reconocimiento facial de Azure AI<br>• Azure AI Custom Vision<br>• Inteligencia de documentos de Azure |
| **Microsoft Foundry** | Orientado a la gestión de **proyectos** de IA. Permite integrar modelos avanzados y flujos de trabajo generativos. | • **Azure OpenAI**<br>• Seguridad del contenido de Microsoft Foundry<br>• Descripción del contenido de Azure<br>• Voz de Azure<br>• Idioma de Azure<br>• Traductor de Azure<br>• Azure Vision<br>• Reconocimiento facial de Azure AI<br>• Inteligencia de documentos de Azure.




# Microsoft Foundry

Microsoft Foundry es una plataforma para el desarrollo de inteligencia artificial en Microsoft Azure. Aunque puede aprovisionar recursos individuales de Foundry Tools y desarrollar aplicaciones que los consuman sin necesidad de Microsoft Foundry, la organización del proyecto, la administración de recursos y las capacidades de desarrollo de inteligencia artificial de Microsoft Foundry hacen que sea la manera recomendada para crear todas las soluciones excepto las más sencillas.
Microsoft Foundry proporciona el portal de Microsoft Foundry, una interfaz visual basada en web para trabajar con proyectos de inteligencia artificial. También proporciona el SDK de Microsoft Foundry, que puede usar para crear soluciones de INTELIGENCIA ARTIFICIAL mediante programación.

## Laboratorio: Desarrolla soluciones de IA generativa en Azure

En este ejercicio, utilizarás el portal Microsoft Foundry para crear un proyecto, listo para desarrollar una solución de IA.

Este ejercicio dura aproximadamente 30 minutos.

**Nota:** Algunas de las tecnologías utilizadas en este ejercicio están en fase de vista previa o en desarrollo activo. Es posible que experimente comportamientos inesperados, advertencias o errores.

### Requisitos previos
Antes de comenzar este ejercicio, asegúrese de tener:

- Una suscripción activa a Azure
- Visual Studio Code instalado
- Python versión 3.13.xx instalado*
- Git instalado y configurado
- Azure CLI instalado

*Python 3.14 está disponible, pero algunas dependencias aún no se han compilado para esa versión. El laboratorio se ha probado con éxito con Python 3.13.12.

### Crear un proyecto de Microsoft Foundry
Microsoft Foundry utiliza proyectos para organizar modelos, recursos, datos y otros activos utilizados para desarrollar una solución de IA.

1. En un navegador web, abre el portal de Microsoft Foundry en [https://ai.azure.com] e inicia sesión con tus credenciales de Azure. Cierra cualquier consejo o panel de inicio rápido que se abra la primera vez que inicies sesión y, si es necesario, usa el logotipo de Foundry en la esquina superior izquierda para ir a la página principal.
2. Si aún no está habilitada, en la barra de herramientas en la parte superior de la página, habilite la opción "New Foundry". Luego, cree un nuevo proyecto con un nombre único; expanda el área de Opciones avanzadas para especificar la siguiente configuración para su proyecto:
   - **Recurso de Foundry**: utilice el nombre predeterminado para su recurso (normalmente {nombre_del_proyecto}-recurso).
   - **Suscripción**: Su suscripción a Azure
   - **Grupo de recursos**: Crear o seleccionar un grupo de recursos
   - **Región**: Seleccione cualquiera de las regiones recomendadas por AI Foundry.
   - **Consejo**: Anota la región que seleccionaste. ¡La necesitarás más adelante!
3. Selecciona **Crear**. Espera a que se cree tu proyecto.
4. Cuando esté listo, se abrirá la página principal del proyecto.

### Implementar y probar un modelo
En el núcleo de cualquier proyecto de IA generativa, hay al menos un modelo de IA generativa.

1. En el menú "**Comenzar a crear**", seleccione "**Buscar modelos**" (o en la página "**Descubrir**", seleccione la pestaña "**Modelos**") para ver el catálogo de modelos de Microsoft Foundry.
2. Busca el modelo `gpt-4.1` y, a continuación, selecciónalo en los resultados de la búsqueda para ver su ficha técnica. Las fichas de modelos proporcionan información sobre los mismos para ayudarle a comprender sus capacidades y limitaciones, y a determinar si se ajustan a sus necesidades.
3. Seleccione **Implementar (Deploy)** con la configuración predeterminada para crear una implementación del modelo. Las implementaciones (Deployments) de modelos le permiten trabajar con un modelo en su proyecto.
4. Una vez desplegado el modelo, el **área de juegos (Playground)** se abrirá automáticamente para que pueda probarlo.
5. En el cuadro de **Instrucciones**, introduzca las siguientes instrucciones:

```text
# Eres un asistente de IA que puede proporcionar información y consejos sobre el desarrollo de software de IA.
You are an AI assistant that can provide information and advice about AI software development.
```

6. En la ventana de chat, introduce una consulta como esta y observa la respuesta:
```text
# Describe tres consideraciones clave al trabajar con Modelos de Lenguaje Grande para el desarrollo de aplicaciones de IA.
Describe three key considerations for working with Large Language Models for AI application development.
```

¡Esperamos que el modelo les haya brindado algunas consideraciones clave para que las tengan en cuenta!

### Ver los puntos de conexión (Endpoints) de los recursos y proyectos de Foundry Azure.
1. En el portal de Foundry, en la barra de menú superior, seleccione **Operar**. El centro de operaciones es donde puede supervisar sus proyectos, ver alertas, controlar el rendimiento y las cuotas de los agentes y gestionar los recursos.
2. En el panel de navegación izquierdo, seleccione la página de **administración** para ver los detalles.
   - El nivel de **recurso** se refiere al recurso de Foundry creado en Azure para dar soporte a su proyecto. Este recurso incluye conexiones a los servicios y modelos de Foundry, y proporciona un lugar centralizado para gestionar el acceso de los usuarios a los proyectos de desarrollo de IA.
   - El nivel de **proyecto** se refiere a tu proyecto individual, donde puedes agregar y administrar recursos específicos del proyecto. Un recurso puede dar soporte a varios proyectos (el primero que se crea es el proyecto predeterminado del recurso).
3. Seleccione el enlace al recurso principal asociado con el proyecto. Se deben mostrar los detalles de configuración del recurso.
4. Tenga en cuenta que el recurso Foundry tiene un **punto de conexión (Endpoint)** a través del cual las aplicaciones cliente pueden acceder a la funcionalidad a nivel de recurso (como las herramientas de Foundry que se comparten entre todos los proyectos del recurso).
5. En la barra de menú superior, seleccione **Inicio** para volver a la página principal del proyecto.
6. Tenga en cuenta la **clave (Key)**, el **punto de conexión (Endpoint)** del proyecto y el **punto de conexión (Endpoint)** de Azure OpenAI. Esta información se utiliza para conectarte con los recursos de tu proyecto desde las aplicaciones cliente.
   - La **clave (Key)** se utiliza para la autenticación basada en claves en modelos y herramientas (aunque en la mayoría de los escenarios de producción debería considerar el uso de la autenticación Microsoft Entra ID basada en identidades de usuario y aplicación autenticadas).
   - El **punto de conexión (Endpoint)** del proyecto se utiliza para acceder a los modelos proporcionados directamente en Foundry (incluidos los modelos de OpenAI) mediante la API de recursos de OpenAI, y para acceder a las API específicas de Foundry (como el servicio Foundry Agent).
   - El **punto de conexión (Endpoint)** de OpenAI se utiliza para acceder a los modelos mediante las API de OpenAI, incluidas la API de finalización de chat y la API de recursos.

### Instala la extensión AI Toolkit para Visual Studio Code.
Como desarrollador, es posible que dediques tiempo a trabajar en el portal de Foundry; pero también es probable que pases mucho tiempo en Visual Studio Code. La extensión para Microsoft Foundry ofrece una forma práctica de trabajar con los recursos del proyecto Foundry sin salir del entorno de desarrollo.

1. Abre Visual Studio Code y, en la barra de navegación de la izquierda, accede a la página de **Extensiones**.
2. Busca en el mercado de extensiones `AI Toolkit` e instala la extensión AI Toolkit.
3. Tras instalar la extensión, seleccione su página en la barra de navegación izquierda.
4. En el panel de AI Toolkit, utilice el botón **Establecer proyecto predeterminado** para conectarse a Azure (iniciando sesión con sus credenciales) y seleccione el proyecto de Foundry que creó anteriormente.
5. Tras configurar el proyecto predeterminado, en el panel de AI Toolkit, expanda el proyecto, expanda **Modelos** y seleccione el modelo `gpt-4.1` que implementó (deployed) previamente. Aquí puede consultar los detalles de la implementación (deployment) del modelo.
6. En el panel de Herramientas de IA, en la sección Herramientas para desarrolladores, seleccione **Model playground** y seleccione el modelo `gpt-4.1`.
7. En Visual Studio Code se abre un entorno interactivo donde puedes probar el modelo.

### Limpieza
Si ya ha terminado de explorar el portal de Foundry, debería eliminar los recursos que ha creado en este ejercicio para evitar incurrir en costes innecesarios de Azure.
1. En el portal de Azure [https://portal.azure.com], consulte el contenido del grupo de recursos donde implementó los recursos utilizados en este ejercicio.
2. En la barra de herramientas, seleccione **Eliminar grupo de recursos**.
3. Introduzca el nombre del grupo de recursos y confirme que desea eliminarlo.

### Código Completo del Laboratorio: Desarrolla soluciones de IA generativa en Azure

```text
# En este laboratorio no hay código de programación (Python/C#), solo instrucciones de sistema (prompts) para el área de juegos (Playground).

# Prompt del Sistema:
# Eres un asistente de IA que puede proporcionar información y consejos sobre el desarrollo de software de IA.
You are an AI assistant that can provide information and advice about AI software development.

# Mensaje de Usuario:
# Describe tres consideraciones clave al trabajar con Modelos de Lenguaje Grande para el desarrollo de aplicaciones de IA.
Describe three key considerations for working with Large Language Models for AI application development.
```

---

# Evaluación del módulo


1.  **¿Qué recurso de Azure proporciona servicios de lenguaje y visión desde un único punto de conexión?**

    a. Idioma de Azure

    b. Azure Vision

    c. Herramientas de fundición ✅

**Justificación:** Los recursos de "Herramientas de fundición" (Foundry Tools) están diseñados para agrupar varios servicios de IA de Azure, como Lenguaje y Visión, bajo un único punto de conexión y una sola clave de API, simplificando su gestión y uso en las aplicaciones.

**Glosario / Comentarios:**
*   **"Idioma de Azure"**: Traducción de *Azure AI Language*.
*   **"Herramientas de fundición"**: Traducción de *Foundry Tools* (En el ecosistema Azure real, esto equivale a un recurso multi-servicio de **Azure AI Services**).

2.  **Tiene previsto crear una aplicación de chat sencilla que use un modelo de IA generativa. ¿Qué tipo de proyecto debe crear?**

    a. Proyecto de Microsoft Foundry. ✅

    b. Proyecto basado en azure AI Hub.

    c. Proyecto de Custom Vision de Azure AI.

**Justificación:** Microsoft Foundry es la plataforma recomendada para el desarrollo de proyectos de IA, ya que integra modelos avanzados como los de Azure OpenAI (necesarios para IA generativa) y proporciona herramientas para la gestión del ciclo de vida del proyecto.

**Glosario / Comentarios:**
*   **"Proyecto basado en azure AI Hub"**: Traducción de *Azure AI Hub-based project*.

3.  **¿Qué SDK le permite conectarse a los recursos de un proyecto?**

    a. Foundry Tools SDK

    b. SDK de núcleo semántico

    c. Microsoft Foundry SDK ✅

**Justificación:** El SDK de Microsoft Foundry está específicamente diseñado para interactuar mediante programación con los recursos que se encuentran dentro de un proyecto de Microsoft Foundry, permitiendo la creación y gestión de soluciones de IA.

**Glosario / Comentarios:**
*   **"SDK de núcleo semántico"**: Traducción literal de *Semantic Kernel SDK*.
