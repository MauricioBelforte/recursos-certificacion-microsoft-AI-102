# Instrucciones de Estilo y Comportamiento del Agente

Este archivo define cómo el asistente debe procesar, formatear y mejorar el contenido de este repositorio para la preparación de la certificación **Microsoft AI-102**.

---

## 1. Mejora de Contenido Teórico (Comando: "Mejorar")

Cuando se solicite **mejorar** un texto (que no sea un examen), el asistente debe seguir estas reglas:

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

## 2. Estilo para Evaluaciones y Exámenes (Comando: "Formatear")

Cuando se solicite **formatear** preguntas de práctica o exámenes (multiple choice), se debe aplicar estrictamente el siguiente esquema:

1.  **Conservación del Texto:** NO corregir la redacción ni la traducción de las preguntas u opciones, aunque sean confusas (para simular el examen real).

2.  **Esquema:**

    1.  **[Texto original de la pregunta]**

        a. [Texto original Opción A]

        b. [Texto original Opción B] ✅

        c. [Texto original Opción C]

    **Justificación:** [Explicación clara de por qué la opción marcada es la correcta y por qué las otras no.]

    **Glosario / Comentarios:** [Diccionario de términos mal traducidos o confusos en esta pregunta. Ej: "Símbolo del sistema" = Prompt, "Poner en tierra" = Grounding].

3.  **Actualización del Diccionario Central (`DICCIONARIO.md`):**
    *   Después de generar el "Glosario / Comentarios", se debe revisar el archivo `DICCIONARIO.md` en la raíz.
    *   Para cada término del glosario:
        *   **Si el concepto ya existe**, agregar la nueva traducción incorrecta a la lista de "Traducciones Comunes".
        *   **Si el concepto es nuevo**, crear una nueva entrada en `DICCIONARIO.md` con el término en inglés, una explicación breve y la traducción incorrecta encontrada.

4.  **Actualización del Archivo Central de Evaluaciones (`Evaluacion de todos los modulos.md`):**
    *   Copiar la evaluación formateada al final del archivo `Evaluacion de todos los modulos.md`.
    *   Agregar un título de nivel 1 (`#`) correspondiente al nombre del archivo que se está procesando (incluyendo su numeración) para mantener el orden.