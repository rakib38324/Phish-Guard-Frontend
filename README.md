# PhishGuard — AI Phishing Detection System

AI-powered phishing URL detector using XGBoost + SHAP explanations.

## Stack
- **Backend**: Python / Flask / XGBoost / SHAP
- **Frontend**: Vanilla HTML/CSS/JS (drop-in, no build step)
- **Features**: 32 URL + SSL signals extracted per URL

## Setup

### 1. Install dependencies
```bash
pip install -r backend/requirements.txt
```

### 2. Train the model (run once)
```bash
python backend/train_model.py
```
This generates `models/xgb_model.pkl`, `models/shap_explainer.pkl`, `models/feature_names.pkl`.

### 3. Start the API server
```bash
python backend/app.py
```
API runs on `http://localhost:5001`

### 4. Open the frontend
Open `frontend/index.html` in any browser (or serve with `python -m http.server`).

## API

### POST /analyze
```json
{ "url": "https://example.com" }
```
Returns:
```json
{
  "phishing_score": 87.3,
  "risk_level": "phishing",
  "risk_label": "High Risk",
  "domain": "example.com",
  "ssl": { "valid": true, "issuer": "Let's Encrypt", "days_remaining": 82 },
  "features": { "url_length": 55, "suspicious_keyword_count": 4, ... },
  "shap_factors": [{ "feature": "suspicious_keyword_count", "shap_value": 0.82, "impact": "phishing" }, ...],
  "reasons": [{ "type": "warning", "text": "5 suspicious keywords detected..." }, ...]
}
```

### GET /health
Returns `{ "status": "ok", "model": "xgboost" }`

## Features Analyzed
| Category | Features |
|---|---|
| URL Structure | length, dots, hyphens, at-signs, slashes, digits, path length, query params |
| Domain | IP usage, subdomains, brand impersonation, suspicious TLD, domain length |
| Content | Suspicious keywords, hex encoding, URL shorteners, port numbers |
| SSL | Certificate validity, days remaining, issuer trust |

## Project Structure
```
phishing-detector/
├── backend/
│   ├── app.py              # Flask API
│   ├── feature_extractor.py # URL feature extraction
│   ├── train_model.py       # Model training
│   └── requirements.txt
├── models/                  # Generated after training
│   ├── xgb_model.pkl
│   ├── shap_explainer.pkl
│   └── feature_names.pkl
├── frontend/
│   └── index.html           # UI (no build step needed)
└── README.md
```

## Extending with Real Data
Replace `generate_synthetic_data()` in `train_model.py` with a real dataset.
Recommended: [PhiUSIIL Phishing URL Dataset](https://archive.ics.uci.edu/dataset/967/phiusiil+phishing+url+dataset)
or [ISCX-URL-2016](https://www.unb.ca/cic/datasets/url-2016.html)
