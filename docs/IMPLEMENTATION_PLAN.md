# IMPLEMENTATION_PLAN.md

Each **Batch** = one Claude Free chat session (small, testable, shippable). Do batches in order unless PROGRESS.md says otherwise. Every batch: code + tests + doc updates.

## Roadmap Overview
| Batch | Milestone | Deliverables | Depends on |
|---|---|---|---|
| 0 | Architecture & docs | 12 docs (DONE) | — |
| 1 | Repo scaffold & backend skeleton | folder tree, FastAPI app, config, health route, Docker Postgres, Alembic init, pytest harness, ruff, requirements split, `.env.example`, Makefile | 0 |
| 2 | DB models + migrations | all SQLAlchemy models from ARCHITECTURE §6, first Alembic migration, session deps, model tests | 1 |
| 3 | Auth + courses | register/login (JWT), `/me`, course CRUD, ownership checks, tests | 2 |
| 4 | Document upload & job queue | upload endpoint, file store, sha256 dedupe, `processing_jobs`, worker runner (no-op steps), status endpoints | 3 |
| 5 | Text/PDF/PPTX parsing + chunker | parsers, structure-aware chunker, locators, unit tests with fixtures | 4 |
| 6 | Embeddings + FAISS + search | ML registry, embedding wrapper (fake+real), indexer, `/search` dense, BM25 + RRF, tests | 5 |
| 7 | RAG tutor v1 | retriever, reranker, prompts, LLM client (Ollama), `/chat` with citations + abstention, conversation memory | 6 |
| 8 | Audio pipeline | ffmpeg, faster-whisper, transcript segments, audio chunks w/ timestamps | 4,6 |
| 9 | Image/OCR pipeline | OCR (printed), HTR (handwriting), VLM caption, image chunks, scanned-PDF OCR path | 4,6 |
| 10 | Study tools | notes, topic extraction, topic summaries, MCQ/exam gen, flashcards + SM-2 | 7 |
| 11 | Adaptive learning + analytics | mastery (BKT/Elo), difficulty selection, empirical difficulty, events, analytics endpoints | 10 |
| 12 | Frontend A | Vite+TS+Tailwind scaffold, auth, courses, upload/library, job status | 3,4 (can start after 4) |
| 13 | Frontend B | chat with citations viewer, search, notes/summary view | 7,10,12 |
| 14 | Frontend C | quiz, flashcards, adaptive session, analytics dashboard | 11,13 |
| 15 | Cross-modal alignment (research) | aligner, gold-label tool, eval script | 8,5 |
| 16 | Evidence verification (research) | NLI verifier, faithfulness eval, abstention thresholds | 7 |
| 17 | Experiments & benchmarks | retrieval comparison, alignment eval, faithfulness eval, adaptive simulation, result tables/figures | 6,15,16,11 |
| 18 | Hardening & integration tests | e2e tests, error handling, perf logs, security pass | all |
| 19 | Report & demo | final docs, README, demo script, thesis tables | all |

Frontend batches (12–14) can interleave with backend batches 8–11 to keep a visible demo.

## Batch Definitions of Done (generic)
- Code runs locally; tests pass (`pytest -q`); lint clean (`ruff check`).
- Docs updated: PROGRESS, TASK_LOG, DECISIONS (if any), API_DOCUMENTATION (if endpoints), TESTING, KNOWN_ISSUES.
- No placeholder/stub left unrecorded (record in KNOWN_ISSUES).

## Batch 1 Detail (next)
Create: root files (`.gitignore`, `.env.example`, `docker-compose.yml`, `Makefile`), `backend/` skeleton per ARCHITECTURE §8 with empty `__init__.py`, `core/config.py` (pydantic-settings), `main.py` with `/api/v1/health`, `alembic init` config pointing to settings, `tests/conftest.py` + health test, `requirements/{base,dev}.txt` (ML extras deferred to Batch 6, but `ml-cpu.txt` drafted), SETUP.md refinements. Verify: `docker compose up -d db`, `uvicorn app.main:app`, `pytest`.

## Risk Register
| Risk | Mitigation |
|---|---|
| Free-tier usage limits per chat | small batches; docs as memory; paste-ready doc updates |
| CPU too slow for LLM/ASR | small models, int8/Q4, background jobs, optional hosted LLM, Colab for benchmarks |
| Handwriting OCR quality | test TrOCR vs VLM on own samples; document accuracy honestly |
| Lost context across accounts | CLAUDE.md protocol; repo is truth |
| Model license issues | verify model cards; record in DECISIONS |
| Scope creep | MoSCoW in PROJECT_CONTEXT; research items isolated in `research/` |
