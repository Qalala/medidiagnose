# MediDiagnose

An AI medical diagnosis platform where patients describe symptoms in natural language and receive disease predictions from a fine-tuned **BERT** model, with explainability support and separate **patient** / **doctor** dashboards.

The stack is:
- **Frontend**: React + Vite + Tailwind CSS
- **Backend**: Node.js + Express + MongoDB
- **AI**: Python + PyTorch / Transformers (fine-tuned BERT) with SHAP/LIME-oriented explainability hooks

> Educational / research prototype. Not a real medical device and not a substitute for professional clinical judgment.

## Tech stack
- React
- Vite
- Tailwind CSS
- Node.js / Express
- MongoDB / Mongoose
- JWT authentication
- Python
- PyTorch
- Fine-tuned BERT (Transformers)
- SHAP / LIME (explainability)

## Architecture (high level)

```
Patient / Doctor browsers
        │
        ▼
┌─────────────────────┐
│ React + Vite UI     │  landing, auth, symptom checker, dashboards
│ frontend/           │
└──────────┬──────────┘
           │ REST + JWT
           ▼
┌─────────────────────┐
│ Express API         │  auth, diagnosis sessions, roles, uploads
│ backend/server.js   │
└──────────┬──────────┘
           │
     ┌─────┴──────┐
     ▼            ▼
 MongoDB     Python inference
             predict_disease.py
             symptom_disease_model/
```

### Frontend pages (`frontend/src/pages`)
- `Landing` — product entry
- `Login` / `Register` — authentication
- `SymptomChecker` — patient symptom description → diagnosis request
- `PatientDashboard` / `PatientProfile` / `DiagnosisHistory`
- `DoctorDashboard` / `DoctorPatients` / `DoctorProfile`
- `AdminDashboard` / `ManageUsers` — admin-oriented management

### Backend responsibilities
- JWT auth + role separation (patient / doctor / admin patterns)
- Diagnosis request orchestration
- Calls into Python prediction script(s)
- Persistence of users/sessions/cases in MongoDB
- Optional XAI figure generation utilities

### AI / explainability
- Fine-tuned BERT classifier for symptom text → disease prediction
- Tokenizer/config metadata ships under `backend/symptom_disease_model/`
- **Binary weights** (`model.safetensors`, etc.) are **not included** in this GitHub repo
- Place your trained weights in `backend/symptom_disease_model/` or retrain via `backend/train_model.py`
- SHAP/LIME-oriented tooling supports explainable symptom importance views

### Datasets / notebooks
- CSV disease/symptom resources in the repo root
- `notebooks/` for training analysis and presentation figures

---

## Running the project

### Prerequisites
- Node.js 16+ (18+ recommended)
- Python 3.8+
- MongoDB local or Atlas
- pip packages from `backend/requirements.txt`
- Model weights if you want live AI inference

### 1) Python dependencies

```bash
cd backend
pip install -r requirements.txt
copy .env.example .env
```

Example `.env`:

```env
PORT=5000
NODE_ENV=development
MONGODB_URI=mongodb://localhost:27017/medical-diagnosis
JWT_SECRET=change_me_to_a_long_random_string
FRONTEND_URL=http://localhost:5173
PYTHON_PATH=python
```

### 2) Node dependencies

From repo root:

```bash
npm run install:all
```

### 3) Model weights
Ensure `backend/symptom_disease_model/` contains your fine-tuned files, especially:
- `config.json` + tokenizer files (included)
- `model.safetensors` or equivalent weights (**you provide**)

Without weights, API/UI can still be explored, but prediction will fail or degrade.

### 4) Start development

```bash
npm run dev
```

- Frontend: `http://localhost:5173`
- Backend: `http://localhost:5000`

You can also run sides separately:

```bash
npm run dev:backend
npm run dev:frontend
```

---

## How to navigate the repo
- UI: `frontend/src/pages`, `frontend/src/components`
- API entry: `backend/server.js`
- Routes / models / middleware: `backend/routes`, `backend/models`, `backend/middleware`
- Inference: `backend/predict_disease.py`
- Training: `backend/train_model.py`
- Model folder: `backend/symptom_disease_model/`
- Research notebooks: `notebooks/`
- Docs: `documentations/`

---

## Demo walkthrough
1. Start MongoDB.
2. Install Python + Node dependencies and configure `.env`.
3. Place model weights (for AI path).
4. Run `npm run dev`.
5. Register a patient account, open Symptom Checker, submit a symptom description.
6. Review diagnosis output / history on the patient dashboard.
7. Register/login as doctor and review patient cases on the doctor dashboard.

### Notes / limitations
- Model binaries intentionally omitted from GitHub
- `.env` and `node_modules` are gitignored
- Medical predictions are experimental and for academic demonstration only
- Some advanced XAI visuals depend on optional Python scientific packages and generated figures

---

## Project purpose
Graduation-style full-stack + NLP system: turn free-text symptoms into disease predictions with role-based dashboards and explainability support.
