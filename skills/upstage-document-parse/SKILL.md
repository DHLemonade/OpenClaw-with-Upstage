---
name: upstage-document-parse
description: Parse documents (PDF, images, etc.) using Upstage Document Parse API. Use when you need to extract structured text, HTML, or markdown from documents like invoices, forms, or scanned pages.
homepage: https://developers.upstage.ai/docs/apis/document-digitization
metadata:
  {
    "openclaw":
      {
        "emoji": "📄",
        "requires": { "env": ["UPSTAGE_API_KEY"] },
        "primaryEnv": "UPSTAGE_API_KEY",
      },
  }
---

# Upstage Document Parse

Extract structured content from documents using Upstage's Document Parse API.

## API Endpoint

```
POST https://api.upstage.ai/v1/document-digitization
```

## Available Models

| Alias | Description | RPS |
|-------|-------------|-----|
| `document-parse` | Stable alias (currently points to document-parse-251217) | 1 (Sync) / 2 (Async) |
| `document-parse-260128` | Latest version | 1 (Sync) / 2 (Async) |
| `document-parse-nightly` | Nightly build | 1 (Sync) / 2 (Async) |

**Recommendation:** Use the `document-parse` alias instead of hardcoding model names.

## Authentication

Set the `UPSTAGE_API_KEY` environment variable with your API key (format: `up_*`).

## Usage Examples

### Python

```python
import requests

api_key = "up_*************************"  # Your API key
filename = "document.png"  # Path to your file

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

### cURL

```bash
curl -X POST "https://api.upstage.ai/v1/document-digitization" \
  -H "Authorization: Bearer $UPSTAGE_API_KEY" \
  -F "document=@document.png" \
  -F "ocr=force" \
  -F "base64_encoding=['table']" \
  -F "model=document-parse"
```

### Node.js

```javascript
const FormData = require('form-data');
const fs = require('fs');
const fetch = require('node-fetch');

const form = new FormData();
form.append('document', fs.createReadStream('document.png'));
form.append('ocr', 'force');
form.append('base64_encoding', "['table']");
form.append('model', 'document-parse');

const response = await fetch('https://api.upstage.ai/v1/document-digitization', {
  method: 'POST',
  headers: {
    'Authorization': `Bearer ${process.env.UPSTAGE_API_KEY}`,
  },
  body: form,
});

const result = await response.json();
console.log(result);
```

## Response Structure

The API returns structured content with:

- **`content`**: Parsed document content
  - `html`: HTML representation with styling
  - `markdown`: Markdown representation
  - `text`: Plain text representation

- **`elements`**: Array of detected elements with:
  - `category`: Element type (heading1, paragraph, list, table, etc.)
  - `content`: Element content in html/markdown/text
  - `coordinates`: Bounding box coordinates (normalized 0-1)
  - `id`: Element identifier
  - `page`: Page number

- **`usage`**: API usage info (pages processed)

## Supported File Types

- Images: PNG, JPG, JPEG, GIF, BMP, TIFF
- Documents: PDF

## Guardrails

- Store API key in environment variable, never hardcode in scripts.
- Be mindful of rate limits (1 RPS for sync, 2 RPS for async).
- For large documents, consider using async API to avoid timeouts.
- Coordinates are normalized (0-1), multiply by image dimensions for pixel values.
