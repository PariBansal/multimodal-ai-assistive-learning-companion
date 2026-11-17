# CLAUDE.md — Read This First (Every Session, Every Account)

## Project
**Multimodal AI Assistive Learning Companion** — B.Tech AI & ML final-year project.
Ingests PDF / PPT(X) / lecture audio / images & handwritten notes / text → transcription, OCR, semantic search, RAG tutoring with source citations, notes, summaries, MCQs, flashcards, adaptive learning, analytics.

## Working Environment (IMPORTANT)
- Developer uses **Claude Free on the website (not Claude Code)**, across **multiple accounts**.
- Claude **cannot** see the developer's local files or run their commands. It sees only files the developer uploads/pastes in the chat.
- The **repository is the single source of truth**. Chat history is NOT memory. These docs ARE memory.
- Developer manually downloads Claude's files into local VS Code.

## Session Start Protocol (mandatory)
1. Read, in order: `CLAUDE.md` → `PROJECT_CONTEXT.md` → `PROGRESS.md` → `TASK_LOG.md` (last 5 entries) → `DECISIONS.md` → `KNOWN_ISSUES.md`. Read `ARCHITECTURE.md` / `API_DOCUMENTATION.md` / `TESTING.md` only if the task needs them.
2. If the developer pasted/uploaded source files, **inspect them** before writing anything. If the needed files were not provided, **ask the developer to upload them** (list exact paths). Never assume contents.
3. Identify the **next batch** from `IMPLEMENTATION_PLAN.md` / `PROGRESS.md`. State it in 2 lines, then build it.

## Rules
- **Never recreate completed features.** Check `PROGRESS.md` status first. Never overwrite working code; extend it. If a rewrite is truly needed, record why in `DECISIONS.md`.
- Follow the folder structure and interfaces in `ARCHITECTURE.md`. Do not invent new layers.
- Do not change a recorded decision silently. Add a new ADR in `DECISIONS.md` that supersedes it.
- **No novelty claims** without verified literature (see `RESEARCH_NOTES.md`). Citations marked UNVERIFIED must be checked by the developer.
- Do not invent package versions, model licenses, or benchmark numbers. Mark unverified items `UNVERIFIED`.
- Assume **no NVIDIA GPU**. CPU-first. GPU paths are optional.
- Prefer small pretrained models; never train large models from scratch.
- Keep batches small (one milestone slice per chat) to fit Claude Free limits. Provide complete files, no "rest unchanged" placeholders for files being delivered.
- Every code batch ships with its tests (see `TESTING.md`).

## Delivery Protocol (end of every batch)
Deliver:
1. Complete new/changed source files (downloadable if file creation is available; else labelled code blocks with exact repo paths).
2. **Doc updates** as paste-ready entries: `PROGRESS.md` status rows changed, a new `TASK_LOG.md` entry, new `DECISIONS.md` ADRs, new `KNOWN_ISSUES.md` items, `API_DOCUMENTATION.md` additions, `TESTING.md` additions. For small edits give the snippet + location; for large changes deliver the whole file.
3. "Run & verify" commands for the developer (exact, copy-paste) and expected output.
4. "Next batch" line.

## Status Vocabulary
`NOT STARTED` · `IN PROGRESS` · `IMPLEMENTED (untested)` · `DONE (tested)` · `BLOCKED` · `DEFERRED`

## Handoff Prompt for a New Claude Account
> Read the attached docs (CLAUDE.md first) and any source files I uploaded. Follow the Session Start Protocol. Continue with the next batch in PROGRESS.md. Do not redo completed work.

## Doc Index
PROJECT_CONTEXT · ARCHITECTURE · IMPLEMENTATION_PLAN · PROGRESS · TASK_LOG · DECISIONS · API_DOCUMENTATION · TESTING · SETUP · KNOWN_ISSUES · RESEARCH_NOTES
