# 02 — EPIC, Features, Historias de Usuario e Issues — LibroTech → ChatTech

**Proyecto:** LibroTech escalado con módulo ChatTech  
**Versión:** 2.0.0  
**Fecha:** 20/05/2026  
**Equipo:** Salvador Aponte, Alejandro Rodriguez, Camilo Mitnick, Luis Mejia  

---

## EPIC-01 — Escalamiento de LibroTech a ChatTech con IA en tiempo real

Como equipo de desarrollo, queremos escalar el proyecto LibroTech existente agregando un módulo ChatTech, para permitir comunicación en tiempo real entre bibliotecarios con historial persistente e integración de IA contextual.

---

## Reparto equitativo

| Integrante | Issues | Story Points |
|---|---:|---:|
| Salvador Aponte | ISSUE-001, ISSUE-005, ISSUE-009 | 11 |
| Luis Mejia | ISSUE-002, ISSUE-006, ISSUE-010 | 11 |
| Alejandro Rodriguez | ISSUE-003, ISSUE-007, ISSUE-011 | 11 |
| Camilo Mitnick | ISSUE-004, ISSUE-008, ISSUE-012 | 11 |

---

## FEATURE-01 — Configuración técnica del escalamiento

Permite preparar el proyecto LibroTech existente para soportar MongoDB, WebSocket, Spring AI y variables seguras.

---

### ISSUE-001 — Configurar dependencias y propiedades base para ChatTech

| Campo | Valor |
|---|---|
| Epic | EPIC-01 |
| Feature | FEATURE-01 — Configuración técnica del escalamiento |
| RF/RNF | RNF-01, RNF-03, RNF-05 |
| HU asociada | HU-CONFIG-001 |
| Responsable | Salvador Aponte |
| Tipo | Requisito Técnico |
| Prioridad | 🔴 Alta |
| Labels | `config`, `dependencies`, `backend`, `chattech` |
| Story Points | 3 |
| Rama sugerida | `issue-001-config-chattech-dependencies` |
| Depende de | Sin dependencias |
| Bloquea | ISSUE-002, ISSUE-003, ISSUE-004, ISSUE-007 |

#### Historia de Usuario

Como desarrollador, quiero preparar el proyecto LibroTech con las dependencias necesarias de ChatTech, para poder implementar MongoDB, WebSocket y Spring AI sin afectar el módulo existente.

#### Encabezado de Código / Guía Técnica

Archivos esperados:

```text
pom.xml
src/main/resources/application.properties
.gitignore
```

Dependencias a agregar:

```text
spring-boot-starter-data-mongodb
spring-boot-starter-websocket
spring-ai-openai-spring-boot-starter
```

Propiedades esperadas:

```properties
spring.data.mongodb.uri=${MONGODB_URI:mongodb://localhost:27017/librotech_chat}
spring.ai.openai.api-key=${OPENAI_API_KEY}
spring.ai.openai.chat.options.model=gpt-4o-mini
spring.ai.openai.chat.options.temperature=0.7
```

#### Criterios de aceptación

1. [ ] El proyecto conserva las dependencias existentes de Web, JPA, Thymeleaf, H2 y Swagger.
2. [ ] Se agregan dependencias necesarias para MongoDB, WebSocket y Spring AI.
3. [ ] La API key de OpenAI se lee desde variable de entorno.
4. [ ] `.gitignore` evita subir `.env`, logs y propiedades locales.
5. [ ] `mvn test` no rompe los tests existentes.

#### Casos de prueba asociados

`CP-CONFIG-001`

#### Pruebas unitarias sugeridas

No aplica test unitario directo. Validar con `contextLoads()` y compilación.

---

## FEATURE-02 — Persistencia documental del chat con MongoDB

Permite guardar y consultar mensajes del chat sin afectar la persistencia relacional actual de LibroTech.

---

### ISSUE-002 — Implementar documento, repositorio y servicio de mensajes

