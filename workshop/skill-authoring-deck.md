---
marp: true
theme: default
paginate: true
---

<!-- markdownlint-disable MD025 -->

# How to make a Skill for your framework

## Theory 1 - Write for the source-blind consumer

Target: a coding agent in **Databricks Genie Code** with your package installed
but **no repo, no docs, no internet**. (Theory: slides 1-3 - DQX practice: 4-6.)

**Two agents, two worlds.**

- **You (author)** have the source: code, tests, docs, the whole repo.
- **The consumer agent** has **only the installed `.whl`** - it runs code and
  calls the public API, but it **cannot open your files**.

**Golden rule:** everything the consumer needs lives **inside the skill**.

> If the agent ever needs to "go look at the source", the skill has failed.

---

# Theory 2 - Self-containment is the whole job

The non-negotiables for every file you write:

- **No source paths, no "see file X"** - inline complete, copy-pasteable examples.
- **Cross-link sibling skills by name**, never by file path.
- **Wire the framework's runtime doc / discovery API** as the escape hatch for
  detail you did not inline.
- **Prefer the framework's public primitives** over hand-rolled code.
- **Never hardcode env values** (paths, catalogs) - compose them from the
  config object, or a clearly-marked placeholder if there is no config object.
- The frontmatter **`description` is the most important line** - third person,
  **WHAT + WHEN**, real trigger terms. It decides whether the skill activates.

---

# Theory 3 - Structure, process, proof

**Structure (the family):**

- Always-injected **workspace instructions** ("read the entry skill first").
- An **entry router** skill (`<fw>`): decision tree + skill index.
- **One capability skill per domain** (`<fw>-<capability>`).
- Progressive disclosure; keep each `SKILL.md` under ~500 lines.

**Process (four phases):** analyze -> map to a capability taxonomy ->
generate -> validate.

**Proof (the only test that matters):** a **no-source simulation** - a fresh
agent with *only* the skills + the `.whl` runs each real task. An invented API,
a wrong enum, or "I need to see the source" is a gap **in the skill**.

---

# Practice 1 - Source-blind, with DQX

Demo library: **`databricks-labs-dqx`** (PySpark data quality).

The consumer only ran `pip install databricks-labs-dqx`. So the skill must
inline the real call surface - never point at the DQX repo:

```python
from databricks.sdk import WorkspaceClient
from databricks.labs.dqx.engine import DQEngine

dq_engine = DQEngine(WorkspaceClient())
good_df, bad_df = dq_engine.apply_checks_by_metadata_and_split(df, checks)
```

```yaml
- criticality: error      # error = quarantine, warn = flag only
  check: { function: is_not_null, arguments: { column: id } }
```

If it is not in the skill, the consumer cannot know it.

---

# Practice 2 - Self-containment, with DQX

Apply each rule to the DQX skill:

- **Use DQX's own primitives:** `check_funcs`, `DQRowRule` / `DQDatasetRule`,
  `DQEngine.validate_checks(checks)`, `DQProfiler` - do not hand-roll
  validation logic.
- **Discovery escape hatch:** point the agent at `DQEngine.validate_checks(...)`
  and the `check_funcs` registry instead of guessing function names.
- **Compose from config:** resolve workspace context from `WorkspaceClient()`;
  never hardcode a catalog or table name.
- **A description that fires:** "Data quality checks with DQX. Use when the user
  asks about data quality, quality checks, data validation, or profiling."

---

# Practice 3 - Structure, proof, deploy (DQX)

**The regenerated DQX family** (entry router + capability skills): defining
checks, applying/splitting checks, profiling and rule generation, storage.

**Prove it (no-source):** with only the wheel, ask the agent to *"add a check
that drops rows where `price` is negative and split good vs bad."* It must
produce valid DQX - real `criticality`, real function names - without the repo.

**Deploy to Genie Code:**

- Drop each skill folder under `.assistant/skills/`.
- Install DQX on the compute (cluster libraries, or a serverless base
  environment) so the imports actually resolve.

Now: we delete DQX's shipped `skills/` and rebuild it live. See the runbook.
