# 03 — Casos de Prueba Gherkin — LibroTech + ChatTech

**Proyecto:** LibroTech escalado con ChatTech  
**Versión:** 2.0.0  
**Fecha:** 20/05/2026  

---

## Nota sobre Gherkin y pruebas unitarias

Los casos Gherkin documentan comportamiento esperado desde el punto de vista del usuario o del sistema.

No reemplazan las pruebas unitarias.

- Gherkin valida aceptación y comportamiento.
- Tests unitarios validan clases y métodos en aislamiento.
- Tests de integración validan interacción entre capas.

---

## Módulo Configuración

### CP-CONFIG-001 — Configuración segura de dependencias y variables

Asociado a: `ISSUE-001`

```gherkin
Feature: Configuración base de ChatTech
  Como desarrollador
  Quiero configurar dependencias y variables seguras
  Para integrar ChatTech sin romper LibroTech

  Scenario: El proyecto compila después de agregar dependencias
    Given el proyecto LibroTech existente
    When se agregan dependencias de MongoDB, WebSocket y Spring AI
    Then el proyecto debe compilar correctamente
    And los tests existentes deben seguir pasando

  Scenario: La API key no está escrita en application.properties
    Given el archivo application.properties
    When reviso la configuración de OpenAI
    Then la API key debe leerse desde OPENAI_API_KEY
    And no debe existir una clave real escrita en el repositorio
```

---

## Módulo Mensajes MongoDB

### CP-MSG-001 — Guardar mensaje en MongoDB

Asociado a: `ISSUE-002`

```gherkin
Feature: Persistencia de mensajes
  Como sistema
  Quiero guardar mensajes en MongoDB
  Para conservar historial del chat

  Scenario: Guardar mensaje válido
    Given un mensaje con remitente "Luis" y contenido "Hola"
    When el sistema guarda el mensaje
    Then el mensaje debe quedar persistido en MongoDB
    And debe tener fecha de envío

  Scenario: Rechazar mensaje sin contenido
    Given un mensaje con remitente "Luis" y contenido vacío
    When el sistema intenta guardar el mensaje
    Then debe rechazar el mensaje
    And no debe persistirse en MongoDB
```

### CP-MSG-002 — Consultar historial reciente

Asociado a: `ISSUE-002`, `ISSUE-008`

```gherkin
Feature: Historial reciente
  Como sistema
  Quiero consultar mensajes recientes
  Para construir contexto para la IA

  Scenario: Obtener últimos mensajes
    Given existen mensajes guardados en MongoDB
    When el sistema consulta el historial reciente
    Then debe retornar una lista de mensajes
    And debe estar limitada a una cantidad razonable

  Scenario: Historial vacío
    Given no existen mensajes guardados
    When el sistema consulta el historial
    Then debe retornar una lista vacía
    And no debe lanzar error
```

### CP-MSG-003 — Pruebas de repositorio y servicio

Asociado a: `ISSUE-003`

```gherkin
Feature: Calidad de persistencia
  Como desarrollador
  Quiero probar repository y service
  Para asegurar la integridad del módulo de mensajes

  Scenario: Ejecutar pruebas de persistencia
    Given existen tests para MensajeRepository y MensajeService
    When ejecuto mvn test
    Then las pruebas deben pasar
    And las pruebas existentes de Libro y Categoria no deben fallar
```

---

## Módulo WebSocket

### CP-WS-001 — Configuración WebSocket

Asociado a: `ISSUE-004`

```gherkin
Feature: Configuración WebSocket
  Como sistema
  Quiero habilitar STOMP y SockJS
  Para permitir comunicación en tiempo real

  Scenario: Registrar endpoint del chat
    Given la aplicación está iniciada
    When el cliente intenta conectarse a /chat-websocket
    Then la conexión debe estar disponible
```

### CP-WS-002 — Enviar mensaje por WebSocket

Asociado a: `ISSUE-005`

```gherkin
Feature: Envío de mensajes en tiempo real
  Como bibliotecario
  Quiero enviar mensajes al chat
  Para comunicarme con otros usuarios

  Scenario: Enviar mensaje válido
    Given el usuario está conectado al chat
    When envía "Hola equipo" a /app/enviar
    Then el mensaje debe guardarse
    And debe publicarse en /tema/mensajes

  Scenario: Mensaje vacío
    Given el usuario está conectado al chat
    When intenta enviar un mensaje vacío
    Then el sistema no debería guardar contenido inválido
```

### CP-WS-003 — Dos pestañas reciben mensajes

Asociado a: `ISSUE-006`, `ISSUE-011`

```gherkin
Feature: Chat en dos pestañas
  Como bibliotecario
  Quiero ver mensajes en tiempo real
  Para confirmar que el chat funciona sin recargar

  Scenario: Mensaje aparece en ambas pestañas
    Given tengo dos pestañas abiertas en /admin/chat
    When envío un mensaje desde la pestaña 1
    Then el mensaje aparece en la pestaña 1
    And el mensaje aparece en la pestaña 2
```

