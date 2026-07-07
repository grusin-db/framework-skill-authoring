# How to make a Skill for your framework

## Theory 1 - Write for the source-blind consumer

Target: a coding agent in **Databricks Genie Code** with your package installed
but **no repo, no docs, no internet**.

**Two agents, two worlds.**

- **You (author)** have the source: code, tests, docs, examples.
- **The consumer agent** has **only the installed `.whl`** - it can call the
  public API but **cannot open your files**.

**Golden rule:** everything the consumer needs lives **inside the skill**.

> If the agent ever needs to "go look at the source", the skill has failed.

## Theory 2 - Get activated first, then be self-contained

**The frontmatter `description` is THE most important line in the skill.**
Get it wrong and the agent never loads the skill - nothing else you wrote
matters.

**WHAT it does + WHEN to use it** - pack in the trigger words a user would actually type.

Once it loads, **everything the skill needs is inside the skill:**

- Complete, copy-pasteable examples - no source paths, no "see src/file X".
- Cross-link sibling skills by name, or **other.md** in-skill file name or folder
- Prefer the framework's public primitives over hand-rolled code.
- Never hardcode env values - compose from config, or a clear placeholder.
- Can't inline some detail? Tell the agent to call the framework's own
  runtime validate / list functions instead of guessing.
- Docs / examples are not accesible anymore. Need them? Pack them as .whl resources
  and load them on runtime
- No external links to websites. Agents are trained not to open external "insecure" urls

## Theory 3 - Structure, process, proof

**Structure (the family):**

- Always-injected **workspace instructions** ("read the entry skill first before...").
- An **entry router** skill (`<fw>`): decision tree + skill index.
- **One capability skill per domain** (`<fw>-<capability>`).
- Progressive disclosure; keep each `SKILL.md` under ~500 lines.
- Skills can reference each other - Onthology

**Process (four phases):** analyze -> map to a capability taxonomy ->
generate -> validate.

**Proof (the only test that matters):** a **no-source simulation** - a fresh
agent with *only* the skills + the `.whl` runs each real task. An invented API,
a wrong enum, or "I need to see the source" is a gap **in the skill**.

## Practice 1 - Source-blind, with DQX

**Scope today:** design a skill for the **Genie Code audience**, where the
consumer agent has **only the wheel** (`databricks-labs-dqx`) - no repo, no docs,
no internet.

> DQX is a data quality framework for Apache Spark that enables you to define, monitor, and address data quality issues in your Python-based data pipelines.

**Example flow:** load checks -> validate their structure -> apply the checks.

The agent must be able to build this from the skill alone.

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

## Practice 2 - The same rules, in the DQX skill

Apply each rule to the DQX skill:

- **A description that fires:** "Data quality checks with DQX. Use when the user
  asks about data quality, quality checks, data validation, or profiling."
- **Use DQX's own primitives:** `check_funcs`, `DQRowRule` / `DQDatasetRule`,
  `DQEngine.validate_checks(checks)`, `DQProfiler` - do not hand-roll
  validation logic. DQX has it all.
- **Fast Runtime Validation:** point the agent at `DQEngine.validate_checks(...)`
  and the `check_funcs` registry instead of guessing function names.
- **Compose from config:** resolve workspace context from `WorkspaceClient()`;
  never hardcode a catalog or table name.

## Practice 3 - The DQX family you ship

The exact split can deviate - this is an example. The fixed principle is a
**clear cut between the router and one skill per capability**: `dqx` routes,
each `dqx-<capability>` owns a single piece of functionality.

```text
.assistant/skills/             # Genie Code loads skill folders from here
├── dqx/                       # router + onboarding hub (tree, index, quickstart)
├── dqx-define-checks/         # author the rules (YAML / Python)
├── dqx-apply-checks/          # run checks, split good vs bad rows
├── dqx-profile-and-generate/  # profile data, suggest candidate rules
└── dqx-storage/               # load / save checks (files, tables, volumes)
```

---

**Now go build it:** [`exercises.md`](exercises.md)