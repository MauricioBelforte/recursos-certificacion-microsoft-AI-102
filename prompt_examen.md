# FLUJO DE PROCESAMIENTO DE PREGUNTAS DE EXAMEN AI-102

> Este archivo describe el flujo exacto que el asistente debe seguir cuando el usuario envía el comando `e` con contenido en `traductor.md`.

---

## CONTEXTO

El usuario está preparando la certificación **Microsoft AI-102**. Realiza simulacros de examen en Microsoft Learn y pega cada pregunta cruda en `traductor.md`. El asistente debe procesarla, registrarla y extraer conocimiento útil.

---

## TRIGGER

- El usuario escribe **`e`** (o **`examen`**).
- El contenido de la pregunta está en **`traductor.md`**.

---

## FLUJO PASO A PASO

### 1. LEER `traductor.md`
- Leer el contenido completo del archivo.
- Identificar: número de pregunta, enunciado, opciones, respuesta correcta y justificación original de Microsoft.

### 2. VERIFICAR DUPLICADOS
- Buscar en **`Examenes/Evaluacion de práctica EXAMEN.md`** si la pregunta ya existe.
- Usar `grep_search` con palabras clave únicas del enunciado (NO buscar solo por número de pregunta, porque se repiten entre intentos).
- **Si ya existe**: Informar al usuario ("Esta ya la tenemos, es la Pregunta X") y saltar al paso 6 (vaciar traductor).
- **Si NO existe**: Continuar al paso 3.

### 3. REGISTRAR EN ARCHIVO DE EXAMEN
- Archivo destino: **`Examenes/Evaluacion de práctica EXAMEN.md`**
- Obtener el número de la última pregunta registrada y usar el siguiente número correlativo (NO usar el número del simulacro de Microsoft, usar numeración interna continua).
- Formato obligatorio:

```
[N]. **[Enunciado reformulado con claridad, incluyendo términos en inglés entre paréntesis]**

    a. [Opción A]

    b. [Opción B] ✅ (si es correcta)

    c. [Opción C]

    d. [Opción D]

    **Justificación:** [Explicación clara y técnica de por qué la correcta es correcta y por qué las otras no.]

    **Glosario / Comentarios:**
    *   **"[Término traducido]"**: Traducción de *[Término original en inglés]*.
```

- Insertar al final del archivo, después del último bloque existente.
- **VERIFICAR** que la escritura fue exitosa antes de continuar.

### 4. ACTUALIZAR GUÍA EXPRESS
- Archivo: **`GUIA_EXPRESS_SALVAVIDAS_AI102.md`**
- Evaluar si la pregunta revela un **"truco" o patrón recurrente** útil para el examen.
- Si sí: Agregar un nuevo punto numerado en la sección "⚡ TRUCOS RÁPIDOS" con formato:
```
[N]. **¿[Pregunta corta del patrón]?**
    *   ✅ Respuesta: **[Respuesta clave en negrita]**. [Explicación breve].
```
- Si la pregunta es una repetición de un patrón ya documentado, NO agregar.

### 5. ACTUALIZAR DICCIONARIO (si aplica)
- Archivo: **`DICCIONARIO.md`**
- Si la pregunta contiene traducciones confusas o incorrectas que NO están ya en el diccionario, agregarlas.
- Si todos los términos ya están documentados, saltar este paso.

### 6. VACIAR `traductor.md`
- **SIEMPRE** al final, sin excepción, vaciar `traductor.md` usando `write_to_file` con contenido de un solo espacio en blanco.
- El archivo debe quedar **COMPLETAMENTE en blanco**. Sin comentarios, sin placeholders.

### 7. REPORTAR AL USUARIO
- Confirmar qué número de pregunta se registró.
- Mencionar el "truco" o punto clave extraído (1-2 líneas máximo).
- Confirmar que `traductor.md` fue vaciado.

---

## REGLAS CRÍTICAS

1. **NUNCA omitir el paso 6** (vaciar traductor). Hacerlo en CADA ejecución, incluso si la pregunta ya existía.
2. **NUNCA duplicar preguntas**. Siempre verificar antes de insertar.
3. **NUNCA usar el número de pregunta del simulacro de Microsoft** como ID interno. Usar numeración correlativa propia.
4. **SIEMPRE verificar que la escritura al archivo de examen fue exitosa** antes de reportar al usuario.
5. **Buscar duplicados por contenido**, no por número. Microsoft mezcla las preguntas entre intentos.
6. Si la pregunta introduce un **concepto nuevo no cubierto** en los módulos de estudio, evaluar si debe ir también en **`extras.md`**.

---

## ARCHIVOS INVOLUCRADOS

| Archivo | Rol |
|---|---|
| `traductor.md` | Buffer de entrada (el usuario pega aquí). Se vacía después de cada procesamiento. |
| `Examenes/Evaluacion de práctica EXAMEN.md` | Registro maestro de todas las preguntas procesadas con justificación y glosario. |
| `GUIA_EXPRESS_SALVAVIDAS_AI102.md` | Guía de estudio rápido. Se actualiza con "trucos" detectados en las preguntas. |
| `DICCIONARIO.md` | Diccionario de traducciones incorrectas/confusas del examen. |
| `extras.md` | Temas técnicos críticos detectados que requieren estudio adicional. |

---

## EJEMPLO DE EJECUCIÓN

1. Usuario pega pregunta en `traductor.md` y escribe `e`.
2. Asistente lee `traductor.md`.
3. Busca "transcripción por lotes" en el archivo de examen → NO encontrado.
4. Agrega como Pregunta 73 al final de `Evaluacion de práctica EXAMEN.md`.
5. Detecta truco: "Batch Transcription = Azure Storage". Lo agrega a la guía.
6. Vacía `traductor.md`.
7. Reporta: "Pregunta 73 registrada. Truco: Batch Transcription siempre usa Azure Storage. Traductor vacío."
