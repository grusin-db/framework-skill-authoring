# Decision-tree pattern catalog

Decision trees are how skills turn a user's intent into the one correct
choice. They are the highest-value content in a skill family because they
encode the framework's real constraints. This file gives you reusable
**shapes** to instantiate from the framework's actual options.

## The universal shape

Every decision tree answers one question and routes each answer to a
consequence plus the fields/flags that answer requires.

```
<Question the user is implicitly asking>?
├── <Option A> -> <what happens> + <required fields/flags> [+ which skill]
├── <Option B> -> <what happens> + <required fields/flags>
└── <Option C> -> <what happens> + <required fields/flags>
```

Rules for good trees:

- Branch on the framework's **real enum values**, not invented ones.
- Each leaf states the **consequence** and the **minimum required input**.
- Put the **preferred / default** path first and label it.
- Keep trees shallow (2-3 levels). Deeper logic -> link to a sub-guide.
- Drop any tree whose decision the framework does not actually expose.

## Where each tree lives

- The **framework-selection** tree lives in the entry router (`<fw>`).
- Authoring trees (source-format, load-type, transform-approach, ...) live in
  the relevant authoring skill.
- Operational trees (compute/mode, deploy/action, change-classification) live
  in the compute / deploy / lifecycle skills.

## Catalog

Instantiate the ones that apply; delete the rest.

### 1. Framework-selection / "which primitive" (entry router)

```
What do you need to do?
├── <ingest-from-a-source>?      -> use <primitive-A>  (start: <fw>-onboarding)
├── <build-a-curated-product>?   -> use <primitive-B>  (start: <fw>-<product>)
├── <general pipeline pattern>?  -> use <primitive-C>  (start: <fw>-<pipeline>)
├── <schedule/orchestrate>?      -> use <primitive-D>  (start: <fw>-orchestration)
├── <add quality checks>?        -> <inline> or <library>  (see <fw>-data-quality)
└── <explore before authoring>?  -> <fw>-explore-* skills
```

### 2. Source-format / reader-choice

```
What format is the source data in?
├── <format-1> -> set <format field>=<v1> [+ <required opts, e.g. delimiter/header>]
├── <format-2> -> set <format field>=<v2> [+ <type-preservation flag>]
├── <columnar/native> -> set <format field>=<v3> (auto: <implied settings>)
└── <special/custom> -> use a per-entity <source reader> (see <fw>-source-readers)
```

### 3. Load-type

```
Is each delivery a complete snapshot?
├── YES -> <FULL> : replace target each run; key optional (recommended)
└── NO
    ├── Has a key + tracks inserts/updates/deletes? -> <INCREMENTAL/CDC>
    │     requires: >=1 key; recommended: an ordering timestamp
    └── Insert-only, no updates/deletes? -> <APPEND>
    (+ <PARTIAL SNAPSHOT> if the source delivers subsets per run)
```

### 4. CDC / delete-handling

```
Does the source send delete markers?
├── NO  -> no delete config needed
└── YES
    ├── Deleted rows should disappear?     -> <hard-delete condition>
    └── Deleted rows kept with a flag?     -> <soft-delete condition>
          └── Delete events lack attributes? -> <carry-forward flag>
```

### 5. Ingestion landing pattern

```
How does source data land in storage?
├── In partitioned/snapshot folders? -> <snapshotted pattern> (a snapshot column is added)
└── Directly in place / table-native? -> <direct/lakehouse pattern>
      (full loads may need an explicit "latest" condition)
```

### 6. Transformation approach (per field)

```
What does this column need?
├── Parse a date/number from a string in a standard format -> <format-string>
├── Custom logic / non-standard format / combine columns   -> <free expression> [+ synthetic flag]
├── Conditional value                                       -> <expression> with CASE-style logic
└── Plain type change only                                  -> just set <type> (auto-cast)
(Rule: <format> and <expression> are usually mutually exclusive.)
```

### 7. Compute / mode

```
Starting or re-onboarding a system?
├── Managed/serverless available? -> <serverless>  [preferred] (no sizing; pin <required flags>)
├── Otherwise modern/managed catalog? -> <classic> (set pool + min/max workers)
└── Stuck on legacy? -> <legacy>  [deprecated] (plan migration to modern)
```

### 8. Data-quality severity + scope

```
How should a failing row be handled?
├── Keep it, just record -> <warn>
├── Remove it from output -> <drop>
└── Stop the run         -> <fail/quarantine>

What is the check shape?
├── Single-row, simple expression -> <inline assertion>
└── Across rows / datasets / custom -> <quality library> (see <fw>-data-quality)
```

### 9. Deploy mode / action

```
What feedback do you want?
├── Fast local validation       -> mode <verify> (no side effects)
├── Interactive real objects    -> mode <draft>
└── Production deploy           -> mode <prod-target>
Action sequence: plan -> verify -> deploy -> run -> validate
```

### 10. Change classification (gates destructive ops)

```
What kind of metadata/config change is this?
├── Additive / non-breaking (new column, new check, schedule change)
│     -> apply normally; NEVER trigger a destructive reload
└── Breaking (rename, type change, key change, load-type swap)
      -> propose non-destructive alternative FIRST
      -> only with explicit user consent: <destructive full reload>
```

### 11. Connector archetype

```
How does the source arrive?
├── Real-time change replication        -> <CDC connector skill>
├── A managed-ELT vendor destination    -> <managed-ELT connector skill>
├── An external orchestrator writes it  -> <external-orchestrator connector skill>
└── A database reached via a gateway     -> <gateway-DB connector skill>
```

### 12. Explore-before-author

```
What are you inspecting?
├── Files / a storage location -> <fw>-explore-files
├── A REST/HTTP API            -> <fw>-explore-api
└── Tables / a query store     -> <fw>-explore-sql
```

## Reusing trees inside the entry router

The entry router should contain the framework-selection tree (1) plus a
compact "skill index" so any branch lands on a named skill. Keep deep trees
(2-12) in their owning skills and only summarize them in the router.
