# KNOWN_ISSUES.md
Format: ID · Severity · Status · Description · Workaround/Plan.

| ID | Sev | Status | Description | Plan |
|---|---|---|---|---|
| KI-001 | Info | OPEN | No source code exists yet; all features NOT STARTED | Batches 1–19 |
| KI-002 | Med | OPEN | Model licenses/sizes in ARCHITECTURE §7 are UNVERIFIED | Verify on model cards when each module is implemented; record in DECISIONS |
| KI-003 | Med | OPEN | Handwriting recognition (TrOCR is line-level) may be poor on real notes; CPU slow | Evaluate TrOCR vs VLM on student's own samples in Batch 9; report honestly |
| KI-004 | Low | OPEN | Exact package versions unpinned | Pin after first successful install (Batch 1/6) |
| KI-005 | Med | OPEN | Small local LLMs (≤3B) may be weak at citation formatting & MCQ JSON | Strict prompts, JSON repair, optional hosted LLM or 7B on capable hardware |
| KI-006 | Low | OPEN | Literature references in RESEARCH_NOTES are UNVERIFIED | Developer verifies before citing |
