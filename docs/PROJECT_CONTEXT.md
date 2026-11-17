# PROJECT_CONTEXT.md

## Identity
- **Name:** Multimodal AI Assistive Learning Companion
- **Type:** B.Tech (AI & ML) final-year project; software + research component.
- **Developer constraints:** Claude Free (web), multiple accounts, no guaranteed GPU, local VS Code development, limited chat usage per account.

## Problem
Students hold course material in many forms (PDF notes, slides, lecture recordings, photos of handwritten notes). Studying across them is slow; generic chatbots hallucinate and cannot cite the student's own material.

## Objective
A platform that ingests multimodal course material, builds a unified searchable knowledge base, and provides **evidence-grounded** tutoring and study tools that **adapt to the learner**.

## Inputs
PDF, PPT/PPTX, audio (mp3/wav/m4a), images (png/jpg, incl. handwritten notes), text (txt/md).

## Features (target scope)
| # | Feature | Priority |
|---|---------|----------|
| F1 | Document understanding (parse, structure, chunk) | Must |
| F2 | Audio transcription with timestamps | Must |
| F3 | OCR + image understanding (printed + handwritten) | Must |
| F4 | Transformer semantic search (dense + BM25 hybrid) | Must |
| F5 | RAG with citations to source (page/slide/timestamp/region) | Must |
| F6 | Context-aware AI tutor (conversation memory per course) | Must |
| F7 | Auto study notes | Must |
| F8 | Topic-wise summaries | Must |
| F9 | MCQ / exam question generation | Must |
| F10 | Flashcards + spaced repetition | Must |
| F11 | Adaptive learning (difficulty + topic mastery) | Must (research) |
| F12 | Learning analytics dashboard | Should |
| F13 | Evidence-based answers with verification/abstention | Must (research) |
| F14 | Lecture-audio ↔ slide cross-modal alignment | Should (research) |

## Tech Stack (fixed by developer)
Backend: Python, FastAPI, SQLAlchemy, PostgreSQL, Alembic.
AI/ML: PyTorch, HF Transformers, Sentence-Transformers, Whisper, FAISS, OCR, vision-language models.
Frontend: React, Vite, TypeScript, Tailwind CSS.
Testing: Pytest, API integration tests, frontend tests.
(Additions beyond this require an ADR in DECISIONS.md. Current approved additions: see DECISIONS.md.)

## Research Contributions (to *investigate*, not claim)
1. Cross-modal lecture-slide alignment.
2. Evidence-grounded answers (citation faithfulness).
3. Adaptive difficulty-based learning.
4. Embedding/retrieval model comparison on educational material.
Details/methodology: `RESEARCH_NOTES.md`.

## Non-Goals (v1)
Training large models; multi-tenant cloud scaling; mobile apps; real-time live-lecture streaming; video processing (audio track only); proctoring.

## Glossary
RAG, chunk, locator (page/slide/timestamp/bbox pointer to source), citation, mastery, ZPD, RRF (reciprocal rank fusion), SM-2 (spaced repetition), BKT (Bayesian Knowledge Tracing), ADR (architecture decision record).

## Success Criteria
- End-to-end demo: upload → process → ask question → cited answer → quiz → flashcards → analytics.
- Runs on a 16 GB RAM CPU-only laptop (slower but functional).
- Measured experiments for the 4 research items with reproducible scripts.
- Test suite passing; documented setup reproducible by a new machine.
