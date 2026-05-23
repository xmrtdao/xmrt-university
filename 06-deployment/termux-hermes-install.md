# Termux Hermes Install Guide

## Install Process
```bash
pkg update && pkg upgrade -y
pkg install python
pkg install npm
pkg install git
pkg install ollama
git clone https://github.com/xmrtdao/mobilemonero.git
cd mobilemonero
```

## Ollama Setup
- OAuth sign-in is **normal behavior**
- **$20/month Ollama subscription** required for recommended models
- Recommended: `deepseek-coder`, `qwen2.5`, `kimi`
- DO NOT use Llama/Gemma for troubleshooting

## Troubleshooting
1. Talk to a model directly: `ollama run deepseek-coder`
2. Most common fix: `pkg reinstall curl`
3. Retry install from step 1
