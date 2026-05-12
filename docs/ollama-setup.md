# Ollama Connection Guide

This app requires a locally running Ollama instance. Follow the steps below for your operating system.

---

## 1. Install Ollama

### macOS
Download and run the installer from [ollama.com/download](https://ollama.com/download), or use Homebrew:
```bash
brew install ollama
```

### Linux
```bash
curl -fsSL https://ollama.com/install.sh | sh
```
This installs Ollama and registers it as a systemd service.

### Windows
Download the installer from [ollama.com/download](https://ollama.com/download) and run it. Ollama will start automatically as a background service.

---

## 2. Pull a Model

After installing, pull at least one model to use:
```bash
ollama pull llama4
```

Other popular options: `mistral`, `gemma4`, `qwen2.6`, `phi4`.

---

## 3. Allow Browser Access (OLLAMA_ORIGINS)

By default Ollama only accepts requests from `localhost`. Since this app is served from a remote origin, you need to explicitly allow it.

Set `OLLAMA_ORIGINS` to include this page's domain **before** starting Ollama.

### macOS — persistent (launchd)
```bash
launchctl setenv OLLAMA_ORIGINS "https://your-username.github.io"
pkill ollama && ollama serve
```

### macOS / Linux — single session
```bash
OLLAMA_ORIGINS="https://your-username.github.io" ollama serve
```

### Linux — systemd service
```bash
sudo systemctl edit ollama.service
```
Add the following, then save:
```ini
[Service]
Environment="OLLAMA_ORIGINS=https://your-username.github.io"
```
```bash
sudo systemctl daemon-reload && sudo systemctl restart ollama
```

### Windows — PowerShell
```powershell
$env:OLLAMA_ORIGINS="https://your-username.github.io"
ollama serve
```
To make it permanent, set it via **System Properties → Environment Variables**.

> **Multiple origins?** Comma-separate them:
> `OLLAMA_ORIGINS="https://your-username.github.io,http://localhost:3000"`
>
> **Local dev only?** A wildcard is fine:
> `OLLAMA_ORIGINS="*"`

---

## 4. Verify the Connection

With Ollama running, open a terminal and check:
```bash
curl http://localhost:11434/api/tags
```
You should get a JSON list of your installed models. If this works but the app still can't connect, double-check the `OLLAMA_ORIGINS` value and restart Ollama.

---

## 5. Optimization Tips

### Keep a model warm in memory
By default Ollama unloads a model 5 minutes after last use, causing a cold-start delay on the next request. To keep it loaded indefinitely:
```bash
# Load and keep in memory (timeout = 0 means never unload)
curl http://localhost:11434/api/generate -d '{"model":"llama3.2","keep_alive":-1}'
```
Or set a specific duration, e.g. `"keep_alive": "1h"`.

To control this globally, set the environment variable:
```bash
OLLAMA_KEEP_ALIVE="-1" ollama serve   # never unload
OLLAMA_KEEP_ALIVE="1h" ollama serve   # keep warm for 1 hour
```

### Limit or expand GPU memory usage
Ollama auto-detects your GPU. To cap VRAM usage (useful on shared machines):
```bash
OLLAMA_MAX_LOADED_MODELS=1 ollama serve   # only one model in memory at a time
```

### Run multiple models simultaneously
If you have enough VRAM/RAM:
```bash
OLLAMA_MAX_LOADED_MODELS=3 ollama serve
```

### Use a faster context window
Smaller context = faster responses + less memory. Pass `num_ctx` per request:
```json
{ "model": "llama4", "options": { "num_ctx": 2048 } }
```
Default is usually 2048–4096 depending on the model.

### Enable Flash Attention (where supported)
```bash
OLLAMA_FLASH_ATTENTION=1 ollama serve
```
Reduces VRAM usage and speeds up inference on longer contexts. Works on most modern NVIDIA and Apple Silicon GPUs.

### Check what's currently loaded
```bash
ollama ps
```
or
```bash
curl http://localhost:11434/api/ps
```
Shows running models, their VRAM usage, and when they'll be evicted.
