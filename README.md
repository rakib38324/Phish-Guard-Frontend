# 🛡️ PhishGuard — AI Phishing Detection System

PhishGuard is an AI-powered phishing URL detection system using **XGBoost + SHAP explanations**.  
It analyzes URLs using **32+ security features** including URL structure, domain signals, and SSL certificate data.

---

## 🚀 Live Architecture

### 🟢 Frontend UI
👉 https://rakib38324.github.io/Phish-Guard-Frontend/

- Static HTML 
- Fetches predictions from backend API
- Displays risk score + explanations

---

## 🧠 Tech Stack
- **Backend**: Python / Flask / XGBoost / SHAP
- **Frontend**: Vanilla HTML/CSS/JS (drop-in, no build step)
- **Features**: 32 URL + SSL signals extracted per URL
---

## ⚙️ Backend Setup (Phish-Guard-Server)

### 1. Clone the repository
```bash
https://github.com/rakib38324/Phish-Guard-Server.git
cd Phish-Guard-Server
```
### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Train the model (run once)
```bash
python train_model.py
```
This generates `models/xgb_model.pkl`, `models/shap_explainer.pkl`, `models/feature_names.pkl`.

### 4. Start the API server
```bash
python app.py
```
API runs on `http://127.0.0.1:5001`, and in the frontend, you need to change the API live to `http://127.0.0.1:5001`.

### 5. Open the frontend
clone frontend repository
```bash
https://github.com/rakib38324/Phish-Guard-Frontend.git
cd Phish-Guard-Frontend
```
Open `index.html` in any browser (or serve with `python -m http.server`).


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

Phish-Guard-Server/
│   ├── app.py              # Flask API
│   ├── feature_extractor.py # URL feature extraction
│   ├── train_model.py       # Model training
│   ├── requirements.txt
│   ├── models/                  # Generated after training
│         ├── xgb_model.pkl
│         ├── shap_explainer.pkl
│         └── feature_names.pkl
└── README.md
```
```
Phish-Guard-Frontend/
├──  index.html           # UI (no build step needed)
└── README.md
```
