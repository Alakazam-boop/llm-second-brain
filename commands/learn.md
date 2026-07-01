---
description: Understand the goal first (interview), map everything needed to reach it, then research → store cited notes → prove mastery with a blind, leak-proof, WEIGHTED exam — easy recall questions up to hard out-of-box Transfer questions (gaokao-style marks), whose size is computed from the goal's scope. For topics, concepts, skills, job roles/titles, or "build me a tool that…".
argument-hint: <topic | concept | job role | "build a tool that …" | goal>
---

# /learn — Understand the goal, learn everything to reach it, prove it

You are the **ORCHESTRATOR** for the request: **$ARGUMENTS**

The request may be a topic, a concept, a skill, a **job role/title**, or **"build a tool that does X."**
In every case the real target is a **goal**: master the knowledge needed to *do* that thing. You must
first understand the goal, map what mastering it requires, size the exam to that scope, then learn + test.

Vault: `your vault root`. Read `CLAUDE.md` (incl. §10 gated content) and
`DOCUMENTATION-METHOD.md` first; obey both.

## Five isolated roles (each a fresh-context subagent) + you as blind courier
Researcher (learns, blind to exam) · Examiner (writes the **weighted** exam, blind to notes) ·
**Gap-finder/Solver** (checks each question is build-able from the notes; solution SEALED, never reaches
the Student) · Student (answers from notes only, **showing working**) · Grader (scores with **partial
credit**, audits, independent). You move artifacts and write files; you NEVER author questions/answers/
grades or pass one agent another's reasoning.

> 📐 **Exam upgrade (weighted, out-of-box):** the exam uses 4 difficulty **rungs** with **marks** —
> Recall(1) · Apply(2) · Analyze(3) · **Transfer(5, out-of-box)** — graded by a **point-breakdown** with
> **cite-the-principle** partial credit, passed on a **separate bar per rung**. Full design + rationale:
> `methods\weighted-exam-design.md`. This file is the operational spec.

---

## PHASE A — INTAKE INTERVIEW (understand the goal BEFORE spending anything)
Do NOT research yet. First make the goal unambiguous.
1. Classify the request: **concept · topic · skill · job-role/title · tool-to-build · broad-goal.**
2. Run an **adaptive clarifying interview** with the AskUserQuestion tool. Ask only what you genuinely
   need; **stop as soon as the goal is unambiguous** (typically 2–6 questions, fewer if already clear).
   Pin down:
   - **End goal & success criteria** — what does "done/able" look like? (For a tool: what it must *do*;
     for a role: what an expert in it must be able to *perform*.)
   - **Depth tier** — Awareness / Working / Expert.
   - **Scope boundaries** — what's in, what's out, what he already knows (skip it), constraints.
3. **Restate** your understanding in 3–4 lines + the success criteria, and get an explicit "yes" before
   proceeding. If corrected, restate again. Never start researching on a fuzzy goal.

## PHASE B — BLUEPRINT (decompose the goal into learning objectives)
Build a **capability map**: the complete set of distinct, atomic **learning objectives** ("must-know" /
"must-be-able-to") required to hit the confirmed goal.
- For a **job role/title**: derive domains from **authoritative external sources** — real job
  descriptions, certification syllabi, standard curricula — not invented.
- For a **tool to build**: decompose into the knowledge/skills its components demand (e.g. a Telegram
  bot → API + auth + webhooks + hosting + the domain logic + testing).
- Tag each objective with a **depth tier** (Awareness/Working/Expert) and an **importance weight**.
This blueprint is the auditable artifact behind the exam size. Save it to
`wiki/exams/<slug>/blueprint.md`.

## PHASE C — COMPUTE THE EXAM SIZE (factual, hardwired, anti-gaming)
The number of questions is **computed from the blueprint, never guessed.** Hardwired rules:
1. **Formula:** `N = Σ objectives × questions-per-objective`, where Q/objective by depth tier =
   **Awareness 2 · Working 4 · Expert 6** (spread across difficulty types).
2. **Coverage floor:** EVERY objective gets **≥2 questions across ≥2 difficulty types** (e.g. recall +
   apply). No objective may be dropped to shrink N.
3. **No-duplication ceiling:** no two questions may test the same *(objective × angle)*; duplicates are
   deleted. This caps padding — N can't be inflated with rephrasings.
