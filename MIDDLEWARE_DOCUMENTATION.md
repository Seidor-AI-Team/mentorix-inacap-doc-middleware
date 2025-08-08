# Documentación API Middleware Mentorix-Inacap

## Introducción

Esta API middleware actúa como una capa de conversión entre la nueva API comercial de Mentorix y el sistema cliente de Inacap. Permite mantener la integración existente con mínimos cambios mientras se accede a las funcionalidades mejoradas.

## Base URLs

**Producción:**
```
https://api-mentorix-prod-875064431579.southamerica-west1.run.app
```

**Desarrollo:**
```
https://api-mentorix-dev-875064431579.southamerica-west1.run.app
```

---

## 📚 1. TUTORES INTELIGENTES (Socráticos y Prácticos)

### 1.1 Iniciar Conversación Socrática
**Endpoint:** `POST api/v2/socratic/threads`

**Descripción:** Inicia una nueva sesión de tutoría socrática para un estudiante.

**Campos de entrada:**
```json
{
    "message": "Hola que tal",
    "student_id": "12345678-k",
    "section_id": "d06efb84-7072-4608-83d7-b57576100f01"
}
```

**Respuesta exitosa:**
```json
{
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3OC1rIiwicnV0IjoiMTIzNDU2NzgtayIsInJvbGUiOiJTVFVERU5UIiwiaWF0IjoxNzU0NjcwMDU4LCJleHAiOjE3NTUxMDIwNTh9.4YKVrdy3VL3SJVl33tDNpdEX3mm0b9KFv5FLQ4zJBLE",
    "assistant_message": "¡Hola Juan! 😊 ¿Tienes alguna duda sobre el curso de GitFlow que quieras conversar hoy? ¡Cuéntame en qué te gustaría profundizar!",
    "message_id": "6a6b1202-377b-4355-a1e2-5e4cc7dfc355",
    "thread_id": "468531f9-44f4-41ac-96e4-5f46671d039d"
}
```

**⚠️ IMPORTANTE:** Debes guardar tanto el `thread_id` como el `access_token` para enviarlos en todas las siguientes peticiones de la conversación.

### 1.2 Continuar Conversación Socrática
**Endpoint:** `POST api/v2/socratic/threads/{thread_id}/messages`

**Descripción:** Envía un nuevo mensaje en una conversación socrática existente.

**Campos de entrada:**
```json
{
    "message": "que es un branch?",
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3OC1rIiwicnV0IjoiMTIzNDU2NzgtayIsInJvbGUiOiJTVFVERU5UIiwiaWF0IjoxNzU0NjcwMDU4LCJleHAiOjE3NTUxMDIwNTh9.4YKVrdy3VL3SJVl33tDNpdEX3mm0b9KFv5FLQ4zJBLE"
}
```

**Respuesta exitosa:**
```json
{
    "assistant_message": "¡Excelente pregunta, Juan! 😃\n\nAntes de darte una definición, ¿te animas a contarme para qué crees que sirve un branch en GitFlow? ¿Cómo imaginas que ayuda en el trabajo con código?",
    "message_id": "2ef04306-ae92-4228-9ce8-86fc39d220b1",
    "thread_id": "468531f9-44f4-41ac-96e4-5f46671d039d"
}
```

### 1.3 Iniciar Conversación de Planificación
**Endpoint:** `POST api/v2/planning/threads`

**Descripción:** Inicia una nueva sesión de tutoría práctica/planificación para un estudiante.

**Campos de entrada:**
```json
{
    "message": "string (requerido) - Primera pregunta o mensaje del estudiante",
    "student_id": "string (requerido) - ID único del estudiante", 
    "section_id": "string (requerido) - ID de la sección del curso"
}
```

**Respuesta:** Igual que la conversación socrática.

### 1.4 Continuar Conversación de Planificación
**Endpoint:** `POST api/v2/planning/threads/{thread_id}/messages`

**Descripción:** Envía un nuevo mensaje en una conversación de planificación existente.

**Campos de entrada y respuesta:** Igual que continuar conversación socrática.

### 1.5 Actualizar Feedback de Mensajes
**Endpoint:** `POST api/v2/feedback/update-feedback`

