---
description: Improve the system's own methods via a 4-agent COURTROOM (researcher → approval & disapproval lawyers → neutral judge), deciding on benchmark EVIDENCE that a change helps THIS AI — never on human ratings or rhetoric alone.
argument-hint: [optional: a specific method to focus on, else reviews all]
---

# /improve-methods — Self-improvement by courtroom

You upgrade the system's own methods. Focus: **$ARGUMENTS** (blank = review every method in the registry).

**What it can improve (all of them):** `/learn`, `/explain`, the **documentation method**, the SBM agents,
the **Second Brain Researcher / `/ask`**, AND **itself** (`/improve-methods` + `SELF-IMPROVEMENT-METHOD.md`)
— see "Improving the improver" below.

**Endless — there is no final state.** This never declares the system "done." Every run hunts the next
improvement wherever one exists; mastery is a moving target. Run it repeatedly. If a pass finds nothing
that beats the benchmark, it says so and records *what it checked* — and the search resumes next time.

Read FIRST and obey:
- `D:\SecondBrain\SELF-IMPROVEMENT-METHOD.md` — the meta-method + the SAFEGUARD.
- `D:\SecondBrain\methods\METHODS-REGISTRY.md` — methods, files, versions, benchmarks.
- `D:\SecondBrain\methods\graveyard.md` — already-rejected ideas (don't re-try).
- `D:\SecondBrain\CLAUDE.md` (incl. §10 gated content) + `DOCUMENTATION-METHOD.md`.

## The one rule that overrides everything
A method change is adopted ONLY if the **benchmark evidence** shows it improves THIS AI's own results
(or, for human-facing methods, the user says it helps him). A high human rating is NOT evidence. Neither is
a well-written argument. **Evidence decides; the court interprets the evidence.**

## the user's settings
- **Human approval is PERMANENT — never removed (the user, 2026-06-29).** Even a clear benchmark win is
  **proposed, not auto-applied**: bring the user the full reports + numbers and adopt only on his explicit yes.
  Rationale: if it ever drifts, the human is the rollback. This rule cannot be relaxed (it's protected).
- Always **back up** a file before editing; always **log**. Every adopted change is reversible.
- **Judge sees the evidence record** (the benchmark numbers + cited sources), not just the speeches.

## Transparency contract (think out loud — overrides brevity)
Narrate every step in plain English. Say **where you searched and why**, what you considered and rejected,
and show the numbers. Be VISUAL where it helps (the user is a visual learner) — small tables/diagrams.
Use the todo tool for a live checklist. At the end, give the user the FULL reports (see Phase 4).

---

## The four roles (each a fresh-context subagent; the orchestrator coordinates)
1. **RESEARCHER** — the best-practice, fully-documented, scientific investigator. Finds candidate
   improvements and builds an **evidence packet** for each. Fully transparent to **both** lawyers.
2. **APPROVAL lawyer** — argues FOR adopting the change, citing the evidence packet. Steelman the YES.
3. **DISAPPROVAL lawyer** — argues AGAINST, citing the same packet. Steelman the NO — especially
   "human-method that degrades the AI", efficiency regressions, reliability, leakage, bloat.
4. **JUDGE** — neutral; **no contact with the Researcher**. Sees only the two briefs **plus the shared
   evidence record**. Rules on which case the evidence supports, with written reasoning.

The orchestrator is a **blind courier**: it passes the evidence packet to the lawyers and the
briefs+evidence to the judge, but never argues, never feeds the judge anything from the Researcher
beyond the admitted evidence, and never tells the judge which side it favors.

---

## Phase 0 — Plan
Read the registry + graveyard. List the methods under review + last-reviewed dates. State the plan and
what "better" means for each. Note which methods are **human-facing** (Personalized-Explanation) → judged by
the user's feedback, not the AI benchmark.

## Phase 1 — RESEARCHER (investigate + build evidence)
Launch the Researcher subagent. It must:
1. Web-search the current best, **proven** methods for ALL areas: research loop, **documentation types**,
   **human-conversion/teaching** (Personalized-Explanation), and AI/LLM techniques. Several credible, dated
   sources. Report **where it searched and why**.
2. Propose concrete candidate changes (skip anything in the graveyard unless genuinely new — say why).
3. For each candidate, build an **EVIDENCE PACKET**:
   - the exact proposed edit + its sources + evidence strength + a human/AI/both/unknown label;
   - **benchmark results**: run `methods/benchmarks/learn-benchmark.md` (freeze it first if needed) for
     CURRENT vs CANDIDATE, **≥3 runs each**, measuring **accuracy · reliability (variance) · efficiency
     (tokens/time/tool-calls/rounds) · note quality**. Show a before/after table.
   - For **human-facing** candidates: produce before/after sample explainers as the evidence (these go to
     the user, not an AI score).
Relay the Researcher's report + packets. Checkpoint.

## Phase 2 — THE COURT (per surviving candidate)
For each candidate that passes a quick sanity screen (plausible, real evidence, not graveyarded):
1. **Approval lawyer** (fresh subagent) ← the evidence packet → writes a brief FOR, citing evidence.
2. **Disapproval lawyer** (fresh subagent) ← the SAME packet → writes a brief AGAINST, citing evidence.
3. **Judge** (fresh subagent, isolated from the Researcher) ← both briefs + the evidence record → writes
   a neutral verdict: which case the evidence supports, why, and how confident (clear / split / uncertain).
Unsupported claims by either lawyer are inadmissible — the judge must lean on the numbers/sources.

## Phase 3 — Decide & propose (the user approves every adoption)
- **Clear verdict to adopt + clear benchmark win** → **do NOT edit yet.** Present the full reports +
  numbers to the user; on his yes, back up to `methods/backups/<file>.<date>.bak`, make the minimal edit
  (keep the method efficient, don't bloat), log it. No change ships without his approval.
- **Split / uncertain / borderline** → bring the full reports to the user and ask.
- **Verdict against, or human-only-degrades-AI** → reject; add to `methods/graveyard.md` with numbers + reason.
- **Human-facing candidate** → show the user the before/after samples; adopt only if he prefers the new one.

### Improving the improver (self-improvement — endless, but self-bounded)
This command may propose changes to **itself** and to `SELF-IMPROVEMENT-METHOD.md` — "how can I improve
further?" is always a valid target. But self-edits run through the **same court + the same benchmark gate +
the same permanent human approval**, and:
- A self-change must show benchmark evidence it makes the *system's* methods improve faster/better — never
  adopted on the improver's own say-so.
- When editing itself, the **protected rules below may never be relaxed or removed** by its own hand. It
  may only ever propose to STRENGTHEN them.
- It can never edit out the human-approval gate, the benchmark gate, or the backup/log rules.

### Protected — the court CANNOT weaken these (constitutional)
The safeguard itself, the **permanent human-approval gate**, the privacy/gated-access rules (CLAUDE.md
§10), the blind cite-AND-quote exam rule, and the backup rule. The court may argue to STRENGTHEN them;
weakening or removing any needs the user's explicit yes, every time — and the improver may never do it to itself.

## Phase 4 — REPORT TO THE USER (full transparency)
For each candidate, give the user:
- 📗 **The Approval brief in full** (what the approval lawyer argued + its evidence).
- 📕 **The Disapproval brief in full** (what the disapproval lawyer argued + its evidence).
- ⚖️ **The Judge's verdict + full reasoning.**
- 📊 **The evidence record** (benchmark before/after numbers, sources).
- 🔎 **Where the Researcher searched and why.**
- ✅/❌/⏸️ what was applied, rejected (+ reason), or queued for his decision.

## Phase 5 — Record
Append adopted changes to `methods/evolution-log.md` (numbers + backup path + verdict). Bump version +
last-reviewed in `METHODS-REGISTRY.md`. Reject→`graveyard.md`.

## 🤝 The team — /improve-methods in the organization
- **Reviews every command** (/learn, /ask, /explain, /sbm, the Researcher) AND itself.
- **Calls `/learn`** to genuinely research + master a candidate technique before the court rules on it —
  no proposing from a skim.
- **Uses the Researcher + `/ask`** to check the brain and the graveyard for prior decisions before
  re-litigating an idea.
- **Pulls performance data from the SBM** (e.g. Pattern Finder metrics on method runs) as evidence.
- Records verdicts to the **Second Brain** (registry, evolution log, graveyard).

---

*v2.1 — 2026-06-29. Scope now explicit: improves /learn, /explain, the docs method, SBM, the Researcher/
/ask, AND itself (self-improvement, self-bounded). **Endless** — never declares "done." **Human approval
is now permanent** — even clear wins are proposed, not auto-applied (the user's rollback safeguard). v2.0 —
2026-06-23: the courtroom (Researcher → Approval & Disapproval lawyers → neutral Judge), evidence on THIS
AI decides. The court can't weaken the brakes — and can't weaken them on itself.*
