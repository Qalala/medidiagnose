# MediDiagnose

## Brief description
An AI medical diagnosis platform where patients describe symptoms and receive disease predictions from a fine-tuned BERT model, with explainable results and separate patient and doctor dashboards.

## Project purpose
Graduation project demonstrating full-stack engineering plus applied NLP: symptom text in, disease prediction out, with XAI-style explanations and role-based dashboards.

## Project requirements
- Node.js 16+
- Python 3.8+ with packages from `backend/requirements.txt`
- MongoDB (local or Atlas)
- Fine-tuned model weights (not included in this repo — see below)

### Model weights
Binary model files such as `model.safetensors` are intentionally **not** uploaded (size / distribution). Tokenizer/config metadata under `backend/symptom_disease_model/` is included. Place your trained weights in that folder before enabling AI inference, or retrain using `backend/train_model.py`.

## How to navigate
- `frontend/` — React + Vite patient/doctor UI
- `backend/` — Express API, auth, prediction scripts
- `backend/predict_disease.py` — Python inference entry
- `backend/symptom_disease_model/` — tokenizer/config (+ your weights)
- `notebooks/` — experimentation notebooks
- `documentations/` — project docs

## How to run
```bash
# Python deps
cd backend
pip install -r requirements.txt
copy .env.example .env

# Node deps
cd ..
npm run install:all

# Dev (API + UI)
npm run dev
```

- Frontend: `http://localhost:5173`
- Backend: `http://localhost:5000`