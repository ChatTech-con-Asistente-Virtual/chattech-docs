# 04 — Plan de Pruebas — LibroTech + ChatTech

**Proyecto:** LibroTech escalado con ChatTech  
**Versión:** 2.0.0  
**Fecha:** 20/05/2026  

---

## 1. Alcance de pruebas

### En alcance

1. Regresión del módulo existente de libros y categorías.
2. Persistencia de mensajes en MongoDB.
3. Servicio de historial reciente.
4. BotIAService con contexto.
5. Controlador WebSocket.
6. Vista Thymeleaf `/admin/chat`.
7. API REST `/api/mensajes`.
8. Flujo completo con dos pestañas.
9. Seguridad básica de variables de entorno.

### Fuera de alcance

1. Autenticación.
2. Roles y permisos.
3. Búsqueda vectorial.
4. Pruebas de carga masivas.
5. Despliegue cloud.

---

## 2. Tipos de prueba

| Tipo | Descripción | Herramienta | Responsable |
|---|---|---|---|
| Unitarias | Prueban servicios/controladores en aislamiento | JUnit 5, Mockito | Cada responsable |
| Repositorio | Prueban acceso a datos JPA/Mongo | `@DataJpaTest`, `@DataMongoTest` | Alejandro / Luis |
| Integración | Validan interacción entre capas | Spring Boot Test | Camilo |
| Sistema manual | Validan dos pestañas, IA y persistencia | Navegador/Postman | Equipo |
| Regresión | Validan que LibroTech no se rompió | Maven + navegador | Camilo |
| Seguridad | Verifican que no haya secretos en repo | Revisión manual/GitHub | Salvador |

---

## 3. Ambiente de pruebas

| Parámetro | Valor |
|---|---|
| Sistema operativo | Ubuntu / Windows / macOS |
| Lenguaje | Java 17 |
| Framework | Spring Boot 3.3.5 base del proyecto |
| Base relacional actual | H2 file database |
| Base documental nueva | MongoDB |
| IA | Spring AI + OpenAI |
| Comando pruebas | `mvn test` |
| Ejecución app | `mvn spring-boot:run` |

---

## 4. Pruebas existentes que deben seguir pasando

```text
LibroTechApplicationTests
LibroRepositoryTest
CategoriaRepositoryTest
```

Estas pruebas validan que el escalamiento no rompa el módulo base.

---

## 5. Pruebas unitarias sugeridas por módulo

### MensajeServiceTest

| Prueba | Objetivo |
|---|---|
| `shouldSaveMessage()` | Verificar que guarda un mensaje válido. |
| `shouldReturnHistory()` | Verificar que retorna historial. |
| `shouldReturnRecentHistoryLimited()` | Verificar límite de mensajes recientes. |
| `shouldRejectEmptyContent()` | Verificar validación de contenido vacío. |

### BotIAServiceTest

| Prueba | Objetivo |
|---|---|
| `shouldGenerateResponseUsingHistory()` | Verificar que usa historial como contexto. |
| `shouldSaveBotMessage()` | Verificar que guarda respuesta del bot. |
| `shouldHandleEmptyHistory()` | Verificar comportamiento sin historial. |

### MensajeRestControllerTest

| Prueba | Objetivo |
|---|---|
| `shouldReturnMessageHistory()` | Verificar `GET /api/mensajes`. |

### ChatUIControllerTest

| Prueba | Objetivo |
|---|---|
| `shouldLoadChatViewWithHistory()` | Verificar que `/admin/chat` carga vista e historial. |

### ChatSocketControllerTest

| Prueba | Objetivo |
|---|---|
| `shouldSaveUserMessage()` | Verificar guardado desde WebSocket. |
| `shouldReturnSavedMessage()` | Verificar mensaje devuelto al canal. |
| `shouldTriggerBotResponse()` | Verificar llamada al servicio IA. |

---

## 6. Relación entre Gherkin y tests unitarios

| Tipo | Archivo | Propósito |
|---|---|---|
| Gherkin | `03_CASOS_PRUEBA_GHERKIN` | Describe comportamiento esperado. |
| Unit test | `src/test/java/...` | Verifica clases/métodos. |
| Integration test | `src/test/java/...` | Verifica interacción entre componentes. |
| Manual evidence | `docs/evidencias/` | Evidencia visual o pasos de ejecución. |

Los casos Gherkin **sí están en la documentación**, pero los tests unitarios deben implementarse en el código.

---

## 7. Criterios de entrada

1. [ ] Rama creada desde `develop`.
2. [ ] Issue asignada.
3. [ ] Dependencias configuradas.
4. [ ] MongoDB disponible.
5. [ ] `OPENAI_API_KEY` configurada cuando se pruebe IA.

---

## 8. Criterios de salida

1. [ ] `mvn test` pasa.
2. [ ] `/admin/libros` sigue funcionando.
3. [ ] `/api/libros` sigue funcionando.
4. [ ] `/admin/chat` funciona.
5. [ ] Mensajes se guardan en MongoDB.
6. [ ] WebSocket transmite en tiempo real.
7. [ ] LibroBot IA responde.
8. [ ] No hay secretos en el repo.
9. [ ] PR aprobado y mergeado a `develop`.

---

## 9. Matriz de pruebas

| ID CP | Módulo | Tipo | Prioridad | Estado |
|---|---|---|---|---|
| CP-CONFIG-001 | Configuración | Integración | Alta | ⬜ Pendiente |
| CP-MSG-001 | MongoDB | Unitaria/Integración | Alta | ⬜ Pendiente |
| CP-MSG-002 | MongoDB | Unitaria | Alta | ⬜ Pendiente |
| CP-MSG-003 | MongoDB | Test suite | Media | ⬜ Pendiente |
| CP-WS-001 | WebSocket | Integración | Alta | ⬜ Pendiente |
| CP-WS-002 | WebSocket | Unitaria/Integración | Alta | ⬜ Pendiente |
| CP-WS-003 | WebSocket | Sistema manual | Alta | ⬜ Pendiente |
| CP-IA-001 | IA | Unitaria/Integración | Alta | ⬜ Pendiente |
| CP-IA-002 | IA/WebSocket | Integración | Alta | ⬜ Pendiente |
| CP-IA-003 | IA | Unitaria | Media | ⬜ Pendiente |
| CP-REST-001 | REST | Controller test | Media | ⬜ Pendiente |
| CP-UI-001 | UI | MVC test | Alta | ⬜ Pendiente |
| CP-UI-002 | UI | Sistema manual | Media | ⬜ Pendiente |
| CP-QA-001 | Regresión | Sistema | Alta | ⬜ Pendiente |
| CP-QA-002 | Integración final | Sistema | Alta | ⬜ Pendiente |
| CP-QA-003 | Persistencia | Sistema | Alta | ⬜ Pendiente |
