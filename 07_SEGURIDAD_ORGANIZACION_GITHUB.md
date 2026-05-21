# 07 — Seguridad de Organización GitHub — LibroTech + ChatTech

**Objetivo:** proteger la organización, los repositorios y las ramas principales durante el desarrollo.

---

## 1. Repositorios

```text
librotech-docs
librotech-app
```

O si la organización ya usa otros nombres:

```text
chattech-docs
chattech-app
```

---

## 2. Equipos recomendados

| Equipo | Integrantes | Permiso |
|---|---|---|
| Admins | Luis Mejia, Salvador Aponte | Admin/Maintain |
| Developers | Salvador, Alejandro, Camilo, Luis | Write |

Evitar que todos tengan permisos de Admin si no es necesario.

---

## 3. Ramas protegidas

Proteger:

```text
main
develop
```

`main` es producción/entrega estable.  
`develop` es preproducción/integración.

---

## 4. Reglas para `main`

Activar:

```text
Require a pull request before merging
Require approvals: 1
Dismiss stale approvals
Require conversation resolution
Block force pushes
Restrict deletions
Do not allow bypassing settings
```

Regla:

```text
Nadie trabaja directamente en main.
main solo recibe merge desde develop.
```

---

## 5. Reglas para `develop`

Activar:

```text
Require a pull request before merging
Require approvals: 1
Require conversation resolution
Block force pushes
Restrict deletions
```

Regla:

```text
Nadie trabaja directamente en develop.
Cada issue entra mediante Pull Request.
```

---

## 6. Protección de secretos

Nunca subir:

```text
API keys
Tokens
.env
application-secret.properties
application-local.properties
Logs con credenciales
```

En `application.properties`:

```properties
spring.ai.openai.api-key=${OPENAI_API_KEY}
```

En `.gitignore`:

```gitignore
.env
application-local.properties
application-dev.properties
application-secret.properties
*.log
target/
.idea/
.vscode/
```

---

## 7. GitHub Secrets

Crear secreto:

```text
OPENAI_API_KEY
```

Ruta:

```text
Repository → Settings → Secrets and variables → Actions → New repository secret
```

---

## 8. CODEOWNERS sugerido

Archivo:

```text
.github/CODEOWNERS
```

Contenido base:

```text
* @usuario-luis @usuario-salvador

/src/main/java/com/librotech/chattech/service/ @usuario-alejandro @usuario-luis
/src/main/java/com/librotech/chattech/controller/ @usuario-camilo @usuario-salvador
/src/main/java/com/librotech/chattech/repository/ @usuario-luis
/src/main/resources/templates/ @usuario-alejandro
/src/main/resources/application.properties @usuario-camilo @usuario-salvador
*.md @usuario-luis @usuario-salvador
```

Reemplazar por los handles reales.
