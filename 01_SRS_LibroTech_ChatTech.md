# 01 — SRS — Especificación de Requisitos — LibroTech + ChatTech

**Proyecto:** LibroTech escalado con módulo ChatTech  
**Versión:** 2.0.0  
**Fecha:** 20/05/2026  
**Equipo:** Salvador Aponte, Alejandro Rodriguez, Camilo Mitnick, Luis Mejia  
**Clasificación:** Académico / Uso interno  

---

## 1. Introducción

### 1.1 Propósito

Este documento define los requisitos del escalamiento del sistema **LibroTech** hacia un módulo de comunicación llamado **ChatTech**.

El objetivo es mantener las funcionalidades existentes de LibroTech y agregar una sala de chat en tiempo real para bibliotecarios, con historial persistente en MongoDB y respuestas automáticas de un asistente IA.

---

### 1.2 Alcance

En esta versión el sistema debe:

1. Mantener el catálogo existente de libros y categorías.
2. Mantener la interfaz administrativa `/admin/libros`.
3. Agregar una sala de chat web en `/admin/chat`.
4. Guardar mensajes de usuarios y bot en MongoDB.
5. Transmitir mensajes en tiempo real usando WebSocket/STOMP.
6. Integrar Spring AI para responder con contexto.
7. Exponer una API REST para consultar el historial de mensajes.
8. Probar el flujo funcional principal.

Lo que el sistema NO hace en esta versión:

1. No implementa autenticación de usuarios.
2. No implementa roles avanzados.
3. No implementa búsqueda vectorial.
4. No implementa múltiples salas de chat.
5. No implementa moderación automática avanzada.
6. No migra el catálogo existente de H2/JPA a MongoDB.

---

### 1.3 Contexto del problema

LibroTech ya permite gestionar libros y categorías. Sin embargo, los bibliotecarios no cuentan con un canal interno en tiempo real para resolver dudas operativas o consultar rápidamente información apoyada por IA.

ChatTech resuelve este problema agregando una sala de comunicación integrada al panel administrativo, donde los mensajes se conservan en MongoDB y el bot puede responder usando el contexto reciente del historial.

---

### 1.4 Supuestos y dependencias

| ID | Supuesto / Dependencia |
|---|---|
| S1 | El proyecto base LibroTech compila correctamente antes del escalamiento. |
| S2 | El equipo cuenta con acceso a GitHub y puede trabajar por ramas. |
| S3 | MongoDB estará disponible localmente o mediante Docker. |
| S4 | La clave de OpenAI se configurará mediante variable de entorno. |
| S5 | El proyecto conserva Spring Boot 3.3.5 y Java 17 salvo decisión explícita de migración. |

---

### 1.5 Restricciones

| ID | Restricción |
|---|---|
| R1 | El módulo nuevo debe integrarse al paquete raíz `com.librotech`. |
| R2 | No se deben romper endpoints existentes de libros y categorías. |
| R3 | `develop` se usará como rama de preproducción. |
| R4 | Las ramas `main` y `develop` deben protegerse contra push directo. |
| R5 | La API key de OpenAI no debe subirse al repositorio. |

---

## 2. Descripción general

### 2.1 Roles del sistema

| Rol | Descripción y permisos |
|---|---|
| Bibliotecario | Usa el panel administrativo, consulta libros y participa en el chat. |
| Sistema | Persiste mensajes, retransmite eventos y coordina la respuesta del bot. |
| LibroBot IA | Asistente virtual que responde usando contexto reciente del chat. |
| Desarrollador | Implementa issues, pruebas, documentación y Pull Requests. |

---

### 2.2 Funciones principales por rol

#### Bibliotecario

1. Consultar catálogo de libros.
2. Crear libros desde el panel administrativo.
3. Entrar a la sala `/admin/chat`.
4. Enviar mensajes al chat.
5. Ver mensajes de otros usuarios en tiempo real.
6. Recibir respuestas de LibroBot IA.

#### Sistema

1. Guardar mensajes en MongoDB.
2. Cargar historial al abrir la sala.
3. Transmitir mensajes con WebSocket.
4. Enviar el historial reciente al servicio de IA.
5. Exponer historial mediante API REST.

#### LibroBot IA

1. Recibir una pregunta del usuario.
2. Leer historial reciente como contexto.
3. Generar respuesta.
4. Guardar respuesta como mensaje.
5. Publicar respuesta en el canal WebSocket.

---

## 3. Requisitos funcionales

### Módulo actual: Catálogo LibroTech

| ID | Requisito | Actor | Prioridad |
|---|---|---|---|
| RF-LIB-01 | El sistema debe mantener el CRUD REST de libros. | Bibliotecario | Alta |
| RF-LIB-02 | El sistema debe mantener el CRUD REST de categorías. | Bibliotecario | Alta |
| RF-LIB-03 | El sistema debe mantener la vista `/admin/libros`. | Bibliotecario | Alta |
| RF-LIB-04 | El sistema debe mantener paginación y ordenamiento en `/api/libros`. | Bibliotecario | Media |

### Módulo nuevo: ChatTech

