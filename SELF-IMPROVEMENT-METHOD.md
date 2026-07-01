---
title: Self-Improvement Method — How the methods improve themselves (safely)
type: method
status: proven
created: 2026-06-23
updated: 2026-06-23
tags: [meta, self-improvement, safeguard, evaluation]
---

# Self-Improvement Method

> The meta-method. It improves the *other* methods — `/learn` (research), `DOCUMENTATION-METHOD.md`,
> and any future method — by researching better techniques and **adopting them only if they are
> proven to help THIS AI**, measured on real results. Run via the `/improve-methods` command.

## Question / goal
Keep every research/learning/documentation method at the current state of the art, **without ever
making the AI worse.** Improve continuously; regress never.

---

## ⚠️ THE SAFEGUARD (the most important part)

**A method is adopted ONLY if it measurably improves THIS AI's own results — never because it is
highly rated for humans.**

Most "proven" study/research methods were designed for *human* brains (limited working memory, the
forgetting curve, fatigue, needing sleep between sessions). For an AI:
- some human methods help (e.g. active recall → forces using the notes; atomic notes → cleaner retrieval),
- some are irrelevant (e.g. handwriting notes, Pomodoro breaks, memory palaces),
- some actively **hurt** (e.g. deliberately spacing learning across days wastes the AI's strength of
  holding everything at once; "re-reading" wastes tokens for little gain).

So every candidate method goes through this gate:

### 1. Label the candidate
Tag who it was designed for: **human · AI · both · unknown.**
`human` and `unknown` candidates get *extra* scrutiny — they must prove clear AI gains, not coast on
reputation. A high human-rating is **not** evidence for the AI; only the benchmark is.

### 2. Test it on a frozen benchmark (A/B)
- A **benchmark** = a fixed topic + a fixed, frozen gold-standard test with a known answer key
  (stored in `methods/benchmarks/`). It does not change between runs, so comparisons are fair.
- Run the **current** method and the **candidate** method on the same benchmark.
- Run each **≥ 3 times** (LLMs are stochastic) and compare averages — a single lucky run doesn't count.
- **Most rigorous:** the benchmark's answer key should be external / human-made, so the AI isn't
  grading its own taste at the meta level too. If none exists, freeze a self-made one and note the caveat.

### 3. Measure four things (efficiency counts, not just score)
| Metric | What it means |
|--------|---------------|
| **Accuracy** | exam score / correctness |
| **Reliability** | low variance across the runs (consistent, not lucky) |
| **Efficiency** | tokens, wall-time, tool calls, and rounds needed to pass |
| **Note quality** | are the produced notes cleaner / better-cited / more atomic |

### 4. Decision rule (status-quo bias = safety)
- **Adopt** only if: accuracy improves, **OR** accuracy ties AND efficiency improves — *with no
  regression* on any other metric.
- **Borderline / mixed results** → do NOT auto-apply. Pause and ask the human.
- **Worse, or "human-only and it degraded the AI"** → **reject**, and write it to
  `methods/graveyard.md` with the numbers and the reason (so it's never blindly re-tried).

### 5. Reversibility & protected rules
- **Back up** any method file to `methods/backups/<file>.<date>.bak` BEFORE editing it. Every change
  is reversible.
- **PROTECTED — cannot be weakened by this loop:** this safeguard itself, the privacy/no-leak rules,
  the "blind, cite-or-zero" exam rule, and the backup rule. The loop may *strengthen* them, but
  weakening or removing any of them requires **explicit human approval**, every time. The system may
  not edit away its own brakes.

---

## The courtroom (how a candidate is decided)
Decisions are made by an adversarial **court**, not a single mind:
- **Researcher** builds the evidence (sources + the benchmark A/B numbers) and is fully transparent to
  both lawyers.
- **Approval lawyer** argues FOR; **Disapproval lawyer** argues AGAINST (its job is to catch
  human-methods-that-degrade-the-AI, regressions, leakage, bloat). Both cite the same evidence.
- **Judge** is neutral and **isolated from the Researcher** — it sees the two briefs **plus the admitted
  evidence record** (so it rules on facts, not eloquence) and delivers a reasoned verdict.
Evidence still rules: an eloquent brief with no benchmark support loses. Clear verdict + clear benchmark
win → auto-apply; split/uncertain → the user decides. The court can never weaken the protected rules above.

## Method (the loop — implemented by `/improve-methods`)
1. **Load** `methods/METHODS-REGISTRY.md` (every method, its file, version, last-reviewed date).
2. **Research** the current best, proven research / learning / documentation / AI-prompting methods
   (web + sources). Clean, dedupe, cite. Say what you considered and *rejected*.
3. **Propose candidates** — concrete edits to specific method files, each as a card: the change, its
   source, claimed benefit, the human/AI/both/unknown label, and your hypothesis of its effect on the AI.
4. **Run the safeguard** (section above) on each candidate.
5. **Apply or reject** per the decision rule — back up, edit, and log adopted changes; graveyard the rest.
6. **Record** to `methods/evolution-log.md`, bump the version + date in the registry.

## Results / Discussion / Conclusion
Filled in per run by `/improve-methods` (this is a `type: method` doc, so each run is reproducible).
The evolution log is the running record of what was tried, what the numbers were, and what was kept.

*References: A/B testing & controlled experiments; reproducibility (IMRaD); Dunlosky et al. 2013 on
which learning techniques actually transfer; the principle that human-cognition evidence does not
automatically transfer to machine cognition — hence: measure, don't assume.*
