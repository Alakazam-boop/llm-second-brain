---
title: Methods Registry
type: reference
created: 2026-06-23
updated: 2026-06-23
tags: [meta, methods, registry]
---

# Methods Registry

> Every method the self-improvement loop watches over. `/improve-methods` reads this first, then
> reviews each one. To bring a NEW method under self-improvement, add a row here.

| Method | File | Version | Last reviewed | Benchmark |
|--------|------|---------|---------------|-----------|
| Research / learning loop | `commands/learn.md` | v3.1 | 2026-06-29 (fact-check bar: ≥2 sources/claim; scholar sources; Researcher D0; team wiring) | `methods/benchmarks/learn-benchmark.md` ✅ FROZEN (Newton's mechanics) |
| Ask-the-brain | `commands/ask.md` | v1.0 | 2026-06-29 (created) | `learn-benchmark.md` (indirect — shares the Researcher) |
| Second Brain Researcher | `tools/researcher.py` | v1.1 | 2026-06-30 (graph excludes inferred links — catalog-only, consistency cleanup) | `methods/benchmarks/sbm-retrieval-benchmark.md` ✅ FROZEN |
| SBM (5-agent system) | `tools/*.py` + `commands/sbm.md` | v1.1 | 2026-06-30 (courtroom: links_in fix in cleaner + writeback idempotency in connect adopted; B held pending) | `methods/benchmarks/sbm-retrieval-benchmark.md` ✅ FROZEN (Researcher retrieval, n=10) + per-fix reproductions |
| SBM Researcher retrieval benchmark | `methods/benchmarks/sbm-retrieval-benchmark.md` + `tools/benchmarks/` | v1.0 | 2026-06-30 (created; 10-query gold-set, P@3/recall/MRR regression guard) | self (frozen) |
| Documentation standard | `DOCUMENTATION-METHOD.md` | v1.1 | 2026-06-23 (added TYPE 7 Personalized-Explanation) | `methods/benchmarks/learn-benchmark.md` |
| Human-conversion (Personalized-Explanation) | `commands/explain.md` + `wiki/entities/learning-profile.md` | v1.1 | 2026-06-29 (Researcher pre-pass) | **the user's feedback** (not an AI exam) |
| Self-improvement (meta) | `SELF-IMPROVEMENT-METHOD.md` + `commands/improve-methods.md` | v2.1 | 2026-06-29 (scope = all commands + itself; endless; permanent human-approval gate) | — (protected; human-approved changes only) |

---
*Version bumps and review dates are updated automatically by `/improve-methods`.*
