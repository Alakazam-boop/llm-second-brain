# 🧠 LLM Second Brain — A Self-Improving Knowledge Engine

> A framework that turns any LLM agent (e.g. Claude Code) + an [Obsidian](https://obsidian.md)
> vault into a **persistent, compounding, self-improving knowledge base** — one that researches
> topics, writes cited notes, proves its own mastery with exams, and *improves its own methods
> only when the evidence says it should*.

![Type](https://img.shields.io/badge/type-framework-blueviolet)
![Runtime](https://img.shields.io/badge/runtime-Claude%20Code%20%2F%20any%20agentic%20LLM-000000)
![Store](https://img.shields.io/badge/store-Obsidian%20%2B%20Markdown-7c3aed)
![Skills](https://img.shields.io/badge/skills-5%20commands-informational)
![License](https://img.shields.io/badge/license-MIT-green)

> **This is a de-personalised, reusable template.** Clone it, point your agent at it, and start
> your own brain. It ships with the *system* (rules, methods, skills, benchmarks) — not anyone's
> private notes.

---

## 📑 Table of Contents
1. [Why this exists](#1-why-this-exists)
2. [Core philosophy](#2-core-philosophy)
3. [Architecture](#3-architecture)
4. [The five skills (commands)](#4-the-five-skills-commands)
5. [The documentation method](#5-the-documentation-method-how-notes-are-written)
6. [The self-improvement loop](#6-the-self-improvement-loop)
7. [Repository layout](#7-repository-layout)
8. [Getting started](#8-getting-started)
9. [Daily workflow](#9-daily-workflow)
10. [Stats (reference deployment)](#10-stats-reference-deployment)
11. [Limitations & shortcomings](#11-limitations--shortcomings)
12. [Roadmap](#12-roadmap)
13. [FAQ](#13-faq)

---

## 1. Why this exists

Normal chat with an LLM **forgets** everything between sessions. Plain RAG **re-derives** answers
from scratch every time and never gets smarter. Neither *accumulates* understanding.

This project treats knowledge like a **codebase that compounds**:

> **The wiki is the codebase · Obsidian is the IDE · the LLM is the programmer · you are the product owner.**

You curate sources and ask questions; the LLM does all the writing, filing, linking, and
bookkeeping — building an interlinked knowledge base that gets richer with every source and every
question, and a *method layer* that measurably improves over time.

---

## 2. Core philosophy

| Principle | What it means |
|-----------|---------------|
| **Compounding, not disposable** | Every answer is captured as a durable, cited note — future questions build on it. |
| **Cite everything** | Claims link back to a source note. No uncited assertions. |
| **Atomic notes** | One subject per page, densely cross-linked with `[[wikilinks]]`. |
| **Different knowledge → different documentation** | A source summary, a doc you must memorise, and a method record are three different jobs with three different templates (see §5). |
| **Evidence over habit** | Methods change only when benchmarked to help *this* AI — never because a technique is popular with humans (see §6). |
| **Human curates, LLM maintains** | The human owns `raw/` (sources) and asks questions; the LLM owns `wiki/` entirely. |

---

## 3. Architecture

### The three layers
```
┌──────────────────────────────────────────────────────────────────┐
│  LAYER 3 — SCHEMA & METHODS  (the rulebook, co-evolved over time)  │
│  CLAUDE.md · DOCUMENTATION-METHOD.md · SELF-IMPROVEMENT-METHOD.md   │
└───────────────────────────────┬──────────────────────────────────┘
                                 │ governs
┌───────────────────────────────▼──────────────────────────────────┐
│  LAYER 2 — WIKI  (LLM-owned: created, updated, cross-linked)       │
│  wiki/concepts · wiki/entities · wiki/sources · wiki/exams         │
│  index.md (catalog) · log.md (timeline)                            │
└───────────────────────────────▲──────────────────────────────────┘
                                 │ derived from + cites
┌───────────────────────────────┴──────────────────────────────────┐
│  LAYER 1 — RAW  (human-owned, immutable source of truth)          │
│  raw/  ← articles, notes, transcripts, PDFs, images               │
└──────────────────────────────────────────────────────────────────┘
```

### The control loop
```
  You ─┬─ drop a source into raw/ ───────────────▶ agent ingests → writes wiki/sources note
       ├─ /learn <goal> ─────────▶ research → cited notes → BLIND EXAM → mastery score
       ├─ /ask <question> ───────▶ retrieve → synthesise → cited answer → save as note
       ├─ /explain <topic> ──────▶ re-teach a mastered topic the easiest way for the learner
       ├─ /sbm ──────────────────▶ catalog, organise, clean, find patterns & gaps
       └─ /improve-methods ──────▶ research a better method → benchmark → adopt only if it wins
```

---

## 4. The five skills (commands)

Installed as Claude Code slash-commands (`commands/*.md`). Each is a multi-agent workflow.

| Command | One-liner | Internal design |
|---------|-----------|-----------------|
| **`/learn`** | Goal → research → cited notes → **prove mastery with a blind, weighted exam** | Interview the goal → map scope → research → store notes → generate a *leak-proof* exam whose **size scales with the goal**, graded from easy recall to hard transfer questions |
| **`/ask`** | Ask the brain a question, get an **expert, cited answer** | Researcher gathers every relevant note → deep study → highly-prompted expert synthesises → falls back to web only if the brain doesn't know → saves the answer as a new cited note |
| **`/explain`** | Re-explain a **mastered** topic the fastest, easiest way for the learner | Combines mastered notes with the learner's profile (the "human conversion") |
| **`/sbm`** | **Second Brain Management** — catalog, organise, clean, find patterns & connections | 5-agent system with hardwired safety (sensitive-content gating) that maintains the catalog and surfaces gaps/links |
| **`/improve-methods`** | Improve the system's **own methods**, safely | 4-agent **courtroom** (researcher → approval lawyer → disapproval lawyer → neutral judge) that decides on **benchmark evidence**, never rhetoric |

> Each command file is self-contained and documented inline — open `commands/<name>.md` to read the
> full multi-phase workflow, agent roles, and safeguards.

---

## 5. The documentation method (how notes are written)

`DOCUMENTATION-METHOD.md` defines **eight document types**, each mapped to an empirically-supported
technique — so the brain documents the way the evidence says works best, not by habit:

| # | Document type | Used for | Based on |
|---|---------------|----------|----------|
| 1 | **Source note** | Recording what one source said | Progressive summarisation |
| 2 | **Concept note** | Pinning one idea so it can be linked & reused | Atomic / evergreen notes (Zettelkasten) |
| 3 | **Learning doc** | Something you must actually learn & recall | Active recall + spaced repetition cues |
| 4 | **Research-method doc** | A repeatable process / experiment | Structured protocol write-up |
| 5 | **Synthesis / mastery doc** | "What do we know about X overall?" | Elaborative interrogation |
| 6 | **How-to doc** | Step-by-step procedure | Worked-example effect |
| 7 | **Reference doc** | Exact values / specs / lists | Single-source-of-truth |
| 8 | **Personalized explanation** | Explain a topic so a specific learner gets it fast | Feynman + learner profile |

Every page carries YAML frontmatter (`title`, `type`, `created`, `updated`, `tags`, `sources`) so
Obsidian's Dataview can query the brain.

---

## 6. The self-improvement loop

The system can **improve its own methods** — and this is its most distinctive feature.

### The safeguard (the whole point)
> **A method is adopted ONLY if it measurably improves *this* AI's results — never because it is
> highly rated for humans.**

Most "proven" study/research methods were designed for *human* brains (limited working memory, the
forgetting curve, fatigue). For an LLM, some transfer (active recall, atomic notes), some are
irrelevant (handwriting, Pomodoro, memory palaces). So every proposed change is put on trial.

### The courtroom (`/improve-methods`)
```
Researcher  →  finds a candidate improvement + evidence
   │
Approval Lawyer   ──┐
Disapproval Lawyer ─┤→  argue for / against, citing the benchmark
   │                │
Neutral Judge  ─────┘  rules on EVIDENCE:  ADOPT · REJECT · USER-DECIDES
```

### The evidence base
- `methods/METHODS-REGISTRY.md` — the canonical list of active methods
- `methods/benchmarks/` — the test suites the changes are scored against (`learn-benchmark.md`, `sbm-retrieval-benchmark.md`)
- `methods/evolution-log.md` — every change, its verdict, and why
- `methods/graveyard.md` — rejected/retired methods, so mistakes aren't repeated

---

## 7. Repository layout
```
llm-second-brain/
├── CLAUDE.md                     # THE RULEBOOK — schema, layers, conventions (agent reads first)
├── MASTER-PROMPT.md              # Bootstrapping prompt / operating contract for the agent
├── DOCUMENTATION-METHOD.md       # HOW to write each of the 8 document types
├── SELF-IMPROVEMENT-METHOD.md    # The meta-method + safeguard
├── commands/                     # The 5 skills (Claude Code slash-commands)
│   ├── learn.md  ask.md  explain.md  sbm.md  improve-methods.md
├── methods/
│   ├── METHODS-REGISTRY.md       # active methods
│   ├── evolution-log.md          # change history + verdicts
│   ├── graveyard.md              # retired methods
│   └── benchmarks/               # the test suites methods are scored on
├── wiki/                         # LLM-owned knowledge (starts empty)
│   ├── concepts/ entities/ sources/ exams/
├── raw/                          # YOUR immutable sources (starts empty)
├── templates/                    # note scaffolds
├── tools/                        # optional helper scripts
├── index.md                      # catalog of wiki pages
└── log.md                        # append-only action timeline
```

---

## 8. Getting started

### Prerequisites
- **[Obsidian](https://obsidian.md)** (free) — to browse/edit the vault as a graph
- An **agentic LLM** — built for **[Claude Code](https://claude.com/claude-code)**; adaptable to any tool that can read files and run slash-commands

### Setup
```bash
# 1. Clone
git clone https://github.com/Alakazam-boop/llm-second-brain.git my-brain
cd my-brain

# 2. Open the folder as an Obsidian vault (Obsidian → Open folder as vault)

# 3. Install the skills for Claude Code
#    Copy the command files where Claude Code looks for user commands:
cp commands/*.md ~/.claude/commands/         # macOS/Linux
#    (Windows: copy commands\*.md to %USERPROFILE%\.claude\commands\)

# 4. Start your agent IN the vault folder so it reads CLAUDE.md first, then:
#    /learn <a topic>      · /ask <a question>      · /sbm
```

> The agent always reads `CLAUDE.md` first — that file is what turns a generic chatbot into a
> disciplined wiki maintainer.

---

## 9. Daily workflow
1. **Feed it** — drop articles/notes/PDFs into `raw/`.
2. **Ask & learn** — run `/ask` for answers, `/learn` to master a topic end-to-end.
3. **Let it prove itself** — `/learn` ends in a **blind exam**; a low score means the notes need work.
4. **Maintain** — run `/sbm` periodically to catalog, de-duplicate, and surface knowledge gaps.
5. **Evolve** — run `/improve-methods` occasionally to keep the methods at the state of the art.
6. **Review** — read the graph in Obsidian; you read, the LLM writes.

---

## 10. Stats (reference deployment)

Numbers from the original private deployment this template was distilled from — indicative of the
scale a single motivated user reaches:

| Metric | Value |
|--------|------:|
| Wiki pages | **~80** (49 concepts · 24 sources · 5 entities · 2 exams) |
| Total markdown notes (incl. system) | **~150** |
| Skills / commands | **5** |
| Documented note types | **8** |
| Benchmark suites | **2** (learn, sbm-retrieval) |
| Method-improvement decisions logged | tracked in `evolution-log.md` |
| Core system docs | **4** (schema, doc-method, self-improvement, master-prompt) |

> Your numbers start at zero — the framework is what's shipped here.

---

## 11. Limitations & shortcomings

A candid, precise accounting — this is a personal-scale research system, not enterprise software.

**Design / scope**
- 🧍 **Single-user by design.** No multi-user auth, no concurrency control, no merge strategy for two agents writing at once.
- 📓 **Obsidian-coupled.** Frontmatter/Dataview/`[[wikilinks]]` conventions assume Obsidian; other markdown viewers lose the querying/graph features.
- ✍️ **Manual source ingestion.** You must drop sources into `raw/`; there is no automated crawler, RSS, or connector layer.

**Reliability / quality**
- 🎲 **Non-deterministic quality.** Note quality depends on the underlying model and prompt; results vary run to run.
- 🤥 **Hallucination risk remains.** Citations reduce but do not eliminate fabrication — the agent can mis-summarise a source or cite loosely. Human spot-checks are still required.
- 🧪 **Benchmarks are small & partly subjective.** The `/improve-methods` "courtroom" is only as good as its two benchmark suites; a change that games the benchmark could be wrongly adopted. Benchmarks need expanding and periodic re-validation.
- 📝 **Exam integrity depends on the agent.** `/learn`'s "blind, leak-proof" exam relies on the agent honouring separation between study and test context; a careless run can leak answers.

**Operational**
- 💸 **LLM cost & latency.** Deep `/learn` and `/ask` runs spawn multiple agents and can be slow/expensive on large models.
- 🔌 **No CI / automated tests** beyond the method benchmarks; no schema validation that every note's frontmatter is well-formed.
- 🗃️ **No built-in backup/versioning** other than git; the reference deployment relied on manual backups.
- 🔎 **Retrieval is filesystem + grep-scale.** It works well into the low thousands of notes; there is no vector index, so very large vaults will need a retrieval upgrade.

**Security / privacy**
- 🔐 **Sensitive-content gating is convention + a hook, not a guarantee.** Keep genuinely private material in `private/` (git-ignored) and review before sharing a vault.
- 🌐 **Web-fallback trust.** `/ask`'s web fallback inherits the open web's reliability problems.

---

## 12. Roadmap
- [ ] Optional **vector-index** retrieval for large vaults
- [ ] **Frontmatter schema validator** (CI check for every note)
- [ ] Expanded, versioned **benchmark suites** + anti-gaming checks
- [ ] Source **connectors** (URL, RSS, PDF batch import)
- [ ] Model-agnostic command spec (run on non-Claude agents)
- [ ] Spaced-repetition **review scheduler** surfaced in Obsidian

---

## 13. FAQ

**Q. Do I need Claude specifically?**
Built and tuned for Claude Code, but the skills are plain markdown workflows — adaptable to any agent that can read files and follow multi-step instructions.

**Q. Will my private notes end up on GitHub?**
No — `private/`, anything matching `*SENSITIVE*`/`*credentials*`, and `.env*` are git-ignored. This template ships with **no** personal notes.

**Q. Is this a replacement for RAG?**
It's complementary. RAG retrieves; this *builds and maintains* a curated, cited, cross-linked knowledge base — and improves how it does so over time.

---

_Built as a personal knowledge-engineering project. Released as a reusable framework under the [MIT License](LICENSE)._
