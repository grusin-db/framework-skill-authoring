# Phase 1 -- Analyze the framework

Goal: build a complete picture of **what the framework does** and **how a
user drives it**, so you can decide which skills to write and fill them with
real, working detail. Record findings in
[capability-inventory.md](../assets/templates/capability-inventory.md).

You have the source now. The consumer will not. Capture aggressively.

## How to work this phase

Prefer parallel, read-only exploration. Spread the signals below across a few
exploration passes (or subagents) rather than reading the whole repo
linearly. Stop when the inventory is filled, not when you have read every
file.

## Signal 1 -- The public API surface

This is the contract the consumer actually calls. Find it first.

- Top-level package `__init__` exports / `__all__` -- the curated surface the
  authors intend users to import. This is your shortlist of primitives.
- Package and module docstrings -- often state the purpose and the "start
  here" object in one paragraph.
- Re-exports: things imported into the top level from deep modules are
  "blessed" public API; deep-only symbols are usually internal.

Record: the handful of objects/functions a user imports to do real work.

## Signal 2 -- The config / settings object

Almost every framework has a single source of environment-dependent
settings (paths, catalog/database names, endpoints, credentials handles).

- Find the settings singleton or loader (often `config`, `settings`, a
  `*Settings` model, or an env-driven loader).
- Note what it resolves: storage paths, target namespaces, environment name,
  service identities.
- Note how it differs per environment (dev/test/prod) and how it is selected
  (profile, env var, workspace context).

Record this as a hard rule for the generated skills: **never hardcode any
value the config resolves; always compose it from config**. Source-blind
agents otherwise invent wrong paths.

## Signal 3 -- The primary declarative models (the authoring surface)

Frameworks that are "metadata/config-driven" expose models the user fills in
(pydantic models, dataclasses, schema objects, YAML schemas). These ARE what
the user authors.

- Identify the 1-3 central models and how they nest (system -> entity ->
  attribute, or pipeline -> component, etc.).
- For each, capture: required fields, key enums, defaults, validation rules,
  and field tags/metadata (primary key, timestamp, exclude-from-persist).
- Capture how a model is created, validated, saved, and loaded.

Record: minimal valid examples for each model (you will inline these in
skills). Mine real examples from tests/examples rather than inventing.

## Signal 4 -- CLI entry points

- Read `pyproject.toml` / `setup.py` (or equivalent) `console_scripts` /
  entry-points to list installed commands.
- Run each command's `--help` (read-only) to capture subcommands, flags, and
  defaults.
- Map each command to the capability it serves (deploy, validate, run,
  explore, scaffold).

Record: the command cheat-sheet per capability. CLIs are first-class for the
consumer because they need no source.

## Signal 5 -- Runtime documentation / discovery APIs

This is the most important signal for source-blind usability. Look for
functions that return docs, schemas, or component lists **at run time**.

- Examples of the shape: `get_docs("<component>")`, `list_docs(...)`,
  `help()` on a model, schema-dump methods, a registry that enumerates
  available components.
- These let the consumer fetch field-level detail you did not inline.

Record: the exact call(s) and what they return. Every generated skill that
references a large catalog should tell the consumer to call these instead of
guessing. If the framework has NO such API, you must inline more detail and
note the gap.

## Signal 6 -- CUJ and golden-example sources

Critical User Journeys (CUJs) are the end-to-end things users actually do.
Find them in:

- Notebooks / quickstarts / `examples/` -- the intended happy paths.
- Tests -- especially end-to-end / integration tests; they encode real,
  working sequences and edge cases.
- README / docs -- the authors' own framing of "getting started".
- Tutorials and migration guides -- reveal old-vs-new patterns.

Record: the top CUJs ("ingest a new source", "build a data product",
"schedule a job", "add a quality check"), each as an ordered step list. These
become the onboarding/hub skill and the per-skill "quick start".

## Signal 7 -- Decision points (when to use A vs B)

Find every place the framework forces a choice. These become decision trees.

- Multiple primitives that overlap (which one for which job?).
- Enum fields with branchy consequences (load type, mode, severity, engine).
- "Use X unless Y, then Z" guidance in docs/comments.
- Validation that rejects certain combinations (encodes real constraints).

Record: each decision as `question -> options -> consequence + required
fields`. See [decision-tree-patterns.md](decision-tree-patterns.md).

## Signal 8 -- Architecture, layers, security, scheduling, lifecycle

- **Layers/stages**: how data or work flows through named stages; which
  primitive owns each stage.
- **Security/governance**: access control, masking, policy mechanisms, the
  identity model.
- **Scheduling/orchestration**: how work is scheduled and run; the job/DAG
  model; trigger mechanisms.
- **Lifecycle**: create/update/decommission; what is destructive; what
  requires consent.
- **Technical/system columns or metadata** the framework injects
  automatically -- enumerate them.

Record these as the conceptual vocabularies (see
[conceptual-taxonomies.md](conceptual-taxonomies.md)).

## Signal 9 -- Failure modes and guardrails

- Common errors and their fixes (from tests, error messages, troubleshooting
  docs) -> per-skill troubleshooting tables.
- Destructive operations (full reloads, drops, irreversible migrations) ->
  consent gates and read-only-by-default rules in the generated skills.
- Anti-patterns the authors warn against -> "don't do X" callouts.

## Distinguish end-user vs contributor surface

Separate two audiences as you inventory:

- **End-user / consumer capabilities** -- using the framework (authoring
  configs, deploying, operating). These get shipped skills.
- **Contributor / framework-developer capabilities** -- building, testing,
  releasing the framework itself. These are out of scope for the source-blind
  consumer; either skip them or mark them clearly as developer-only and do
  not ship them to the consumer deploy target.

## Output of Phase 1

A filled [capability-inventory.md](../assets/templates/capability-inventory.md)
containing: the primitives, the config object, the central models with
minimal examples, the CLI cheat-sheet, the runtime doc API, the top CUJs, the
decision points, and the conceptual vocabularies. When that is complete, move
to Phase 2 ([universal-taxonomy.md](universal-taxonomy.md)).
