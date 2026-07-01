# The Complete Guide: Setup, Commands, Agents, and Costs

This is the deep-dive companion to the [README](README.md). It explains, with precision:

1. Exactly what you need to download and install, step by step.
2. How each of the five commands is designed, how many agents each one runs, and what every agent does.
3. Roughly how many tokens each command spends, and what that costs on the major AI models (Claude, GPT, Gemini, Groq, DeepSeek), including a per-agent and per-run breakdown.
4. Where to paste the Master Prompt in each major AI, and exactly how to wire the system into Claude and the others.

Everything here is written to be reusable by anyone. Nothing is specific to one person's setup.

---

## Part 1: What you need, and how to install it

You need three things: a place to store notes (Obsidian), an AI agent that can read files and run commands (Claude Code is the reference), and this repository.

### Step 1: Install Obsidian (the note store)

Obsidian is a free app that reads and writes plain Markdown files and draws a link graph between them. It is where you (the human) read the knowledge base.

1. Download Obsidian from [obsidian.md](https://obsidian.md) and install it.
2. You will point it at this repository folder in Step 3. Do not create a vault yet.

Why Obsidian: the notes use `[[wikilinks]]` and YAML frontmatter. Obsidian turns those into a navigable graph and lets you query notes with its Dataview plugin. Any plain text editor can open the files, but you lose the graph and the queries.

### Step 2: Install an agent that can read files and run commands

The system is built and tuned for **Claude Code** (the reference implementation), but the commands are plain Markdown instructions, so any capable agentic assistant that can read local files works with light adaptation.

- Claude Code: install it from [claude.com/claude-code](https://claude.com/claude-code). This gives you the `claude` command in your terminal and the slash-command system the five commands plug into.
- Sign in with your Claude account when prompted.

If you only have access to a chat interface (Claude on the web, ChatGPT, or Gemini) with no file access, you can still use the system in a reduced form. See Part 5 for how the Master Prompt covers that case.

### Step 3: Download this repository

Pick one of the two options below.

Option A, with Git (recommended, keeps history):

```bash
git clone https://github.com/Alakazam-boop/llm-second-brain.git my-brain
cd my-brain
```

Option B, without Git: click "Code" then "Download ZIP" on the GitHub page, then unzip it into a folder called `my-brain`.

### Step 4: Open the folder as an Obsidian vault

1. Open Obsidian.
2. Choose "Open folder as vault" and select the `my-brain` folder from Step 3.
3. Obsidian now shows the `wiki/`, `raw/`, and system files. This is your brain.

### Step 5: Install the five commands into Claude Code

Claude Code loads user commands from a `commands` folder in your Claude configuration directory. Copy the five command files there.

On macOS or Linux:

```bash
cp commands/*.md ~/.claude/commands/
```

On Windows (PowerShell):

```powershell
Copy-Item commands\*.md "$env:USERPROFILE\.claude\commands\"
```

After this, typing `/learn`, `/ask`, `/explain`, `/sbm`, or `/improve-methods` inside Claude Code will trigger the workflows.

### Step 6: Install the Master Prompt

Open [MASTER-PROMPT.md](MASTER-PROMPT.md), fill in the two placeholder lines (who you are, and where your vault lives), and paste the code block into your assistant's custom instructions. Part 5 of this guide gives the exact location for each major AI.

That is the whole install. To summarize what gets downloaded and where it goes:

| Item | Source | Where it goes | Required |
|------|--------|---------------|----------|
| Obsidian | obsidian.md | Installed app | Yes (to read the brain) |
| Claude Code | claude.com/claude-code | Installed CLI | Yes (reference runtime) |
| This repository | GitHub | A local folder (your vault) | Yes |
| The five commands | `commands/` in this repo | `~/.claude/commands/` | Yes (to run the workflows) |
| The Master Prompt | `MASTER-PROMPT.md` | Your AI's custom instructions | Strongly recommended |

---

## Part 2: How each command is designed, and every agent in it

A quick word on the design philosophy shared by all five commands. Each command is a small **team of isolated sub-agents** coordinated by an orchestrator. Isolation matters: when one agent must not be influenced by another (for example, the agent taking an exam must not see the answer key), it runs in a fresh context and the orchestrator passes only what is allowed. This prevents the model from quietly grading its own homework.

Below, "round" means one full pass of the workflow. Some commands loop for several rounds when the first pass is not good enough, which is why cost varies with difficulty.

### 2.1 `/learn`: research a goal, then prove mastery with a blind exam

**Purpose:** take a topic, skill, or role from zero to proven mastery. It researches with strict fact-checking, writes cited notes, then tests itself with an exam it cannot cheat on.

**Number of agents:** five isolated roles, plus the orchestrator acting as a blind courier that carries information between them without leaking it.

**The five roles:**

1. **Researcher.** Investigates the topic on the web with a strict evidence bar (at least two independent, dated sources per claim, scholarly sources preferred), then writes the cited notes into `wiki/`. The Researcher never sees the exam, so it cannot study to the test.
2. **Examiner.** Writes a weighted exam whose questions run from easy recall up to hard "transfer" questions (apply the idea to a new situation). The Examiner is blind to the notes, so the exam tests the subject, not the specific wording of what was written.
3. **Student.** Sits the exam using only the notes as source material. Any answer that relies on a fact not present in a cited note is disallowed and marked "not in notes," which keeps the test honest.
4. **Grader.** Grades each answer against the cited principle in the notes. A step built on an ungrounded or outside fact scores zero for that step.
5. **Variance checker.** Re-runs a borderline case to confirm the score is stable rather than a fluke (only used when a result is close to the pass line).

**How it flows (ten phases, A through J):** intake interview to understand the goal, blueprint the objectives, compute the exam size by a fixed formula so it cannot be gamed, research, write the exam, sit the exam, grade, check variance if borderline, loop deeper on whatever was missed, and finally certify or document the outcome. The loop in Phase I re-tests only the gaps (a "frozen bank" of already-passed questions is not re-run), which is the single biggest cost saver on multi-round runs.

**Model tiering:** the integrity-critical roles (Examiner and Grader) run on the strongest model; the Researcher and Student can run on a faster, cheaper model. This is deliberate: it puts the money where correctness matters and saves it everywhere else.

**Rounds:** one round for an easy topic, up to several for a hard one. The exam size scales with the scope of the goal, so a small concept might be fifty questions and a full job role might be two hundred.

### 2.2 `/ask`: get an expert, cited answer from what you already know

**Purpose:** answer a question from the brain first, with citations, before ever touching the web.

**Number of agents:** two effective roles.

1. **Researcher (shared service).** Gathers every relevant note with a broad, tiered search and returns them grouped as "core" and "related."
2. **Expert synthesizer.** Studies those notes and writes the answer with `[[citations]]`, then saves the answer back as a new note so the brain gets richer with every question. If the brain genuinely does not know, it falls back to `/learn` to research the web.

**Rounds:** normally one. It is the cheapest and fastest command.

### 2.3 `/explain`: re-teach a mastered topic in your style

**Purpose:** take something already mastered and re-explain it the fastest, easiest way for a specific learner.

**Number of agents:** one to two roles (a Researcher pull plus the writer). It is a light, single-pass workflow.

**How it works:** it reads the learner's profile (how they learn, what to avoid, their goals), pulls everything related through the Researcher, then writes a plain, visual-first explanation grounded only in the mastered notes. It never adds facts the brain does not already hold. Because it only runs on topics that are already proven, it does no web research and stays cheap.

**Rounds:** one, with an optional follow-up if the learner gives feedback.

### 2.4 `/sbm`: Second Brain Management (keep the vault healthy)

**Purpose:** catalog, clean, organize, and find patterns across the whole vault, and build a model of the user over time.

**Number of agents:** a five-agent team that shares one SQLite "blackboard" so information flows in both directions. Any agent can read what another produced and can ask another for help mid-run.

**The five agents (team, not a pipeline):**

1. **Cataloger.** Scans every file and records metadata (including how often an agent has deeply read each file, which is the key signal for what is actually important).
2. **Organizer.** Proposes and applies structure (folders, naming, placement).
3. **Cleaner.** Finds duplicates, dead links, and stale files, and fixes them.
4. **Connector.** Surfaces patterns and links between notes that were not linked before ("quality of connections over volume of text").
5. **Modeler.** Builds a model of the user from what the vault reveals, gated so sensitive material never leaks into general notes.

The orchestrator is the team lead: it can run agents in parallel, route help-requests between them over the blackboard, and it saves every agent's report to a `reports` store. All five call the same shared Researcher for retrieval, so none of them re-implements search.

**Rounds:** you typically run one agent at a time on demand (catalog first, then whichever the vault needs). A full sweep runs all five.

### 2.5 `/improve-methods`: improve the system's own methods, safely

**Purpose:** keep every method (including this one) at the state of the art, and never make the AI worse. It changes a method only when the evidence proves the change helps this AI, not because a technique is popular with humans.

**Number of agents:** four, arranged as a courtroom, with the orchestrator acting as a blind courier.

**The four roles:**

1. **Researcher.** A rigorous, fully-documented investigator that finds a candidate improvement and builds an evidence packet for it (with before and after benchmark numbers). Fully transparent to both lawyers.
2. **Approval lawyer.** Argues the strongest possible case FOR adopting the change, citing the evidence packet.
3. **Disapproval lawyer.** Argues the strongest possible case AGAINST, citing the same packet.
4. **Judge.** Neutral, with no contact with the Researcher. Sees only the two briefs plus the shared evidence record, and rules on evidence: ADOPT, REJECT, or USER-DECIDES.

The orchestrator passes the evidence to the lawyers and the briefs to the judge, but never argues and never tells the judge which side it favors. A change is adopted only after this process, and human approval is permanent: even a clear benchmark win still needs your sign-off.

**How it flows (Phases 0 through 4):** plan, research and build evidence, hold the court per surviving candidate, decide, then report everything back to you in full. Because the Researcher effectively runs a `/learn`-style investigation and then a full trial happens per candidate, this is the most expensive command per run.

**Rounds:** one court per candidate improvement. A single invocation may evaluate several candidates.

### Quick comparison

| Command | Agents | Isolated? | Web research | Typical rounds | Relative cost |
|---------|:------:|:---------:|:------------:|:--------------:|:-------------:|
| `/ask` | 2 | partial | only as fallback | 1 | Low |
| `/explain` | 1 to 2 | no | no | 1 | Low |
| `/learn` | 5 | yes | yes (strict) | 1 to 4 | High |
| `/sbm` | 5 | shared blackboard | no | 1 per agent | Medium |
| `/improve-methods` | 4 | yes | yes (per candidate) | 1 per candidate | Highest |

---

## Part 3: Token costs, per model and per agent

This section gives you a realistic way to estimate what each command costs. Two things to understand first.

- **Tokens** are the unit AI models bill in. Roughly, one token is about four characters of English, so about 750 words is 1,000 tokens. Every model charges a different price for tokens you send in (input) and tokens it generates (output).
- **These are estimates.** Real usage swings a lot with the size of your topic, the size of your vault, how many rounds a command runs, and how much "thinking" the model does. Treat the numbers below as order-of-magnitude, not invoices.

### 3.1 Per-model pricing (per one million tokens)

Claude prices are current and authoritative. The other providers are approximate and move often, so verify them at the provider's pricing page before relying on them.

| Model | Input ($ / 1M) | Output ($ / 1M) | Notes |
|-------|---------------:|----------------:|-------|
| **Claude Opus 4.8** | 5.00 | 25.00 | Most capable Claude; use for integrity-critical roles |
| **Claude Sonnet 4.6** | 3.00 | 15.00 | Balanced; good default for Researcher and Student roles |
| **Claude Haiku 4.5** | 1.00 | 5.00 | Fastest and cheapest Claude |
| **Claude Fable 5** | 10.00 | 50.00 | Anthropic's most capable model; premium pricing |
| OpenAI GPT-5 class (approx) | ~1.25 | ~10.00 | Verify at OpenAI pricing |
| Google Gemini 2.5 Pro (approx) | ~1.25 | ~10.00 | Verify at Google pricing; higher above long-context threshold |
| Groq (Llama 3.3 70B, approx) | ~0.60 | ~0.80 | Very cheap; the DME project uses Groq's free tier |
| DeepSeek V3 (approx) | ~0.30 | ~1.10 | Very cheap; verify at DeepSeek pricing |

Two levers cut Claude cost dramatically and both are built into the commands:

- **Prompt caching.** Re-sending the same context (the schema, the notes) can cost as little as one tenth of the input price on a cache hit. Long multi-round runs benefit the most.
- **Model tiering.** Running the cheap roles on Haiku or Sonnet and reserving Opus for the Examiner and Grader can cut a `/learn` run's cost by roughly half with no loss of integrity.

### 3.2 How many tokens a single agent uses

A rough working model. One sub-agent call is either "light" or "heavy":

| Agent weight | Input tokens | Output tokens | Example roles |
|--------------|-------------:|--------------:|---------------|
| Light | ~10,000 | ~3,000 | Researcher gather step, Student answering, Explain writer |
| Heavy | ~40,000 | ~10,000 | Deep web research, Examiner, Grader, Judge, courtroom lawyers |

Cost of one agent call = (input tokens times input price + output tokens times output price), divided by one million.

Worked example, one heavy agent on Claude Opus 4.8:
(40,000 x 5.00 + 10,000 x 25.00) / 1,000,000 = (200,000 + 250,000) / 1,000,000 = **about $0.45**.

The same heavy agent on Claude Haiku 4.5:
(40,000 x 1.00 + 10,000 x 5.00) / 1,000,000 = **about $0.09**.

That five-times difference is exactly why the commands put only the integrity-critical roles on the expensive model.

### 3.3 Estimated cost per full run, by command and model

These combine the agent counts from Part 2 with the token model above. "All-Opus" means every agent runs on Opus 4.8 (the safe, expensive choice). "Tiered" means the cheap roles run on Sonnet or Haiku and only the integrity roles run on Opus (the recommended choice). All figures are per run, in US dollars, rounded.

| Command | Rough tokens per run (in / out) | All-Opus 4.8 | Tiered (Opus + Sonnet/Haiku) | All-Haiku 4.5 | All-Groq (approx) |
|---------|-------------------------------|:------------:|:----------------------------:|:-------------:|:-----------------:|
| `/ask` | 30k / 8k | ~$0.35 | ~$0.20 | ~$0.07 | ~$0.03 |
| `/explain` | 20k / 5k | ~$0.23 | ~$0.13 | ~$0.05 | ~$0.02 |
| `/learn` (1 round) | 120k / 30k | ~$1.35 | ~$0.80 | ~$0.27 | ~$0.10 |
| `/learn` (3 rounds) | ~260k / 65k | ~$2.95 | ~$1.70 | ~$0.59 | ~$0.21 |
| `/sbm` (full sweep) | 150k / 25k | ~$1.38 | ~$0.70 | ~$0.28 | ~$0.11 |
| `/improve-methods` (1 candidate) | 200k / 40k | ~$2.00 | ~$1.20 | ~$0.40 | ~$0.16 |

How to read this: a quick question with `/ask` costs a few cents; mastering a hard topic end to end with `/learn` over several rounds costs a couple of dollars on the safe setting and well under a dollar when tiered; a full method-improvement trial is the priciest at roughly two dollars per candidate on all-Opus.

### 3.4 Cost per agent, inside one `/learn` round (worked example)

To make the per-agent picture concrete, here is a single `/learn` round on the recommended tiered setting (Researcher and Student on Sonnet 4.6, Examiner and Grader on Opus 4.8):

| Agent | Model | Tokens (in / out) | Cost |
|-------|-------|-------------------|------|
| Researcher | Sonnet 4.6 | 40k / 10k | ~$0.27 |
| Examiner | Opus 4.8 | 20k / 6k | ~$0.25 |
| Student | Sonnet 4.6 | 30k / 8k | ~$0.21 |
| Grader | Opus 4.8 | 25k / 8k | ~$0.33 |
| **Round total** | | ~115k / 32k | **~$1.06** |

Switch every role to Haiku and the same round is roughly $0.30; switch every role to Opus and it is roughly $1.35. The tiered setting sits in between while keeping the exam and grading on the strongest model.

### 3.5 Practical ways to keep the bill low

- Use `/ask` and `/explain` for everyday questions; save `/learn` for things you truly need to master.
- Run the cheap roles on Sonnet or Haiku (model tiering) and keep only the Examiner and Grader on Opus.
- Let prompt caching work: do not change the schema or tool list mid-session, or you throw away the cache.
- For the AI features in related tools, a free or near-free provider like Groq (Llama) is often enough for the Researcher and Student roles.

---

## Part 4: Exactly how to use each command

All commands are typed as slash-commands inside Claude Code, from within your vault folder. Whatever you type after the command name becomes the topic or question.

| Command | Type this | What happens | When to use it |
|---------|-----------|--------------|----------------|
| `/learn` | `/learn quantum entanglement` or `/learn become a bioinformatics analyst` | Interviews the goal, researches with strict fact-checking, writes cited notes, gives itself a blind weighted exam, and certifies mastery in `learned-topics.md` | You want to master a topic, skill, or job role from end to end |
| `/ask` | `/ask what did I conclude about vector databases?` | Gathers every relevant note, studies them, answers with citations, and saves the answer as a new note | Quick, grounded answers from what you already know |
| `/explain` | `/explain transformers` | Re-explains an already-mastered topic in your personal learning style, visual-first | You learned it but want it re-taught the easy way |
| `/sbm` | `/sbm` or `/sbm find gaps in my biology notes` | Runs the maintenance team: catalog, clean, connect, and surface gaps and patterns | Housekeeping; run it periodically |
| `/improve-methods` | `/improve-methods` or `/improve-methods /learn` | Runs the courtroom to test a better technique on the benchmarks, then asks your approval before adopting it | Occasionally, to keep the methods current |

First-time tips:

- Before `/explain` works well, fill in `templates/learning-profile.md` and save your copy as `wiki/entities/learning-profile.md`.
- `/learn` writes into `wiki/` and updates `learned-topics.md`, `index.md`, and `log.md` for you.
- To add a source, drop any file into `raw/` and say "ingest this." The agent writes a matching note into `wiki/sources/`.

---

## Part 5: Where to paste the Master Prompt in each major AI

The Master Prompt in [MASTER-PROMPT.md](MASTER-PROMPT.md) is what turns a general assistant into a disciplined brain-first agent. Fill in the two placeholder lines first (who you are, and your vault path), then paste the code block into the right place for your AI.

### Claude Code (full power, file access)

You do not strictly need the Master Prompt here because Claude Code reads `CLAUDE.md` from the vault automatically. For best results, still add it:

1. Create or open a `CLAUDE.md` at the root of the folder you run Claude Code in (or use the vault's own `CLAUDE.md`).
2. Paste the Master Prompt block near the top.
3. Run Claude Code from inside the vault folder so it picks up both files, then use the slash-commands.

### Claude on the web, and Claude Projects

1. Open [claude.ai](https://claude.ai).
2. For account-wide behavior: open Settings, then Profile or Personalization, and paste the Master Prompt into the custom instructions box.
3. For a single body of work: create a Project, open its custom instructions or "project knowledge," and paste the Master Prompt there. A Project keeps the instruction attached to every chat in that Project.

Note: on the web there is no local file access, so the assistant follows the operating contract (brain first, then the web) and tells you what to file later, but it cannot run the file-writing steps itself.

### ChatGPT (OpenAI)

1. Open ChatGPT.
2. Click your name, then Settings, then Personalization, then Custom Instructions (or "Customize ChatGPT").
3. Paste the Master Prompt into the "How would you like ChatGPT to respond?" box.
4. For a self-contained setup, create a Custom GPT and paste the Master Prompt into its Instructions field instead.

### Google Gemini

1. Open Gemini.
2. Open Settings, then "Saved info" (also shown as personal context or custom instructions).
3. Paste the Master Prompt there so it applies to future chats.
4. Alternatively, in Google AI Studio create a system instruction and paste the Master Prompt into the system prompt field.

### Any other assistant with a system prompt or custom-instructions field

The rule is the same everywhere: find the field labeled "system prompt," "custom instructions," "personalization," or "saved info," and paste the Master Prompt code block into it. The only lines you personalize are the two placeholders.

---

## Part 6: A realistic first session

1. Fill in `templates/learning-profile.md` and save it as `wiki/entities/learning-profile.md`.
2. Paste the Master Prompt into your AI (Part 5).
3. Start Claude Code in the vault folder.
4. Run `/ask what's already in my brain?` to see it read the empty index.
5. Run `/learn <a small topic you want>` and watch the five-agent flow: research, exam, grade, certify.
6. Open Obsidian and read the new notes in `wiki/`, and the entry added to `learned-topics.md`.
7. Later, run `/sbm` to catalog and connect what you have accumulated.

That is the whole loop. You feed sources and ask questions; the system builds, tests, and maintains the knowledge; and over time it also improves the way it does all of that.

---

_Built as an ongoing personal project and released as a reusable framework under the [MIT License](LICENSE). Pricing figures for non-Claude providers are approximate; check each provider's current pricing page._
