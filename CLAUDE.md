# Jarvis — guía para agentes

Instalar y ejecutar:

```bash
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
python server.py   # http://localhost:4700
```

- El chat llama a `claude -p` (ver `ask_claude` en `server.py`): usa la sesión de Claude Code del usuario. Si `claude` no está en el PATH, avisa al usuario en vez de inventar una API key.
- `workspaces/` se crea solo en el primer arranque y contiene datos personales (ignorado por git).
- Nunca leas, imprimas ni subas `credentials/` ni `token.json`.
- Google (Classroom/Calendar) y Firebase son opcionales; guía de credenciales en `README.md`.
- No hagas push ni cambies el remoto sin que el usuario lo pida.
