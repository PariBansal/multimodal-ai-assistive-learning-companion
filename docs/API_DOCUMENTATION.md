# API_DOCUMENTATION.md

Status: DESIGN (no endpoint implemented yet). Implementation status column must be updated per batch.
Base path: `/api/v1`. JSON; auth via `Authorization: Bearer <JWT>` unless noted. Errors: `{ "detail": str, "code": str }`. IDs are UUIDs. Pagination: `?limit=&offset=`. Interactive docs auto-served at `/docs` (OpenAPI).

## Endpoints
| Area | Method & Path | Purpose | Batch | Status |
|---|---|---|---|---|
| Health | GET `/health` | liveness + DB check | 1 | NOT STARTED |
| Auth | POST `/auth/register` | create user | 3 | NOT STARTED |
| Auth | POST `/auth/login` | returns `{access_token, token_type}` | 3 | NOT STARTED |
| Auth | GET `/auth/me` | current user | 3 | NOT STARTED |
| Courses | POST/GET `/courses` · GET/PATCH/DELETE `/courses/{id}` | course CRUD | 3 | NOT STARTED |
| Documents | POST `/courses/{id}/documents` (multipart, multiple files) | upload + enqueue ingest | 4 | NOT STARTED |
| Documents | GET `/courses/{id}/documents` · GET/DELETE `/documents/{id}` | list/detail/delete | 4 | NOT STARTED |
| Documents | GET `/documents/{id}/status` | job status + progress | 4 | NOT STARTED |
| Documents | GET `/documents/{id}/chunks` · `/documents/{id}/transcript` · `/documents/{id}/slides` | derived content | 5–9 | NOT STARTED |
| Documents | GET `/documents/{id}/file` | original file (for citation viewer) | 4 | NOT STARTED |
| Search | POST `/courses/{id}/search` `{query, k, mode: dense|bm25|hybrid, rerank}` | semantic search | 6 | NOT STARTED |
| Chat | POST `/courses/{id}/chat` `{conversation_id?, message, stream?}` | RAG answer with citations (SSE if `stream`) | 7 | NOT STARTED |
| Chat | GET `/courses/{id}/conversations` · GET `/conversations/{id}` | history | 7 | NOT STARTED |
| Topics | GET `/courses/{id}/topics` · POST `/courses/{id}/topics/extract` | topic map | 10 | NOT STARTED |
| Notes | POST `/courses/{id}/notes` `{scope: document|topic|course, ref_id, kind: notes|summary}` · GET list/detail | generated study notes | 10 | NOT STARTED |
| Quizzes | POST `/courses/{id}/quizzes/generate` `{topic_id?, n, difficulty?, kind}` · GET list/detail | question generation | 10 | NOT STARTED |
| Quizzes | POST `/quizzes/{id}/attempts` · POST `/attempts/{id}/answers` · POST `/attempts/{id}/finish` | take quiz | 10–11 | NOT STARTED |
| Flashcards | POST `/courses/{id}/flashcards/generate` · GET `/courses/{id}/flashcards/due` · POST `/flashcards/{id}/review` `{quality 0-5}` | SM-2 spaced repetition | 10 | NOT STARTED |
| Adaptive | GET `/courses/{id}/learning/next` | next recommended question/topic | 11 | NOT STARTED |
| Adaptive | GET `/courses/{id}/learning/mastery` | per-topic mastery | 11 | NOT STARTED |
| Analytics | GET `/courses/{id}/analytics/overview` · `/analytics/topics` · `/analytics/activity` | dashboard data | 11 | NOT STARTED |
| Alignment | GET `/courses/{id}/alignments?slide_doc=&audio_doc=` | slide↔audio segments | 15 | NOT STARTED |
| Admin/Research | POST `/research/eval/run` (dev only, flag-gated) | run experiments | 17 | NOT STARTED |

## Core Schemas
**Locator** (in chunks/citations):
```json
{"type":"pdf","page":4}
{"type":"slide","slide":7}
{"type":"audio","start":132.4,"end":171.0}
{"type":"image","bbox":[x0,y0,x1,y1]}
{"type":"text","line_start":10,"line_end":22}
```
**Chat response**:
```json
{
 "conversation_id":"uuid","message_id":"uuid",
 "answer":"... [S1] ... [S2]",
 "citations":[{"marker":"S1","chunk_id":"uuid","document_id":"uuid","document_name":"Lec3.pdf","locator":{"type":"pdf","page":4},"snippet":"...","score":0.82,"supported":true}],
 "confidence":0.74,"abstained":false
}
```
**Document status**: `{status, step, progress, error}`.
**Search hit**: `{chunk_id, document_id, text, locator, score, modality}`.

## Conventions
- All course-scoped routes verify the caller owns the course (403/404).
- Long-running work is always async via jobs; endpoints return `202` + job/document id.
- Update this file whenever a route is added/changed (Status → IMPLEMENTED/DONE).
