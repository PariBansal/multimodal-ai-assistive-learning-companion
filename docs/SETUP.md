# SETUP.md

## Developer Environment (fill in during Batch 1)
OS: ___ · RAM: ___ · CPU: ___ · GPU: none/___ · Python: ___ · Node: ___ · Docker: yes/no

## Prerequisites
- Python 3.11 (3.10–3.12 acceptable), Node.js LTS, Git, VS Code.
- PostgreSQL: via Docker Desktop (recommended) **or** native install.
- System tools (needed from the batch that uses them): `ffmpeg` (Batch 8), `tesseract-ocr` (Batch 9), `poppler` (scanned PDF → image, Batch 9), optional LibreOffice (slide thumbnails).
- Optional: Ollama (local LLM, Batch 7).

## First-time Setup (target commands; verified/updated in Batch 1)
```bash
git clone <repo> && cd mm-learning-companion
cp .env.example .env                      # edit secrets
docker compose up -d db                   # PostgreSQL
cd backend
python -m venv .venv && source .venv/bin/activate     # Windows: .venv\Scripts\activate
pip install -r requirements/base.txt -r requirements/dev.txt
alembic upgrade head
uvicorn app.main:app --reload             # http://localhost:8000/docs
pytest -q
```
ML extras (Batch 6+): `pip install -r requirements/ml-cpu.txt` (torch CPU wheel index; see file header).
Worker (Batch 4+): `python -m app.workers.runner`.
Frontend (Batch 12+): `cd frontend && npm install && npm run dev` → http://localhost:5173.
Local LLM (Batch 7+): `ollama pull <model from .env LLM_MODEL>`.

## Environment Variables (`.env.example`, defined in Batch 1)
`DATABASE_URL`, `SECRET_KEY`, `ACCESS_TOKEN_EXPIRE_MINUTES`, `DATA_DIR`, `ML_BACKEND` (real|fake), `EMBEDDING_MODEL`, `RERANKER_MODEL`, `ASR_MODEL_SIZE`, `OCR_ENGINE`, `VLM_MODEL`, `LLM_BASE_URL`, `LLM_MODEL`, `LLM_API_KEY` (optional), `HF_HOME`, `CORS_ORIGINS`.
Never commit `.env`, uploaded files, FAISS indexes, or model caches.

## Using Claude Across Accounts
1. Open a new chat in any account; upload `CLAUDE.md`, `PROJECT_CONTEXT.md`, `PROGRESS.md`, last entries of `TASK_LOG.md`, `DECISIONS.md`, `KNOWN_ISSUES.md`.
2. Also upload the specific source files the next batch touches (Claude cannot see your disk).
3. Paste the Handoff Prompt from `CLAUDE.md`.
4. After the batch: save files, run verify commands, paste doc updates into the repo, commit.
Tip: commit to Git after each batch so you can roll back bad generations.

## Troubleshooting
(Append entries as problems are solved.)
