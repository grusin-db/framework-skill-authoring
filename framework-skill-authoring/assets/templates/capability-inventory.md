# Capability inventory -- <framework name>

Phase-1 output. Fill from the source; everything here gets reused in the
generated skills. Keep examples real (mined from tests/examples), not invented.

## Identity

- Framework name / import name: __________
- Chosen skill prefix `<fw>`: __________
- One-line purpose: __________
- Consumer platform(s): __________

## Public API surface

The objects a user imports to do real work (from top-level exports):

- `__________` -- __________
- `__________` -- __________

## Config / settings object

- Object / loader: __________
- Resolves: paths __________, namespaces __________, environment __________,
  identities __________
- Selected via: __________ (profile / env var / context)
- Rule for skills: never hardcode the above; compose from this object.

## Primary declarative model(s)

For each central model:

- Model: __________   (nests: __________)
  - Required fields: __________
  - Key enums: __________
  - Defaults worth noting: __________
  - Field tags/metadata: __________
  - Minimal valid example:

```
<paste a real minimal example>
```

## CLI commands

| Command | Purpose | Key flags |
|---------|---------|-----------|
| `__________` | __________ | __________ |

## Runtime doc / discovery API

- Call(s): `__________`  -> returns: __________
- Absent? __________  (if absent, plan to inline more detail)

## Top CUJs (critical user journeys)

1. __________ -> steps: __________
2. __________ -> steps: __________
3. __________ -> steps: __________

## Decision points (-> become decision trees)

- Question: __________ ? options -> consequences: __________
- Question: __________ ? options -> consequences: __________

## Conceptual vocabularies (term / values / default / consequence)

- Layers/stages: __________
- Load semantics: __________
- CDC / delete: __________
- Change-tracking (SCD): __________
- Schema handling + evolution: __________
- Type/transformation authoring: __________
- Key / layout flags: __________
- Component model + materialization kinds: __________
- Processing engines: __________
- Quality severity / scope / mechanism: __________
- Compute model: __________
- Scheduling / trigger grammar: __________
- Orchestration model: __________
- Access-control mechanisms / classes / identity: __________
- Deploy modes / action verbs: __________
- Change classification (breaking vs not): __________
- Environment tiers + promotion: __________
- Naming / FQN conventions: __________
- Reconciliation / monitoring: __________
- Technical / system columns: __________

## Layouts to inline (artifact layout the consumer needs)

```
<paste the real config/metadata/file layout as an ASCII tree>
```

## Failure modes / guardrails

- Common errors -> fixes: __________
- Destructive operations (need consent): __________
- Anti-patterns to warn against: __________

## Audience split

- End-user / consumer capabilities (ship): __________
- Contributor-only (do not ship to consumer): __________