**Descripción:** Permite al estudiante dar feedback sobre las respuestas del tutor.

**Campos de entrada:**
```json
{
    "message_id": "6a6b1202-377b-4355-a1e2-5e4cc7dfc355",
    "feedback": 1
}
```

**Valores de feedback:**
- `1`: Positivo (like) 
- `0`: Neutral (por defecto para todos los mensajes)
- `-1`: Negativo (dislike)

**Respuesta:**
```json
{
    "status": "success",
    "message": "Feedback registrado correctamente"
}
```

---

## 📝 2. SISTEMA DE EVALUACIONES

### 2.1 Crear Evaluación para Estudiante
**Endpoint:** `POST api/v2/test/create_test`

**Descripción:** Crea una nueva evaluación personalizada para un estudiante.

**Campos de entrada:**
```json
{
    "section_id": "d06efb84-7072-4608-83d7-b57576100f01",
    "student_id": "12345678-k",
    "indication": "Que haya al menos una pregunta muy dificil",
    "topics": [
        {
            "content": "Diseño de Agentes",
            "course_id": "26c7db3d-add2-4ff5-8d93-6248e2ceaace",
            "created_at": "2025-05-11T18:53:04.995201",
            "id": "2b9056f4-d074-4ddb-a70f-59227b0a5fa0"
        }
    ],
    "options": [
        "Dificultad: Medio",
        "Total de preguntas: 5",
        "Tipo: mix"
    ]
}
```

### 2.2 Revisar y Evaluar Respuestas de Evaluación
**Endpoint:** `POST api/v2/test/save_test_answer`

**Descripción:** **Endpoint de revisión de la prueba** donde la IA analiza las respuestas del estudiante y evalúa la prueba proporcionando retroalimentación inteligente.

**Campos de entrada:**
```json
{
    "multiple_choice_answers": [
        {
            "answer": "a2",
            "question_id": "5c08a615-dc2c-59f1-90c0-b891838b2809"
        },
        {
            "answer": "a3",
            "question_id": "4333d20c-a0dd-54f7-9669-38cb47d2c3ee"
        },
        {
            "answer": "a2",
            "question_id": "2ed302c2-2968-5462-8431-50c5e783cc68"
        }
    ],
    "open_question_answers": [
        {
            "answer": "Son dispositivos pequeños y modernos que ayudan a que el auto sea más cómodo y lujoso.",
            "question_id": "1494537c-80bd-56b5-ba1f-efd85191f3ad"
        },
        {
            "answer": "Los sensores hacen que el auto sea más caro y complicado, pero permiten agregar pantallas y sistemas inteligentes.",
            "question_id": "c5f466ee-bfbd-5e3e-9a5c-f1f355994386"
        }
    ],
    "test_id": "366a6835-edbe-4e3b-9326-79e64e3777ff"
}
```

**Respuesta:**
```json
{
    "multiple_choice_revision": [
        {
            "correct_option": "a2",
            "is_correct": true,
            "justification": "El patrón descentralizado permite que muchos agentes trabajen en igualdad de condiciones, delegando tareas entre ellos y permitiendo que cada agente tome control total de la ejecución sin que el agente original deba permanecer involucrado, lo que mejora la flexibilidad y especialización.",
            "question_id": "5c08a615-dc2c-59f1-90c0-b891838b2809",
            "user_answer": "a2"
        },
        {
            "correct_option": "a2",
            "is_correct": false,
            "justification": "El agente 'triage_agent' evalúa las consultas de los usuarios inicialmente y decide a qué agente especializado debe delegar la tarea con el fin de optimizar la atención y especialización en el flujo de trabajo.",
            "question_id": "4333d20c-a0dd-54f7-9669-38cb47d2c3ee",
            "user_answer": "a3"
        }
    ],
    "open_question_revision": [
        {
            "expected_answer": "El patrón manager utiliza un agente central que controla y delega tareas a agentes especializados mediante llamadas a herramientas garantizando un control cohesivo del flujo. En cambio, el patrón descentralizado permite que agentes transfieran el control directamente entre ellos sin un control central, favoreciendo la descentralización y especialización autónoma.",
            "justification": "La respuesta no aborda en absoluto la diferencia entre el patrón manager y el patrón descentralizado en la orquestación de agentes. La respuesta mencionada está fuera del contexto y no muestra comprensión del tema. Se recomienda estudiar la definición y función de ambos patrones para mejorar la respuesta.",
            "question_id": "1494537c-80bd-56b5-ba1f-efd85191f3ad",
            "score": 0,
            "user_answer": "Son dispositivos pequeños y modernos que ayudan a que el auto sea más cómodo y lujoso."
        }
    ]
}
```

