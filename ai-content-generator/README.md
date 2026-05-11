# AI Content Generator

A clean, production-ready Flask web app for AI-powered content generation.
Plug in **OpenAI** or **Groq** with a single line change.

---

## 🚀 Quick Start

```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Set your API key (pick one)
export OPENAI_API_KEY="sk-..."
# export GROQ_API_KEY="gsk_..."

# 3. Configure app.py (uncomment your provider)
# See the "Configure your AI provider" section near the top of app.py

# 4. Run
python app.py
```

Open → http://localhost:5000

---

## 🔌 Switching Providers

Open `app.py` and edit the provider block:

### OpenAI
```python
from openai import OpenAI
client = OpenAI(api_key=os.environ.get("OPENAI_API_KEY"))
MODEL = "gpt-4o-mini"
```

### Groq (free tier available!)
```python
from groq import Groq
client = Groq(api_key=os.environ.get("GROQ_API_KEY"))
MODEL = "llama3-8b-8192"
```

---

## 📁 Project Structure

```
ai-content-generator/
├── app.py                  ← Flask app + API routes
├── requirements.txt
├── README.md
├── templates/
│   └── index.html          ← Full UI (single-file, no build step)
└── prompts/
    └── library.json        ← Prompt library (auto-created, editable)
```

---

## ✨ Features

| Feature | Details |
|---|---|
| **Prompt Library** | Save, load, and delete reusable prompts |
| **System Prompt** | Customise AI persona per generation |
| **Parameters** | Adjust temperature & max tokens via sliders |
| **Copy Output** | One-click clipboard copy |
| **Keyboard shortcut** | `Cmd/Ctrl + Enter` to generate |
| **Categories** | Organise prompts by category |
| **Mock Mode** | Works without an API key for UI development |

---

## 🔌 REST API

```
POST /api/generate
  body: { system_prompt, user_prompt, temperature, max_tokens }
  → { content }

GET  /api/prompts         → list all prompts
POST /api/prompts         → create prompt
DELETE /api/prompts/:id   → delete prompt
```

---

## 📦 Extending

- Add **streaming** responses with `stream=True` and Server-Sent Events
- Add **history** by saving generations to SQLite
- Add **export** (Markdown / PDF) from the output panel
- Deploy to **Railway**, **Render**, or **Fly.io** in minutes
