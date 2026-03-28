# Instrucciones de Estilo y Comportamiento del Agente

Este archivo define cómo el asistente debe procesar, formatear y mejorar el contenido de este repositorio para la preparación de la certificación **Microsoft AI-102**.

---

## Estructura General de un Módulo

Cada archivo de módulo debe seguir el siguiente orden de contenido para mantener la consistencia:

1.  **Contenido Teórico Principal:** Todo el texto mejorado del módulo, separado por títulos.
2.  **Laboratorio:** La guía práctica del módulo, adaptada para la lectura.
3.  **Evaluación:** Las preguntas de práctica del módulo, formateadas según las reglas.
4.  **Resumen:** El resumen final de los puntos clave aprendidos.

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

3.  **Destino Dual (Obligatorio):**
    - **Archivo Central:** Agregar la evaluación formateada al final de `Evaluacion de todos los modulos.md`.
    - **Archivo Principal del Módulo:** Agregar la misma evaluación al **archivo principal del módulo actual** (el archivo `.md` numerado donde está el contenido teórico).
      - **Posición:** Debe quedar **ANTES** de la sección de "Resumen". Si el resumen ya existe en el archivo, insertar la evaluación antes de él. Si no existe, agregar al final (el resumen vendrá después).

---

## 3. Formato de Laboratorios (Comando: "lab")

Cuando se solicite procesar un **laboratorio** (comando: `lab`), el asistente debe aplicar una automatización total e integrarlo al archivo de trabajo:

1.  **Fuente:** El contenido original estará en `traductor.md`.
2.  **Guía y Código en el Módulo Activo:**
    - El título principal de la sección debe ser `## Laboratorio: [Nombre del Laboratorio]`.
    - Realizar una traducción completa del contenido del laboratorio desde `traductor.md`.
    - **No resumir ni crear una narrativa.** Se debe mantener la estructura original del laboratorio, incluyendo todos los bloques de código.
    - Dentro de los bloques de código, traducir los comentarios y los prompts al español. Además, se deben agregar **comentarios explicativos de alto nivel** en los fragmentos clave.
    - Inmediatamente después de la guía paso a paso del laboratorio, agregar la sección `### Código Completo del Laboratorio: [Nombre del Laboratorio]`.
    - Esta subsección final contendrá **únicamente el código completo del laboratorio**, formateado dentro de bloques de código Markdown. Si involucra múltiples archivos (ej. `functions.py`), separarlos por bloques indicando su nombre original.
    - El código debe estar **profusamente comentado en español** (flujo, arquitectura, "qué sucede aquí"). Los prompts del sistema también traducidos.
3.  **Destino (Obligatorio):**
    - Insertar todo este contenido (Guía + Código Completo) directamente en el **archivo principal del módulo actual** en el que se está trabajando.
4.  **Limpieza Automática:** Editar y vaciar automáticamente `traductor.md` tras completar la tarea.

---

## 4. Formato de Exámenes de Práctica (Comando: "examen")

Cuando se solicite procesar un **examen** (comando: `examen`), se debe seguir el mismo esquema de formato que en las "Evaluaciones del Módulo" (Sección 2), pero con un destino diferente.

1.  **Fuente:** El contenido original estará en `traductor.md`.
2.  **Formato:** Aplicar estrictamente el esquema de preguntas, opciones, justificación y glosario definido en la **Sección 2 (Formato de Evaluaciones)**.
3.  **Destino Único:**
    - Agregar las preguntas formateadas al final del archivo `Examenes/Evaluacion de práctica EXAMEN.md` mediante las herramientas de edición.
    - **NO** agregar a los archivos de módulo ni al archivo central de evaluaciones de módulos.
4.  **Limpieza Automática:** Editar y vaciar automáticamente `traductor.md` usando las herramientas tras completar la tarea.

---

## 5. Automatizaciones y Flujos de Trabajo

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
  1.  Procesar el contenido según las reglas del comando invocado.
  2.  **Determinar Destino (Regla de Consolidación):**
      - **Carpeta Específica:** Si el usuario indica un número de carpeta explícito, el archivo debe guardarse obligatoriamente en esa carpeta.
      - **Nuevo Archivo:** SOLO si el texto es la **Introducción** de un nuevo módulo (inicia un tema nuevo).
        - **Ubicación:** Identificar la carpeta de tema con la numeración más alta y guardar el archivo dentro. NUNCA crear archivos en la raíz.
      - **Mismo Archivo:** Si son secciones teóricas, laboratorios, resúmenes o evaluaciones (comando `formatear`), se agregan al **archivo principal del módulo actual**. **NO crear archivos separados**.
        - **Nota:** Las **Evaluaciones** deben colocarse antes del Resumen.
  3.  **Ejecutar los Cambios Directamente:** El asistente **DEBE usar sus herramientas de edición** (ej. lectura/escritura) para insertar el texto definitivo directamente en los archivos correspondientes (módulo actual, diccionario, etc.), eliminando totalmente la necesidad de que el usuario copie y pegue a mano.
  4.  **Limpiar `traductor.md` Automáticamente:** Posteriormente a la inserción exitosa, el asistente siempre modificará y vaciará el contenido de `traductor.md` de manera automática en todos los flujos (`mejorar`, `formatear`, `lab`, `examen`).

### D. Actualización del Resumen Técnico (`RESUMEN.md`)

- **Cuándo:** Al generar el resumen de un módulo o procesar un laboratorio.
- **Tarea:** Evaluar si el contenido procesado introduce **nuevos objetos, clases o funciones** del SDK que sean relevantes para el examen y que no existan aún en `RESUMEN.md`.
- **Acción:** Si hay nuevos elementos, agregarlos a `RESUMEN.md` siguiendo el formato establecido:
  - **Título:** Nombre del objeto.
  - **¿Para qué sirve?:** Explicación clara y sencilla.
  - **¿Cómo se importa?:** Código de importación.
  - **¿Cómo se usa?:** Ejemplo de código comentado.
- **Trampas:** Registrar también cualquier "objeto falso" detectado en las evaluaciones.

---

## 6. Estructura de Carpetas y Numeración

Al iniciar un **nuevo tema** o bloque de estudio:

1.  **Nueva Carpeta:** Se debe verificar que exista una carpeta nueva numerada secuencialmente para el tema (ej. `2) Desarrollo de agentes...`).
2.  **Reinicio de Numeración:** Dentro de la nueva carpeta de tema, la numeración de los módulos (archivos `.md`) se reinicia obligatoriamente desde **1**.
    - _Incorrecto:_ `.../2) Nuevo Tema/9) Modulo.md` (Continuando la numeración de la carpeta anterior).
    - _Correcto:_ `.../2) Nuevo Tema/1) Modulo.md`.
