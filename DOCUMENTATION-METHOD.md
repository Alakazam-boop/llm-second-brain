---
title: Documentation Method — Types, Styles & Proven Templates
type: overview
created: 2026-06-23
updated: 2026-06-23
tags: [documentation, method, standard, learning-science]
---

# Documentation Method

> The rulebook for **HOW to write** each kind of page in this brain. `CLAUDE.md` says where files
> go and the golden rules; **this file says what each document type looks like and why.**
>
> Core principle: **different knowledge needs different documentation.** A note that captures a
> source, a doc you must *memorise*, and a doc that records a *method* are three different jobs.
> Each type below uses a method that is **empirically supported**, so the brain documents the way
> the evidence says works best — not by habit.
>
> Any LLM (Claude, or later the local agent) picks the right type for the job, then follows that template.

---

## How to choose a type (decision rule)

Ask: **what is this document FOR?**

- Recording what a single source said → **Source note**
- Pinning down one idea so it can be linked & reused → **Concept note**
- Something a human (or model) must actually *learn and recall* → **Learning doc**
- Recording a process / experiment / method so it can be repeated → **Research-method doc**
- "What do we know about X overall?" → **Synthesis / mastery doc**
- "How do I *do* this, step by step?" → **How-to doc**
- "What is the exact value / spec / list?" → **Reference doc**
- "Explain a topic so **the learner personally** learns it fast and easily" → **Personalized-Explanation doc**

Every document, regardless of type, keeps the standard `CLAUDE.md` frontmatter, cites its sources,
flags contradictions with `> ⚠️ Conflict:`, and links liberally with `[[wikilinks]]`.

---

## TYPE 1 — Source note  ·  `type: source`  ·  `wiki/sources/`
**Method: Zettelkasten *literature note*.** Capture a source in your *own words* with full
provenance, so claims can always be traced back. Don't copy-paste walls of text — distil.

```markdown
---
title: <Source title>
type: source
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [...]
source_url: <url or "local: raw/...">
author: <who>            # if known
source_date: <when the source was published>
credibility: high | medium | low   # and one line why
---

## What it is
One line: what this source is and why it's here.

## Key claims (in my own words)
- Claim 1 — short, neutral. (verbatim quote only if the exact wording matters)
- Claim 2 …

## Notable data / numbers
- <figure> (as of <date>)

## How much to trust it
Why this source is/ isn't authoritative; any bias or age caveats.

## Feeds these pages
[[concept-a]], [[concept-b]]
```

## TYPE 2 — Concept note  ·  `type: concept`  ·  `wiki/concepts/`
**Method: Zettelkasten *permanent note* — atomicity.** **One idea per page**, written so it stands
alone, and densely linked. Atomic + linked notes are easier to retrieve and recombine than long
mixed pages.

```markdown
---
title: <The one idea>
type: concept
created/updated/tags/sources: ...
---

## In one sentence
The idea, compressed. (If you can't, the page isn't atomic — split it.)

## Explanation
Plain-English, own words. Cite each claim: … (see [[source-page]]).

## Why it matters / where it's used
## Related
[[neighbour-idea]] · [[contrasting-idea]]
```

## TYPE 3 — Learning doc  ·  `type: learning`  ·  `wiki/concepts/` (suffix `-learn`)
**Method: learning science.** Use this when the goal is to *remember and understand*, not just
store. It bakes in the techniques with the strongest evidence:
- **Active recall / testing effect** (Roediger & Karpicke) → built-in self-test questions.
- **Cornell note system** (Pauk) → cue column + notes + summary layout.
- **Feynman technique** → an "Explain simply" section that exposes gaps.
- **Elaborative interrogation / self-explanation** (Dunlosky et al.) → always answer *why/how*.
- **Spaced repetition** (Ebbinghaus) → a "review on" date to revisit.
- **Dual coding** (Paivio) → add a diagram/table when it helps.

```markdown
---
title: <Topic> — Learning Doc
type: learning
created/updated/tags/sources: ...
review_on: <YYYY-MM-DD>     # next spaced-repetition review
mastery: not-started | learning | mastered
---

## Explain simply (Feynman)
As if to a smart 12-year-old. If this is hard to write, you don't understand it yet.

## Core ideas (Cornell layout)
| Cue (question) | Notes (answer, cited) |
|----------------|-----------------------|
| Why does X happen? | … (see [[source]]) |

## Why / how it really works
The mechanism, not just the fact (elaborative interrogation).

## Common misconceptions
- ❌ <wrong belief> → ✅ <correction> (see [[source]])

## Diagram / worked example
(text diagram or table when it aids understanding)

## Self-test (active recall — answers hidden below)
1. …
2. …
<details><summary>Answers</summary>
1. … 2. …
</details>

## One-line summary
The whole page in a sentence.
```

## TYPE 4 — Research-method doc  ·  `type: method`  ·  `wiki/concepts/`
**Method: IMRaD + reproducibility.** The structure scientific papers use. Document any process,
experiment, or method (including `/learn` itself and each exam round) so someone could **repeat it
and get the same result**. Reproducibility is the test of a good method doc.

