# Ollama Connection Guide

This app requires a locally running Ollama instance. Get up and running in three steps.

## 1. Install Ollama

Download and run the installer from [ollama.com/download](https://ollama.com/download).

## 2. Pull a Model

```bash
ollama pull gemma4
```

*This downloads ~9 GB. Only 
Only needed once.*

There are smaller models (e.g. `gemma4:e2b`) or bigger ones, in case you possess more RAM (e.g. `gemma4:26b`).
See full list [here](https://ollama.com/search).

## 3. Allow AGNT App to Connect

Ollama blocks requests from external origins by default. To allow this app, you need to set the `OLLAMA_ORIGINS` environment variable.

If you are running Ollama as a systemd service (the default installation method), edit your service configuration:

1. Run:
```bash
sudo systemctl edit ollama.service
```

2. Add the following lines to the file:
```ini
[Service]
Environment="OLLAMA_ORIGINS=https://scrumcoin.github.io"
```

3. Save and exit, then reload and restart the service:
```bash
sudo systemctl daemon-reload
sudo systemctl restart ollama
```

Ollama should now be running and accepting requests from AGNT app page.
