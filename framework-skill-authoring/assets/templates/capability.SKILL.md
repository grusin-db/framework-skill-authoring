<!--
TEMPLATE: one granular capability skill (`<fw>-<capability>`).

Directory: skills/<fw>-<capability>/SKILL.md
The frontmatter `name` MUST equal the directory name.
Keep the body under ~500 lines; push depth into sibling sub-guides.
Fill placeholders from capability-inventory.md, then DELETE this comment and
any sections that do not apply.
-->
---
name: <fw>-<capability>
description: >-
  <Third person: what this skill does -- e.g. "Authors and validates <model>
  for <capability>.">  Use when <trigger conditions / phrases the user would
  say>, including <synonyms and primitive names>.
---

# <Framework> -- <Capability>

## BEFORE YOU START

- Read this whole file.
- Prerequisite skills: <e.g. `<fw>-onboarding` must be done first>, or "none".
- Resolve env-specific values from the config object; never hardcode.
- Deep detail not inlined here: call `<runtime doc API>`.

## What it does

<3-6 lines: the capability and how it fits the framework's flow.>

## Decision tree

```
<the question this capability forces>?
├── <option A> -> <consequence> + <required fields/flags>
├── <option B> -> <consequence> + <required fields/flags>
└── <option C> -> <consequence> + <required fields/flags>
```

## Quick reference

<The minimal, complete, copy-pasteable example for the most common case.>

## Field / parameter reference

| Field | Type | Default | Meaning |
|-------|------|---------|---------|
| `__________` | __________ | __________ | __________ |
| `__________` | __________ | __________ | __________ |

<For large catalogs: list the common fields here and tell the consumer to
call the runtime doc API for the rest.>

## Conceptual vocabulary

<The relevant enums for this capability, with values, default, and the
consequence of each. e.g. load types / severities / modes.>

| Value | Consequence | Requires |
|-------|-------------|----------|
| `__________` | __________ | __________ |

## Common pitfalls

| Mistake | Fix |
|---------|-----|
| __________ | __________ |
| Hand-rolling <X> instead of using <primitive> | use `<primitive>` |

## Guardrails

- <Destructive op in this domain> requires explicit consent; offer the
  non-destructive alternative first. (Delete if not applicable.)

## When NOT to use this skill

- For <adjacent task>, use `<sibling skill>` instead.

## Troubleshooting

| Symptom | Likely cause | Fix |
|---------|--------------|-----|
| __________ | __________ | __________ |

## Checklist

- [ ] __________
- [ ] __________
- [ ] Output uses real framework primitives (no invented APIs/fields/enums).

## Related skills

`<fw>` (router), `<sibling-a>`, `<sibling-b>`.
