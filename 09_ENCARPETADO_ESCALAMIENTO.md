# 09 — Encarpetado de Escalamiento — LibroTech → ChatTech

Este encarpetado parte del proyecto existente. No se crea un proyecto nuevo.

---

## 1. Rama correcta

Crear una rama desde `develop`:

```bash
git checkout develop
git pull origin develop
git checkout -b issue-001-config-chattech-dependencies
```

---

## 2. Carpetas nuevas sugeridas

Desde la raíz del proyecto donde está `pom.xml`:

```bash
mkdir -p src/main/java/com/librotech/chattech/config
mkdir -p src/main/java/com/librotech/chattech/model
mkdir -p src/main/java/com/librotech/chattech/repository
mkdir -p src/main/java/com/librotech/chattech/service
mkdir -p src/main/java/com/librotech/chattech/controller/rest
mkdir -p src/main/java/com/librotech/chattech/controller/ui
mkdir -p src/main/java/com/librotech/chattech/controller/ws

mkdir -p src/main/resources/templates/chat

mkdir -p src/test/java/com/librotech/chattech/service
mkdir -p src/test/java/com/librotech/chattech/repository
mkdir -p src/test/java/com/librotech/chattech/controller
```

---

## 3. Archivos nuevos esperados

```bash
touch src/main/java/com/librotech/chattech/config/WebSocketConfig.java

touch src/main/java/com/librotech/chattech/model/Mensaje.java
touch src/main/java/com/librotech/chattech/repository/MensajeRepository.java

touch src/main/java/com/librotech/chattech/service/MensajeService.java
touch src/main/java/com/librotech/chattech/service/BotIAService.java

touch src/main/java/com/librotech/chattech/controller/ws/ChatSocketController.java
touch src/main/java/com/librotech/chattech/controller/rest/MensajeRestController.java
touch src/main/java/com/librotech/chattech/controller/ui/ChatUIController.java

touch src/main/resources/templates/chat/sala.html

touch src/test/java/com/librotech/chattech/service/MensajeServiceTest.java
touch src/test/java/com/librotech/chattech/service/BotIAServiceTest.java
touch src/test/java/com/librotech/chattech/controller/MensajeRestControllerTest.java
touch src/test/java/com/librotech/chattech/controller/ChatUIControllerTest.java
```

---

## 4. Estructura final esperada

```text
src/main/java/com/librotech/
├── controller/
├── model/
├── repository/
├── service/
├── config/
└── chattech/
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

---

## 5. Commit del encarpetado

```bash
git status
git add src/main/java/com/librotech/chattech
git add src/main/resources/templates/chat
git add src/test/java/com/librotech/chattech
git commit -m "chore(structure): agregar encarpetado base del modulo chattech"
git push origin issue-001-config-chattech-dependencies
```

---

## 6. Pull Request

Base:

```text
develop
```

Compare:

```text
issue-001-config-chattech-dependencies
```

Título:

```text
ISSUE-001 — Configurar base técnica del módulo ChatTech
```
