# 06 — Matriz de Trazabilidad — LibroTech + ChatTech

**Proyecto:** LibroTech escalado con ChatTech  
**Versión:** 2.0.0  
**Fecha:** 20/05/2026  

---

## Leyenda

```text
⬜ Pendiente
🔧 En desarrollo
🧪 En prueba
✅ Completo
```

---

## Módulo existente: LibroTech

| RF | Requisito | Feature | HU | Issue | CP | Responsable | Estado |
|---|---|---|---|---|---|---|---|
| RF-LIB-01 | Mantener CRUD REST de libros | FEATURE-06 | HU-QA-001 | ISSUE-012 | CP-QA-001 | Camilo Mitnick | ⬜ Pendiente |
| RF-LIB-02 | Mantener CRUD REST de categorías | FEATURE-06 | HU-QA-001 | ISSUE-012 | CP-QA-001 | Camilo Mitnick | ⬜ Pendiente |
| RF-LIB-03 | Mantener vista `/admin/libros` | FEATURE-06 | HU-QA-001 | ISSUE-012 | CP-QA-001 | Camilo Mitnick | ⬜ Pendiente |
| RF-LIB-04 | Mantener paginación de libros | FEATURE-06 | HU-QA-001 | ISSUE-012 | CP-QA-001 | Camilo Mitnick | ⬜ Pendiente |

---

## Módulo nuevo: ChatTech

| RF/RNF | Requisito | Feature | HU | Issue | CP | Responsable | Rama | Estado |
|---|---|---|---|---|---|---|---|---|
| RNF-01 | API key fuera del repositorio | FEATURE-01 | HU-CONFIG-001 | ISSUE-001 | CP-CONFIG-001 | Salvador Aponte | `issue-001-config-chattech-dependencies` | ⬜ Pendiente |
| RNF-03 | Separar ChatTech en subpaquete | FEATURE-01 | HU-CONFIG-001 | ISSUE-001 | CP-CONFIG-001 | Salvador Aponte | `issue-001-config-chattech-dependencies` | ⬜ Pendiente |
| RF-CHAT-01 | Guardar mensajes en MongoDB | FEATURE-02 | HU-MSG-001 | ISSUE-002 | CP-MSG-001 | Luis Mejia | `issue-002-mongodb-mensajes` | ⬜ Pendiente |
| RF-CHAT-02 | Consultar historial reciente | FEATURE-02 | HU-MSG-001 | ISSUE-002 | CP-MSG-002 | Luis Mejia | `issue-002-mongodb-mensajes` | ⬜ Pendiente |
| RF-CHAT-01 | Probar persistencia de mensajes | FEATURE-02 | HU-MSG-002 | ISSUE-003 | CP-MSG-003 | Alejandro Rodriguez | `issue-003-tests-mongodb-chat` | ⬜ Pendiente |
| RF-CHAT-03 | Configurar WebSocket STOMP | FEATURE-03 | HU-WS-001 | ISSUE-004 | CP-WS-001 | Camilo Mitnick | `issue-004-websocket-config` | ⬜ Pendiente |
| RF-CHAT-04 | Recibir mensaje por `/app/enviar` | FEATURE-03 | HU-WS-002 | ISSUE-005 | CP-WS-002 | Salvador Aponte | `issue-005-chat-socket-controller` | ⬜ Pendiente |
| RF-CHAT-05 | Retransmitir a `/tema/mensajes` | FEATURE-03 | HU-WS-002 | ISSUE-005 | CP-WS-002 | Salvador Aponte | `issue-005-chat-socket-controller` | ⬜ Pendiente |
| RF-CHAT-05 | Validar dos pestañas | FEATURE-03 | HU-WS-003 | ISSUE-006 | CP-WS-003 | Luis Mejia | `issue-006-test-websocket-tabs` | ⬜ Pendiente |
| RF-CHAT-06 | Generar respuesta con Spring AI | FEATURE-04 | HU-IA-001 | ISSUE-007 | CP-IA-001 | Alejandro Rodriguez | `issue-007-bot-ia-service` | ⬜ Pendiente |
| RF-CHAT-07 | Usar historial como contexto | FEATURE-04 | HU-IA-001 | ISSUE-007 | CP-IA-001 | Alejandro Rodriguez | `issue-007-bot-ia-service` | ⬜ Pendiente |
| RNF-02 | Limitar contexto del prompt | FEATURE-04 | HU-IA-002 | ISSUE-008 | CP-IA-003 | Camilo Mitnick | `issue-008-limitar-contexto-ia` | ⬜ Pendiente |
| RF-CHAT-08 | API REST `/api/mensajes` | FEATURE-05 | HU-REST-001 | ISSUE-009 | CP-REST-001 | Salvador Aponte | `issue-009-rest-mensajes` | ⬜ Pendiente |
| RF-CHAT-09 | UI Controller `/admin/chat` | FEATURE-05 | HU-UI-001 | ISSUE-010 | CP-UI-001 | Luis Mejia | `issue-010-ui-chat-controller` | ⬜ Pendiente |
| RF-CHAT-10 | Vista diferencia usuario/bot | FEATURE-05 | HU-UI-002 | ISSUE-011 | CP-UI-002 | Alejandro Rodriguez | `issue-011-vista-chat-thymeleaf` | ⬜ Pendiente |
| RF-CHAT-11 | Respuesta IA por WebSocket | FEATURE-03/04 | HU-WS-002 | ISSUE-005 | CP-IA-002 | Salvador Aponte | `issue-005-chat-socket-controller` | ⬜ Pendiente |
| RF-CHAT-12 | Historial persiste tras reinicio | FEATURE-06 | HU-QA-001 | ISSUE-012 | CP-QA-003 | Camilo Mitnick | `issue-012-integracion-final` | ⬜ Pendiente |

---

## Resumen de cobertura

| Módulo | Total RF/RNF trazados | Pendientes | En dev/prueba | Completos |
|---|---:|---:|---:|---:|
| LibroTech existente | 4 | 4 | 0 | 0 |
| ChatTech nuevo | 17 | 17 | 0 | 0 |
| Total | 21 | 21 | 0 | 0 |
