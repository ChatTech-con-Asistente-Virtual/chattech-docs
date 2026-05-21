# 00 — Contexto de Escalamiento — LibroTech → ChatTech

**Proyecto base:** LibroTech  
**Módulo nuevo:** ChatTech  
**Versión documental:** 2.0.0  
**Fecha:** 20/05/2026  
**Equipo:** Salvador Aponte, Alejandro Rodriguez, Camilo Mitnick, Luis Mejia  
**Clasificación:** Académico / Uso interno  

---

## 1. Situación actual del proyecto

LibroTech ya cuenta con una base funcional desarrollada en Spring Boot.

Actualmente el sistema permite:

- Gestionar libros por API REST.
- Gestionar categorías por API REST.
- Consultar libros con paginación y ordenamiento.
- Usar una interfaz web administrativa con Thymeleaf.
- Persistir datos relacionales con H2 + JPA.
- Visualizar documentación técnica mediante Swagger.
- Ejecutar pruebas de repositorio con `@DataJpaTest`.

---

## 2. Estructura actual detectada

```text
src/main/java/com/librotech/
├── LibroTechApplication.java
├── config/
│   └── DataSeed.java
├── controller/
│   ├── LibroController.java
│   ├── CategoriaController.java
│   ├── GlobalExceptionHandler.java
│   └── ui/
│       └── LibroUIController.java
├── model/
│   ├── Libro.java
│   └── Categoria.java
├── repository/
│   ├── LibroRepository.java
│   └── CategoriaRepository.java
└── service/
    ├── LibroService.java
    └── CategoriaService.java
```

```text
src/main/resources/
├── application.properties
└── templates/
    ├── libros/
    │   ├── lista.html
    │   └── formulario.html
    └── layout/
        └── componentes.html
```

---

## 3. Nuevo objetivo del escalamiento

Agregar un módulo llamado **ChatTech** para permitir comunicación en tiempo real entre bibliotecarios, con un asistente IA capaz de responder usando el historial del chat como contexto.

El módulo ChatTech debe integrar:

- MongoDB para historial de mensajes.
- WebSocket + STOMP + SockJS para comunicación en tiempo real.
- Thymeleaf para la vista `/admin/chat`.
- API REST para consultar historial.
- Spring AI + OpenAI para generar respuestas automáticas.
- Pruebas unitarias, integración y aceptación.

---

## 4. Decisión de arquitectura

No se recomienda renombrar todo el proyecto a `com.chattech`, porque el proyecto base ya está organizado como `com.librotech`.

La decisión recomendada es mantener el paquete raíz:

```text
com.librotech
```

Y agregar el módulo nuevo como subpaquete:

```text
com.librotech.chattech
```

Esto evita romper el código existente y permite que Spring Boot detecte los nuevos componentes por component scan.

---

## 5. Arquitectura resultante

```text
LibroTech
├── Módulo catálogo existente
│   ├── Libro
│   ├── Categoria
│   ├── API REST
│   ├── UI Thymeleaf
│   └── H2/JPA
│
└── Módulo ChatTech nuevo
    ├── Mensaje
    ├── MongoDB
    ├── WebSocket/STOMP
    ├── UI Thymeleaf
    ├── API REST de historial
    └── Spring AI
```

---

## 6. Nueva estructura sugerida

```text
src/main/java/com/librotech/chattech/
├── config/
│   └── WebSocketConfig.java
├── controller/
│   ├── rest/
│   │   └── MensajeRestController.java
│   ├── ui/
│   │   └── ChatUIController.java
│   └── ws/
│       └── ChatSocketController.java
├── model/
│   └── Mensaje.java
├── repository/
│   └── MensajeRepository.java
└── service/
    ├── MensajeService.java
    └── BotIAService.java
```

```text
src/main/resources/templates/chat/
└── sala.html
```

---

## 7. Restricciones del escalamiento

1. No romper los endpoints existentes de libros y categorías.
2. No eliminar el panel `/admin/libros`.
3. No subir claves de OpenAI al repositorio.
4. No mezclar lógica de catálogo con lógica de chat.
5. No hacer push directo a `main` ni a `develop`.
6. Mantener `develop` como rama de preproducción.
7. Integrar cambios mediante Pull Request.