4. **Goal-traceability:** every objective must trace to the goal AND be backed by an authoritative
   source; objectives that don't map are cut (stops arbitrary scope inflation).
5. **Ownership:** the **Examiner** (blind to the notes) computes and owns N from the blueprint — never
   the Researcher or you — so the test can't be sized to what was conveniently learned.
6. **Independent audit:** the Grader sanity-checks the blueprint for *missing coverage* (push N up) or
   *redundancy* (push N down) before the exam is finalized.
7. **Transparency:** show the user the blueprint + computed N + per-domain breakdown ("47 objectives →
   138 questions"). He can approve or adjust. N naturally ranges (50, 100, 200…) by real scope.
8. **Weighted rung composition (per objective):** each objective's questions are spread across the 4
   rungs **by its depth tier** — Awareness → mostly **Recall(1)/Apply(2)**; Working → adds **Analyze(3)**;
   Expert → adds **Transfer(5, out-of-box)**. The Examiner owns the exact **marks and per-rung counts**
   (defaults 1/2/3/5, tunable per topic with a written reason). **Small scope collapses the top rungs** so
   every *present* rung has ≥2 questions (a per-rung bar needs enough items to mean something).
9. **Intensity override:** the user may set the run's intensity — `quick check` (low rungs only) ↔
   `full mastery gauntlet` (all rungs, more Transfer) — overriding the scope default.

## PHASE D — RESEARCH (Researcher subagent — run on a faster model, e.g. sonnet)
Check the ledger (`learned-topics.md`) first; if already mastered, refresh only what's new since then.

**D0 — consult the Second Brain first (don't re-learn what we already have).** Run the shared retrieval
service to pull everything the vault already holds on the blueprint objectives:
`python tools/researcher.py "<vault>" "<objective/topic>" --json`. Use its **core/related** notes
as the starting base; only research the web for what's missing or stale. (Respect any gated-file
permission request it returns.)

**D1 — sources: prefer authoritative + scholarly.** Research **only the blueprint objectives** (no
open-ended crawling). Pull from the strongest free sources available, matched to the domain:
- **Academic indexes:** OpenAlex, Semantic Scholar, Crossref (citations, DOIs, peer-reviewed work).
- **Science & medicine:** PubMed / Europe PMC, arXiv, bioRxiv/medRxiv (primary research).
- **Encyclopedic / textbooks:** Wikipedia, Britannica, LibreTexts, OpenStax (vetted tertiary).
- **Stats & live data:** Our World in Data, World Bank, official government data portals (for numbers).
Prefer primary/peer-reviewed over blogs; record each source's **title, author/org, and date**.

**D2 — FACT-CHECK BAR (hardwired — the user's "never learn anything false or outdated" rule):**
1. **≥2 independent, dated sources per factual claim.** Nothing enters a note on a single source. If only
   one source exists, mark the claim **`⚠️ single-source — unverified`**.
2. **Every number/statistic is cited with its source + year** (e.g. "≈3.5% (World Bank, 2024)").
3. **Recency check:** note each key fact's date; if a claim may be **superseded/outdated**, find the
   current consensus. Flag stale facts.
4. **Quarantine false/outdated content:** never state it as fact. If it must be included (common
   misconception, historical view, or the user asked), put it in a clearly-marked
   **`> ⚠️ OUTDATED / FALSE — why:`** block explaining *what's wrong, why, and what's current instead*,
   in detail.
5. **Explain WHY, not just what:** for each major claim, give the mechanism/evidence for *why it's true*
   (or why the field believes it) — thorough, detailed, not a bare assertion.
6. **Flag conflicts** between sources explicitly with both positions + which is better-supported.

Write **concise, token-bounded** notes per `DOCUMENTATION-METHOD.md` (source + atomic concept + learning
docs) honoring D2, cite every claim, update `index.md` + `log.md`. Researcher never sees the exam.

## PHASE E — EXAMINE (Examiner subagent — strongest model)

### E1 — write the weighted exam + key. **Examiner precision rules (hardwired):**
- Exactly **one unambiguous correct answer** per question (the key lists accepted variants). No
  double-barrelled questions unless explicitly multi-part with separate sub-marks.
- **Define any term the question uses**; never conflate categories. No trick wording.
- Historical/empirical facts with source variance → **state/accept a range**.
- Each item carries its **objective ID · rung (Recall/Apply/Analyze/Transfer) · mark value · sourced
  answer · a point-breakdown** (how the marks split step by step — e.g. a 5-mark Transfer item:
  *+1* setup/forces · *+2* apply the governing law · *+1* final expression · *+1* interpretation).
- **Transfer(rung-4) items are OUT-OF-BOX:** a scenario described in **no note**, solvable using **only
  note-principles**. They **may span two brain topics** — prefer a genuinely *related* topic the brain
  knows; if none, a single-topic out-of-box question. Never require an outside everyday fact.
- **Per-rung pass bars** (Examiner sets, defaults, ±10% per topic *with a written reason*, hard floors):
  **Recall 100% (floor 95) · Apply 90 (80) · Analyze 80 (70) · Transfer 70 (60).**
- **Blind to note organization** so questions are unpredictable, not echoes of the note structure.

### E2 — GAP-FINDER / SOLVER (the exam grows the brain — "tools, not chairs")
Before any question is finalized, the **Gap-finder** attempts to solve it from the notes (main topic **+
neighbour notes**). Its worked solution is **SEALED — never given to the Student** (this is why checking
solvability does not leak the question).
- **Solvable now →** the question is fair; keep it.
- **A principle is missing →** do NOT discard the question. Send the **Researcher** to **learn the gap
  PROPERLY** (real research on the missing piece **and** a revisit of the main topic; full D1/D2
  fact-check), add cited notes, then re-check. **Add only the TOOL** — the general building-block
  principle (e.g. a "normal force" note) — **never a note that states the specific scenario's answer**
  (that would turn an out-of-box question into a look-up). Loop until solvable or the §I stuck-rule trips.

**Isolation:** the Examiner returns questions + key + point-breakdowns to you. You pass **questions INLINE**
to the Student and the **key + breakdowns INLINE** to the Grader. **Do NOT write the key (or any exam file)
to disk yet.**

## PHASE F — TEST (Student subagent — faster model, e.g. sonnet)
Paste the questions inline. Student reads ONLY `wiki/` knowledge notes (never `wiki/exams/`, no web, no
memory). (A hook also hard-blocks reading any `*-key.md`.) By rung:
- **Recall/Apply:** answer + `[[note]]` citation + **verbatim quote**; no fact beyond the quote; else
  "NOT IN NOTES".
- **Analyze/Transfer (show your working — CITE-THE-PRINCIPLE):** give the full step-by-step derivation;
  **tag every step to the note-principle it stands on** (`[[note]]` + the principle used). Any step that
  relies on a fact **not grounded in a cited note is disallowed** — mark it "NOT IN NOTES" rather than
  guess. Reasoning must be *built from* the notes, never imported from memory or the web.

## PHASE G — GRADE (Grader subagent — strongest model)
Pass the Grader the questions, the Student's answers, and the **key + point-breakdowns (inline)**. It:
- awards **partial credit per the point-breakdown** — each step earns its marks only if validly built on a
  **cited note-principle** (**cite-the-principle**); a step using an ungrounded/outside fact **scores 0 for
  that step** (not the whole question). Verbatim-quote check still applies to Recall/Apply.
- verifies every quote/citation against the notes (**memory-leak = 0** for that step/item);
- computes a **score PER RUNG** (marks earned ÷ marks available, for each of Recall/Apply/Analyze/Transfer)
  **and** the weighted total.

## PHASE H — VARIANCE (only if borderline)
If **any rung** lands within ~5 points of its bar, run **one** more independent Student + Grader on that
rung; it must clear the bar on the **lower** attempt. If every rung is clearly above (or clearly below)
its bar, skip — one attempt suffices (saves cost).

## PHASE I — LOOP (per-rung, always-go-deeper, NO teaching-to-the-test)
**Pass = EVERY rung clears its own bar** (Recall/Apply/Analyze/Transfer — §E1), not a single flat %.
While any rung is below its bar **and** the stuck-rule below has **not** tripped:
1. **Diagnose** each miss — *missing-fact* vs *reasoning-slip* — but **always go deeper regardless**:
   send the Researcher the **gap log at the OBJECTIVE level only** (missed *topics*, NEVER question
   wording) to **learn the gap PROPERLY** (the missing piece **+ neighbours + revisit the main topic**,
   "tools not chairs"), AND have the Student **re-derive** the reasoning misses.
2. Re-test in two parts (unchanged cost rule):
   - **Gap objectives → FRESH, unseen questions** by the blind Examiner (can't be memorized).
   - **Already-passed objectives → reuse the FROZEN questions** as a cheap regression check.
3. Mastery requires passing BOTH the fresh gap questions and the frozen regression set — **on every rung**.

**Stuck-rule (stop only when provably looping — ALL must hold):** attempts on the failing rung **≥ 4**
**AND** no meaningful score gain between the last two attempts **AND** (same failure signature repeats
*or* the deeper research surfaced no new relevant material). Only then stop, and write an **honest
post-mortem**: which rung, which question(s), every attempt made, the exact sticking point, and the
evidence it was looping (so the user sees *where, what, why*). Phase J still always runs.

## PHASE J — CERTIFY-OR-DOCUMENT (ALWAYS runs — even at the cap or on stop)
This step runs no matter how the loop ends, so records are never left stale:
1. Write/refresh the **Synthesis/mastery** page (TYPE 5) AND a **run-record** (TYPE 4 IMRaD) with the
   **per-rung scores + marks**, trajectory, and any open gaps.
2. Update `learned-topics.md` (topic · mastery page · **per-rung scores** · rounds · sources · open gaps).
   **Certified = every rung cleared its bar.** If any rung fell short at the stuck-stop, record it as
   **near-mastery, not certified**, naming the failing rung(s) + gap list — never claim mastery.
3. Refresh `index.md`; append a `learn` line to `log.md`.
4. Tell the user: the goal, the score, the page link, and open gaps. Use today's real date everywhere.

## COST CONTROLS (hardwired — cut spend, keep integrity)
- **Model tiering:** Examiner + Grader = strongest model (integrity-critical). Researcher + Student =
  faster/cheaper model (e.g. sonnet) — quality preserved where it matters, big savings on the bulk work.
- **Frozen bank + gap-only re-tests** (Phase I) — the largest saving on multi-round runs.
- **Single Student attempt** unless borderline (Phase H).
- **Blueprint-bounded research** + concise notes — no open-ended crawling, smaller Student reads.
- **Stop-early:** round-1 ≥98% → skip extra rounds; obvious early failure → fail fast and report.

## 🤝 The team — /learn in the organization
The commands work as one org, not in isolation:
- **Calls the Researcher** (`tools/researcher.py`) first (Phase D0) — reuse what the brain holds
  before touching the web.
- **Is called BY `/ask`** (web fallback when the brain can't answer) and **BY `/improve-methods`** (when
  the court needs a technique genuinely researched + mastered before proposing it).
- **Hands off to `/explain`** — after mastering a topic, suggest `/explain <topic>` for a the user-friendly
  version (never auto-run).
- Writes everything to the **Second Brain** (shared memory) so every other command benefits.

---

*v3.2 — 2026-07-01. **Weighted out-of-box exam** (the user's idea). 4 mark-weighted rungs — Recall(1)/
Apply(2)/Analyze(3)/Transfer(5, out-of-box, cross-topic) — with per-item **point-breakdowns** and
**cite-the-principle** partial credit; **separate pass-bar per rung** (graduated 100/90/80/70, examiner
±10 w/ reason, floors). New **Gap-finder/Solver** role: hard questions may reach beyond current notes; the
exam then **learns the gap properly** and adds the TOOL (principle), never the answer ("tools not chairs"),
so the exam grows the brain and stays fair — sealed solution, no leak. Loop now passes **every rung**,
**always goes deeper** on a miss, and stops only when **provably stuck** (attempts≥4 + no gain + repeat
signature/no new material) with an **honest post-mortem**. Phases C/E/F/G/H/I/J updated; 5 isolated agents.
Full design: `methods/weighted-exam-design.md`. — v3.1 — 2026-06-29. Adds to Phase D: consult the Second Brain Researcher first (D0), scholarly-source
access (D1: OpenAlex/Semantic Scholar/Crossref, PubMed/arXiv, Wikipedia/LibreTexts/OpenStax, Our World in
Data/World Bank), and the hardwired FACT-CHECK BAR (D2: ≥2 independent dated sources/claim, numbers cited
with year, recency check, quarantine false/outdated with a why-block, explain why-true). v3.0 — 2026-06-25:
intent interview (A), goal→blueprint→computed exam size (B–C), examiner precision rules, hardwired
isolation, always-run certify/document (J), cost controls. Four isolated agents; orchestrator a blind courier.*