### 2.3 Crear Evaluación para Profesor
**Endpoint:** `POST api/v2/test/create_test_teacher`

**Descripción:** Permite a un profesor crear una evaluación para sus estudiantes.

**Campos de entrada:**
```json
{
    "teacher_id": "87654321-9",
    "section_id": "d06efb84-7072-4608-83d7-b57576100f01",
    "indication": "Hacer preguntas sobre diseño de agentes",
    "topics": [
        {
            "content": "Diseño de Agentes",
            "course_id": "26c7db3d-add2-4ff5-8d93-6248e2ceaace",
            "created_at": "2025-05-11T18:53:04.995201",
            "id": "2b9056f4-d074-4ddb-a70f-59227b0a5fa0"
        }
    ],
    "options": [
        "Dificultad: Medio",
        "Total de preguntas: 5",
        "Tipo: mix"
    ]
}
```

### 2.4 Confirmar Evaluación del Profesor
**Endpoint:** `POST api/v2/test/confirm_test_teacher`

**Descripción:** El profesor revisa, edita si es necesario, y confirma las preguntas generadas. **Al confirmar, la prueba se distribuye automáticamente a los estudiantes** de la sección (se enlazan en la base de datos).

**⚠️ CAMPOS EDITABLES POR EL PROFESOR:**

**Para preguntas de alternativas:**
- Solo puede editar las `options` (alternativas)
- Solo puede editar `correct_answer` (alternativa correcta)
- **NO debe manipular otros campos**

**Para preguntas abiertas:**
- Solo puede editar `question` (texto de la pregunta)
- Solo puede editar `expected_answer` (respuesta esperada)
- **NO debe manipular otros campos**

**Campos de entrada:**
```json
{
    "test_id": "e79d5b54-e85e-4eec-8c65-41b5877d1fb5",
    "teacher_id": "87654321-9",
    "questions": [
        {
            "id": "5c08a615-dc2c-59f1-90c0-b891838b2809",
            "type": "multiple_choice",
            "options": ["Opción A editada", "Opción B editada", "Opción C editada", "Opción D editada"],
            "correct_answer": "a2"
        },
        {
            "id": "1494537c-80bd-56b5-ba1f-efd85191f3ad",
            "type": "open_question",
            "question": "Pregunta editada por el profesor",
            "expected_answer": "Respuesta esperada editada"
        }
    ]
}
```

**Respuesta:**
```json
{
    "message": "Test confirmado exitosamente"
}
```

### 2.5 Verificar Evaluaciones Disponibles (Estudiante)
**Endpoint:** `POST api/v2/test/check_available_tests`

**Descripción:** Verifica si hay evaluaciones pendientes para un estudiante.

**Campos de entrada:**
```json
{
    "student_id": "12345678-k",
    "section_id": "d06efb84-7072-4608-83d7-b57576100f01"
}
```