| Campo | Valor |
|---|---|
| Epic | EPIC-01 |
| Feature | FEATURE-02 — Persistencia documental del chat con MongoDB |
| RF/RNF | RF-CHAT-01, RF-CHAT-02, RF-CHAT-12 |
| HU asociada | HU-MSG-001 |
| Responsable | Luis Mejia |
| Tipo | Historia de Usuario |
| Prioridad | 🔴 Alta |
| Labels | `mongodb`, `backend`, `feature`, `mensajes` |
| Story Points | 5 |
| Rama sugerida | `issue-002-mongodb-mensajes` |
| Depende de | ISSUE-001 |
| Bloquea | ISSUE-005, ISSUE-007, ISSUE-009, ISSUE-010 |

#### Historia de Usuario

Como sistema, quiero guardar mensajes enviados por usuarios y bot en MongoDB, para mantener historial persistente y dar contexto al asistente IA.

#### Encabezado de Código / Guía Técnica

Crear el módulo dentro del paquete existente:

```text
src/main/java/com/librotech/chattech/model/Mensaje.java
src/main/java/com/librotech/chattech/repository/MensajeRepository.java
src/main/java/com/librotech/chattech/service/MensajeService.java
```

Métodos mínimos esperados en `MensajeService`:

```text
guardarMensaje(Mensaje mensaje)
obtenerHistorial()
obtenerHistorialReciente()
```

#### Criterios de aceptación

1. [ ] Existe el documento `Mensaje` con `id`, `remitente`, `contenido` y `fechaEnvio`.
2. [ ] `MensajeRepository` extiende de `MongoRepository<Mensaje, String>`.
3. [ ] `MensajeService` permite guardar mensajes.
4. [ ] `MensajeService` permite obtener historial desde MongoDB.
5. [ ] Existe método para consultar historial reciente.
6. [ ] El código compila sin errores.

#### Casos de prueba asociados

`CP-MSG-001`, `CP-MSG-002`

#### Pruebas unitarias sugeridas

```text
MensajeServiceTest
- shouldSaveMessage()
- shouldReturnHistory()
- shouldReturnRecentHistoryLimited()
```

---

### ISSUE-003 — Crear pruebas de persistencia del módulo ChatTech

| Campo | Valor |
|---|---|
| Epic | EPIC-01 |
| Feature | FEATURE-02 — Persistencia documental del chat con MongoDB |
| RF/RNF | RF-CHAT-01, RF-CHAT-02, RNF-05 |
| HU asociada | HU-MSG-002 |
| Responsable | Alejandro Rodriguez |
| Tipo | Requisito Técnico |
| Prioridad | 🟡 Media |
| Labels | `test`, `mongodb`, `quality` |
| Story Points | 3 |
| Rama sugerida | `issue-003-tests-mongodb-chat` |
| Depende de | ISSUE-002 |
| Bloquea | ISSUE-012 |

#### Historia de Usuario

Como desarrollador, quiero probar la persistencia de mensajes del chat, para asegurar que el historial se guarda y consulta correctamente.

#### Encabezado de Código / Guía Técnica

Archivos esperados:

```text
src/test/java/com/librotech/chattech/service/MensajeServiceTest.java
src/test/java/com/librotech/chattech/repository/MensajeRepositoryTest.java
```

#### Criterios de aceptación

1. [ ] Existe prueba para guardar mensaje.
2. [ ] Existe prueba para consultar historial.
3. [ ] Existe prueba para historial reciente.
4. [ ] Los tests no rompen las pruebas existentes de Libro y Categoría.
5. [ ] `mvn test` pasa correctamente.

#### Casos de prueba asociados

`CP-MSG-003`

#### Pruebas unitarias sugeridas

```text
MensajeServiceTest con Mockito
MensajeRepositoryTest con @DataMongoTest si el ambiente lo permite
```

---

## FEATURE-03 — Comunicación en tiempo real con WebSocket

Permite que los mensajes enviados por un bibliotecario aparezcan en tiempo real en todas las pestañas conectadas.

---

### ISSUE-004 — Configurar WebSocket con STOMP y SockJS

