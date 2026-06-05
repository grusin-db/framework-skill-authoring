# Phase 2 -- The universal capability taxonomy

A de-branded checklist of capability **domains** that data/ETL/platform
frameworks tend to expose. For each domain ask: **does this framework have
it?** If yes, emit a skill from
[capability.SKILL.md](../assets/templates/capability.SKILL.md). If no, skip
it. Many frameworks only cover a subset -- that is expected.

This is a checklist, not a quota. Do not invent a capability the framework
does not have just to fill a tier.

How to read each entry: **Domain** -- what it is -- *signals that it exists*.
The "examples" are illustrative of the generic concept; replace them with the
framework's real terms.

## Tier 0 -- Cross-cutting outputs (almost every framework gets these)

- **Always-injected workspace instructions** -- a root file your consumer
  platform always loads, telling every agent "read the entry skill first" +
  a dependency/install-recovery recipe. *Signal: there is a recommended
  bootstrap or an import that must succeed first.*
- **Entry-point / selection router** -- the single most important skill:
  decision tree, mental model, quick references, skill index, config rules.
  *Signal: more than one primitive exists and users must choose between
  them.*
- **Config & paths reference** -- the env-driven settings object and the
  "never hardcode, compose from config" rule. *Signal: Phase-1 Signal 2.*
- **Migration from raw / ad-hoc code** -- recipes to convert hand-written
  code into framework primitives. *Signal: the framework replaces something
  people currently do by hand.*

## Tier 1 -- Authoring primitives (the "what you write")

- **Declarative pipeline authoring** -- describe a graph of inputs ->
  transforms -> outputs as config. *Signal: a pipeline/flow object built from
  components.*
- **Declarative config / metadata authoring** -- fill in schema/entity/
  load-mode models that drive behaviour. *Signal: central declarative models
  (Phase-1 Signal 3).*
- **Curated product authoring** -- author higher-level products (e.g. SQL
  views / semantic models / reports). *Signal: a product/use-case container
  primitive.*
- **Transformation & type-casting expressions** -- per-field casts, format
  parsing, derived columns. *Signal: an expression/format/type system on
  fields.*
- **Configuration serialization & round-trip** -- load/save/convert the
  models across formats and stores. *Signal: serializer mixins, save/load to
  multiple targets.*

## Tier 2 -- Getting data in (sources)

- **End-to-end onboarding / first-run** -- the hub journey from zero to a
  running pipeline. *Signal: an onboarding command/notebook orchestrating the
  full flow.*
- **Pluggable source readers / format adapters** -- per-input overrides for
  non-default formats. *Signal: a reader/adapter abstraction with multiple
  implementations.*
- **Source-system connectors (a family)** -- one skill per connector
  archetype the framework supports (e.g. CDC/replication, managed-ELT vendor,
  vendor extract via an external orchestrator, database via a gateway).
  *Signal: connector-specific entry points or notebooks.*

## Tier 3 -- Quality, validation & safe iteration

- **Inline row-level quality assertions** -- lightweight checks declared on
  fields (warn/drop/fail). *Signal: expectation/constraint fields.*
- **Quality-rules framework** -- richer checks (dataset-level, cross-dataset,
  custom). *Signal: a dedicated quality engine/integration.*
- **Source-vs-target reconciliation** -- compare counts/claims to landed
  data. *Signal: a reconciliation/processing-metadata model.*
- **Dry-run / preview before deploy** -- simulate transforms on real data
  first. *Signal: a preview/dry-run API.*

## Tier 4 -- Compute, orchestration & runtime ops

- **Compute / resource sizing & tuning** -- configure compute, performance,
  physical layout, metastore/catalog migration. *Signal: a compute/cluster
  settings model.*
- **Job orchestration & scheduling** -- build multi-task jobs/DAGs with
  triggers. *Signal: a workflow/job builder + schedule config.*
- **Running-job operations & diagnosis** -- trigger, monitor, summarize job
  failures. *Signal: job run + monitoring/formatting helpers.*
- **Running-pipeline operations & diagnosis** -- same for pipelines, plus
  performance regressions. *Signal: pipeline run + monitoring helpers.*
- **Deployment CLI & CI/CD** -- plan/verify/deploy/run from the terminal.
  *Signal: deploy-oriented CLI commands.*

## Tier 5 -- Governance & security

- **Row-level security / access control** -- restrict which rows a user/group
  sees. *Signal: row-filter / security model.*
- **Attribute-based access control & column masking** -- tag-driven
  governance policies; sometimes a migration variant per source type.
  *Signal: tag/policy model for masking or filtering.*

## Tier 6 -- Observability & delivery

- **Observability / monitoring & dashboards** -- health, error, quality
  reporting views and dashboards. *Signal: a monitoring layer / event-log
  views.*
- **Downstream publishing / delivery** -- push curated outputs to a BI or
  external consumer. *Signal: a publish/export integration.*

## Tier 7 -- Lifecycle

- **Decommission / offboarding** -- scoped, often irreversible teardown.
  *Signal: offboard/cleanup APIs.*

## Tier 8 -- Exploration / reconnaissance (pre-authoring)

- **Interactive file & schema exploration** -- list, detect format, infer
  schema, sample. *Signal: file-explorer utilities.*
- **API endpoint reconnaissance** -- probe auth/pagination/response shape
  before building a connector. *Signal: REST/HTTP exploration utilities.*
- **Interactive query / table exploration** -- sample/validate/format
  queries, extract dependencies. *Signal: SQL/query utilities.*

## Tier 9 -- Operations knowledge

- **Operations runbook / knowledge base** -- install/upgrade, environment
  promotion order, contacts; often backed by an external wiki reachable via a
  connector/MCP. *Signal: an institutional-knowledge dependency outside the
  code.*

## Tier 10 -- Contributor (developer) skills -- usually NOT shipped to consumers

Generate these only if the user explicitly wants a contributor skill pack;
they require the source repo and are out of scope for the source-blind
consumer. Keep them out of the consumer deploy target.

- Local deployment / dev loop.
- Repo architecture / framework ops.
- Internal testing harness.
- Skill QA / validation framework.
- Documentation validation tooling.
- Reference-doc example authoring.
- VCS / code-review automation.

## Mapping rules

1. **One domain -> one skill** by default. Split a skill only when it would
   exceed ~500 lines or covers clearly separate sub-journeys.
2. **Connector family -> one skill per archetype**, not one giant skill.
3. **Hub-and-spoke**: the onboarding/first-run skill and the entry router are
   hubs that route to specialists via a `CUJ -> skill` index.
4. **Prereqs and ordering**: post-setup overlays (security, monitoring,
   delivery, lifecycle) declare the onboarding skill as a prerequisite in
   their `BEFORE YOU START`.
5. **Name** each skill `<fw>-<domain>` (entry router is just `<fw>`).
6. Record the chosen set in your plan; each becomes a Phase-3 deliverable.

When the in-scope set is decided, capture the framework's conceptual
vocabularies with [conceptual-taxonomies.md](conceptual-taxonomies.md), then
generate.
