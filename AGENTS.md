# Instrucciones de Estilo y Comportamiento del Agente

Este archivo define cómo el asistente debe procesar, formatear y mejorar el contenido de este repositorio para la preparación de la certificación **Microsoft AI-102**.

---

## Estructura General de un Módulo

Cada archivo de módulo debe seguir el siguiente orden de contenido para mantener la consistencia:

1.  **Contenido Teórico Principal:** Todo el texto mejorado del módulo, separado por títulos.
2.  **Laboratorio:** La guía práctica del módulo, adaptada para la lectura.
3.  **Resumen:** El resumen final de los puntos clave aprendidos.
4.  **Evaluación:** Las preguntas de práctica del módulo, formateadas según las reglas.

---

## 1. Mejora de Contenido Teórico (Comando: "mejorar")

Cuando se solicite **mejorar** un texto (que no sea un examen o laboratorio), el asistente debe seguir estas reglas:

1.  **Interpretación y Claridad:** El texto original suele ser una traducción automática del inglés. El asistente debe interpretar el significado original y **reescribirlo en un español claro, natural y simple**, eliminando frases robóticas o confusas.
2.  **Formato Markdown:** Utilizar encabezados, listas (`-`), negritas (`**`) y bloques de código para estructurar visualmente la información.
3.  **Terminología de Certificación (Inglés/Español):** Es crucial para el examen conocer los términos técnicos en inglés.
    - Si aparece un término técnico traducido, se debe incluir el término original en inglés entre paréntesis.
    - **Ejemplos obligatorios:**
      - "Punto de conexión" -> **Punto de conexión (Endpoint)**
      - "Clave" -> **Clave (Key)**
      - "Implementación" -> **Implementación (Deployment)**
      - "Área de juegos" -> **Área de juegos (Playground)**
4.  **Eliminar textos irrelevantes:** Si el texto contiene información que no es relevante para la preparación del examen, como instrucciones de navegación , información del sitio o avisos de privacidad, se debe omitir en la mejora.

---

## 2. Formato de Evaluaciones (Comando: "formatear")

Cuando se solicite **formatear** una evaluación, se debe aplicar estrictamente el siguiente esquema:

1.  **Conservación del Texto:** NO corregir la redacción ni la traducción de las preguntas u opciones, aunque sean confusas (para simular el examen real).

2.  **Esquema:**
    1.  **[Texto original de la pregunta]**

        a. [Texto original Opción A]

        b. [Texto original Opción B] ✅

        c. [Texto original Opción C]

    **Justificación:** [Explicación clara de por qué la opción marcada es la correcta y por qué las otras no.]

    **Glosario / Comentarios:** [Diccionario de términos mal traducidos o confusos en esta pregunta. Ej: "Símbolo del sistema" = Prompt, "Poner en tierra" = Grounding].

---

## 3. Formato de Laboratorios (Comando: "lab")

Cuando se solicite procesar un **laboratorio** (comando: `lab`), el asistente debe seguir este nuevo flujo de trabajo para evitar el truncamiento de respuestas:

1.  **Fuente:** El contenido original estará en `traductor.md`.
2.  **Primer Paso: Generar `traducido.md`**
    - Realizar una traducción completa del contenido del laboratorio desde `traductor.md`.
    - **No resumir ni crear una narrativa.** Se debe mantener la estructura original del laboratorio, incluyendo todos los bloques de código.
    - Dentro de los bloques de código, traducir los comentarios y los prompts al español.
    - **NO incluir la sección "Código Completo"** en este archivo.
    - Escribir el resultado en el archivo existente `traducido.md` en la raíz del proyecto, sobrescribiendo su contenido.

3.  **Segundo Paso: Generar `codigo_traducido.md`**
    - Escribir en el archivo existente en la raíz llamado `codigo_traducido.md`.
    - Este archivo contendrá **únicamente el código completo del laboratorio**, formateado como si fuera un archivo `.py` pero dentro de bloques de código Markdown.
    - Si el laboratorio involucra múltiples archivos de código (ej. `functions.py`, `agent.py`), deben representarse en este único archivo, separados por bloques de código y con comentarios que indiquen el nombre del archivo original.
    - El código debe estar **totalmente comentado en español** (línea por línea) explicando qué hace cada función, bloque e importación.
    - Los prompts y mensajes del sistema dentro del código deben estar traducidos o explicados.

4.  **Sin Limpieza:** No se debe generar un diff para limpiar `traductor.md`. El usuario se encargará de esa tarea manualmente.

---

## 4. Automatizaciones y Flujos de Trabajo

### A. Actualización del Diccionario Central (`DICCIONARIO.md`)

- Al usar el comando `formatear`, se debe revisar el archivo `DICCIONARIO.md`.
- Para cada término del glosario generado:
  - **Si el concepto ya existe**, agregar la nueva traducción incorrecta a la lista de "Traducciones Comunes".
  - **Si el concepto es nuevo**, crear una nueva entrada en `DICCIONARIO.md` con el término en inglés, una explicación breve y la traducción incorrecta encontrada.

### B. Actualización del Archivo Central de Evaluaciones (`Evaluacion de todos los modulos.md`)

- Al usar el comando `formatear`, se debe copiar la evaluación formateada al final del archivo `Evaluacion de todos los modulos.md`.
- Se debe agregar un título de nivel 1 (`#`) correspondiente al nombre del archivo que se está procesando.

### C. Flujo de Trabajo Automático (`traductor.md` -> Módulo Activo)

- Al recibir una solicitud (`mejorar`, `formatear`, `lab`) sobre el contenido de `traductor.md`:
  1.  Procesar el contenido según las reglas del comando invocado (para `lab`, seguir el nuevo flujo de dos archivos).
  2.  **Determinar Destino (Regla de Consolidación):**
      - **Carpeta Específica:** Si el usuario indica un número de carpeta explícito, el archivo debe guardarse obligatoriamente en esa carpeta.
      - **Nuevo Archivo:** SOLO si el texto es la **Introducción** de un nuevo módulo (inicia un tema nuevo).
        - **Ubicación:** Identificar la carpeta de tema con la numeración más alta y guardar el archivo dentro. NUNCA crear archivos en la raíz.
      - **Mismo Archivo:** Si son secciones teóricas, laboratorios, resúmenes o evaluaciones, se **anexan** al final del archivo del módulo actual. **NO crear archivos separados** (ej. 3, 4, 5) para partes de un mismo módulo.
  3.  **Mover** el resultado: Generar un diff que inserte el contenido en el archivo determinado.
  4.  **Limpiar** `traductor.md`: Generar un diff que vacíe este archivo para dejarlo listo para el siguiente uso, **excepto cuando el comando es `lab`**.

---

## 5. Estructura de Carpetas y Numeración

Al iniciar un **nuevo tema** o bloque de estudio:

1.  **Nueva Carpeta:** Se debe verificar que exista una carpeta nueva numerada secuencialmente para el tema (ej. `2) Desarrollo de agentes...`).
2.  **Reinicio de Numeración:** Dentro de la nueva carpeta de tema, la numeración de los módulos (archivos `.md`) se reinicia obligatoriamente desde **1**.
    - _Incorrecto:_ `.../2) Nuevo Tema/9) Modulo.md` (Continuando la numeración de la carpeta anterior).
    - _Correcto:_ `.../2) Nuevo Tema/1) Modulo.md`.