---

## Módulo IA

### CP-IA-001 — Bot responde usando historial

Asociado a: `ISSUE-007`

```gherkin
Feature: Respuesta del bot con contexto
  Como bibliotecario
  Quiero que LibroBot IA responda usando el historial
  Para recibir ayuda contextual

  Scenario: Generar respuesta con historial
    Given existen mensajes recientes en MongoDB
    When el usuario pregunta algo en el chat
    Then LibroBot IA debe generar una respuesta
    And la respuesta debe considerar el historial reciente

  Scenario: Generar respuesta sin historial
    Given no existen mensajes anteriores
    When el usuario pregunta algo
    Then LibroBot IA debe responder de forma general
    And no debe fallar por historial vacío
```

### CP-IA-002 — Publicar respuesta IA por WebSocket

Asociado a: `ISSUE-005`, `ISSUE-007`

```gherkin
Feature: Publicación de respuesta IA
  Como usuario del chat
  Quiero recibir la respuesta del bot dinámicamente
  Para no recargar la página

  Scenario: Respuesta del bot aparece en el canal
    Given el usuario envía una pregunta
    When LibroBot IA genera respuesta
    Then la respuesta debe guardarse
    And debe enviarse a /tema/mensajes
```

### CP-IA-003 — Limitar contexto del prompt

Asociado a: `ISSUE-008`

```gherkin
Feature: Contexto limitado de IA
  Como sistema
  Quiero limitar el contexto enviado al modelo
  Para evitar problemas de rendimiento

  Scenario: Usar solo mensajes recientes
    Given existen muchos mensajes en MongoDB
    When BotIAService construye el prompt
    Then solo debe incluir mensajes recientes
    And no debe enviar todo el historial
```

---

## Módulo REST y UI

### CP-REST-001 — Consultar historial por API REST

Asociado a: `ISSUE-009`

```gherkin
Feature: API REST de mensajes
  Como bibliotecario
  Quiero consultar mensajes por API
  Para verificar el historial guardado

  Scenario: Consultar historial
    Given existen mensajes en MongoDB
    When hago GET /api/mensajes
    Then recibo HTTP 200
    And recibo una lista JSON de mensajes
```

### CP-UI-001 — Cargar vista /admin/chat

Asociado a: `ISSUE-010`

```gherkin
Feature: Vista administrativa del chat
  Como bibliotecario
  Quiero abrir /admin/chat
  Para acceder a la sala de conversación

  Scenario: Cargar sala con historial
    Given existen mensajes guardados
    When accedo a /admin/chat
    Then veo la vista de sala
    And veo el historial renderizado
```

### CP-UI-002 — Diferenciar mensajes de usuario y bot

Asociado a: `ISSUE-011`

```gherkin
Feature: Diferenciación visual
  Como bibliotecario
  Quiero distinguir mensajes del bot y del usuario
  Para entender mejor la conversación

  Scenario: Mostrar estilos diferentes
    Given hay mensajes de usuario y de LibroBot IA
    When se renderiza la sala
    Then los mensajes del bot tienen estilo diferente
    And los mensajes del usuario tienen estilo diferente
```

---

## Integración final

### CP-QA-001 — Regresión LibroTech

Asociado a: `ISSUE-012`

```gherkin
Feature: Regresión del módulo existente
  Como equipo
  Quiero validar LibroTech después del escalamiento
  Para asegurar que ChatTech no rompió funcionalidades existentes

  Scenario: Validar panel de libros
    Given la aplicación está iniciada
    When accedo a /admin/libros
    Then el catálogo debe cargar correctamente

  Scenario: Validar API de libros
    Given la aplicación está iniciada
    When hago GET /api/libros
    Then recibo una respuesta exitosa
```

### CP-QA-002 — Flujo completo ChatTech

Asociado a: `ISSUE-012`

```gherkin
Feature: Flujo completo de ChatTech
  Como bibliotecario
  Quiero usar el chat con IA
  Para validar la entrega final

  Scenario: Chat con persistencia e IA
    Given MongoDB está activo
    And OPENAI_API_KEY está configurada
    When abro dos pestañas en /admin/chat
    And envío un mensaje
    Then el mensaje aparece en ambas pestañas
    And LibroBot IA responde
    And la respuesta queda guardada
```

### CP-QA-003 — Persistencia tras reinicio

Asociado a: `ISSUE-012`

```gherkin
Feature: Persistencia de historial
  Como bibliotecario
  Quiero conservar mensajes tras reinicio
  Para no perder el historial del chat

  Scenario: Recargar historial después del reinicio
    Given existen mensajes guardados en MongoDB
    When reinicio la aplicación
    And vuelvo a entrar en /admin/chat
    Then el historial anterior debe mostrarse
```