**Respuesta:**
```json
{
    "available_tests": [
        {
            "message": "Test disponible",
            "questions": {
                "multiple_choice_questions": [
                    {
                        "a1": "las enfermedades del cuerpo",
                        "a2": "el funcionamiento de los órganos y sistemas en condiciones normales",
                        "a3": "la cirugía médica",
                        "a4": "la farmacología",
                        "correct_option": "a2",
                        "created_at": null,
                        "id": "b0f1c8cb-5fa6-5639-a681-e4471b16168f",
                        "justification": "La fisiología médica estudia cómo funcionan los órganos y sistemas del cuerpo en condiciones normales, no las enfermedades ni tratamientos específicos.",
                        "question": "Complete la oración con la opción correcta: La fisiología médica se centra en el estudio de A) las enfermedades del cuerpo, B) el funcionamiento de los órganos y sistemas en condiciones normales, C) la cirugía médica, D) la farmacología.",
                        "topic_id": "7f3c0e45-fc7e-45aa-aef5-fc0fd5d17710",
                        "type": "multiple_choices"
                    }
                ],
                "open_questions": [
                    {
                        "created_at": null,
                        "expected_answer": "los procesos vitales",
                        "id": "23a28476-8267-597a-9ee6-26e949448436",
                        "question": "Complete la siguiente oración: La fisiología médica es la disciplina que estudia __________, enfocándose en el funcionamiento normal de los sistemas del cuerpo humano.",
                        "score": null,
                        "topic_id": "7f3c0e45-fc7e-45aa-aef5-fc0fd5d17710",
                        "type": "open_question"
                    }
                ]
            },
            "test": {
                "created_at": "2025-08-06T13:20:02.724Z",
                "edited": false,
                "id": "812e2bad-9a30-41bd-ae95-afd46c7934d4",
                "rules": null,
                "student_id": null,
                "updated_at": "2025-08-06T13:20:02.724Z"
            }
        }
    ],
    "count": 16,
    "message": "16 evaluaciones disponibles"
}
```

### 2.6 Verificar Evaluaciones Disponibles (Profesor)
**Endpoint:** `POST api/v2/test/check_available_tests_teacher`

**Descripción:** Verifica evaluaciones pendientes de revisión para un profesor.

**Campos de entrada:**
```json
{
    "teacher_id": "87654321-9",
    "section_id": "d06efb84-7072-4608-83d7-b57576100f01"
}
```

---

## 📖 3. GESTIÓN DE TEMAS/TOPICS

### 3.1 Obtener Topics de una Sección
**Endpoint:** `POST api/v2/topic/topic`

**Descripción:** Obtiene todos los temas/topics disponibles para una sección específica del curso.

**Campos de entrada:**
```json
{
    "section_id": "d06efb84-7072-4608-83d7-b57576100f01"
}
```

**Respuesta exitosa:**
```json
{
    "topics": [
        {
            "id": "2b9056f4-d074-4ddb-a70f-59227b0a5fa0",
            "content": "Diseño de Agentes",
            "course_id": "26c7db3d-add2-4ff5-8d93-6248e2ceaace",
            "created_at": "2025-07-31T03:32:21.032Z"
        },
        {
            "id": "55fd6db4-1dc2-4476-854f-e3065522b873",
            "content": "Guardrails y Seguridad",
            "course_id": "26c7db3d-add2-4ff5-8d93-6248e2ceaace",
            "created_at": "2025-07-31T03:32:21.032Z"
        },
        {
            "id": "1750df25-52e5-4d6f-8557-41d7e8103af9",
            "content": "Casos de Uso Ideales",
            "course_id": "26c7db3d-add2-4ff5-8d93-6248e2ceaace",
            "created_at": "2025-07-31T03:32:21.032Z"
        }
    ]
}
```

**Campos de respuesta:**
- `topics`: Array de temas disponibles
  - `id`: Identificador único del tema
  - `content`: Nombre/descripción del tema
  - `course_id`: ID del curso al que pertenece el tema
  - `created_at`: Fecha de creación del tema

---

## 🎤 4. FUNCIONALIDADES DE AUDIO

### 4.1 Subir Audio (Transcripción de Voz a Texto)
**Endpoint:** `POST api/v2/audio/upload`

**Descripción:** Convierte audio grabado por el usuario a texto. **Recomendación de implementación:** Agregar un botón de "🎤 Grabar" en el campo de texto del mensaje para que el usuario pueda grabar su voz y enviarla a este endpoint.

**⚠️ FLUJO DE TRABAJO IMPORTANTE:**
1. El cliente envía el audio grabado a `/audio/upload`
2. El endpoint devuelve el texto transcrito en el campo `transcription`
3. **El cliente debe tomar este texto y enviarlo al tutor correspondiente** usando:
   - `POST /socratic/threads` o `/socratic/threads/{thread_id}/messages` para tutoría socrática
   - `POST /planning/threads` o `/planning/threads/{thread_id}/messages` para tutoría práctica
