# 08 — Flujo Git y Convenciones — LibroTech + ChatTech

---

## 1. Ramas principales

```text
main       → rama estable
develop    → rama de preproducción
issue-*    → ramas de trabajo
```

---

## 2. Regla principal

```text
No trabajar directo en main.
No trabajar directo en develop.
Todo cambio entra por Pull Request.
```

---

## 3. Convención de ramas

Formato:

```text
issue-<numero>-<modulo>-<descripcion-corta>
```

Ejemplos:

```text
issue-001-config-chattech-dependencies
issue-002-mongodb-mensajes
issue-003-tests-mongodb-chat
issue-004-websocket-config
issue-005-chat-socket-controller
issue-006-test-websocket-tabs
issue-007-bot-ia-service
issue-008-limitar-contexto-ia
issue-009-rest-mensajes
issue-010-ui-chat-controller
issue-011-vista-chat-thymeleaf
issue-012-integracion-final
```

---

## 4. Flujo para iniciar una issue

```bash
git checkout develop
git pull origin develop
git checkout -b issue-002-mongodb-mensajes
```

---

## 5. Convención de commits

Formato:

```text
<tipo>(<alcance>): <descripcion corta>
```

Tipos:

| Tipo | Uso |
|---|---|
| feat | Nueva funcionalidad |
| fix | Corrección |
| docs | Documentación |
| test | Pruebas |
| refactor | Reorganización sin cambio funcional |
| chore | Mantenimiento |
| config | Configuración |

Scopes recomendados:

```text
config
mongodb
websocket
ai
rest
ui
thymeleaf
tests
docs
security
```

Ejemplos:

```bash
git commit -m "config(chattech): agregar dependencias de mongodb websocket y spring ai"
git commit -m "feat(mongodb): agregar documento repositorio y servicio de mensajes"
git commit -m "feat(websocket): crear controlador de chat en tiempo real"
git commit -m "feat(ai): crear servicio de respuesta con contexto"
git commit -m "test(mongodb): agregar pruebas unitarias de mensaje service"
```

---

## 6. Pull Request

Base:

```text
develop
```

Compare:

```text
issue-xxx-descripcion
```

Título:

```text
ISSUE-002 — Implementar persistencia de mensajes con MongoDB
```

Descripción:

```markdown
## Resumen
Implementa la ISSUE-XXX.

## Cambios
- [ ] Cambio 1
- [ ] Cambio 2

## Pruebas
- [ ] mvn test
- [ ] prueba manual si aplica

## Issue relacionada
Closes #NUMERO
```

---

## 7. Kanban

Columnas:

```text
Backlog
Ready
In Progress
In Review
Testing
Done
```

Reglas:

1. Una issue pasa a `In Progress` cuando alguien crea la rama.
2. Pasa a `In Review` cuando abre PR.
3. Pasa a `Testing` cuando el PR fue revisado o está listo para validar.
4. Pasa a `Done` después del merge y cierre de issue.