| Campo | Valor |
|---|---|
| Epic | EPIC-01 |
| Feature | FEATURE-03 — Comunicación en tiempo real con WebSocket |
| RF/RNF | RF-CHAT-03 |
| HU asociada | HU-WS-001 |
| Responsable | Camilo Mitnick |
| Tipo | Requisito Técnico |
| Prioridad | 🔴 Alta |
| Labels | `websocket`, `config`, `realtime` |
| Story Points | 3 |
| Rama sugerida | `issue-004-websocket-config` |
| Depende de | ISSUE-001 |
| Bloquea | ISSUE-005 |

#### Historia de Usuario

Como sistema, quiero habilitar un canal WebSocket con STOMP, para permitir comunicación bidireccional en tiempo real.

#### Encabezado de Código / Guía Técnica

Archivo esperado:

```text
src/main/java/com/librotech/chattech/config/WebSocketConfig.java
```

Rutas esperadas:

```text
Endpoint SockJS: /chat-websocket
Prefijo de envío: /app
Canal de suscripción: /tema
```

#### Criterios de aceptación

1. [ ] Existe `WebSocketConfig`.
2. [ ] Se habilita `@EnableWebSocketMessageBroker`.
3. [ ] El broker simple usa `/tema`.
4. [ ] El prefijo de aplicación usa `/app`.
5. [ ] El endpoint `/chat-websocket` usa SockJS.

#### Casos de prueba asociados

`CP-WS-001`

#### Pruebas unitarias sugeridas

Validar por prueba de contexto o prueba de integración ligera.

---

### ISSUE-005 — Implementar controlador WebSocket del chat

| Campo | Valor |
|---|---|
| Epic | EPIC-01 |
| Feature | FEATURE-03 — Comunicación en tiempo real con WebSocket |
| RF/RNF | RF-CHAT-04, RF-CHAT-05, RF-CHAT-11 |
| HU asociada | HU-WS-002 |
| Responsable | Salvador Aponte |
| Tipo | Historia de Usuario |
| Prioridad | 🔴 Alta |
| Labels | `websocket`, `controller`, `feature` |
| Story Points | 5 |
| Rama sugerida | `issue-005-chat-socket-controller` |
| Depende de | ISSUE-002, ISSUE-004, ISSUE-007 |
| Bloquea | ISSUE-006, ISSUE-012 |

#### Historia de Usuario

Como bibliotecario, quiero enviar mensajes al chat y verlos reflejados en tiempo real, para comunicarme con otros usuarios de la sala.

#### Encabezado de Código / Guía Técnica

Archivo esperado:

```text
src/main/java/com/librotech/chattech/controller/ws/ChatSocketController.java
```

Dependencias internas:

```text
MensajeService
BotIAService
SimpMessagingTemplate
```

Rutas esperadas:

```text
@MessageMapping("/enviar")
@SendTo("/tema/mensajes")
```

#### Criterios de aceptación

1. [ ] El controlador recibe mensajes desde `/app/enviar`.
2. [ ] El mensaje del usuario se guarda en MongoDB.
3. [ ] El mensaje del usuario se retransmite a `/tema/mensajes`.
4. [ ] La respuesta IA se genera sin bloquear el retorno inmediato del mensaje del usuario.
5. [ ] La respuesta IA se envía con `SimpMessagingTemplate`.

#### Casos de prueba asociados

`CP-WS-002`, `CP-IA-002`

#### Pruebas unitarias sugeridas

```text
ChatSocketControllerTest
- shouldSaveUserMessage()
- shouldReturnSavedUserMessage()
- shouldTriggerBotResponse()
```

---

### ISSUE-006 — Validar flujo WebSocket en dos pestañas

| Campo | Valor |
|---|---|
| Epic | EPIC-01 |
| Feature | FEATURE-03 — Comunicación en tiempo real con WebSocket |
| RF/RNF | RF-CHAT-04, RF-CHAT-05 |
| HU asociada | HU-WS-003 |
| Responsable | Luis Mejia |
| Tipo | Requisito Técnico |
| Prioridad | 🟡 Media |
| Labels | `test`, `websocket`, `manual` |
| Story Points | 3 |
| Rama sugerida | `issue-006-test-websocket-tabs` |
| Depende de | ISSUE-005, ISSUE-011 |
| Bloquea | ISSUE-012 |