4. El tutor procesa el texto como si fuera un mensaje escrito normal

**Formato:** FormData
```
Content-Type: multipart/form-data
audio: [archivo de audio WebM/Blob]
```
**Respuesta:**
```json
{
    "transcription": "Esta es una prueba de audio"
}
```

### 4.2 Generar Audio (Síntesis de Voz)
**Endpoint:** `POST api/v2/audio/generate`

**Descripción:** Convierte texto a audio hablado. **Recomendación de implementación:** Agregar un botón de "🔊 Escuchar" en cada mensaje del tutor para que el usuario pueda escuchar la respuesta en audio.

**Campos de entrada:**
```json
{
    "text": "Hola que tal"
}
```

**Respuesta:**
```json
{
    "audio": "//PkxABlpDmwAVrAAD+fD8dD2YjqPjSAgSfNvLOzjPXdP5uPVkOxKN0sM8QAQwtgYIcZ5AbtubVaaU2ZUeY0aY0eZEmZUqZUedoG5hqUaEGhRsYb..."
}
```

### 💡 Recomendaciones de Integración de Audio

**Para el Frontend:**
1. **Campo de mensaje:** Agregar botón "🎤 Grabar" que:
   - Inicie grabación de audio
   - Envíe a `/audio/upload` 
   - Coloque la transcripción en el campo de texto

2. **Mensajes del tutor:** Agregar botón "🔊 scuchar" que:
   - Envíe el texto del mensaje a `/audio/generate`
   - Reproduzca el audio base64 recibido

3. **Indicadores visuales:**
   - Mostrar estado "Grabando..." durante captura
   - Mostrar estado "Transcribiendo..." durante procesamiento
   - Mostrar estado "Generando audio..." durante síntesis

---

## 🔄 FLUJOS TÍPICOS DE USO

### Flujo 1: Sesión de Tutoría Completa
1. **Iniciar:** `POST api/v2/socratic/threads` o `POST api/v2/planning/threads`
   - Guardar `thread_id` y `access_token` de la respuesta
2. **Continuar:** `POST api/v2/{tipo}/threads/{thread_id}/messages` (repetir según necesidad)
   - Enviar `access_token` en cada petición
3. **Feedback:** `POST api/v2/feedback/update-feedback` (opcional)
   - Enviar feedback como entero: 1 (positivo), 0 (neutral), -1 (negativo)

### Flujo 2: Evaluación de Estudiante
1. **Verificar:** `POST api/v2/test/check_available_tests` (consultar pruebas disponibles)
2. **Crear/Obtener:** `POST api/v2/test/create_test` (si es evaluación adaptativa) 
3. **Responder y Evaluar:** `POST api/v2/test/save_test_answer` (IA revisa y califica)

### Flujo 3: Evaluación por Profesor
1. **Crear:** `POST api/v2/test/create_test_teacher`
2. **Revisar y editar** las preguntas generadas (solo campos permitidos)
3. **Confirmar:** `POST api/v2/test/confirm_test_teacher` (distribuye automáticamente a estudiantes)
4. **Verificar:** `POST api/v2/test/check_available_tests_teacher`

### Flujo 4: Interacción con Audio
1. **Subir audio:** `POST api/v2/audio/upload` (para obtener transcripción)
2. **Usar transcripción** en endpoints de conversación
3. **Generar respuesta de audio:** `POST api/v2/audio/generate` (opcional)

---

## ❌ MANEJO DE ERRORES

Todos los endpoints retornan errores en el formato:
```json
{
    "error": "string - Descripción del error"
}
```

**Códigos de estado HTTP:**
- `200`: Éxito
- `400`: Error en los datos enviados
- `500`: Error interno del servidor

---

## 📞 SOPORTE

Para preguntas técnicas o problemas con la integración, contactar al equipo de desarrollo de Mentorix.

**Notas importantes:**
- Todos los endpoints usan método POST
- Mantener los `thread_id` para continuar conversaciones
- Los `section_id` y `student_id` deben ser válidos en el sistema Inacap
