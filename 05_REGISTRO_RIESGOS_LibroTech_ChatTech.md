# 05 — Registro de Riesgos — LibroTech + ChatTech

**Proyecto:** LibroTech escalado con ChatTech  
**Versión:** 2.0.0  
**Fecha:** 20/05/2026  

---

## Escala

| Valor | Probabilidad | Impacto |
|---|---|---|
| 1 | Baja | Bajo |
| 2 | Media | Medio |
| 3 | Alta | Alto |

| Nivel | Rango | Acción |
|---|---|---|
| 🟢 Bajo | 1-2 | Monitorear |
| 🟡 Medio | 3-4 | Mitigar |
| 🔴 Alto | 6-9 | Acción inmediata |

---

## Registro

| ID | Categoría | Riesgo | P | I | Nivel | Mitigación | Dueño |
|---|---|---|---:|---:|---:|---|---|
| R-001 | Técnico | Romper el proyecto base al mover paquetes o renombrar `com.librotech`. | 2 | 3 | 6 🔴 | Mantener `com.librotech` y agregar subpaquete `chattech`. | Luis Mejia |
| R-002 | Integración | Dependencias de Spring AI incompatibles con la versión base. | 2 | 3 | 6 🔴 | Probar primero compilación en ISSUE-001. | Salvador Aponte |
| R-003 | Seguridad | Subir la API key de OpenAI al repositorio. | 2 | 3 | 6 🔴 | Usar `${OPENAI_API_KEY}`, `.gitignore` y revisión de PR. | Salvador Aponte |
| R-004 | Tiempo | El proyecto debe quedar para mañana. | 3 | 3 | 9 🔴 | Dividir en 12 issues, 3 por integrante. | Equipo |
| R-005 | Rendimiento | Enviar todo el historial de MongoDB al prompt. | 2 | 3 | 6 🔴 | Limitar a últimos mensajes en ISSUE-008. | Camilo Mitnick |
| R-006 | Calidad | No implementar tests unitarios y confundirlos con Gherkin. | 2 | 2 | 4 🟡 | Documentar ambos y asignar pruebas por módulo. | Alejandro Rodriguez |
| R-007 | Integración | WebSocket funciona pero la UI no se suscribe correctamente. | 2 | 2 | 4 🟡 | Validar con dos pestañas y evidencia. | Luis Mejia |
| R-008 | Datos | MongoDB no está disponible localmente. | 2 | 2 | 4 🟡 | Usar Docker o instalación local validada. | Camilo Mitnick |
| R-009 | Regresión | El módulo de libros deja de funcionar. | 1 | 3 | 3 🟡 | Ejecutar pruebas existentes y validar `/admin/libros`. | Camilo Mitnick |
| R-010 | Alcance | Intentar agregar autenticación o vector search sin tiempo. | 2 | 2 | 4 🟡 | Mantener fuera de alcance para esta entrega. | Equipo |
