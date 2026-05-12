# Ollama Connection Guide

This app requires a locally running Ollama instance. Get up and running in three steps.

## 1. Install Ollama

Download and run the installer from [ollama.com/download](https://ollama.com/download).

## 2. Pull the Model

```powershell
ollama pull gemma4
```

*This downloads ~9 GB. Only needed once.*

There are smaller models (e.g. `gemma4:e2b`) or bigger ones, in case you possess more RAM (e.g. `gemma4:26b`).
See full list [here](https://).

## 3. Allow AGNT App to Connect

Ollama blocks requests from external origins by default. To allow this app, you need to set the `OLLAMA_ORIGINS` environment variable.

1. Open **PowerShell** and run the following command to set the environment variable for your user:

```powershell
[System.Environment]::SetEnvironmentVariable("OLLAMA_ORIGINS", "https://scrumcoin.github.io", "User")
```

2. Close Ollama from the system tray.
3. Restart Ollama.

Ollama should now be running and accepting requests from AGNT app page.
