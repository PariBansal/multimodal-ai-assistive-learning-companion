# TESTING.md

## Strategy
| Level | Tool | Scope | Notes |
|---|---|---|---|
| Unit (backend) | pytest | chunker, locators, fusion (RRF), SM-2, mastery update, prompt builders, citation parser | pure functions, fast |
| ML-contract tests | pytest + `ML_BACKEND=fake` | services use interfaces with deterministic fake embedder/LLM/ASR/OCR | no model download in CI/default |
| Integration (API) | pytest + httpx `TestClient`/`AsyncClient` + test PostgreSQL (docker or SQLite only where schema-compatible — prefer Postgres) | auth, courses, upload→job→ready, search, chat shape, quiz flow | DB rolled back per test; Alembic migrations applied in test DB |
| Real-model smoke | pytest marker `@pytest.mark.slow` | 1 tiny sample per modality with real small models | run manually: `pytest -m slow` |
| Frontend | Vitest + React Testing Library (+ MSW for API mocks) | components, forms, API client, citation viewer | Playwright e2e optional (Batch 18) |
| Research eval | scripts in `research/eval/` | retrieval/alignment/faithfulness/adaptive metrics | not part of unit suite; results in `research/results/` |

## Conventions
- Layout: `backend/tests/{unit,integration,fixtures}`; fixtures: small sample PDF, PPTX, 10-s WAV, handwritten-note image, txt.
- Markers: `slow`, `integration`, `requires_ml`.
- Naming: `test_<module>_<behavior>`.
- Coverage target: ≥ 70% backend services/pipelines (report only; no vanity).
- Every batch adds tests; developer reports pass/fail output back into TASK_LOG.
- Lint/type: `ruff check`, `ruff format --check`, optional `mypy app`; frontend `eslint`, `tsc --noEmit`.

## Commands (final forms to be confirmed in Batch 1)
```
cd backend && pytest -q                    # fast suite (fake ML)
pytest -m "not slow" --cov=app             # with coverage
pytest -m slow                             # real small models
cd frontend && npm test                    # vitest
```

## Acceptance Tests (end-to-end, Batch 18)
1. Register → create course → upload PDF+PPTX+audio+image → all `ready`.
2. Ask question answerable from PDF → cited answer with correct page.
3. Ask out-of-scope question → `abstained=true`.
4. Generate quiz → attempt → mastery changes → `learning/next` adapts.
5. Flashcard review schedules next due date per SM-2.

## Benchmark Log (fill with measured values; never guess)
| Date | Machine | Task | Model | Time | Notes |
|---|---|---|---|---|---|
| — | — | — | — | — | — |

## Test Status
No tests exist yet (Batch 1 adds the harness).
