# Ollama Connection Guide

This app requires a locally running Ollama instance. Get up and running in three steps.

## 1. Install Ollama

Download and run the installer from [ollama.com/download](https://ollama.com/download), or via Homebrew:

```bash
brew install ollama
```

## 2. Pull a Model

```bash
ollama pull gemma4
```

*This downloads ~9 GB. Only needed once.*

There are smaller models (e.g. `gemma4:e2b`) or bigger ones, in case you possess more RAM (e.g. `gemma4:26b`).
See full list [here](https://ollama.com/search).

## 3. Allow AGNT App to Connect

Ollama blocks requests from external origins by default. Run this command to allow this app permanently:

```bash
launchctl setenv OLLAMA_ORIGINS "https://scrumcoin.github.io"
```
and restart ollama:
```bash
pkill ollama && ollama serve
```

Ollama should now be running and accepting requests from AGNT app page.
