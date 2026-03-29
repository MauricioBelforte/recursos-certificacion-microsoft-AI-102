# Selección, implementación y evaluación de modelos en el catálogo en el portal de Microsoft Foundry

## Laboratorio: Seleccione e implemente un modelo de lenguaje

*Nota: Este ejercicio está obsoleto pero se mantiene para fines de estudio de la interfaz del catálogo de modelos.*

El catálogo de modelos de Microsoft Foundry sirve como repositorio central donde puede explorar y utilizar una variedad de modelos, lo que facilita la creación de su escenario de IA generativa.

En este ejercicio, explorarás el catálogo de modelos en el portal Foundry y compararás posibles modelos para una aplicación de IA generativa que ayude a resolver problemas.

Este ejercicio durará aproximadamente 25 minutos.

**Nota:** Algunas de las tecnologías utilizadas en este ejercicio están en fase de vista previa o en desarrollo activo. Es posible que experimente comportamientos inesperados, advertencias o errores.

### Explorar modelos
Comencemos por iniciar sesión en el portal de Foundry y explorar algunos de los modelos disponibles.

1. En un navegador web, abra el portal de Foundry en [https://ai.azure.com] e inicie sesión con sus credenciales de Azure. Cierre cualquier panel de consejos o de inicio rápido que se abra la primera vez que inicie sesión y, si es necesario, use el logotipo de Foundry en la esquina superior izquierda para navegar a la página de inicio.
2. Revise la información en la página principal.
3. En la página principal, en la sección **Explorar modelos y capacidades**, busque el modelo `gpt-4o` que utilizaremos en nuestro proyecto.
4. En los resultados de la búsqueda, seleccione el modelo `gpt-4o` para ver sus detalles.
5. Lea la descripción y revise la demás información disponible en la pestaña **Detalles**.
6. En la página de `gpt-4o`, consulte la pestaña "**Benchmarks**" (Puntos de referencia) para ver cómo se compara el modelo con otros modelos utilizados en escenarios similares, según algunos parámetros de rendimiento estándar.
7. Utilice la flecha hacia atrás que aparece junto al título de la página `gpt-4o` para volver al catálogo de modelos.
8. Busque `Phi-4-reasoning` y consulte los detalles y los puntos de referencia del modelo de razonamiento Phi-4.

### Comparar modelos
Has revisado dos modelos diferentes, ambos utilizables para implementar una aplicación de chat con IA generativa. Ahora comparemos visualmente las métricas de estos dos modelos.

1. Utilice la flecha hacia atrás para volver al catálogo de modelos.
2. Seleccione **Comparar modelos**. Se mostrará un gráfico visual para comparar modelos con una selección de modelos comunes.
3. En el panel de modelos a comparar, tenga en cuenta que puede seleccionar tareas populares, como contestar preguntas, para seleccionar automáticamente los modelos de uso común para tareas específicas.
4. Utilice el icono de la papelera para eliminar todos los modelos preseleccionados.
5. Utilice el botón **+ Modelo para comparar** para agregar el modelo `gpt-4o` a la lista. Luego, utilice el mismo botón para agregar el modelo `Phi-4-reasoning` a la lista.
6. Revise el gráfico, que compara los modelos según el **Índice de Calidad (Quality Index)** y el **Costo**. Puede ver los valores específicos de un modelo manteniendo el puntero del mouse sobre el punto que lo representa en el gráfico.
7. En el menú desplegable del eje X, en Calidad, seleccione las siguientes métricas y observe cada gráfico resultante antes de pasar al siguiente:
   - Precisión (Accuracy)
   - Índice de Calidad (Quality index)
   
*Según los puntos de referencia, el modelo `Phi-4-reasoning` parece ofrecer el mejor rendimiento general a un costo menor.*

8. En la lista de modelos para comparar, seleccione el modelo `gpt-4o` para volver a abrir su página de puntos de referencia (**Benchmarks**).
9. En la página del modelo `gpt-4o`, seleccione la pestaña **Descripción general (Overview)** para ver los detalles del modelo.

### Crear un proyecto de Foundry
Para utilizar un modelo, debe crear un proyecto de Foundry.

1. En la parte superior de la página de descripción general del modelo `gpt-4o`, seleccione **Usar este modelo**.
2. Cuando se le solicite crear un proyecto, ingrese un nombre válido para su proyecto y expanda las Opciones avanzadas.
3. Especifique la siguiente configuración para su proyecto:
   - **Recurso de Foundry:** Un nombre válido para su recurso de Foundry
   - **Suscripción:** Su suscripción de Azure
   - **Grupo de recursos:** Cree o seleccione un grupo de recursos
   - **Región:** Seleccione cualquiera recomendada por AI Foundry
4. Seleccione **Crear** y espere a que se cree su proyecto. Si se le solicita, implemente el modelo `gpt-4o` utilizando el tipo de **Implementación estándar global (Global standard deployment)** y personalice los detalles de implementación para establecer un límite de **Tokens por minuto (TPM)** de 50K (o el máximo disponible si es inferior a 50K).

*Nota: Reducir el TPM ayuda a evitar el uso excesivo de la cuota disponible. 50,000 TPM deberían ser suficientes para los datos utilizados en este ejercicio.*

5. Cuando se cree su proyecto, el **área de juegos (Playground)** del chat se abrirá automáticamente para que pueda probar su modelo.

### Chatea con el modelo gpt-4o
Ahora que tiene una implementación de modelo, puede usar el **área de juegos (Playground)** para probarla.

1. En el área de juegos del chat, en el panel de **Configuración (Setup)**, asegúrese de que esté seleccionado su modelo `gpt-4o` y, en el campo de instrucciones y contexto, configure el mensaje del sistema de la siguiente manera:
   - *System prompt:* `You are an AI assistant that helps solve problems.`
2. Seleccione **Aplicar cambios** para actualizar el **mensaje del sistema (system prompt)**.
3. En la ventana de chat, ingrese la siguiente consulta:

```text
# Tengo un zorro, una gallina y un saco de grano que debo llevar por un río en una barca. Solo puedo llevar una cosa a la vez. Si dejo la gallina y el grano solos, la gallina se comerá el grano. Si dejo el zorro y la gallina solos, el zorro se comerá a la gallina. ¿Cómo puedo hacer cruzar los tres sin que se coman a ninguno?
I have a fox, a chicken, and a bag of grain that I need to take over a river in a boat. I can only take one thing at a time. If I leave the chicken and the grain unattended, the chicken will eat the grain. If I leave the fox and the chicken unattended, the fox will eat the chicken. How can I get all three things across the river without anything being eaten?
```

4. Vea la respuesta. Luego, ingrese la siguiente consulta de seguimiento:

```text
# Explica tu razonamiento.
Explain your reasoning.
```

### Implementar otro modelo
Usemos el modelo de razonamiento Phi-4 que también evaluamos.

1. En la barra de navegación de la izquierda, en la sección **Mis activos (My assets)**, seleccione **Modelos + Puntos de conexión (Endpoints)**.
2. En la pestaña **Implementaciones de modelos (Model deployments)**, en la lista desplegable **+ Implementar modelo (Deploy model)**, seleccione **Implementar modelo base (Deploy base model)**. Luego busque `Phi-4-reasoning` y confirme su selección.
3. Acepte la licencia del modelo.
4. Implemente un modelo `Phi-4-reasoning` con la siguiente configuración:
   - **Nombre de implementación:** Un nombre válido
   - **Tipo de implementación:** Global Standard
   - **Detalles de implementación:** Usar la configuración predeterminada
5. Espere a que se complete la implementación (Deployment).

### Chatea con el modelo Phi-4
1. En la barra de navegación, seleccione **Playgrounds**. Luego seleccione Abrir en el área de juegos (Open in playground).
2. En el panel de **Configuración (Setup)**, asegúrese de que su modelo `Phi-4-reasoning` esté seleccionado. Como este modelo no admite un mensaje del sistema propiamente dicho, proporcione la instrucción en la primera línea del cuadro de chat: `System message: You are an AI assistant that helps solve problems.`
3. En una nueva línea (debajo del mensaje del sistema), ingrese la misma consulta del acertijo del zorro y la gallina:

```text
I have a fox, a chicken, and a bag of grain that I need to take over a river in a boat. I can only take one thing at a time. If I leave the chicken and the grain unattended, the chicken will eat the grain. If I leave the fox and the chicken unattended, the fox will eat the chicken. How can I get all three things across the river without anything being eaten?
```

4. Vea la respuesta y envíe el seguimiento:

```text
Explain your reasoning.
```

### Realice una comparación adicional
Use la lista desplegable en el panel de **Configuración (Setup)** para alternar entre sus modelos, probando ambos con el siguiente acertijo (¡la respuesta correcta es 40!):

```text
# Tengo 53 calcetines en mi cajón: 21 calcetines azules idénticos, 15 calcetines negros idénticos y 17 calcetines rojos idénticos. Las luces están apagadas y está completamente a oscuras. ¿Cuántos calcetines debo sacar para estar totalmente seguro de tener al menos un par de calcetines negros?
I have 53 socks in my drawer: 21 identical blue, 15 identical black and 17 identical red. The lights are out, and it is completely dark. How many socks must I take out to make 100 percent certain I have at least one pair of black socks?
```

### Reflexiona sobre los modelos
Has comparado dos modelos que pueden diferir tanto en su capacidad para generar respuestas adecuadas como en su costo. En cualquier escenario generativo, necesitas encontrar un modelo que ofrezca el equilibrio adecuado entre su idoneidad para la tarea que debes realizar y el costo de su uso. Los detalles y las comparaciones visuales del catálogo son un excelente punto de partida para identificar candidatos, y el **área de juegos (Playground)** te permite evaluarlos con distintos prompts.

### Limpieza
Si ya ha terminado de explorar el portal, debe eliminar los recursos que ha creado para evitar incurrir en costos innecesarios en Azure.
1. Abra el portal de Azure y vea el grupo de recursos utilizado.
2. Seleccione **Eliminar grupo de recursos**.
3. Confirme la eliminación.


### Código Completo del Laboratorio: Seleccione e implemente un modelo de lenguaje

```text
# En este laboratorio no hay código de programación (Python/C#), solo instrucciones de sistema (prompts) y consultas de usuario para probar las diferencias de razonamiento entre modelos.

# --- PRUEBA 1: GPT-4o ---
# Prompt del Sistema:
You are an AI assistant that helps solve problems.

# Consulta de Usuario (Acertijo del Río):
I have a fox, a chicken, and a bag of grain that I need to take over a river in a boat. I can only take one thing at a time. If I leave the chicken and the grain unattended, the chicken will eat the grain. If I leave the fox and the chicken unattended, the fox will eat the chicken. How can I get all three things across the river without anything being eaten?

# Consulta de Seguimiento:
Explain your reasoning.


# --- PRUEBA 2: Phi-4-reasoning ---
# Al no tener Prompt de Sistema directo, se envía como primera línea del mensaje de usuario junto con la consulta:

System message: You are an AI assistant that helps solve problems.
I have a fox, a chicken, and a bag of grain that I need to take over a river in a boat. I can only take one thing at a time. If I leave the chicken and the grain unattended, the chicken will eat the grain. If I leave the fox and the chicken unattended, the fox will eat the chicken. How can I get all three things across the river without anything being eaten?

# Consulta de Seguimiento:
Explain your reasoning.


# --- PRUEBA 3: Comparación con otro Acertijo Matemático ---
I have 53 socks in my drawer: 21 identical blue, 15 identical black and 17 identical red. The lights are out, and it is completely dark. How many socks must I take out to make 100 percent certain I have at least one pair of black socks?
```

## Resumen

En este módulo, ha aprendido a:

- Seleccionar un modelo de lenguaje en el catálogo de modelos.
- Desplegar un modelo en un punto final.
- Probar un modelo y mejorar el rendimiento del modelo.

---

## Evaluación del módulo

1.  **¿Dónde puede probar un modelo implementado en el portal de Microsoft Foundry?**

    a. Área de juegos de chat ✅

    b. Espacio aislado

    c. Cuadro de herramientas de desarrollo

**Justificación:** El "Área de juegos de chat" (Chat playground) en el portal de Microsoft Foundry es la interfaz diseñada específicamente para probar e interactuar con los modelos de lenguaje implementados.

**Glosario / Comentarios:**
*   **"Área de juegos de chat"**: Traducción de *Chat playground*.
*   **"Implementado"**: Traducción de *Deployed*.
*   **"Espacio aislado"**: Traducción de *Sandbox*.

2.  **Quiere especificar el tono, el formato y el contenido para cada interacción con el modelo en el área de juegos. ¿Qué debe usar para personalizar la respuesta del modelo?**

    a. Puntos de referencia

    b. Base

    c. Mensaje del sistema ✅

**Justificación:** El "Mensaje del sistema" (system message) se utiliza para proporcionar instrucciones, contexto y directrices al modelo antes de la interacción. Esto permite definir el tono, el formato de la respuesta y el comportamiento general del modelo.

**Glosario / Comentarios:**
*   **"Mensaje del sistema"**: Traducción de *System message*.
*   **"Puntos de referencia"**: Traducción de *Benchmarks*.
*   **"Base"**: Traducción de *Grounding* (o *Baseline* en otros contextos).

3.  **¿Qué opción de implementación debe elegir para hospedar un modelo de OpenAI en un recurso de Microsoft Foundry?**

    a. Implementación estándar ✅

    b. Proceso sin servidor

    c. Cómputo gestionado

**Justificación:** Para alojar un modelo de OpenAI dentro de un recurso de Microsoft Foundry, la opción correcta es la "Implementación estándar" (Standard deployment). Esta opción gestiona la infraestructura necesaria para servir el modelo a través de un punto de conexión.

**Glosario / Comentarios:**
*   **"Implementación estándar"**: Traducción de *Standard deployment*.
*   **"Proceso sin servidor"**: Traducción de *Serverless compute*.