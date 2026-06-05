# Authoring plan -- <framework name>

Copy this into your working plan and complete it top to bottom. Do not write
any skill before the inventory (step 1) is filled.

Framework prefix `<fw>`: __________   (short, lowercase; e.g. the import name)
Consumer platform / deploy target: __________
Workspace-instructions mechanism on that platform: __________

## Phase 1 -- Analyze   (guide: references/analyze-framework.md)

- [ ] Public API surface captured (top-level exports / `__all__`).
- [ ] Config/settings object identified (+ what it resolves per env).
- [ ] Primary declarative model(s) captured with minimal valid examples.
- [ ] CLI commands captured (`--help` per command).
- [ ] Runtime doc/discovery API captured (exact call + output) -- or noted as
      absent.
- [ ] Top CUJs listed as ordered step lists.
- [ ] Decision points listed (question -> options -> consequence).
- [ ] Conceptual vocabularies captured (load types, modes, severities, ...).
- [ ] End-user vs contributor surface separated.
- [ ] Inventory written to capability-inventory.md.

## Phase 2 -- Map   (guides: universal-taxonomy.md, conceptual-taxonomies.md)

In-scope capability skills to emit (tick the domains the framework has):

- [ ] Entry router (`<fw>`)  -- always
- [ ] Workspace-instructions file  -- always
- [ ] Config & paths reference
- [ ] Migration from raw code
- [ ] ____________________ (add one row per in-scope domain)
- [ ] ____________________
- [ ] ____________________

Out of scope (framework lacks it / contributor-only): ____________________

## Phase 3 -- Generate   (guides: skill-anatomy.md, decision-tree-patterns.md)

- [ ] Workspace-instructions file written (template: workspace-instructions.md).
- [ ] Entry router written, with framework-selection tree + skill index
      (template: entry-router.SKILL.md).
- [ ] One capability skill per in-scope domain written (template:
      capability.SKILL.md).
- [ ] Conceptual vocabularies placed in their owning skills.
- [ ] Required layouts/paths inlined as ASCII trees.
- [ ] Self-containment pass done (no source paths; examples inline; runtime
      doc API wired; siblings cross-linked by name).

## Phase 4 -- Validate   (guide: references/validate-skills.md)

- [ ] Structural lint clean (frontmatter, name==dir, line budget, link depth).
- [ ] Leakage scan empty (no org/repo/internal names or hardcoded env values).
- [ ] No-source consumer simulation run for each CUJ; all pass.
- [ ] Gaps fixed; simulation re-run.

## Exit

- [ ] Every in-scope CUJ produces correct, framework-idiomatic output from a
      source-blind agent on the first pass.
