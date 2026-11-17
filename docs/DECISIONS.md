# DECISIONS.md — Architecture Decision Records
Format: ID · Status (ACCEPTED / SUPERSEDED / PROPOSED) · Context · Decision · Consequences. Never edit an accepted ADR's decision; supersede it with a new one.

## ADR-001 · ACCEPTED · CPU-first, pretrained models only
Context: developer may lack an NVIDIA GPU; project is a B.Tech final year. Decision: use small pretrained models (int8/Q4 where possible); no large-scale training; GPU optional. Consequences: slower inference; choose small defaults; use Colab/Kaggle for heavy benchmarks only.

## ADR-002 · ACCEPTED · Repository docs are permanent memory
Context: multiple Claude Free accounts, no shared history. Decision: CLAUDE.md protocol + PROGRESS/TASK_LOG/DECISIONS updated every batch. Consequences: each batch must output doc updates.

## ADR-003 · ACCEPTED · FAISS (faiss-cpu) for vector search; PostgreSQL for metadata
Context: stack mandates FAISS + PostgreSQL. pgvector would simplify but is outside the stated stack. Decision: FAISS index per (course, embedding model), vector id = chunk id mapping via `chunks.faiss_id`; indexes rebuildable from DB. Consequences: need index persistence + rebuild script; metadata filtering done post-retrieval or via per-course indexes.

## ADR-004 · ACCEPTED · Unified `chunks` table with JSON locator across modalities
Decision: one retrieval unit type with locator (pdf page / slide / audio time range / image bbox). Consequences: citations and cross-modal alignment are uniform; locator schema documented in API_DOCUMENTATION.

## ADR-005 · ACCEPTED · DB-backed job queue + polling worker (no Celery/Redis in v1)
Context: minimal infrastructure for a student laptop. Decision: `processing_jobs` table, `python -m app.workers.runner`. Consequences: simple, restart-safe; lower throughput. Upgrade path: RQ/Arq/Celery behind the same `enqueue()` interface.

## ADR-006 · ACCEPTED · Split requirements, ML extras optional, pin after first install
Decision: `requirements/{base,ml-cpu,dev}.txt`; torch from CPU wheel index; lock file generated on developer machine in Batch 1/6. Consequences: tests run with `ML_BACKEND=fake` without heavy installs.

## ADR-007 · ACCEPTED · Model registry + interfaces; models chosen via config
Decision: all models behind abstract interfaces in `app/ml/`, selected by settings; defaults per ARCHITECTURE §7. Consequences: embedding comparison study needs no code change; license verification recorded per model before use.

## ADR-008 · ACCEPTED · Local-first LLM via OpenAI-compatible endpoint (Ollama/llama.cpp); optional hosted provider
Decision: `LLMClient` talks to an OpenAI-compatible HTTP API; default base URL = local Ollama. Consequences: no API cost by default; quality limited by small models; prompts must be robust to weaker models (strict output formats + JSON repair).

## ADR-009 · PROPOSED · Avoid PyMuPDF (AGPL) by default
Use pypdf + pdfplumber; revisit only with advisor approval. Confirm in Batch 5.