#### Historia de Usuario

Como tester, quiero validar el chat en dos pestañas, para asegurar que los mensajes se transmiten en tiempo real.

#### Encabezado de Código / Guía Técnica

Documentar evidencia en:

```text
docs/evidencias/websocket-dos-pestanas.md
```

#### Criterios de aceptación

1. [ ] Se abren dos pestañas en `/admin/chat`.
2. [ ] Un mensaje enviado desde pestaña 1 aparece en pestaña 2.
3. [ ] Un mensaje enviado desde pestaña 2 aparece en pestaña 1.
4. [ ] No se requiere recargar página para ver mensajes nuevos.

#### Casos de prueba asociados

`CP-WS-003`

#### Pruebas unitarias sugeridas

No reemplaza test unitario. Es prueba funcional/manual de sistema.

---

## FEATURE-04 — Asistente IA con contexto conversacional

Permite que LibroBot IA responda usando el historial reciente guardado en MongoDB.

---

### ISSUE-007 — Implementar BotIAService con Spring AI

| Campo | Valor |
|---|---|
| Epic | EPIC-01 |
| Feature | FEATURE-04 — Asistente IA con contexto conversacional |
| RF/RNF | RF-CHAT-06, RF-CHAT-07 |
| HU asociada | HU-IA-001 |
| Responsable | Alejandro Rodriguez |
| Tipo | Historia de Usuario |
| Prioridad | 🔴 Alta |
| Labels | `ai`, `spring-ai`, `backend`, `feature` |
| Story Points | 5 |
| Rama sugerida | `issue-007-bot-ia-service` |
| Depende de | ISSUE-001, ISSUE-002 |
| Bloquea | ISSUE-005, ISSUE-008, ISSUE-011 |

#### Historia de Usuario

Como bibliotecario, quiero que LibroBot IA responda usando el historial reciente, para obtener ayuda contextual dentro del chat.

#### Encabezado de Código / Guía Técnica

Archivo esperado:

```text
src/main/java/com/librotech/chattech/service/BotIAService.java
```

Dependencias:

```text
ChatClient
MensajeService
```

Método esperado:

```text
generarRespuestaIA(String preguntaUsuario)
```

#### Criterios de aceptación

1. [ ] `BotIAService` usa `ChatClient`.
2. [ ] Obtiene historial reciente desde `MensajeService`.
3. [ ] Construye un prompt con contexto.
4. [ ] Llama al modelo configurado.
5. [ ] Guarda la respuesta como mensaje de `LibroBot IA`.
6. [ ] Retorna el mensaje guardado del bot.

#### Casos de prueba asociados

`CP-IA-001`

#### Pruebas unitarias sugeridas

```text
BotIAServiceTest
- shouldBuildResponseUsingHistory()
- shouldSaveBotMessage()
- shouldHandleEmptyHistory()
```

---

### ISSUE-008 — Limitar contexto enviado al prompt de IA

| Campo | Valor |
|---|---|
| Epic | EPIC-01 |
| Feature | FEATURE-04 — Asistente IA con contexto conversacional |
| RF/RNF | RF-CHAT-07, RNF-02 |
| HU asociada | HU-IA-002 |
| Responsable | Camilo Mitnick |
| Tipo | Requisito Técnico |
| Prioridad | 🟡 Media |
| Labels | `ai`, `performance`, `prompt` |
| Story Points | 3 |
| Rama sugerida | `issue-008-limitar-contexto-ia` |
| Depende de | ISSUE-002, ISSUE-007 |
| Bloquea | ISSUE-012 |

#### Historia de Usuario

Como sistema, quiero limitar el historial enviado al prompt, para evitar problemas de rendimiento, costo y exceso de contexto.

#### Encabezado de Código / Guía Técnica

Ajustar:

```text
MensajeRepository
MensajeService
BotIAService
```

Consulta sugerida:

```text
findTop10ByOrderByFechaEnvioDesc()
```

#### Criterios de aceptación

