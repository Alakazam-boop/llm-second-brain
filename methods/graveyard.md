---
title: Methods Graveyard — rejected techniques
type: reference
created: 2026-06-23
updated: 2026-06-23
tags: [meta, rejected, anti-repeat]
---

# Methods Graveyard

> Techniques that were researched and **tested but rejected**, so the loop never blindly re-tries
> them. Check this list before testing a new candidate — if it's already here, skip it (unless there's
> a genuinely new variant or new evidence).
>
> The most important column is **why** — especially "highly rated for humans, but degraded the AI."

Entry format:
```
## <technique name>
- Label: human | AI | both | unknown
- Tested: YYYY-MM-DD · Benchmark: <name> · runs: <n>
- Numbers: accuracy <ctrl% vs cand%> · efficiency <ctrl vs cand>
- Verdict: REJECTED — <reason, e.g. "human memory aid; no AI accuracy gain, +40% tokens">
```

---

*(Empty — created 2026-06-23. Fills up as candidates are tested and rejected.)*
