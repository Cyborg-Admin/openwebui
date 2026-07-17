# Open WebUI (Coolify)

Docker Compose stack for [Open WebUI](https://github.com/open-webui/open-webui), ready to deploy on [Coolify](https://coolify.io). It talks to an existing [Ollama](https://ollama.com) host (e.g. Cortex at `http://10.0.0.1:11434`).

## What’s included

| Service     | Image                                      | Port |
|------------|---------------------------------------------|------|
| open-webui | `ghcr.io/open-webui/open-webui:main`        | 8080 |

Data is stored in the named Docker volume `open-webui-data`. Models live on your external Ollama.

## Deploy on Coolify

1. Push this repo to GitHub.
2. In Coolify: **Project → + Add Resource → Docker Compose** (from Git).
3. Connect the GitHub repo and select `docker-compose.yml`.
4. Set environment variables (at least `WEBUI_SECRET_KEY`).
5. Assign a domain to the **open-webui** service as `https://your-domain:8080` (container port).
6. Deploy. Do **not** publish host port `8080` in compose — Coolify’s proxy handles HTTPS.

### Required env vars

| Variable           | Notes                                      |
|--------------------|--------------------------------------------|
| `WEBUI_SECRET_KEY` | Long random string (e.g. `openssl rand -hex 32`) |
| `OLLAMA_BASE_URL`  | Existing Ollama API, e.g. `http://10.0.0.1:11434` |

Optional: `OPENAI_API_KEY`, `OPENAI_API_BASE_URL` for cloud/compatible APIs.

### After first deploy

1. Open your domain and create the first admin account.
2. Models from your Cortex/Ollama host should appear in the model picker.

## Local test (optional)

```bash
cp .env.example .env
# edit WEBUI_SECRET_KEY
# temporarily add under open-webui: ports: ["8080:8080"]
docker compose up -d
```

UI: http://localhost:8080

## Notes for Coolify

- Coolify handles HTTPS / reverse proxy; map the domain to service port `8080`.
- Do not commit `.env` (it is gitignored).
- Ollama needs enough RAM/CPU (or GPU) for the models you pull.
- Set `ENABLE_SIGNUP=False` after creating your admin if you want to lock registration.
