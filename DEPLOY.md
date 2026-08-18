# Cómo se despliega este sitio (runbook)

Este proyecto se publica con **GitHub Pages** en:
**https://chia-fceia-unr.github.io/rastreador-friccion-lectura/**

Repo: `https://github.com/chia-fceia-unr/rastreador-friccion-lectura`
Carpeta local: `C:\Users\evang\OneDrive\Documentos\FCEIA\Herramientas de Inteligencia Artificial\Recursos\rastreador-friccion-lectura`

---

## Lo importante (por qué "funciona en una conversación y en otra no")

- **NO se usa `gh` (GitHub CLI).** En este entorno `gh` **no está logueado**, así que cualquier intento con `gh` falla.
- Lo que funciona es **`git push` por HTTPS**, porque en esta PC **Windows Git Credential Manager ya tiene guardado un token de GitHub** (de un login previo). Cualquier conversación que use `git push` sobre el remoto HTTPS **reutiliza esa credencial guardada** sin pedir login.
- **Pages se activa UNA sola vez.** Después de eso, **cada `git push` reconstruye el sitio solo** (no hay que volver a activar nada).

> En criollo: para actualizar el sitio, el 99% de las veces alcanza con **`git add` + `git commit` + `git push`**. Nada más.

---

## Día a día: actualizar el sitio ya publicado

Desde la carpeta del proyecto (o usando `git -C "<ruta>"`):

```bash
git -C "C:\Users\evang\OneDrive\Documentos\FCEIA\Herramientas de Inteligencia Artificial\Recursos\rastreador-friccion-lectura" add -A
```
```bash
git -C "C:\Users\evang\OneDrive\Documentos\FCEIA\Herramientas de Inteligencia Artificial\Recursos\rastreador-friccion-lectura" commit -m "Describí el cambio acá"
```
```bash
git -C "C:\Users\evang\OneDrive\Documentos\FCEIA\Herramientas de Inteligencia Artificial\Recursos\rastreador-friccion-lectura" push
```

En ~1 minuto Pages reconstruye. Si al abrir la web no ves el cambio, hacé un **refresh fuerte** (Ctrl/Cmd+Shift+R): GitHub cachea el HTML unos minutos.

---

## Desde cero: publicar un repo nuevo (lo que se hizo la primera vez)

### 1) Crear el repo en GitHub
Esto **no se puede hacer por línea de comandos acá** (no hay `gh` logueado). Se crea a mano en la web: en la organización → **New repository** → nombre en minúsculas con guiones → **Public** (obligatorio para Pages gratis) → Create.

### 2) Inicializar y subir (local, sin login extra: usa la credencial guardada)
```bash
git -C "<RUTA_DEL_PROYECTO>" init -b main
```
```bash
git -C "<RUTA_DEL_PROYECTO>" add -A
```
```bash
git -C "<RUTA_DEL_PROYECTO>" commit -m "Versión inicial"
```
```bash
git -C "<RUTA_DEL_PROYECTO>" remote add origin https://github.com/<ORG>/<REPO>.git
```
```bash
git -C "<RUTA_DEL_PROYECTO>" push -u origin main
```

### 3) Activar GitHub Pages (una sola vez)

**Opción A — Web (la más simple, sin comandos):**
Repo → **Settings → Pages → Build and deployment → Source: _Deploy from a branch_ → Branch: `main` / carpeta `/ (root)` → Save.**
En ~1 min queda en `https://<ORG>.github.io/<REPO>/`.

**Opción B — Por API (lo que se usó acá, todo automático desde Git Bash):**
Toma el token guardado en Windows Credential Manager y llama a la API de Pages. El token es tuyo y **no sale de tu PC**; no se imprime en pantalla.
```bash
TOKEN=$(printf 'protocol=https\nhost=github.com\n\n' | git -C "<RUTA_DEL_PROYECTO>" credential fill | sed -n 's/^password=//p'); curl -s -X POST -H "Authorization: Bearer $TOKEN" -H "Accept: application/vnd.github+json" -H "X-GitHub-Api-Version: 2022-11-28" https://api.github.com/repos/<ORG>/<REPO>/pages -d '{"source":{"branch":"main","path":"/"}}'
```
Una respuesta con `"html_url": "...github.io/..."` (HTTP 201) significa que quedó activado.

---

## Verificar el estado del build (opcional)

```bash
TOKEN=$(printf 'protocol=https\nhost=github.com\n\n' | git -C "<RUTA_DEL_PROYECTO>" credential fill | sed -n 's/^password=//p'); curl -s -H "Authorization: Bearer $TOKEN" -H "Accept: application/vnd.github+json" https://api.github.com/repos/<ORG>/<REPO>/pages/builds/latest
```
Buscá `"status": "built"` (y que `"commit"` coincida con tu último push).

---

## Si algo falla

- **`git push` pide usuario/contraseña o falla la autenticación:** se abrirá (una vez) la ventana de login de GitHub de Windows Credential Manager; completala y vuelve a andar. A partir de ahí queda guardada.
- **Alguien intenta con `gh ...`:** no va a funcionar (no está logueado). Usar `git` directamente.
- **La API de Pages devuelve 403/404:** probablemente el token guardado no tiene permisos de Pages; en ese caso activá Pages por la **web (Opción A)**.
- **Pages no aparece / da 404 al abrir:** revisá que el repo sea **público** (Settings → General → Danger Zone → Change visibility).

---

## Para pegarle a otra conversación (Claude Code)

> "El repo `chia-fceia-unr/rastreador-friccion-lectura` ya existe y ya tiene GitHub Pages activado.
> Para deployar cambios NO uses `gh` (no está logueado). Usá `git` directo sobre el remoto HTTPS,
> que reutiliza la credencial guardada en Windows Credential Manager:
> `git -C "<ruta>" add -A` → `git -C "<ruta>" commit -m '...'` → `git -C "<ruta>" push`.
> Pages reconstruye solo en ~1 min. Ver `DEPLOY.md` en el repo para el detalle."
