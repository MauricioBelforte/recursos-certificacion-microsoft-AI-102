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
    *   Si aparece un término técnico traducido, se debe incluir el término original en inglés entre paréntesis.
    *   **Ejemplos obligatorios:**
        *   "Punto de conexión" -> **Punto de conexión (Endpoint)**
        *   "Clave" -> **Clave (Key)**
        *   "Implementación" -> **Implementación (Deployment)**
        *   "Área de juegos" -> **Área de juegos (Playground)**
4. **Eliminar textos irrelevantes:** Si el texto contiene información que no es relevante para la preparación del examen, como instrucciones de navegación , información del sitio o avisos de privacidad, se debe omitir en la mejora.
   
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

Cuando se solicite procesar un **laboratorio** (comando: `lab`), el asistente debe seguir estas reglas:

1.  **Fuente:** El contenido original estará en `traductor.md`.
2.  **Adaptación para Lectura:** El objetivo es transformar las instrucciones prácticas en una descripción narrativa que se pueda entender sin ejecutar los pasos.
    *   Reescribir los pasos de "hacer clic aquí" o "ejecutar este comando" como una explicación de *qué se está haciendo y por qué*.
    *   Estructurar el laboratorio con títulos claros, listas y bloques de código para los ejemplos.
3.  **Manejo de Traducciones y Terminología:**
    *   **Conservar Términos Confusos:** Si el texto original contiene términos mal traducidos (comunes en los exámenes en español), **se deben mantener** para facilitar el reconocimiento durante el examen.
    *   **Aclaración en Inglés:** Inmediatamente después del término mal traducido, colocar entre paréntesis el término correcto en inglés y/o la traducción correcta.
        *   Ejemplo: "Seleccione la base (Grounding)..."
    *   **Notas y Glosario:** Agregar notas aclaratorias o pequeños glosarios integrados en el texto cuando sea necesario para explicar contextos confusos.
    *   **Traducción de Prompts:** Si el laboratorio incluye ejemplos de prompts en inglés (como Mensajes de Sistema o preguntas de usuario), se debe proporcionar una traducción al español debajo del original para clarificar su propósito y contenido.
4.  **Ubicación en el Módulo:** El contenido del laboratorio procesado debe insertarse en el archivo del módulo activo, justo antes del Resumen y la Evaluación.

---

## 4. Automatizaciones y Flujos de Trabajo

### A. Actualización del Diccionario Central (`DICCIONARIO.md`)
*   Al usar el comando `formatear`, se debe revisar el archivo `DICCIONARIO.md`.
*   Para cada término del glosario generado:
    *   **Si el concepto ya existe**, agregar la nueva traducción incorrecta a la lista de "Traducciones Comunes".
    *   **Si el concepto es nuevo**, crear una nueva entrada en `DICCIONARIO.md` con el término en inglés, una explicación breve y la traducción incorrecta encontrada.

### B. Actualización del Archivo Central de Evaluaciones (`Evaluacion de todos los modulos.md`)
*   Al usar el comando `formatear`, se debe copiar la evaluación formateada al final del archivo `Evaluacion de todos los modulos.md`.
*   Se debe agregar un título de nivel 1 (`#`) correspondiente al nombre del archivo que se está procesando.

### C. Flujo de Trabajo Automático (`traductor.md` -> Módulo Activo)
*   Al recibir una solicitud (`mejorar`, `formatear`, `lab`) sobre el contenido de `traductor.md`:
    1.  Procesar el contenido según las reglas del comando invocado.
    2.  **Determinar Destino (Regla de Consolidación):**
        *   **Nuevo Archivo:** SOLO si el texto es la **Introducción** de un nuevo módulo (inicia un tema nuevo).
        *   **Mismo Archivo:** Si son secciones teóricas, laboratorios, resúmenes o evaluaciones, se **anexan** al final del archivo del módulo actual. **NO crear archivos separados** (ej. 3, 4, 5) para partes de un mismo módulo.
    3.  **Mover** el resultado: Generar un diff que inserte el contenido en el archivo determinado.
    4.  **Limpiar** `traductor.md`: Generar un diff que vacíe este archivo para dejarlo listo para el siguiente uso.

---

## 5. Estructura de Carpetas y Numeración

Al iniciar un **nuevo tema** o bloque de estudio:

1.  **Nueva Carpeta:** Se debe verificar que exista una carpeta nueva numerada secuencialmente para el tema (ej. `2) Desarrollo de agentes...`).
2.  **Reinicio de Numeración:** Dentro de la nueva carpeta de tema, la numeración de los módulos (archivos `.md`) se reinicia obligatoriamente desde **1**.
    *   *Incorrecto:* `.../2) Nuevo Tema/9) Modulo.md` (Continuando la numeración de la carpeta anterior).
    *   *Correcto:* `.../2) Nuevo Tema/1) Modulo.md`.