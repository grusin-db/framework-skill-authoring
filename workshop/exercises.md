---
marp: true
theme: default
paginate: false
---

<!-- markdownlint-disable MD025 MD041 -->

# Exercises

One story, three steps:

1. **Genie Code with no skill** — watch it fail at DQX.
2. **Drop in a tiny skill** — watch it get smart.
3. **Build a real skill family** — do it properly.

> Demo data: `samples.nyctaxi.trips` (built into every Databricks workspace).

---

# Setup — a clean Genie Code

Log into your **Databricks workspace** and open **Genie Code**.

Start from **zero skills** — check both locations are empty:

- **User skills:** `/Users/<you>/.assistant/skills/`
- **Workspace skills:** `Workspace/.assistant/skills/`

(if anything's there, move it aside so it doesn't skew the demo)

Make sure the **DQX library is on the compute**:
`pip install databricks-labs-dqx` (cluster library or base environment).

Later on try when it's not there - reset session - somehow genie code know how to install it. Sometimes... :)

---

# Exercise 1 — no skill, it fails

In Genie Code (Agent mode), with **no DQX skill installed**, ask:

```
Using DQX, add data quality checks to samples.nyctaxi.trips:
- drop rows where trip_distance < 0 or
- fare_amount < 0
- split good vs bad rows
- then run it.
```

**Expected: it fails.** It doesn't know DQX, so it will:

- invent class / function names that don't exist,
- guess the check YAML / API shape and throw at runtime,
- or ask you to point it at the source.

That gap is exactly what a skill fills.

---

# Exercise 2 — drop in a tiny skill

Copy the **DQX example slide** (from the authoring deck) into a skill file:

```
/Users/<you>/.assistant/skills/dqx/SKILL.md
```

Include a **firing description** + the copy-pasteable DQX flow:
load checks → `DQEngine.validate_checks(checks)` → apply & split.

**Clear the skill cache:** open a **new chat** *and* **refresh the browser** —
Genie Code caches loaded skills per session.

Re-run the **exact same prompt** from Exercise 1.

**Now it's much smarter:** real DQX calls, runnable code, no "show me the source".

---

# Exercise 3 — build a real skill family

One pasted slide helps, but it isn't a family. Now build it properly:
rebuild DQX's skills **from the wheel**.

1. **Install the wheel** - `pip install databricks-labs-dqx`
2. **Get the source as a ZIP, not git** - `curl -L https://github.com/databrickslabs/dqx/archive/refs/heads/main.zip -o dqx.zip && unzip dqx.zip`
   (no `git clone` - git history lets the agent recover the answer key and cheat)
3. **Delete its `skills/` folder** - the answer key
4. **Build it yourself** - apply the deck's rules (source-blind consumer,
   activating description, structure/process/proof); author the DQX skill family
   into `.agents/skills/` (the OSS standard) so your local agent finds it
5. **Test** - fresh agent, **only your skills + the wheel**:
   *"add a check that drops rows where price < 0 and split good vs bad"* -
   must produce real DQX, never ask for the source
6. **Deploy** - copy the skill folders to `.assistant/skills/` in Genie Code
7. **Try it** - re-run the Exercise 1 prompt; watch your family drive it

---
<!-- 20 minute workshop; once finishes -->
---

# ...oh, now you want to do it at scale?

You hand-built **one** family. Now every framework your teams touch -
ingestion, orchestration, ML, governance. Hand-craft each one?

**Check out this repo:** [`framework-skill-authoring`](https://github.com/grusin-db/framework-skill-authoring) -
a **meta-skill that builds skill families**. Point it at any framework you have
the source for; it runs the four phases: **analyze -> map -> generate -> validate**.

```
use framework-skill-authoring, and based on it and the files in the repo,
build me skills for <your framework>
```

Same rules, now automated and repeatable at scale.
