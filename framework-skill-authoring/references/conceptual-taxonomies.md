# Conceptual taxonomies (the vocabularies to capture verbatim)

The capability taxonomy ([universal-taxonomy.md](universal-taxonomy.md)) tells
you **which skills** to write. This file lists the **conceptual vocabularies**
inside those skills -- the enumerations and modes a source-blind agent will
never invent correctly. These are where generated skills usually fail: the
consumer guesses a value that does not exist, or misses the consequence of
the one it picked.

For every taxonomy below that the framework has, record four things and put
them in the relevant skill:

1. **The framework's term(s)** for it.
2. **The allowed values** (the exact enum literals).
3. **The default**.
4. **The consequence of each value** (what changes downstream) and any
   **required-together / mutually-exclusive** rules.

Skip taxonomies the framework does not have. Add ones it has that are not
listed here.

## Data-shape and modeling vocabularies

- **Layer / stage model** -- the named stages work flows through (a common
  industry example is the medallion pattern: raw -> cleaned -> curated). Which
  primitive owns each stage; "source-aligned" vs "integrated" variants.
- **Load / ingestion semantics** -- e.g. full snapshot, incremental/CDC,
  append-only, partial snapshot. Which require a key or an ordering column;
  note limitations (incremental load often cannot represent deletes).
- **CDC / delete handling** -- hard delete vs soft delete; tombstones; a
  delete-flag column; whether delete events carry full attributes or need
  carry-forward from the last known row.
- **Change-tracking model** -- e.g. SCD Type 1 vs Type 2; which layer or
  primitive each lives in (often history tracking is only in one place).
- **Schema handling** -- keep source types vs cast everything to string;
  keep vs sanitize column names; enforce-declared-schema toggle.
- **Schema evolution modes** -- e.g. rescue/quarantine new columns, add new
  columns, fail on new columns.
- **Key / physical-layout flags** -- primary key; sequence-by / ordering
  timestamp; clustering keys; partition columns.
- **Technical / system columns** -- the metadata columns the framework
  injects automatically (e.g. ingestion timestamp, source file name, run id,
  rescued/quarantine column, delete flag, row/key hashes). Enumerate the full
  set so the consumer knows they exist and does not redefine them.

## Transformation and engine vocabularies

- **Type / transformation authoring** -- the ways to define a column:
  declarative type-cast vs format-string parse vs free expression vs custom
  function; derived/synthetic columns; numeric precision/scale; date-format
  conventions (e.g. Java-style vs strftime -- a classic source of bugs).
- **Component model** -- the catalog of sources / transformations / sinks (or
  the framework's equivalents); enumerate each member.
- **Materialization kinds** -- e.g. view, temporary view, materialized view,
  streaming table; which the framework picks by default per context.
- **Processing engines / execution targets** -- e.g. a managed declarative
  pipeline engine vs an imperative engine vs a warehouse engine; when each is
  used and how to choose.

## Quality vocabularies

- **Severity** -- e.g. warn vs drop vs fail/quarantine; what each does to the
  row and the run.
- **Scope** -- row-level vs dataset-level vs cross-dataset (foreign key) vs
  domain-specific (e.g. geospatial) vs custom.
- **Mechanism** -- inline lightweight checks vs a fuller quality library;
  any "criticality" levels.

## Runtime, compute, and scheduling vocabularies

- **Compute model** -- serverless vs classic vs legacy; performance profile
  (e.g. standard vs performance-optimized); release channel (e.g. current vs
  preview); development vs production mode.
- **Scheduling / trigger grammar** -- the accepted forms: manual, periodic
  ("every N units"), cron, continuous (+ a freshness SLA), and event/file
  triggers. Record the exact grammar and any rejected forms.
- **Orchestration model** -- how work is triggered (trigger-file, schedule,
  manual, blob/event); the job/DAG model (layers/tasks + dependencies); how
  cross-engine tasks are resolved.

## Governance vocabularies

- **Access-control mechanisms** -- row-level filter vs column mask; per-object
  function vs attribute/tag-based policy.
- **Access classes** -- e.g. admin/bypass vs simple column filter vs complex
  join-based filter; the classification that decides which policy shape is
  deployed.
- **Identity model** -- group-based vs principal-based; admin-bypass /
  except-principals; how membership is evaluated.

## Deployment and lifecycle vocabularies

- **Deploy modes** -- e.g. local-verify vs draft vs warehouse-prod vs
  pipeline-prod; per-object overrides; the default per environment.
- **Action verbs** -- the lifecycle verbs the tooling exposes: plan, verify,
  deploy, run, validate, destroy.
- **Change classification** -- additive/non-breaking vs breaking changes;
  exactly which changes are breaking; which require a destructive full reload.
  This drives the consent gates in the generated skills.
- **Lifecycle stages** -- onboard, update, reload (destructive), offboard.

## Environment and naming vocabularies

- **Environment tiers & promotion** -- the tiers (e.g. dev -> test -> accept
  -> prod) and the rule to promote in order (never production first). How
  per-env config is resolved; how secrets/env-specific values are injected
  vs which names are auto-mutated per environment.
- **Naming & FQN conventions** -- per-entity vs per-system folders; canonical
  file names; any trigger-file naming grammar; short vs fully-qualified name
  expansion (e.g. 2-part vs 3-part names); human-readable resource names vs
  raw IDs.

## Observability vocabularies

- **Reconciliation model** -- e.g. three-way counts: source claim vs delivered
  vs landed; what "drift" means and how it is surfaced.
- **Monitoring surface** -- event-log/monitoring views; health rules (e.g.
  run-duration thresholds); the shape of error and performance summaries.

## How to use what you captured

- Put each enum where the consumer needs it: load/CDC/schema vocab in the
  metadata/authoring skill; severity/scope in the quality skill; compute/
  schedule in the compute/orchestration skills; access classes in the
  security skill; deploy modes/verbs in the deploy/CLI skill; env tiers and
  naming in the entry router and config reference; tech columns wherever rows
  are described.
- Turn the branchy ones into decision trees
  ([decision-tree-patterns.md](decision-tree-patterns.md)).
- Turn the "wrong value" traps into pitfalls/troubleshooting rows.
- If a vocabulary is large, point the consumer at the runtime doc API instead
  of inlining all of it -- but always inline the values and consequences they
  must choose between.
