---
title: Methods Evolution Log
type: method
created: 2026-06-23
updated: 2026-06-23
tags: [meta, changelog, evidence]
---

# Methods Evolution Log

> Append-only record of every method change the self-improvement loop **adopted**, with the evidence.
> Each entry is enough to understand *and reverse* the change. Rejected ideas live in `graveyard.md`.

Entry format:
```
## [YYYY-MM-DD] <method file> v<old> → v<new>
- Change: <what changed, one line>
- Source: <where the idea came from> · Label: human | AI | both
- Benchmark: <name> · runs: <n>
- Numbers: accuracy <old%→new%> · efficiency <old→new> · reliability <note>
- Decision: ADOPTED (clear win) | ADOPTED-after-approval
- Backup: methods/backups/<file>.<date>.bak
```

---

## [2026-06-30] sbm_cleaner.py v1.0 → v1.1  (Candidate A — links_in contamination)
- Change: `links_in` count now filters `AND link_type IN('wikilink','markdown')` (line 57), mirroring
  line 56's `links_out` discipline, so Connection-Finder **inferred** proposals (link_type='inferred')
  no longer inflate inbound-link counts feeding orphan detection + Pattern Finder.
- Source: `/improve-methods` slash sbm courtroom · Label: AI (internal correctness)
- Benchmark: deterministic fixture reproduction · runs: end-to-end verified
- Numbers: after a `connect` run, banana.md links_in OLD(buggy)=3 → NEW(fixed)=2; links_in now
  identical before vs after connect (no inflation). Earlier fixture: +5 total inflation eliminated.
- Decision: ADOPTED-after-approval (the user yes, 2026-06-30). Judge verdict: ADOPT-NARROWED (split) —
  narrowed from the originally-proposed `source='catalog'` to the link_type filter to avoid a new
  filter-axis asymmetry.
- Backup: methods/backups/sbm_cleaner.py.2026-06-30.bak

## [2026-06-30] sbm_connect.py v1.0 → v1.1  (Candidate C — writeback idempotency)
- Change: `--writeback` now skips appending an SBM suggested-link block if that exact block+target
  (`> [!note] SBM suggested link\n> [[target]]`) already exists in the note — block-scoped, NOT a bare
  `[[target]]` substring (which would silently suppress on incidental name mentions).
- Source: `/improve-methods` slash sbm courtroom · Label: AI (internal correctness)
- Benchmark: direct idempotency test · runs: 3 write attempts
- Numbers: 3 writeback attempts → exactly 1 block (was: N attempts → N duplicate blocks). Guard does not
  trigger on incidental target-name mentions in prose.
- Decision: ADOPTED-after-approval (the user yes, 2026-06-30). Judge verdict: ADOPT-NARROWED (clear).
- Backup: methods/backups/sbm_connect.py.2026-06-30.bak

## [2026-06-30] researcher.py v1.0 → v1.1  (Candidate B — graph excludes inferred links)
- Change: `_graph()` query now filters `AND source='catalog'` (line 65), so the retrieval graph uses
  real parsed links only and excludes Connection-Finder inferred proposals — matching sbm_connect.py's
  own graph build (the two modules now agree on what an edge is).
- Source: `/improve-methods` slash sbm courtroom · Label: AI (consistency/correctness)
- Benchmark: sbm-retrieval-benchmark (n=10, embeddings ON) · runs: A/B + regression check
- Numbers: current vs fixed = **identical** (P@3 0.400 · recall 1.000 · MRR 1.000). Zero measured
  retrieval impact; the embeddings-OFF grape>cherry swap does not occur with embeddings on.
- Decision: ADOPTED-after-approval (the user yes, 2026-06-30). Judge verdict: USER-DECIDES (uncertain) —
  the user chose to apply as a zero-downside consistency cleanup after the benchmark showed no effect.
- Backup: methods/backups/researcher.py.2026-06-30.bak

## [2026-06-30] NEW asset — SBM Researcher Retrieval Benchmark v1.0
- Change: created `methods/benchmarks/sbm-retrieval-benchmark.md` + `tools/benchmarks/`
  (make_fixture.py, score.py, fixture/). Frozen 10-query retrieval gold-set → P@3/recall/MRR regression
  guard for `researcher.py`. Closes the structural gap where SBM changes had no benchmark gate.
- Source: `/improve-methods` slash sbm · Label: AI (evaluation infrastructure)
- Baseline (embeddings ON): P@3 0.400 · recall 1.000 · MRR 0.950 (no connect) / 1.000 (with connect).
- Note: candidate B (exclude inferred links from researcher graph) A/B'd on this benchmark → **no
  measurable retrieval difference** with embeddings on. B held UNAPPLIED pending the user's call.
- Decision: ADOPTED-after-approval (the user yes, 2026-06-30). Additive (no existing method changed).