1. [ ] El prompt no usa `findAll()` para contextos grandes.
2. [ ] El historial enviado a IA se limita a mensajes recientes.
3. [ ] El orden del contexto es comprensible para el modelo.
4. [ ] Existe prueba o validación del límite de mensajes.

#### Casos de prueba asociados

`CP-IA-003`

#### Pruebas unitarias sugeridas

```text
MensajeServiceTest
- shouldReturnOnlyRecentMessages()
```

---

## FEATURE-05 — Arquitectura híbrida REST + MVC

Expone ChatTech como API REST y como interfaz web integrada al panel administrativo existente.

---

### ISSUE-009 — Crear API REST para historial de mensajes

| Campo | Valor |
|---|---|
| Epic | EPIC-01 |
| Feature | FEATURE-05 — Arquitectura híbrida REST + MVC |
| RF/RNF | RF-CHAT-08 |
| HU asociada | HU-REST-001 |
| Responsable | Salvador Aponte |
| Tipo | Historia de Usuario |
| Prioridad | 🟡 Media |
| Labels | `rest`, `api`, `mensajes` |
| Story Points | 3 |
| Rama sugerida | `issue-009-rest-mensajes` |
| Depende de | ISSUE-002 |
| Bloquea | ISSUE-012 |

#### Historia de Usuario

Como bibliotecario o sistema externo, quiero consultar el historial del chat por API REST, para validar los mensajes guardados.

#### Encabezado de Código / Guía Técnica

Archivo esperado:

```text
src/main/java/com/librotech/chattech/controller/rest/MensajeRestController.java
```

Ruta esperada:

```text
GET /api/mensajes
```

#### Criterios de aceptación

1. [ ] Existe controlador REST para `/api/mensajes`.
2. [ ] El endpoint retorna historial desde MongoDB.
3. [ ] La respuesta es JSON.
4. [ ] No afecta `/api/libros` ni `/api/categorias`.

#### Casos de prueba asociados

`CP-REST-001`

#### Pruebas unitarias sugeridas

```text
MensajeRestControllerTest
- shouldReturnMessageHistory()
```

---

### ISSUE-010 — Crear controlador UI para `/admin/chat`

| Campo | Valor |
|---|---|
| Epic | EPIC-01 |
| Feature | FEATURE-05 — Arquitectura híbrida REST + MVC |
| RF/RNF | RF-CHAT-09, RF-CHAT-12 |
| HU asociada | HU-UI-001 |
| Responsable | Luis Mejia |
| Tipo | Historia de Usuario |
| Prioridad | 🔴 Alta |
| Labels | `ui`, `thymeleaf`, `controller` |
| Story Points | 3 |
| Rama sugerida | `issue-010-ui-chat-controller` |
| Depende de | ISSUE-002 |
| Bloquea | ISSUE-011 |

#### Historia de Usuario

Como bibliotecario, quiero abrir la sala de chat desde `/admin/chat`, para participar en la conversación desde el panel administrativo.

#### Encabezado de Código / Guía Técnica

Archivo esperado:

```text
src/main/java/com/librotech/chattech/controller/ui/ChatUIController.java
```

Ruta esperada:

```text
GET /admin/chat
```

Vista esperada:

```text
chat/sala
```

#### Criterios de aceptación

1. [ ] Existe controlador UI en `/admin/chat`.
2. [ ] El controlador consulta historial desde MongoDB.
3. [ ] El historial se inyecta al `Model`.
4. [ ] Retorna la vista `chat/sala`.

#### Casos de prueba asociados

`CP-UI-001`

#### Pruebas unitarias sugeridas

```text
ChatUIControllerTest
- shouldLoadChatViewWithHistory()
```

---

### ISSUE-011 — Crear vista Thymeleaf de sala de chat

