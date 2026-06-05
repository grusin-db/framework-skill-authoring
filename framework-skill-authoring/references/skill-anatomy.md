# Skill anatomy (the structural grammar of a good skill)

Every generated skill follows a predictable structure. Two shapes recur:

- **Hub skills** (entry router, onboarding/first-run): route the user to the
  right specialist; heavy on decision trees and a `CUJ -> skill` index.
- **Specialist skills** (one capability): go deep on one domain; heavy on
  quick reference, examples, and troubleshooting.
- **Thin ops skills** (run/monitor/diagnose): a short API cookbook with a
  discover -> execute -> diagnose flow and little prose.

Use this as the section checklist when filling
[capability.SKILL.md](../assets/templates/capability.SKILL.md) and
[entry-router.SKILL.md](../assets/templates/entry-router.SKILL.md).

## Frontmatter (required)

```
---
name: <fw>-<capability>          # MUST equal the directory name
description: >-
  <third person, what it does> Use when <trigger conditions / phrases the
  user would say>.
---
```

- Third person ("Generates ...", "Configures ..."), never "I" / "you can".
- Include both WHAT it does and WHEN to trigger it; pack real trigger terms
  the consumer would type.
- Max ~1024 chars; this single line decides whether the skill activates.

## Body sections (in order)

1. **Title** -- `# <Framework> -- <Capability>`.
2. **BEFORE YOU START** -- prerequisites, "read the whole file", ordering vs
   sibling skills, and the config/paths rule if relevant. Hub skills list the
   prerequisite skills to read first.
3. **Sub-guide index** (only if the skill has sub-guides) -- a
   `| Guide | When to read |` table. Keep references one level deep.
4. **CUJ -> skill index** (hub skills only) -- a `| I want to ... | Start here |`
   table routing to specialist skills.
5. **Mental model / what it does** -- 3-6 lines or a small ASCII diagram of
   how the pieces fit. Skip generic background the agent already knows.
6. **Decision tree(s)** -- from
   [decision-tree-patterns.md](decision-tree-patterns.md), when the skill
   forces a choice.
7. **Quick reference** -- the minimal, copy-pasteable example(s). For config
   primitives, a minimal valid config. For APIs, the canonical call. For
   CLIs, the command cheat-sheet.
8. **Field / parameter reference** -- a compact table of the fields/flags the
   user sets, with type, default, and meaning. For large catalogs, give the
   common ones inline and point to the runtime doc API for the rest.
9. **Conceptual vocabularies** -- the relevant enums from
   [conceptual-taxonomies.md](conceptual-taxonomies.md) with values, defaults,
   and consequences (e.g. load types, severities, modes).
10. **Common pitfalls / anti-patterns** -- a `| Mistake | Fix |` table; call
    out "don't hand-roll X; use the primitive".
11. **Guardrails** -- destructive-operation consent gates and any
    read-only-by-default rule, placed where the risk occurs.
12. **Boundary callout** -- "when NOT to use this skill; use `<sibling>`
    instead". Prevents misfires.
13. **Troubleshooting** -- a `| Symptom | Likely cause | Fix |` table built
    from the framework's real errors.
14. **Checklist** -- a copy-paste `- [ ]` quick-start / safety checklist for
    the journey.
15. **Related skills** -- footer cross-linking siblings by **name**.

Not every skill needs all sections. Minimum viable skill = frontmatter +
BEFORE YOU START + quick reference + one decision tree (if any) + pitfalls +
related skills.

## Layout hints to inline (because the consumer is source-blind)

A skill must hard-document any layout the consumer needs, as ASCII trees:

- The framework's **artifact layout** -- where config/metadata/SQL/pipeline
  files live, per-entity vs per-system folders, canonical file names, any
  trigger-file naming grammar, and FQN/name-expansion rules.
- The **config/paths discipline** -- the env-driven settings object and the
  "never hardcode, compose from config" rule, with the per-env differences.

Never replace these with "see the repo layout" -- the consumer has no repo.

## Style rules (condensed; full list in authoring-rules.md)

- Imperative, dense, for an agent reader -- not browser prose.
- Front-load the critical patterns and required fields.
- Concrete, complete examples over explanation.
- One term per concept; reuse it everywhere.
- No source-file paths; cross-link siblings by skill name.
- Keep SKILL.md under ~500 lines; push depth into sub-guides.

## Worked skeletons

See the two templates for ready-to-fill skeletons:

- Hub / router -> [entry-router.SKILL.md](../assets/templates/entry-router.SKILL.md)
- Specialist -> [capability.SKILL.md](../assets/templates/capability.SKILL.md)
