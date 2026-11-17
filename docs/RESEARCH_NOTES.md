# RESEARCH_NOTES.md

**Rule:** no novelty claim until a literature search is done and logged in §5. References below are from model memory and are **UNVERIFIED** — the developer must confirm title/authors/year before citing (KI-006). Do not fabricate DOIs or results.

## 1. Research Questions (RQ)
- **RQ1 Retrieval:** Which embedding/retrieval configuration (dense models, BM25, hybrid RRF, reranker) works best on multimodal educational material (PDF text, slide text, ASR transcripts, OCR text) under CPU constraints?
- **RQ2 Alignment:** How accurately can lecture-audio segments be aligned to slides using embedding similarity with ordering constraints (DTW/Viterbi) vs. naive argmax?
- **RQ3 Grounding:** Does retrieval + reranking + NLI verification + abstention reduce unsupported claims versus plain RAG and no-RAG baselines?
- **RQ4 Adaptivity:** Does difficulty-adaptive question selection (target success probability ≈ 0.7) improve simulated/real learning efficiency over random or fixed-order selection?

## 2. Methodology Outline
### RQ1 — Retrieval comparison
- Corpus: developer's own course material (≥3 courses/≥10 documents across modalities) + optional public sanity sets (e.g. BEIR subsets).
- Queries: build 100–200 question→gold-chunk pairs (manual + LLM-assisted, human-verified); stratify by modality and question type (factoid / conceptual / multi-hop).
- Systems: BM25; all-MiniLM-L6-v2; bge-small/base; e5-small; hybrid (BM25+dense via RRF); +cross-encoder rerank.
- Metrics: Recall@{1,5,10}, MRR@10, nDCG@10, latency/query, index memory, embedding throughput on CPU.
- Ablations: chunk size (128/256/512 tokens), overlap, ASR model size effect on retrieval of audio chunks, OCR noise.
- Stats: bootstrap CIs / paired tests across queries.

### RQ2 — Cross-modal lecture–slide alignment
- Data: lectures with slides (record own or use public lecture+slides); manually label slide-change timestamps for gold (≥5 lectures).
- Method: embed slide text (title+body+notes) and ASR windows (30–60 s); similarity matrix; (a) argmax baseline; (b) monotonic DTW; (c) HMM/Viterbi with transition prior; (d) optionally add slide-image caption text.
- Metrics: slide-assignment accuracy, mean absolute boundary error (s), % boundaries within ±10 s, Window-diff-style segmentation score.
- Output also supports app feature F14.

### RQ3 — Evidence-grounded answers
- Test set: questions answerable (A), partially answerable (P), unanswerable (U) from the corpus.
- Systems: no-RAG LLM; plain RAG; RAG+rerank; RAG+rerank+NLI verification+abstention.
- Metrics: citation precision/recall, claim-level faithfulness (NLI support rate + human audit on a sample), answer correctness (human 1–3 scale), abstention accuracy on U, latency.
- Report inter-rater agreement if more than one rater (e.g., classmate) is used.

### RQ4 — Adaptive learning
- Offline simulation: simulated learners (varying ability, learning rate) answering item bank with known difficulty; compare random, fixed-order, adaptive-ZPD policies on steps-to-mastery and engagement proxy.
- If feasible: small pilot (5–10 peers) with pre/post quiz; keep ethics/consent note; treat as exploratory.
- Mastery models: Elo-style and BKT; difficulty = LLM prior blended with empirical correctness.

## 3. Candidate Literature Leads (UNVERIFIED — verify before use)
- Lewis et al., 2020 — Retrieval-Augmented Generation for Knowledge-Intensive NLP.
- Karpukhin et al., 2020 — Dense Passage Retrieval.
- Reimers & Gurevych, 2019 — Sentence-BERT.
- Johnson, Douze, Jégou — FAISS (billion-scale similarity search with GPUs).
- Radford et al., 2022 — Whisper (robust speech recognition via large-scale weak supervision).
- Thakur et al., 2021 — BEIR benchmark; Muennighoff et al., 2022 — MTEB.
- Xiao et al. — BGE embeddings (C-Pack); Wang et al. — E5 embeddings.
- Gao et al., 2023 — ALCE (enabling LLMs to generate text with citations).
- Es et al., 2023 — RAGAS (RAG evaluation).
- Cormack et al., 2009 — Reciprocal Rank Fusion.
- Li et al., 2021 — TrOCR.
- Corbett & Anderson, 1994/95 — Bayesian Knowledge Tracing; Piech et al., 2015 — Deep Knowledge Tracing; Elo-based learner modelling literature; Wozniak — SM-2 spaced repetition.
- Search terms for novelty check: "lecture video slide alignment", "slide-to-transcript alignment", "lecture segmentation topic", "educational RAG citation", "RAG tutor hallucination education", "adaptive question difficulty LLM generated", "multimodal lecture retrieval".

## 4. Honest Positioning (default until literature check)
The project is an **applied system + empirical comparison study**. Individual components (RAG, Whisper, embeddings, SM-2, BKT) are established. Potential contribution = *integration* for multimodal lecture material and *measured evaluation under CPU constraints*, not algorithmic novelty. Revisit after §5.

## 5. Literature Search Log (developer/Claude to fill)
| Date | Query | Source (Scholar/arXiv/ACL Anthology) | Relevant papers found | Gap observed? |
|---|---|---|---|---|
| — | — | — | — | — |

## 6. Experiment Registry
| Exp ID | RQ | Script | Dataset | Status | Result file |
|---|---|---|---|---|---|
| E1 | RQ1 | research/eval/retrieval_eval.py | own corpus | NOT STARTED | — |
| E2 | RQ2 | research/eval/alignment_eval.py | lectures+slides | NOT STARTED | — |
| E3 | RQ3 | research/eval/faithfulness_eval.py | QA set A/P/U | NOT STARTED | — |
| E4 | RQ4 | research/eval/adaptive_sim.py | simulated | NOT STARTED | — |

## 7. Reproducibility
Fix random seeds; log model names, versions, hardware; save configs in `eval_runs`; store raw outputs in `research/results/`; no test-set tuning.
