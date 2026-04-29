# Open WebUI + Ollama Codespace Configuration

This folder contains a self-contained DevContainer configuration. Opening this folder in VS Code will automatically start Open WebUI with Ollama and a 3.8B model.

## Quick Start

### Option A: VS Code (Local)

1. Install the **Dev Containers** extension
2. Open this folder (`codespace/`) in VS Code
3. Click **Reopen in Container** (popup) or run `Dev Containers: Reopen in Container` from Command Palette
4. Wait 3-5 minutes for first-time setup (pulls images, downloads model)
5. Browser opens automatically to http://localhost:3000

### Option B: GitHub Codespaces

If this repo is on GitHub, you can create a Codespace directly from the repository root (uses `.devcontainer/` config). But if you want to use this `codespace` folder specifically:

1. Push this repository to GitHub
2. Create a new Codespace
3. Once the Codespace starts, open the `codespace` folder as a workspace window:
   - File → Add Folder to Workspace → select `codespace`
   - Then click **Reopen in Container** on that workspace window

Or simply use the root `.devcontainer` config that already exists.

## What Gets Started

| Service | Port | Purpose |
|---------|------|---------|
| Ollama | 11434 | LLM runtime (runs `phi3:3.8b`) |
| Open WebUI | 3000 | Chat interface |

Both start automatically. The model downloads on first run (~2GB).

## Changing the Model

Edit `codespace/docker-compose.yml`:

```yaml
command: >
  sh -c "
    ollama serve &
    OLLAMA_PID=$!
    sleep 5 &&
    ollama pull <your-model-here> &&   # <-- change this
    wait $OLLAMA_PID
  "
```

Popular 3.8B models:
- `phi3:3.8b` (default, fast, good for CPU)
- `llama2:3.8b`
- `mistral:3.8b`
- `openhermes:3.8b`

Larger models (7B) may be slow on CPU.

## Managing the Environment

Once inside the container:

```bash
# View logs of both services
docker compose logs -f

# View only Ollama logs
docker compose logs -f ollama

# Restart a service
docker compose restart open-webui

# Pull another model
docker exec ollama ollama pull mistral:7b

# Stop everything (when closing container)
# Just close the VS Code window; auto-stop happens
```

## Manual Start/Stop (outside VS Code)

If you want to run without VS Code:

```bash
cd codespace
docker compose up -d
# ...
docker compose down
```

## Troubleshooting

**"Server connection error" in UI**
- Wait 30-60 seconds for Ollama to fully start
- Check: `curl http://localhost:11434/api/tags` returns JSON

**Model download stalls**
- Restart Ollama container: `docker compose restart ollama`

**Port already in use**
- Change port mappings in `docker-compose.yml`

**Clear all data (chats, models)**
```bash
docker compose down -v
```

## Notes

- Data is persisted in Docker volumes: `open-webui` (chats) and `ollama` (models)
- Workspace folder is mounted at `/workspace` inside the container for code editing
- Uses CPU-only inference; no GPU drivers needed

Enjoy your hassle-free AI environment!