| Campo | Valor |
|---|---|
| Epic | EPIC-01 |
| Feature | FEATURE-05 — Arquitectura híbrida REST + MVC |
| RF/RNF | RF-CHAT-09, RF-CHAT-10 |
| HU asociada | HU-UI-002 |
| Responsable | Alejandro Rodriguez |
| Tipo | Historia de Usuario |
| Prioridad | 🔴 Alta |
| Labels | `thymeleaf`, `frontend`, `websocket` |
| Story Points | 3 |
| Rama sugerida | `issue-011-vista-chat-thymeleaf` |
| Depende de | ISSUE-004, ISSUE-007, ISSUE-010 |
| Bloquea | ISSUE-006, ISSUE-012 |

#### Historia de Usuario

Como bibliotecario, quiero ver una sala de chat con mensajes diferenciados entre usuario y bot, para entender la conversación claramente.

#### Encabezado de Código / Guía Técnica

Archivo esperado:

```text
src/main/resources/templates/chat/sala.html
```

Debe reutilizar layout existente:

```text
templates/layout/componentes.html
```

Scripts esperados:

```text
SockJS
stomp.js
```

#### Criterios de aceptación

1. [ ] La vista renderiza historial con `th:each`.
2. [ ] Los mensajes del bot se ven diferente a los del usuario.
3. [ ] El formulario envía mensajes por STOMP a `/app/enviar`.
4. [ ] La vista se suscribe a `/tema/mensajes`.
5. [ ] Se reutiliza o respeta el layout actual de LibroTech.

#### Casos de prueba asociados

`CP-UI-002`, `CP-WS-003`

#### Pruebas unitarias sugeridas

No aplica unitario puro. Validar con prueba MVC o prueba manual.

---

## FEATURE-06 — Integración final y regresión

Asegura que el módulo nuevo funciona sin romper LibroTech.

---

### ISSUE-012 — Ejecutar integración final y regresión de LibroTech + ChatTech

| Campo | Valor |
|---|---|
| Epic | EPIC-01 |
| Feature | FEATURE-06 — Integración final y regresión |
| RF/RNF | RF-LIB-01, RF-LIB-02, RF-LIB-03, RF-CHAT-12, RNF-05 |
| HU asociada | HU-QA-001 |
| Responsable | Camilo Mitnick |
| Tipo | Requisito Técnico |
| Prioridad | 🔴 Alta |
| Labels | `qa`, `integration`, `regression` |
| Story Points | 5 |
| Rama sugerida | `issue-012-integracion-final` |
| Depende de | ISSUE-003, ISSUE-006, ISSUE-008, ISSUE-009, ISSUE-011 |
| Bloquea | Release final |

#### Historia de Usuario

Como equipo de desarrollo, quiero validar LibroTech y ChatTech juntos, para entregar una versión estable en `develop`.

#### Encabezado de Código / Guía Técnica

Validar:

```text
/api/libros
/api/categorias
/admin/libros
/admin/chat
/api/mensajes
/chat-websocket
```

#### Criterios de aceptación

1. [ ] `mvn test` pasa.
2. [ ] `/admin/libros` sigue funcionando.
3. [ ] `/api/libros` sigue funcionando.
4. [ ] `/admin/chat` carga historial.
5. [ ] Dos pestañas reciben mensajes en tiempo real.
6. [ ] LibroBot IA responde.
7. [ ] El historial persiste tras reiniciar la app.
8. [ ] No hay API keys en el repositorio.

#### Casos de prueba asociados

`CP-QA-001`, `CP-QA-002`, `CP-QA-003`

#### Pruebas unitarias sugeridas

Ejecutar suite completa:

```bash
mvn test
```

---

## Definition of Done común

1. [ ] Código compila sin errores ni warnings críticos.
2. [ ] Se respetó la arquitectura `com.librotech.chattech`.
3. [ ] No se rompió funcionalidad existente de LibroTech.
4. [ ] No hay `System.out.println` de debug innecesarios.
5. [ ] No hay claves ni secretos subidos.
6. [ ] Criterios de aceptación completados.
7. [ ] Casos Gherkin asociados ejecutados.
8. [ ] Tests unitarios o de integración del módulo pasan.
9. [ ] Pull Request aprobado por al menos 1 compañero.
10. [ ] Rama mergeada a `develop`.
11. [ ] Issue cerrada y Kanban actualizado.
