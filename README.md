# ContentAI — AI Content Generator

OPEN AI:https://generative-ai-1l0y.onrender.com/
> **Text · Code · Images** — powered by Groq LLaMA 3.1 and Pollinations.ai

ContentAI is a production-ready Flask web application that lets you generate text, code, and images through a single, polished interface. It features a persistent prompt library, configurable generation parameters, and a server-side image proxy that eliminates browser CORS issues entirely.

---

## Project Structure

```
contentai/
├── app.py                 # Main Flask application + API routes
├── requirements.txt       # Python dependencies
├── .env                   # Environment variables (API keys)
├── templates/
│   └── index.html         # Full web interface (single-file, no build step)
├── prompts/
│   └── library.json       # Persistent prompt library (auto-created)
└── README.md              # Project documentation
```

---

## Features

| Feature | Details |
|---|---|
| **Text Generation** | System + user prompt, adjustable temperature & max tokens |
| **Code Generation** | 10 languages, low-temperature mode for deterministic output |
| **Image Generation** | Pollinations.ai (free, no key), 5 models, custom dimensions |
| **Server-side proxy** | Flask fetches image bytes — zero browser CORS issues |
| **Prompt Library** | Save, categorise, load, and delete reusable prompts |
| **Keyboard shortcut** | `Cmd/Ctrl + Enter` to generate on any active tab |
| **Mock mode** | Remove the Groq client to run UI-only without an API key |

---

## Installation

### Prerequisites

- Python 3.8 or higher
- A Groq API key — sign up free at [groq.com](https://groq.com)

### Setup Steps

**1. Clone the repository**
```bash
git clone https://github.com/your-username/contentai.git
cd contentai
```

**2. Create a virtual environment**
```bash
python -m venv .venv

# Windows
.venv\Scripts\activate

# macOS / Linux
source .venv/bin/activate
```

**3. Install dependencies**
```bash
pip install -r requirements.txt
```

**4. Configure environment variables**

Create a `.env` file in the project root:
```
GROQ_API_KEY=your_groq_api_key_here
FLASK_SECRET_KEY=your_secret_key_here
```

Or export directly:
```bash
export GROQ_API_KEY="gsk_..."
```

**5. Run the application**
```bash
python app.py
```

**6. Open in your browser**
```
http://localhost:5000
```

---

## Usage

### Text Tab
1. Optionally customise the **System Prompt** to set the AI persona
2. Type your content request in the **Your Prompt** field
3. Adjust **Temperature** (creativity) and **Max Tokens** (length)
4. Click **✦ Generate** or press `Cmd/Ctrl + Enter`
5. Copy the output with one click

### Code Tab
1. Describe what you want to build
2. Select your target **Language**
3. Keep temperature low (0.1–0.4) for deterministic, clean code
4. Click **⌨ Generate**

### Image Tab
1. Write a descriptive image prompt — be specific about style, mood, lighting
2. Choose a **Model** (`flux` is the default; `flux-realism` for photorealism)
3. Set dimensions (1024×1024 recommended)
4. Click **◈ Generate Image** — generation takes 10–30 seconds
5. Download the image directly from the browser

### Prompt Library
- Click **+ Save** to save the current prompt with a name and category
- Click any saved prompt in the sidebar to load it instantly
- Click **✕** on a prompt to delete it
- Prompts persist across sessions in `prompts/library.json`

---

## API Endpoints

### Text Generation
```
POST /api/generate
```
```json
{
  "system_prompt": "You are an expert copywriter.",
  "user_prompt": "Write a tagline for a coffee brand.",
  "temperature": 0.7,
  "max_tokens": 800
}
```
Response: `{ "content": "..." }`

---

### Code Generation
```
POST /api/generate/code
```
```json
{
  "language": "Python",
  "user_prompt": "Write a binary search function.",
  "temperature": 0.3,
  "max_tokens": 1500
}
```
Response: `{ "content": "...", "language": "Python" }`

---

### Image Generation
```
POST /api/generate/image
```
```json
{
  "prompt": "Futuristic city at dusk, neon reflections, cinematic",
  "width": 1024,
  "height": 1024,
  "model": "flux"
}
```
Response: Raw image bytes (`image/jpeg`) — display directly as `<img src="blob:...">`.

---

### Prompt Library
```
GET    /api/prompts             → list all prompts
POST   /api/prompts             → create a prompt
DELETE /api/prompts/<id>        → delete a prompt
```

---

## AI Configuration

| Setting | Value |
|---|---|
| **Provider** | Groq Cloud |
| **Model** | `llama-3.1-8b-instant` |
| **Text temperature** | 0.7 (balanced creativity) |
| **Code temperature** | 0.3 (deterministic output) |
| **Image provider** | Pollinations.ai (free, no key) |
| **Image proxy** | Server-side (Flask fetches bytes) |

---

## How the Image Proxy Works

Most free image APIs block direct browser requests (CORS). ContentAI solves this by having Flask fetch the image on the server and stream the raw bytes back to the client:

```
Browser → POST /api/generate/image
              ↓
         Flask → urllib → Pollinations.ai
                              ↓
         Flask ← raw JPEG bytes
              ↓
Browser ← Response(bytes, mimetype="image/jpeg")
              ↓
         URL.createObjectURL(blob) → <img src="blob:...">
```

No CORS. No mixed content. No third-party credentials in the browser.

---

## Development

### Run in debug mode
```bash
export FLASK_ENV=development
python app.py
```

### Test the API
```bash
# Text generation
curl -X POST http://localhost:5000/api/generate \
     -H "Content-Type: application/json" \
     -d '{"user_prompt": "Write a haiku about coding."}'

# Image generation (saves to file)
curl -X POST http://localhost:5000/api/generate/image \
     -H "Content-Type: application/json" \
     -d '{"prompt":"a mountain at dawn","width":512,"height":512}' \
     --output image.jpg
```

### Switching AI providers

The backend uses the Groq SDK. To switch to OpenAI:

```python
# In app.py
from openai import OpenAI
client = OpenAI(api_key=os.environ.get("OPENAI_API_KEY"))
MODEL  = "gpt-4o-mini"
```

---

## Future Enhancements

- Streaming responses with Server-Sent Events
- SQLite generation history with search
- Export output to Markdown or PDF
- User authentication and personal prompt libraries
- Multi-language UI support
- Deployment configuration for Railway / Render / Fly.io

---

## Tech Stack

- **Backend**: Python · Flask · Groq SDK
- **AI (Text/Code)**: LLaMA 3.1-8B Instant via Groq
- **AI (Images)**: Pollinations.ai (Flux family)
- **Frontend**: Vanilla HTML/CSS/JS · Playfair Display · Sora · DM Mono
- **Storage**: JSON flat-file prompt library

---

## License

MIT License — see the [LICENSE](LICENSE) file for details.

---

## Acknowledgements

- [Groq](https://groq.com) for blazing-fast LLM inference
- [Pollinations.ai](https://pollinations.ai) for free image generation
- [Flask](https://flask.palletsprojects.com) for the web framework
- [Meta AI](https://ai.meta.com) for the LLaMA 3.1 model

---

> **Note:** Always verify AI-generated content before publishing. For code, review and test before deploying to production.
