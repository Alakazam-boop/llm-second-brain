---
description: Take a topic the system has mastered and re-explain it the fastest, easiest way for the learner personally — combining the mastered notes with his learning profile (the "human conversion").
argument-hint: <topic to explain for me>
---

# /explain — Human conversion (Personalized-Explanation)

Produce an **Personalized-Explanation** doc (TYPE 7 in `D:\SecondBrain\DOCUMENTATION-METHOD.md`) for the
topic: **$ARGUMENTS**

The job: turn what the AI knows into the clearest, fastest path for **the user** to learn it — by combining
the **mastered knowledge** with his **learning profile**.

> 🔒 **On-demand ONLY.** This runs *only* when the user explicitly types `/explain <topic>`. Never produce a
> "for the user" version automatically, never as part of `/learn`, and never offer it unprompted. The
> default everywhere else is the normal AI-facing documentation — that is good enough on its own.

## Steps
1. Read `D:\SecondBrain\DOCUMENTATION-METHOD.md` (TYPE 7 template) and
   `D:\SecondBrain\wiki\entities\the user-learning-profile.md` (how he learns, interests, what to avoid).
2. **Pull everything related via the Researcher** (so the explanation draws on the whole brain, not just
   one page): `python D:\a local LLM runtime\sbm\researcher.py "<vault>" "$ARGUMENTS" --json` → use its **core/related**
   notes as your source set. (Honor any gated-file permission request; gated content stays local.)
3. Check `D:\SecondBrain\learned-topics.md`:
   - **If the topic is mastered:** use its mastery page + concept notes as the source of truth.
   - **If notes exist but it isn't mastered:** explain from the notes, and tell him it isn't fully
     verified yet (suggest `/learn <topic>` to master it first).
   - **If nothing exists:** tell him it hasn't been learned yet and offer to run `/learn <topic>` first.
4. Write the doc to `wiki/concepts/<topic-slug>-for-the user.md` using the TYPE 7 template:
   - **Why this matters to you** — hook tied to one of his real goals/interests (data, markets, fitness,
     genomic AI, etc.).
   - **The big idea in one breath** — plainest one-liner.
   - **Building it up** — step by step, each new idea anchored to something he already knows, with an
     analogy from his world. No jargon (define any unavoidable term in plain words).
   - **See it** — a simple diagram/table/worked example.
   - **Quick gut-check** — 2–3 light questions (answers hidden).
   - **TL;DR** — 2–3 sentences.
5. Show him the doc in chat, and link to where it's saved. Append a line to `log.md`.

## Rules
- **Ground every claim in the mastered notes** — don't add facts that aren't backed by the brain.
  Cite the source notes with `[[links]]` so he can dig deeper.
- Keep it **plain, action-first, skimmable** (his stated style).
- After he reads it, **ask what worked and what didn't**, and update
  `wiki/entities/the user-learning-profile.md` with what you learned about how he likes to learn — so the
  next explanation is better.

## 🤝 The team — /explain in the organization
- **Uses the Researcher** (step 2) to pull everything related from the brain.
- **Calls `/learn`** when the topic isn't mastered yet (offer to master it first, then explain).
- **Is the hand-off target of `/learn` and `/ask`** when the user wants a topic re-done in his style.
- Reads/updates the **Second Brain** (esp. `the user-learning-profile`) so explanations keep improving.

*Built 2026-06-23. Pairs with `/learn` (which masters a topic) — `/explain` then converts it for the user.*
