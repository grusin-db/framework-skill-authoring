---
marp: true
theme: default
paginate: true
---

<!-- markdownlint-disable MD001 MD025 MD041 -->
<!-- _class: lead -->

# Exercises

### From "DQX? never heard of it" to a real skill family

1. **No skill** — watch Genie Code fail at DQX
2. **Tiny skill** — watch it get smart
3. **Real skill family** — build it properly

> **Where:** Ex 1–2 in **Genie Code** (browser); Ex 3 on your **laptop** (Cursor / Claude Code).
> **Demo data:** `samples.nyctaxi.trips` — built into every Databricks workspace.

---

# Setup — a clean Genie Code

Log into your **Databricks workspace** → open **Genie Code**.

**Start from zero skills.** Confirm both folders are empty:

- User skills — `/Users/<you>/.assistant/skills/`
- Workspace skills — `Workspace/.assistant/skills/`

> Anything there? Move it aside so it can't skew the demo.

**DQX on the compute:** `pip install databricks-labs-dqx` (cluster library or
base environment).

> Bonus: skip the install, reset the session, and ask Genie Code to set DQX up
> itself — sometimes it figures it out. :)

---

# Exercise 1 — no skill, it fails

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
- or ask you to point it at the source.

> That gap is exactly what a skill fills.

---

# Exercise 2 — drop in a tiny skill

**1. Create the file** `/Users/<you>/.assistant/skills/dqx/SKILL.md`
(swap `<you>` for your username).

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
# paste the code from "Practice 1 — Source-blind, with DQX" here
```
````

**3. Replace that comment** with the code from **"Practice 1 — Source-blind, with
DQX"** in [`skill-authoring-deck.md`](skill-authoring-deck.md).

---

# Exercise 2 — reload & retry

**4. Clear the skill cache** — open a **new chat** *and* **refresh the browser**
(Genie Code caches loaded skills per session).

**5. Re-run the exact same prompt:**

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

---

# Exercise 3 — build a real skill family

Not a family yet. Build DQX's skills **from the code**, on your **laptop**
(Cursor / Claude Code, not the browser).

**1. Get the source as a ZIP** (not `git` — history lets the agent cheat):

```bash
curl -L https://github.com/databrickslabs/dqx/archive/refs/heads/main.zip -o dqx.zip && unzip dqx.zip
```

**2. Delete the `skills/` folder** in the unzipped repo — the answer key.

**3. Back up any existing skills** so only DQX lands in the folder:

```bash
mv ~/.agents/skills ~/.agents/skills.bak; mkdir -p ~/.agents/skills
```

**4. Build the family** into `~/.agents/skills/` — paste
[`skill-authoring-deck.md`](skill-authoring-deck.md) in as the instructions. Each
`SKILL.md` needs frontmatter: `name` (= its folder name) + a firing `description`.

---

# Exercise 3 — test the skill

**5. Test in a new chat** — only your skills + the wheel — paste the task **plus
these constraints** so it can't cheat off the source you just unzipped:

```text
Using DQX, add quality checks to samples.nyctaxi.trips: flag rows where
fare_amount <= 0, drop rows where trip_distance < 0, split good vs bad, run it.

Constraints (do not break):
- Use ONLY the installed databricks-labs-dqx package and the dqx skills.
- Do NOT open, read, list, grep, or search the DQX source, docs, examples,
  tests, or any repository files.
- Do NOT use the web. If the skills don't cover it, say so — never guess.
```

> Belt and braces: also run it in a **new window / project without the DQX source**.

**First win:** it writes **real, runnable DQX** and never reads the source code —
that alone shows the skill works. Now actually run it →

---

# Exercise 3 — run it, 2 ways

**6a. Easy — Databricks notebook.** Paste the generated code into a notebook on
your cluster and run it. Zero local setup — use this if Databricks Connect isn't
working for you.

**6b. Fully local — Databricks Connect.** `pip install databricks-labs-dqx`, then
**add this Spark setup to your prompt** so the agent creates a session (see
[`requirements.md`](requirements.md)):

```text
Spark setup (skip the source line if your shell already loaded the env):
- Load env from repo root: set -a; source .databricks/.databricks.env; set +a
- Create Spark: from databricks.connect import DatabricksSession;
  spark = DatabricksSession.builder.getOrCreate()
```

---

# Exercise 3 — ship to Genie Code

**7. Zip your skills** — the folder holds only DQX now, so zip it all
(`rm` first — `zip` *appends* to an existing archive):

```bash
rm -f ~/dqx-skills.zip; (cd ~/.agents/skills && zip -r ~/dqx-skills.zip .)
```

**8. Import** `dqx-skills.zip` into `.assistant/skills/` in Databricks —
the workspace auto-extracts it.

**9. Try it** — back in Genie Code, re-run the Exercise 1 prompt and watch your
whole family drive it.

> Done? Restore your other skills: `mv ~/.agents/skills.bak/* ~/.agents/skills/`

---

# Exercise 4 — push it further (optional)

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

---

<!-- _class: lead -->

# ...now do it at scale (optional, but recommended :-)

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
