# 🦞 OpenClaw × Upstage

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/openclaw/openclaw/main/docs/assets/openclaw-logo-text-dark.png">
    <img src="https://raw.githubusercontent.com/openclaw/openclaw/main/docs/assets/openclaw-logo-text.png" alt="OpenClaw" width="200">
  </picture>
  <span style="font-size: 48px; margin: 0 20px;">×</span>
  <img src="Upstage_Logo_Purple.svg" alt="Upstage" width="200">
</p>

<p align="center">
  <strong>Personal AI Assistant powered by Upstage Solar & Document Parse</strong>
</p>

---

**OpenClaw** is a personal AI assistant that runs on your own devices, now integrated with **Upstage** for powerful document parsing and Solar LLM capabilities.

## ✨ Features

### 🔋 Upstage Solar Integration
- **Solar Pro 2** as your primary LLM model
- Fast, efficient, and multilingual AI assistant
- Seamless authentication via Upstage API

### 📄 Document Parse Skill
Extract structured content from documents using Upstage's Document Parse API:
- **Supported formats**: PDF, PNG, JPG, JPEG, GIF, BMP, TIFF
- **Output**: HTML, Markdown, or plain text with element coordinates
- **Use cases**: Invoices, forms, scanned documents, receipts

## 🚀 Quick Start

**Runtime:** Node ≥22

```bash
# Clone and setup
git clone https://github.com/openclaw/openclaw.git
cd openclaw

# Install dependencies and build
pnpm install
pnpm ui:build  # auto-installs UI deps on first run
pnpm build

# Run onboarding wizard
pnpm openclaw onboard --install-daemon
```

During onboarding, select **Upstage API Key** as your authentication method.

## 🔑 Upstage Configuration

### 1. Get Your API Key
1. Visit [Upstage Console](https://console.upstage.ai)
2. Create an account or sign in
3. Generate your API key (format: `up_*`)

### 2. Set Up Authentication
During `openclaw onboard`, choose **upstage-api-key** and enter your key.

Or manually set:
```bash
export UPSTAGE_API_KEY="up_your_api_key_here"
```

## 📄 Using Document Parse

Once configured, the `upstage-document-parse` skill enables document parsing:

### API Endpoint
```
POST https://api.upstage.ai/v1/document-digitization
```

### Available Models

| Alias | Description |
|-------|-------------|
| `document-parse` | Stable alias (recommended) |
| `document-parse-260128` | Latest version |
| `document-parse-nightly` | Nightly build |

### Example Usage

```python
import requests

api_key = "up_your_api_key"
filename = "invoice.pdf"

url = "https://api.upstage.ai/v1/document-digitization"
headers = {"Authorization": f"Bearer {api_key}"}
files = {"document": open(filename, "rb")}
data = {
    "ocr": "force",
    "base64_encoding": "['table']",
    "model": "document-parse"
}

response = requests.post(url, headers=headers, files=files, data=data)
print(response.json())
```

### Response Structure

```json
{
  "content": {
    "html": "...",
    "markdown": "...",
    "text": "..."
  },
  "elements": [
    {
      "category": "heading1",
      "content": { "html": "...", "markdown": "...", "text": "..." },
      "coordinates": [{ "x": 0.06, "y": 0.05 }, ...],
      "id": 0,
      "page": 1
    }
  ],
  "usage": { "pages": 1 }
}
```

## 📚 Resources

- [Upstage Console](https://console.upstage.ai) — Get your API key
- [Upstage Docs](https://developers.upstage.ai) — API documentation
- [OpenClaw Docs](https://docs.openclaw.ai) — Full documentation

---

<p align="center">
  Built with 🦞 by OpenClaw × ☀️ Upstage
</p>
