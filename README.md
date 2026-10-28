# Jarvis

Asistente personal web para explorar notas, proyectos y memoria en una interfaz tipo "knowledge galaxy". Incluye chat con voz, proyectos aislados (workspaces) y, opcionalmente, integraciones con Google Classroom, Google Calendar y Firebase.

## Instalar con un prompt (lo más fácil)

Abre tu agente de IA en una carpeta vacía y pega el texto de tu herramienta. Todos hacen lo mismo: clonan, instalan y arrancan Jarvis en `http://localhost:4700`.

**Claude Code**

```text
Clona https://github.com/emilianocros27-bit/jarvis.git, lee su CLAUDE.md y sigue
sus instrucciones: crea un entorno virtual, instala requirements.txt y arranca
python server.py. Avísame antes de cambiar algo del sistema y dime cuando esté
listo en http://localhost:4700. No toques credentials/ ni token.json.
```

**Codex (OpenAI)**

```text
Clona https://github.com/emilianocros27-bit/jarvis.git, lee su AGENTS.md y sigue
sus instrucciones: crea un entorno virtual, instala requirements.txt y arranca
python server.py. Avísame antes de cambiar algo del sistema y dime cuando esté
listo en http://localhost:4700. No toques credentials/ ni token.json.
```

**Gemini CLI, Cursor, Copilot, Windsurf u otro agente**

```text
Clona https://github.com/emilianocros27-bit/jarvis.git, lee su AGENTS.md y sigue
sus instrucciones: crea un entorno virtual, instala requirements.txt y arranca
python server.py. Avísame antes de cambiar algo del sistema y dime cuando esté
listo en http://localhost:4700. No toques credentials/ ni token.json.
```

> **Importante:** instalar y abrir Jarvis funciona con cualquier agente, pero el **chat** responde llamando a la CLI de Claude (`claude -p`) con tu propia sesión y tu plan. Para usar el chat necesitas [Claude Code](https://docs.claude.com/en/docs/claude-code) instalado y con sesión iniciada, aunque hayas instalado Jarvis con otro agente.

## Requisitos

- Python 3.10 o superior
- [Claude Code CLI](https://docs.claude.com/en/docs/claude-code) instalado y con sesión iniciada (`claude`). El chat ejecuta `claude -p` en tu máquina; sin la CLI el chat no responde.
- Opcional: credenciales de Google y/o Firebase (ver abajo)

## Instalación rápida

```bash
git clone https://github.com/emilianocros27-bit/jarvis.git
cd jarvis
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python server.py
```

Abre `http://localhost:4700`.

En el primer arranque se crea la carpeta `workspaces/` con un proyecto "Default" vacío. Tus notas, memoria y proyectos viven ahí y **no se suben a git** (`workspaces/` está en `.gitignore`).

## Credenciales (opcionales)

Todo lo que es secreto vive en `credentials/` y `token.json`, ambos ignorados por git. Sin estos archivos Jarvis arranca igual; solo fallan las integraciones correspondientes.

### Google Classroom y Calendar

1. En [Google Cloud Console](https://console.cloud.google.com/) crea un proyecto y habilita las APIs **Google Classroom API** y **Google Calendar API**.
2. Configura la pantalla de consentimiento OAuth (agrega tu cuenta como usuario de prueba).
3. Crea un **ID de cliente OAuth** de tipo *Aplicación de escritorio* y descarga el JSON.
4. Guárdalo como `credentials/client_secret.json`.
5. La primera vez que uses Classroom o Calendar se abre el navegador para dar consentimiento y se genera `token.json`. Si cambias los scopes en `google_auth.py`, borra `token.json` para repetir el consentimiento.

Scopes: Classroom solo lectura; Calendar lectura y escritura.

### Firebase (DUNK)

`dunk_firebase.py` lee (solo lectura) la base Firestore del juego DUNK. Es específico de ese proyecto: si no lo usas, ignóralo. Si quieres adaptarlo a tu propio Firebase, descarga la clave de cuenta de servicio y guárdala como `credentials/dunk-firebase.json`.

### PIN para acciones destructivas

Borrar un proyecto pide un PIN local. Se guarda hasheado (PBKDF2) en `credentials/pin.json`; el PIN en texto plano nunca se escribe en disco.

## Estructura

| Archivo | Para qué sirve |
| --- | --- |
| `server.py` | Servidor HTTP, chat, voz y endpoints |
| `viewer/index.html` | Interfaz 3D |
| `workspaces.py` | Proyectos aislados (notas, grafo, memoria) |
| `build.py` | Construye el grafo a partir de notas Markdown |
| `google_auth.py`, `classroom.py`, `calendar_api.py` | Integraciones de Google |
| `documents.py` | Genera recibos, cartas y reportes en PDF |
| `dunk_firebase.py` | Lectura de Firestore (DUNK) |
| `project_auth.py` | PIN local |

## Documentos generados

`documents.py` guarda recibos, cartas y reportes en `~/Desktop/jarvis-documents` (ajusta `OUTPUT_ROOT` si quieres otra ruta).

## Despliegue

El servidor lee la variable `PORT` que proporcionan Render, Railway y similares.

- Build command: `pip install -r requirements.txt`
- Start command: `python server.py`

En Render basta crear un **Blueprint** desde el repositorio; `render.yaml` ya incluye esa configuración.

Limitaciones en hosting estándar:

- El chat usa la CLI local de Claude (`claude -p`), así que no funciona hasta que se sustituya por una integración de servidor (por ejemplo, la API de Anthropic).
- El disco de la mayoría de hosts es efímero: los datos de `workspaces/` se pierden al reiniciar.
- Las credenciales deben configurarse de forma segura en el host. Nunca las subas al repositorio.

## Seguridad

No subas `credentials/` ni `token.json`. Si alguna vez se filtran, revoca las claves en Google Cloud / Firebase y genera nuevas.

## Licencia

[MIT](LICENSE)
