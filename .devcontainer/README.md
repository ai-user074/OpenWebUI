# Development Environment Setup

This project uses **GitHub Codespaces** and **VS Code DevContainers** for a seamless development experience.

## Start Developing

Click the button below to open this repository in a GitHub Codespace:

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/open-webui/open-webui)

Alternatively, open locally with VS Code and use the **Dev Containers: Reopen in Container** command.

## What You Get

- Pre-configured VS Code with recommended extensions
- Ollama running with a 3.8B model
- Open WebUI hot-reload development server
- All dependencies pre-installed
- Port forwarding for web access

## Project Structure

- `backend/` — FastAPI Python backend
- `src/` — SvelteKit frontend source
- `.devcontainer/` — Container configuration
- `.vscode/` — Editor settings

## First Run

On first launch, the container will:
1. Build the Open WebUI image
2. Pull the Ollama phi3:3.8b model (~2GB)
3. Start both services
4. Open VS Code with all extensions

Total time: ~5-10 minutes on first run.

## Commands

```bash
# Frontend dev (with hot reload)
npm run dev

# Backend dev (with hot reload)
npm run dev:backend

# Build production
npm run build

# Lint & check
npm run lint
```

## Changing the Model

Edit `.devcontainer/docker-compose.yml` → `ollama.service.command` to pull a different model.

Example:
```yaml
command: >
  sh -c "
    ollama serve &
    OLLAMA_PID=$!
    sleep 5 &&
    ollama pull llama2:7b &&
    wait $OLLAMA_PID
  "
```
