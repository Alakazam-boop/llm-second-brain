---
title: SBM Researcher Retrieval Benchmark — frozen regression guard
type: reference
created: 2026-06-30
updated: 2026-06-30
tags: [meta, benchmark, sbm, retrieval, frozen]
frozen: true
---

# SBM Researcher Retrieval Benchmark (v1.0)

> The Newton **learn-benchmark** measures *research/learning* quality and **cannot** judge the SBM's
> retrieval/catalog code. This benchmark fills that gap for the **Researcher** component
> (`tools/researcher.py`): a frozen retrieval gold-set so any ranking/graph/weighting change
> can be A/B'd for regressions. Created by `/improve-methods` (slash sbm, 2026-06-30).

## What it is (and is NOT)
- ✅ A **regression guard**: "did this change obviously break retrieval?"
- ❌ **NOT** a significance test. n=10 queries is far below the IR rule-of-thumb of ≥25 topics for a
  stable system ranking (Sakai, *Topic Set Size Design*, 2016; Voorhees). Read the per-query table, not
  just the mean. A win here is **necessary but not sufficient** — the synthetic corpus ≠ the user's real vault.

## Files (runnable)
- `tools/benchmarks/make_fixture.py` — builds the FROZEN 10-note fixture deterministically.
- `tools/benchmarks/score.py` — holds the frozen gold-set + scorer; prints per-query + mean
  precision@3 / recall / MRR.
- `tools/benchmarks/fixture\` — the generated corpus (regenerate with `make_fixture.py`).

## Protocol (A/B)
```
cd tools/benchmarks
python score.py                 # current researcher: build fixture, scan, clean, score
# ...apply a candidate change to researcher.py on a branch/backup...
python score.py --no-build      # re-score on the same fixture
```
Adopt a change only if **mean P@3 and mean MRR both rise (or one rises, neither falls)** AND the two
pure-lexical orphan queries (#8 quantum mechanics, #9 ancient pottery) do **not** regress — those guard
against a graph change that helps hubs but breaks isolated-note recall.
`--connect` runs the Connection Finder first, exposing inferred-link behaviour (used to A/B candidate B).

## Frozen gold-set (binary relevance; mirror of `score.py`)
| # | Query | Expected-relevant |
|---|---|---|
| 1 | potassium fruit snack | banana.md |
| 2 | vitamins fiber healthy snack | apple.md |
| 3 | orchard fruit trees seasons | hub-orchard.md, hub-seasons.md |
| 4 | recipes smoothies pies salads | hub-recipes.md |
| 5 | stone fruit cherry | notes/cherry.md |
| 6 | grape vines | notes/grape.md |
| 7 | nutrition vitamins potassium | hub-nutrition.md |
| 8 | quantum mechanics | orphan-one.md  *(pure-lexical orphan)* |
| 9 | ancient pottery | notes/orphan-two.md  *(pure-lexical orphan)* |
| 10 | fruit | apple.md, banana.md, notes/cherry.md, notes/grape.md |

## Metrics
- **precision@3** = |relevant ∩ top-3| / 3 (caps at 0.33 for single-answer queries — expected).
- **recall** = |relevant ∩ returned| / |relevant|.
- **MRR** = 1 / rank-of-first-relevant (0 if none).

## Baseline (frozen reference — current `researcher.py`, embeddings ON, 2026-06-30)
| run | P@3 | recall | MRR |
|---|---|---|---|
| no connect | 0.400 | 1.000 | 0.950 |
| with connect, inferred links IN graph (current) | 0.400 | 1.000 | 1.000 |
| with connect, inferred links OUT (candidate B) | 0.400 | 1.000 | 1.000 |

> 📌 Finding logged: candidate B (excluding inferred links from the Researcher graph) made **no
> measurable difference** to retrieval with embeddings on. The scary grape>cherry swap only appears
> with embeddings OFF. So B is a code-consistency question, not a retrieval-quality one.

## Caveats (state every time this is cited)
- n=10 → regression guard only, never a significance claim. Do NOT apply multiple-comparisons machinery
  (BH etc.) across the 10 queries; at this n it has negligible power and misleads.
- Binary relevance only; graded nDCG omitted (we lack graded judgments; on 10 notes it would be noise).
- Synthetic corpus; a pass here is necessary, not sufficient, for the live vault.

*References: Pinecone, Evaluation Measures in IR (2023-06-30); Sakai, Topic Set Size Design (2016/2018);
precision@k / recall / MRR standard definitions.*
