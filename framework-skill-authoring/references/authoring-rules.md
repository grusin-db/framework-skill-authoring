# Authoring rules

These rules apply to **every file you generate**. They exist because the
consumer agent has only the installed package -- no source, no repo, no docs
site, and possibly no internet.

## The self-containment rules (non-negotiable)

| Rule | Why |
|------|-----|
| No references to source file paths (`src/...`, `tests/...`, internal module files) | They do not exist for a wheel/jar-only consumer |
| Every code example is complete and copy-pasteable | The consumer cannot "see file X" |
| Never say "see the test/example/repo at ..." | Same -- inline the example instead |
| Cross-reference sibling skills by **skill name** | The consumer resolves skills by name, not path |
| Sub-guide links use **relative filenames** within the skill dir | Keeps progressive disclosure portable |
| Use the framework's **runtime doc/discovery API** for deep detail | The consumer's only way to get field-level docs it cannot read from source |
| Prefer the framework's public primitives over hand-rolled code | The skills exist so the consumer uses the framework, not bypasses it |
| Document required layouts/paths as inline ASCII trees | The consumer has no repo to infer structure from |
| Resolve env-specific values from the config object; never hardcode | Values differ per environment; hardcoding silently targets the wrong place |

## Keep it generic and portable

The generated family must carry **no leakage** from the authoring repo or
organization:

- No company, team, product-internal, or repo names.
- No real cluster/pool/catalog/secret-scope names, real hostnames, or
  internal URLs. Use the config object or a clearly-marked placeholder.
- No deploy paths tied to one vendor; keep the deploy root and the
  workspace-instructions mechanism configurable.
- Industry-standard terms (CDC, SCD, medallion, cron) are fine; proprietary
  internal jargon is not -- translate it to the standard term, or define it.

If the framework's own public API uses a domain term (a model name, an enum),
that term is legitimate and should appear -- it is part of the contract the
consumer calls. The ban is on **authoring-environment** leakage, not on the
framework's real public vocabulary.

## Writing effective descriptions

The frontmatter `description` is the single most important line -- it decides
whether the skill activates.

- Third person: "Generates ...", "Configures ...". Not "I can ..." / "You
  can ...".
- Include WHAT it does and WHEN to use it.
- Pack trigger terms the consumer would actually type (synonyms, the names of
  the primitives, the verbs).
- Be specific: "Authors and validates `<model>` config for ingesting a new
  source" beats "metadata helper".
- Max ~1024 chars.

## Concise is key

The context window is shared. Every token competes.

- Assume the agent is smart; add only what it does not already know.
- Cut generic background ("X is a popular format that ...").
- Prefer a working example over a paragraph describing it.
- Keep each `SKILL.md` under ~500 lines; move depth into sub-guides.

## Progressive disclosure

- Put essential, high-frequency content in `SKILL.md`.
- Put deep references, large field tables, and edge cases in sub-guides
  (`reference.md`, `cookbook.md`, layer/topic guides) and link them with a
  `| Guide | When to read |` table.
- Keep references **one level deep** -- link directly from `SKILL.md`. Deeply
  nested links get partially read.

## Degrees of freedom (match specificity to fragility)

| Freedom | Use when | Form |
|---------|----------|------|
| High | many valid approaches, context-dependent | text guidance |
| Medium | a preferred pattern with variation | templates / pseudocode |
| Low | fragile, consistency-critical | exact scripts / commands |

## Consistency and anti-patterns

- One term per concept; never alternate synonyms for the same thing.
- Provide a default with an escape hatch, not a menu of equal options
  ("Use A. For the scanned-document case, use B instead.").
- No time-sensitive notes ("before next release ..."). Put deprecated
  patterns under an "Old patterns" section instead.
- Use forward-slash paths.
- Never vague skill names (`helper`, `utils`, `tools`).

## Scripts (only when they earn their place)

If a fragile operation is better as a shipped script than generated code,
include it under the skill's `scripts/`, document required packages, make
error handling explicit, and say clearly whether the agent should **run** it
or **read** it. Most skills need no scripts.

## Contributor vs consumer content

Skills target the **consumer** (package installed, no source). Do not ship
contributor/developer skills (build, test, release, repo internals) to the
consumer deploy target. If asked for a contributor pack, keep it in a
separate, clearly-labeled location.
