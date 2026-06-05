# Phase 4 -- Validate the generated family

Validation has two layers: a cheap **structural lint** (does each skill obey
the format?) and the decisive **no-source consumer simulation** (can an agent
with only the skills + installed package actually do the work?).

## Layer 1 -- Structural lint

Check every generated skill:

- [ ] `SKILL.md` exists in each skill directory.
- [ ] Frontmatter has `name` and a non-empty `description`.
- [ ] `name` exactly equals the directory name.
- [ ] `name` is lowercase letters/numbers/hyphens, <= 64 chars.
- [ ] `description` is third person and contains trigger terms (WHAT + WHEN).
- [ ] `SKILL.md` body is under ~500 lines.
- [ ] All sub-guide links are relative and **one level deep**; the targets
      exist.
- [ ] Cross-skill references use skill **names**, not file paths.
- [ ] No source-file paths (`src/`, `tests/`, internal module files) appear.
- [ ] Code fences are not indented and have a newline before them.
- [ ] The entry router exists and indexes every capability skill.
- [ ] The workspace-instructions file exists and points at the entry router.

### Leakage scan (must pass)

Grep the whole generated tree for authoring-environment leakage and fix every
hit:

- Company / team / product-internal / repo names.
- Real cluster / pool / catalog / secret-scope names, internal hostnames or
  URLs.
- The author repo's own skill names (if you reverse-engineered from an
  existing family, none of those literal names should appear).
- Hardcoded env-specific values that should come from the config object.

A quick mechanical pass: build a deny-list of known proprietary tokens and
search for each across the generated files; the result must be empty.

## Layer 2 -- No-source consumer simulation (the real test)

This proves self-containment. Give a fresh agent ONLY:

- the generated skill family, and
- the installed package (wheel/jar) -- **not** the source repo.

Then have it perform each top CUJ from your inventory, e.g.:

- "Onboard a new source end to end."
- "Add a quality check that drops bad rows."
- "Schedule this as a job."
- "Restrict which rows a group can see."
- "Diagnose why a run failed."

Run each as an isolated subagent with no repo access. For each run, judge:

- [ ] Did it find the right skill from the description alone?
- [ ] Did it follow the decision tree to the correct choice?
- [ ] Did its output use real, valid framework primitives (not invented
      APIs, fields, or enum values)?
- [ ] Did it avoid needing the source (no "I need to see the implementation")?
- [ ] Did it respect guardrails (no destructive op without consent)?
- [ ] Where it needed deep detail, did it use the runtime doc API rather than
      guess?

Every "no" is a gap in the skills, not in the consumer. Fix the skill and
re-run.

## Common gaps and the fix

| Symptom in simulation | Root cause | Fix in the skill |
|-----------------------|------------|------------------|
| Agent invents a field / enum value | the vocabulary was not inlined | add the enum + values + consequences (conceptual-taxonomies) |
| Agent picks the wrong primitive | weak/duplicated descriptions or missing router branch | sharpen descriptions; add the decision-tree branch |
| Agent asks to see source | detail referenced but not inlined | inline it, or wire the runtime doc API |
| Agent hardcodes a path/name | config discipline not stated | add the "compose from config" rule + example |
| Agent does a destructive action | no guardrail at the point of risk | add a consent gate + non-destructive alternative |
| Agent cannot find the skill at all | description lacks trigger terms | rewrite the description with the user's words |

## Regression habit

When you change a skill, re-run the affected CUJ simulation. Keep the CUJ
list from your inventory as the standing test suite for the family.

## Exit criteria

The family is done when, for every in-scope CUJ, a source-blind agent
produces correct, framework-idiomatic output on the first pass, the
structural lint is clean, and the leakage scan is empty.
