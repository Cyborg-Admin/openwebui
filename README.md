# Open WebUI (Coolify)

Docker Compose stack for [Open WebUI](https://github.com/open-webui/open-webui) + [Ollama](https://ollama.com), ready to deploy on [Coolify](https://coolify.io).

## What’s included

| Service     | Image                                      | Port |
|------------|---------------------------------------------|------|
| open-webui | `ghcr.io/open-webui/open-webui:main`        | 8080 |
| ollama     | `ollama/ollama:latest`                      | 11434 (internal) |

Data is stored in named Docker volumes (`open-webui-data`, `ollama-data`).

## Deploy on Coolify

1. Push this repo to GitHub.
2. In Coolify: **Project → + Add Resource → Docker Compose** (from Git).
3. Connect the GitHub repo and select `docker-compose.yml`.
4. Set environment variables (at least `WEBUI_SECRET_KEY`).
5. Assign a domain / FQDN to the **open-webui** service (port **8080**).
6. Deploy.

### Required env vars

| Variable           | Notes                                      |
|--------------------|--------------------------------------------|
| `WEBUI_SECRET_KEY` | Long random string (e.g. `openssl rand -hex 32`) |

Optional: `OPENAI_API_KEY`, `OPENAI_API_BASE_URL` for cloud/compatible APIs.

### After first deploy

1. Open your domain and create the first admin account.
2. Pull a model into Ollama (from the Coolify server / container terminal):

```bash
docker exec -it ollama ollama pull llama3.2
```

3. In Open WebUI, select the model and chat.

## Local test (optional)

```bash
cp .env.example .env
# edit WEBUI_SECRET_KEY
docker compose up -d
```

UI: http://localhost:8080

## Notes for Coolify

- Coolify handles HTTPS / reverse proxy; map the domain to service port `8080`.
- Do not commit `.env` (it is gitignored).
- Ollama needs enough RAM/CPU (or GPU) for the models you pull.
- Set `ENABLE_SIGNUP=False` after creating your admin if you want to lock registration.
