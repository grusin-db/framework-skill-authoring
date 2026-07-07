# Exercises

The exercises below take you from "DQX? never heard of it" to a real skill
family:

1. **[No skill](#exercise-1--no-skill-it-fails)** — watch Genie Code fail at DQX (or burn tokens trying to learn it)
2. **[Tiny skill](#exercise-2--drop-in-a-tiny-skill)** — watch it get smart
3. **[Real skill family](#exercise-3--set-up-the-dqx-repo)** — build it properly
4. **[Push it further](#exercise-4--push-it-further-optional)** (optional) — stress it with source-blind subagents
5. **[Do it at scale](#exercise-5--do-it-at-scale-optional)** (optional) — automate whole families

> **Demo data:** `samples.nyctaxi.trips` — built into every Databricks workspace.

## Exercise 1 — no skill, it fails

In **Genie Code** (Agent mode), with **no DQX skill installed**, ask:

```text
Using DQX, add data quality checks to samples.nyctaxi.trips:
- drop rows where trip_distance < 0 or fare_amount < 0
- split good vs bad rows
- then run it
```

**It fails.** With no idea what DQX is, it will:

- invent classes / functions that don't exist,
- guess the check YAML and throw at runtime,
- or start **inspecting package, file by file, burning tokens**...
  - that's a new feature in **Genie Code**

> That gap is exactly what a skill fills.

## Exercise 2 — drop in a tiny skill

**1. Create the file** `/Users/<you>/.assistant/skills/dqx/SKILL.md`
(workspace path — not your laptop; swap `<you>` for your username).

**2. Paste this in** — it's the whole skill (frontmatter + body):

````text
---
name: dqx
description: >-
  Data quality checks with DQX (databricks-labs-dqx) on Spark. Use when the user
  asks about data quality, quality checks, data validation, profiling, or
  splitting good vs bad rows.
---

If you hit a "package not available" error, run `pip install databricks-labs-dqx`.

```python
import yaml
from databricks.sdk import WorkspaceClient
from databricks.labs.dqx.engine import DQEngine

df = spark.table("<catalog>.<schema>.transactions")   # placeholder input

checks = yaml.safe_load("""
- criticality: error             # error = quarantine, warn = flag only
  check:
    function: is_not_less_than   # single bound -> NOT is_in_range
    arguments:
      column: price
      limit: 0
""")

status = DQEngine.validate_checks(checks)              # validate first
assert not status.has_errors, status.errors

dq_engine = DQEngine(WorkspaceClient())
good_df, bad_df = dq_engine.apply_checks_by_metadata_and_split(df, checks)
```
````

### Reload & retry

**3. Clear the skill cache**

- open a **new chat** in Genie Code
- **refresh** the browser page to empty skill cache
  - copy url -> close page -> open page -> paste url

**4. Re-run the exact same prompt:**

```text
Using DQX, add data quality checks to samples.nyctaxi.trips:
- drop rows where trip_distance < 0 or fare_amount < 0
- split good vs bad rows
- then run it
```

In the thinking you should now see **"Read dqx skill"** — that means it fired.
Result: real DQX calls and runnable code.

> Notice the **follow-up hints** it now suggests — add more checks, tune
> criticality, save checks to a table — it actually understands DQX.

## Exercise 3 — set up the DQX repo

> **→ Laptop** — Cursor / Claude Code

**1. Download the source** as a ZIP (not `git` — history lets the agent cheat):

Open a terminal, go to your home folder (`cd`), and run:

```bash
curl -L https://github.com/databrickslabs/dqx/archive/refs/heads/main.zip -o dqx.zip && unzip dqx.zip
```

A `dqx-main` folder will appear. Or go to `https://github.com/databrickslabs/dqx` → **Code** → **Download ZIP** and extract it.

**2. Open Cursor or Claude Code** from the `dqx-main` folder.

**3. Delete the `skills/` folder** in the repo — that's the answer key.

**4. Back up any existing skills** so only DQX lands in the folder:

```bash
mv ~/.agents/skills ~/.agents/skills.bak; mkdir -p ~/.agents/skills
```

### Build the skill family

In a new chat, paste this prompt — then append the lab instructions from
[`skill-authoring-deck.md`](skill-authoring-deck.md) (**Theory 1** through
**Practice 3**):

```text
Build the DQX skill family using the instructions below.

DQX source is in this repo root. Use docs/, demos/, and src/ as references.
Put the finished skills into skills/ next to docs/.

====== instructions for the lab =======
<paste skill-authoring-deck.md — Theory 1 through Practice 3>
```

**Skim `skills/` in the file tree** — fix gaps in chat if something's missing:

```text
skills/
├── dqx/                       # router — tree, index, quickstart
│   └── SKILL.md               # frontmatter: name + description
├── dqx-define-checks/         # author rules (YAML / Python)
│   └── SKILL.md
├── dqx-apply-checks/          # run checks, split good vs bad
│   └── SKILL.md
├── dqx-profile-and-generate/  # profile data, suggest rules
│   └── SKILL.md
└── dqx-storage/               # load / save checks
    └── SKILL.md
```

Each `SKILL.md`: copy-pasteable examples, no `src/` paths.

When it looks right, copy the skills into your agent folder (run from `dqx-main`):
(This will make it visible to all agents on your laptop)

```bash
rm -rf ~/.agents/skills
cp -r skills ~/.agents/skills
ls ~/.agents/skills
```

### Test the skill

**5. Test in a new chat**
- Open new agent in **new** Cursor/Claude Code. 
- Make sure that dqx code is not visible from there. Just use some other folder.

```text
Using DQX, add quality checks to samples.nyctaxi.trips: flag rows where
fare_amount <= 0, drop rows where trip_distance < 0, split good vs bad, run it.

Constraints (do not break):
- Use ONLY the installed databricks-labs-dqx package and the dqx skills.
- Do NOT open, read, list, grep, or search the DQX source, docs, examples,
  tests, or any repository files.
- Do NOT use the web. If the skills don't cover it, say so — never guess.

Generate code only. Don't run.
```

**First win:** it writes **real, runnable DQX** from your local skills!

### Run it

> **→ Notebook** — paste & run generated code

**6. Databricks notebook**

- Open new notebook
- Install dqx on the session: `%pip install databricks-labs-dqx`
- Copy and paste the code from the local agent's chat

### Ship to Genie Code

> **→ Genie Code** — import zip, delete Ex 2 `dqx` skill, retry

**7. Zip your skills** — the folder holds only DQX now, so zip it all
(`rm` first — `zip` *appends* to an existing archive):

```bash
rm -f ~/dqx-skills.zip; (cd ~/.agents/skills && zip -r ~/dqx-skills.zip .)
```

**8. Import** `dqx-skills.zip` into `.assistant/skills/` in Databricks —
the workspace auto-extracts it. **Delete the tiny `dqx` skill from Exercise 2
first** (remove `/Users/<you>/.assistant/skills/dqx/`) so it doesn't clash with
the imported family.

**9. Try it** — re-run the Exercise 1 prompt and watch your
whole family drive it.

> Done? Restore your other skills: `mv ~/.agents/skills.bak/* ~/.agents/skills/`

## Exercise 4 — push it further (optional)

Skill works? Stress it. Tell your agent to **add more checks** and **spawn a
source-blind subagent per task**, then compare:

```text
Add more DQX checks for samples.nyctaxi.trips and prove each with its OWN
source-blind subagent (only the dqx skills + the wheel — no source, no web):
- uniqueness across (tpep_pickup_datetime, tpep_dropoff_datetime, pickup_zip, dropoff_zip)
- pickup_zip and dropoff_zip are never null
- fare_amount in [0, 500]; tpep_dropoff_datetime not before tpep_pickup_datetime
- profile the table and auto-generate candidate rules
Report per task: did it invent an API, did it stay source-blind, good/bad counts.
```

Every "I need the source", wrong function, or bad enum is a **gap in the skill** —
fix the skill, not the prompt.

## Exercise 5 — do it at scale (optional)

You built **one** family by hand. But every framework your teams touch —
ingestion, orchestration, ML, governance — needs one.

**[`framework-skill-authoring`](https://github.com/grusin-db/framework-skill-authoring)**
— a meta-skill that builds skill families. Point it at any framework you have the
source for; it runs **analyze → map → generate → validate**.

```text
use framework-skill-authoring, and based on it and the files in the repo,
build me skills for <your framework>
```

Same rules — automated and repeatable.