| ID | Requisito | Actor | Prioridad |
|---|---|---|---|
| RF-CHAT-01 | El sistema debe guardar mensajes de usuario y bot en MongoDB. | Sistema | Alta |
| RF-CHAT-02 | El sistema debe obtener historial reciente de mensajes. | Sistema | Alta |
| RF-CHAT-03 | El sistema debe configurar WebSocket con STOMP y SockJS. | Sistema | Alta |
| RF-CHAT-04 | El sistema debe recibir mensajes mediante `/app/enviar`. | Bibliotecario | Alta |
| RF-CHAT-05 | El sistema debe retransmitir mensajes a `/tema/mensajes`. | Sistema | Alta |
| RF-CHAT-06 | El sistema debe generar respuestas de IA usando Spring AI. | LibroBot IA | Alta |
| RF-CHAT-07 | El sistema debe usar historial reciente como contexto del prompt. | LibroBot IA | Alta |
| RF-CHAT-08 | El sistema debe exponer historial en `/api/mensajes`. | Bibliotecario | Media |
| RF-CHAT-09 | El sistema debe mostrar la sala en `/admin/chat`. | Bibliotecario | Alta |
| RF-CHAT-10 | La vista debe diferenciar mensajes de usuario y bot. | Bibliotecario | Media |
| RF-CHAT-11 | La respuesta de IA debe publicarse dinámicamente por WebSocket. | Sistema | Alta |
| RF-CHAT-12 | El historial debe mantenerse después de reiniciar la aplicación. | Sistema | Alta |

---

## 4. Requisitos no funcionales

| ID | Categoría | Requisito | Criterio de aceptación |
|---|---|---|---|
| RNF-01 | Seguridad | La API key de OpenAI no debe estar escrita en el repositorio. | Se usa `${OPENAI_API_KEY}`. |
| RNF-02 | Rendimiento | El prompt no debe cargar todo MongoDB. | Se limita a los últimos mensajes. |
| RNF-03 | Mantenibilidad | ChatTech debe estar separado del catálogo. | Existe paquete `com.librotech.chattech`. |
| RNF-04 | Disponibilidad | El chat debe funcionar en dos pestañas simultáneas. | Mensaje enviado aparece en ambas pestañas. |
| RNF-05 | Calidad | El proyecto debe compilar antes de hacer merge. | `mvn test` pasa. |
| RNF-06 | Trazabilidad | Cada issue debe mapearse a RF, HU y CP. | Matriz actualizada. |

---

## 5. Reglas de negocio

| ID | Regla |
|---|---|
| RN-01 | Todo mensaje enviado por usuario debe guardarse antes de retransmitirse. |
| RN-02 | Toda respuesta del bot debe guardarse en MongoDB. |
| RN-03 | LibroBot IA solo debe responder usando contexto reciente, no todo el historial. |
| RN-04 | El panel de libros existente no debe verse afectado por ChatTech. |
| RN-05 | Toda integración debe entrar por Pull Request hacia `develop`. |

---

## 6. Modelo de datos

### Entidad relacional existente: Libro

| Campo | Tipo | Restricciones | Descripción |
|---|---|---|---|
| id | Long | PK autoincremental | Identificador del libro |
| titulo | String | Obligatorio, max 150 | Título del libro |
| autor | String | Obligatorio, max 100 | Autor del libro |
| isbn | String | Único, max 20 | Código ISBN |
| anioPublicacion | int | Numérico | Año de publicación |

### Entidad relacional existente: Categoria

| Campo | Tipo | Restricciones | Descripción |
|---|---|---|---|
| id | Long | PK autoincremental | Identificador |
| nombre | String | Obligatorio, único, max 80 | Nombre de la categoría |

### Documento nuevo: Mensaje

| Campo | Tipo | Restricciones | Descripción |
|---|---|---|---|
| id | String | ID MongoDB | Identificador del mensaje |
| remitente | String | Obligatorio | Nombre del usuario o `LibroBot IA` |
| contenido | String | Obligatorio | Texto del mensaje |
| fechaEnvio | LocalDateTime | Se asigna automáticamente | Fecha y hora del envío |

---

## 7. Arquitectura del sistema

```text
Browser
 ├── /admin/libros        → LibroUIController → LibroService → LibroRepository → H2/JPA
 └── /admin/chat          → ChatUIController → MensajeService → MensajeRepository → MongoDB

WebSocket
 └── /app/enviar          → ChatSocketController
                            ├── MensajeService → MongoDB
                            ├── BotIAService → Spring AI/OpenAI
                            └── SimpMessagingTemplate → /tema/mensajes

REST
 ├── /api/libros          → LibroController
 ├── /api/categorias      → CategoriaController
 └── /api/mensajes        → MensajeRestController
```

---

## 8. Glosario

| Término | Definición |
|---|---|
| ChatTech | Módulo de chat inteligente agregado a LibroTech. |
| STOMP | Protocolo de mensajería usado sobre WebSocket. |
| SockJS | Librería para compatibilidad de WebSocket en navegador. |
| Spring AI | Proyecto de Spring para integrar modelos de IA. |
| RAG básico | Técnica donde se recupera información externa y se usa como contexto para generar respuesta. |
| MongoDB | Base de datos documental usada para guardar historial de chat. |
| H2 | Base de datos relacional usada actualmente por LibroTech. |

---

## 9. Control de versiones del documento

| Versión | Fecha | Autor | Descripción |
|---|---|---|---|
| 2.0.0 | 20/05/2026 | Equipo ChatTech | Refactor completo basado en proyecto LibroTech existente. |
