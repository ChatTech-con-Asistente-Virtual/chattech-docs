# Documentación — Escalamiento LibroTech → ChatTech

Este repositorio documenta la evolución del proyecto **LibroTech** hacia el módulo **ChatTech**, una sala de chat en tiempo real con historial persistente en MongoDB y asistente IA usando Spring AI.

## Proyecto base detectado

El ZIP entregado contiene un proyecto Spring Boot existente con:

- Paquete raíz: `com.librotech`
- Spring Boot: `3.3.5`
- Java: `17`
- Persistencia actual: H2 + Spring Data JPA
- Entidades actuales: `Libro`, `Categoria`
- API REST actual:
  - `/api/libros`
  - `/api/categorias`
- UI actual con Thymeleaf:
  - `/admin/libros`
  - `templates/libros/lista.html`
  - `templates/libros/formulario.html`
  - `templates/layout/componentes.html`
- Pruebas actuales:
  - `LibroRepositoryTest`
  - `CategoriaRepositoryTest`
  - `LibroTechApplicationTests`

## Decisión documental importante

ChatTech **no se documenta como una aplicación desde cero**.  
Se documenta como un **módulo de escalamiento** dentro de LibroTech.

Por eso, la estructura técnica sugerida conserva el paquete raíz `com.librotech` y agrega el módulo:

```text
com.librotech.chattech
```

Ejemplo:

```text
src/main/java/com/librotech/chattech/model/Mensaje.java
src/main/java/com/librotech/chattech/service/BotIAService.java
src/main/java/com/librotech/chattech/controller/ws/ChatSocketController.java
```

## Archivos incluidos

```text
00_CONTEXTO_ESCALAMIENTO_LibroTech_ChatTech.md
01_SRS_LibroTech_ChatTech.md
02_EPICS_FEATURES_HU_ISSUES_LibroTech_ChatTech.md
03_CASOS_PRUEBA_GHERKIN_LibroTech_ChatTech.md
04_PLAN_PRUEBAS_LibroTech_ChatTech.md
05_REGISTRO_RIESGOS_LibroTech_ChatTech.md
06_MATRIZ_TRAZABILIDAD_LibroTech_ChatTech.md
07_SEGURIDAD_ORGANIZACION_GITHUB.md
08_FLUJO_GIT_CONVENCIONES.md
09_ENCARPETADO_ESCALAMIENTO.md
```
