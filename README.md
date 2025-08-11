# Survey Data Dashboard (Flask)

Upload, clean, validate, analyze, visualize, and export survey data via a simple Flask web UI.

## Features
- File upload: CSV or Excel (xlsx)
- Schema mapping from JSON
- Missing-value handling (mean/median/KNN)
- Outlier handling (IQR, Z-score, Winsorization)
- Rule-based validation from JSON (min/max per column)
- Weighted statistics (weighted means)
- Plotly summary charts
- Download cleaned data
- Report page with optional PDF export
- Optional AI chat assistant (/chat) with OpenAI-compatible providers (Groq/OpenAI) or local Ollama/LM Studio

## Tech stack
- Flask, Pandas, scikit-learn, Plotly, openpyxl, pdfkit, requests

## Prerequisites
- Python 3.10+
- wkhtmltopdf (only if you will export PDF via pdfkit)
- For optional AI chat: a provider (Groq/OpenAI) API key, or a local server (Ollama/LM Studio)

## Setup
```powershell
cd "C:\Users\D KAVINKUMAR\Desktop\statathon"
python -m venv .venv
. .venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
```

## Run locally
```powershell
# Optional: configure AI provider (pick one)
# Groq (OpenAI-compatible)
$env:OPENAI_API_BASE="https://api.groq.com/openai/v1"
$env:OPENAI_API_KEY="gsk_your_key_here"
$env:OPENAI_MODEL="llama-3.1-8b-instant"
$env:AI_PROVIDER=""  # ensure Ollama not forced

# OR Local LM Studio
# $env:OPENAI_API_BASE="http://localhost:1234/v1"
# $env:OPENAI_API_KEY="lm-studio"
# $env:OPENAI_MODEL="local-model"
# $env:AI_PROVIDER=""

# OR Local Ollama
# $env:AI_PROVIDER="ollama"
# $env:OLLAMA_MODEL="llama3"
# (Start server in another terminal: `ollama serve`)

python gov.py
# Open http://127.0.0.1:5000/
```

## Login
- Username: `admin`
- Password: `admin`

## Usage
1. Log in.
2. Upload a CSV/XLSX survey file.
3. (Optional) Upload JSON schema mapping and/or rules.
4. Choose imputation and outlier methods.
5. (Optional) Enter weights column name.
6. Submit and review the summary, charts, rule warnings, and download link.

## JSON formats
- Schema mapping (columns rename):
```json
{
  "old_col_name": "new_col_name",
  "Age": "age"
}
```
- Rule validation:
```json
{
  "age": { "min": 0, "max": 120 },
  "income": { "min": 0 }
}
```

## API endpoints
- `POST /chat`
  - Body: `{ "message": "your question" }`
  - Reply: `{ "reply": "..." }`
- `GET /report/<filename>?format=html|pdf`
- `GET /download/<filename>`

## Deploy (Render/Railway)
- Repo contains `requirements.txt` and `Procfile`.
- Start command: `python gov.py`
- The app binds to `0.0.0.0` and reads `PORT` from env automatically.
- Set any required environment variables for AI chat on the platform (e.g., `OPENAI_API_BASE`, `OPENAI_API_KEY`, `OPENAI_MODEL`).

## Troubleshooting
- Blank page or not reachable: confirm the server logs show it is running and listening on correct port.
- CSV/Excel read errors: verify file format and encoding.
- KNN imputation: ensure `scikit-learn` is installed.
- PDF export: install wkhtmltopdf and ensure it’s on PATH.
- Weighted mean error: make sure weights column is numeric; the app coerces invalid values to NaN and skips them.
- AI chat:
  - 401/403: invalid/missing key or wrong API base
  - 429: quota/rate-limit
  - Ollama connection refused: start `ollama serve` and pull a model

## Security
- Do not commit secrets. Use environment variables for API keys.

## License
- Add your preferred license here (MIT/Apache-2.0/etc.).