```markdown
---
title: <Method / experiment name>
type: method
created/updated/tags/sources: ...
status: draft | tested | proven
---

## Question / goal
What this method is trying to achieve.

## Background
Why this approach; what was tried/considered and **rejected**, and why.

## Method (steps — detailed enough to repeat)
1. … 2. … 3. …
Inputs, tools, settings, and any decision rules.

## Results
What actually happened. Numbers, scores, outputs. Link the artifacts.

## Discussion
What worked, what didn't, limitations, threats to validity.

## Conclusion & next steps
What we now believe + open questions to improve it.
```

## TYPE 5 — Synthesis / mastery doc  ·  `type: synthesis`  ·  `wiki/concepts/` (suffix `-mastery`)
**Method: systematic synthesis + Minto Pyramid (answer first).** Pulls many sources/notes into one
"this is what we know about X" page. **Lead with the conclusion**, then support it — readers (and
models) absorb top-down. This is what `/learn` writes on certification.

```markdown
---
title: <Topic> — Mastery
type: synthesis
created/updated/tags/sources: ...
exam_score: <NN%>  exam_date: <YYYY-MM-DD>
---

## Bottom line (answer first)
The 3–5 sentence "if you only read this, read this".

## The full picture
Structured synthesis, each claim cited to a [[source]] or [[concept]].

## Where sources disagree
> ⚠️ Conflict: … 

## Open questions / gaps
The things the notes still can't answer (next research targets).

## Sources used
[[...]] · [[...]]
```

## TYPE 6 — How-to & Reference (Diátaxis)  ·  `type: howto` / `type: reference`
**Method: Diátaxis** — the documentation framework that separates docs by user need.
- **How-to** (`type: howto`): goal-oriented steps to *do* a task. Title starts with "How to…".
  Numbered steps, prerequisites first, one clear outcome. No theory — link to it instead.
- **Reference** (`type: reference`): dry, complete, lookup-oriented. Tables/lists of exact values,
  commands, specs. No narrative. Optimised for "find the fact fast", not for reading start-to-end.

## TYPE 7 — Personalized-Explanation doc  ·  `type: explainer`  ·  `wiki/concepts/` (suffix `-for-the user`)
**Method: learning science (TYPE 3) + personalization for ONE known reader.** This is the "human
conversion": it takes what the AI has mastered and re-documents it the fastest, easiest way for **the user
specifically** to get it. It combines two inputs: the mastered topic notes (e.g. a TYPE 5 mastery page)
**and** the [[the user-learning-profile]] (how he learns, what he likes, what to avoid, his goals).

Evidence base it adds on top of TYPE 3:
- **Prior-knowledge anchoring** (Ausubel / constructivism) → connect every new idea to something he
  already knows, using analogies from his world (data/code, markets, fitness, genomics — per the profile).
- **Cognitive-load management** (Sweller) → one idea at a time, worked examples, a visual.
- **Motivation** (self-determination) → open with why it matters *to his goals*.
- **Plain language + action-first** (his stated preferences) → no jargon, lead with what he can do.
- **Light active recall** → a couple of gentle gut-check questions, not a hard exam.
- **VISUAL-FIRST (his core trait)** → he's an imagination/visual learner who learns poorly by reading
  alone and finds dry math graphs "nauseating". Lead with a diagram/mental image/analogy; pair every
  idea with a picture; suggest a video/animation where one would help. Words support the visual, not
  the other way round. (see [[the user-learning-profile]])

> 🔒 **On-demand ONLY.** Never auto-generate this. It is produced solely when the user runs the `/explain`
> command. All other documentation stays in its normal AI-facing type by default.
> ⚠️ **Validated differently:** the success of an Personalized-Explanation doc is whether **the user learns
> faster/easier** — judged by *his* feedback, NOT by an AI exam. (See `/improve-methods`.)

```markdown
---
title: <Topic> — Explained for the user
type: explainer
created/updated/tags: ...
based_on: ["[[<topic>-mastery]]"]     # the source knowledge
profile: ["[[the user-learning-profile]]"]
---

## Why this matters to you
One hook tied to a real goal/interest of his.

## The big idea in one breath
The plainest possible one-liner.

## Building it up (anchored to what you know)
Step by step; each new idea linked to something familiar, with an analogy from his world.

## See it
A simple diagram, table, or worked example.

## Quick gut-check
2–3 light questions (answers hidden) so it sticks.

## TL;DR
The whole thing in 2–3 sentences.
```

---

## Golden rules of this method
1. **Pick the type on purpose.** State which type you're writing and why (transparency).
2. **One job per document.** Don't mix a source note with a synthesis.
3. **Method matches purpose** — learning docs get self-tests; method docs get reproducible steps.
4. **Always cite, always link, always flag conflicts** (inherited from `CLAUDE.md`).
5. **Date everything** — knowledge ages; reviews and refreshes depend on dates.

*References: Zettelkasten (Luhmann; Ahrens, "How to Take Smart Notes"); Roediger & Karpicke 2006
(testing effect); Dunlosky et al. 2013 (what works in learning); Pauk (Cornell notes); Feynman
technique; Ebbinghaus (spacing); Paivio (dual coding); IMRaD (scientific reporting);
Minto (Pyramid Principle); Procida (Diátaxis); Ausubel (advance organizers / prior-knowledge
anchoring); Sweller (cognitive load theory); Deci & Ryan (self-determination / motivation).*
