---
title: Learn Benchmark — frozen test for A/B-ing method changes
type: reference
created: 2026-06-23
updated: 2026-06-29
tags: [meta, benchmark, evaluation]
frozen: true
---

# Learn Benchmark (frozen control test)

> Used by `/improve-methods` to compare a candidate method against the current one **fairly**: same
> topic, same questions, same answer key, every time. **Do NOT change this file** — that's the whole
> point. To test a different area, make a NEW benchmark file instead of editing this one.

## How it is used (the A/B protocol)
For CURRENT vs CANDIDATE method, **≥3 runs each**:
1. Run the method's **research phase only** on the Topic below (fresh notes each run; the method may NOT
   read this benchmark file, and may NOT read any pre-existing brain notes on the topic — research from
   scratch so we measure the *method*, not the brain).
2. A blind **Student** subagent answers the **Frozen questions** from those notes only (cite + quote).
3. A **Grader** scores against the **Gold answer key** below (kept hidden from the Student).
4. Record per run: **accuracy %** (key match), **reliability** (accuracy variance across the 3 runs),
   **efficiency** (tokens · tool-calls · research rounds · wall-time), **note quality** (coverage of the
   15 facts, citation density, conflicts flagged).
5. A candidate wins only if it **beats current on accuracy without a reliability/efficiency regression**
   (or is clearly more efficient at equal accuracy). Ties/regressions → reject.

> 🥇 Caveat (logged): this gold key is **self-made** (Claude-authored from standard physics), not an
> external human key. Acceptable to start; upgrade to a textbook/exam key when available for full rigor.

## Topic (frozen)
**Newtonian mechanics: Newton's three laws of motion + basic kinematics & forces.** Chosen because it is
stable (doesn't drift), factual, unambiguous, has authoritative textbook answers, and is **not** otherwise
in the Second Brain (no leakage).

## Frozen questions (15)
1. State Newton's First Law. *(recall)*
2. What is the everyday name for Newton's First Law? *(recall)*
3. State Newton's Second Law as a word equation and as a formula. *(recall)*
4. In F = ma, give the SI units of F, m, and a. *(recall)*
5. State Newton's Third Law. *(recall)*
6. A 2 kg object accelerates at 5 m/s². What net force acts on it? *(apply)*
7. A 10 N net force acts on a 4 kg mass. What is its acceleration? *(apply)*
8. Define inertia, and state what physical quantity measures it. *(recall)*
9. Distinguish mass from weight, and give the formula for weight near Earth's surface. *(understand)*
10. An action–reaction pair acts on the **same** object — true or false? Explain. *(understand)*
11. Why don't the equal-and-opposite Third-Law forces cancel out to give zero motion? *(understand)*
12. A book rests on a table. Name the two vertical forces on the book and their relationship. *(apply)*
13. Define net (resultant) force. *(recall)*
14. If the net force on a moving object is zero, describe its subsequent motion. *(understand)*
15. State the approximate value of g (acceleration due to gravity) near Earth's surface, with units. *(recall)*

## Gold answer key (hidden from the test-taker)
1. An object remains at rest, or in uniform motion in a straight line, **unless acted on by a net external
   force.**
2. The **law of inertia.**
3. Word: *the net force on an object equals its mass times its acceleration* (rate of change of momentum).
   Formula: **F = ma** (or F = dp/dt).
4. F → **newtons (N)**; m → **kilograms (kg)**; a → **metres per second squared (m/s²)**.
5. For every action there is an **equal and opposite reaction** — when A exerts a force on B, B exerts an
   equal-magnitude, opposite-direction force on A.
6. F = ma = 2 × 5 = **10 N.**
7. a = F/m = 10/4 = **2.5 m/s².**
8. **Inertia** = an object's resistance to change in its state of motion; measured by its **mass.**
9. **Mass** = amount of matter (kg), constant everywhere; **weight** = gravitational force on that mass
   (N), varies with g. **W = mg.**
10. **False.** The two forces of an action–reaction pair act on **different** objects (one on each).
11. Because they act on **different objects**, so they never sum on the same body; cancellation would
    require both on the same object.
12. **Weight** (down, gravity) and the **normal force** (up, from the table); they are **equal in
    magnitude and opposite** (book in equilibrium) — note they are NOT a Third-Law pair.
13. The **vector sum of all forces** acting on an object (the single resultant force).
14. It continues at **constant velocity** (constant speed in a straight line), or stays at rest if it was
    at rest — equilibrium (First Law).
15. **g ≈ 9.8 m/s²** (≈ 9.81; accept 9.8–9.81) directed downward.

## Scoring rubric
- Each question 1 point; numeric items need the correct value **and** unit. Q3/Q9/Q12 need both parts.
- Quote-not-in-notes or memory leak (per `/learn` Phase F/G) = 0 for that item.
- Accuracy % = points / 15. Record mean ± spread across the ≥3 runs.
