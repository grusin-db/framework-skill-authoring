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

> Demo data: `samples.nyctaxi.trips` — built into every Databricks workspace

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

**1. Create a skill file:**

```text
/Users/<you>/.assistant/skills/dqx/SKILL.md
```

**2. Fill it** — a firing description + the copy-pasteable DQX flow
(load checks → `validate_checks` → apply & split).

> Grab the code from **"Practice 1 — Source-blind, with DQX"** in
> [`skill-authoring-deck.md`](skill-authoring-deck.md).

**3. Clear the cache** — new chat **+** browser refresh (skills cache per session).

**4. Re-run the same prompt** → now it's smart: real DQX, runnable, no "show me
the source".

---

# Exercise 3 — build a real skill family

One slide helped, but it isn't a family. Build DQX's skills **from the code**.

**1. Get the source as a ZIP** (not `git` — history lets the agent cheat):

```bash
curl -L https://github.com/databrickslabs/dqx/archive/refs/heads/main.zip -o dqx.zip && unzip dqx.zip
```

**2. Delete its `skills/` folder** — that's the answer key.

**3. Build it yourself** into `.agents/skills/`, applying the deck's rules
(source-blind consumer, activating description, structure / process / proof).

> Hint: paste [`skill-authoring-deck.md`](skill-authoring-deck.md) in as the
> instructions and let the agent build the DQX family from it.

---

# Exercise 3 — run & ship it

**4. Set up to run** (only now you need it):

- `pip install databricks-labs-dqx`
- Spark via Databricks Connect — see [`requirements.md`](requirements.md)
- *No Spark, no DQX run.*

**5. Test** — fresh agent, **only your skills + the wheel**. Ask:

```text
Using DQX, add quality checks to samples.nyctaxi.trips: flag rows where
fare_amount <= 0, drop rows where trip_distance < 0, split good vs bad, run it.
```

→ real, runnable DQX — never asks for the source.

**6. Deploy** — copy the skill folders to `.assistant/skills/` in Genie Code.

**7. Try it** — re-run the Exercise 1 prompt and watch your family drive it.

---

<!-- _class: lead -->

# ...now do it at scale

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
