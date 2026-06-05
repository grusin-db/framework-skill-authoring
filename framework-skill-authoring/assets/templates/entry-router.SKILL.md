<!--
TEMPLATE: the entry-router skill (`<fw>`). This is the most important skill in
the family -- it routes every request to the right specialist.

Directory: skills/<fw>/SKILL.md
The frontmatter `name` MUST equal the directory name (the bare prefix `<fw>`).
Fill placeholders from capability-inventory.md, then DELETE this comment.
-->
---
name: <fw>
description: >-
  Starting point for ANY work with <Framework> -- <list the main capabilities
  / primitives>. Routes to the right specialist skill via a decision tree.
  Use proactively whenever the user mentions <Framework>, <its primitives>,
  <its file/config types>, or asks "which <primitive>", "how do I <core
  task>", or "should I use <A> or <B>". Read this first to pick the right
  approach and route to the granular `<fw>-*` skills.
---

# <Framework>

<One-paragraph description of what the framework is and the value it provides.>

## BEFORE YOU START

- Read this whole file before acting.
- Resolve all environment-specific values (paths, namespaces, identities)
  from the config object -- never hardcode them. See
  [references/config-and-paths.md](references/config-and-paths.md).
- For deep field-level detail you cannot find here, call the runtime doc API:
  `<the framework's get_docs(...) / list_docs(...) call>`.

## Mental model

<3-6 lines or a small ASCII diagram: the core primitives and how data/work
flows through the named stages. Name the primitive that owns each stage.>

## Which approach? (decision tree)

```
What do you need to do?
├── <ingest from a source>?      -> <primitive-A>   (start: <fw>-onboarding)
├── <build a curated product>?   -> <primitive-B>   (start: <fw>-<product>)
├── <general pipeline pattern>?  -> <primitive-C>   (start: <fw>-<pipeline>)
├── <schedule / orchestrate>?    -> <primitive-D>   (start: <fw>-orchestration)
├── <add quality checks>?        -> <inline> or <library> (see <fw>-data-quality)
└── <explore before authoring>?  -> <fw>-explore-* skills
```

## I want to ... (CUJ -> skill index)

| I want to ... | Start here |
|---------------|-----------|
| Onboard a new source end to end | `<fw>-onboarding` |
| Author config/metadata | `<fw>-metadata` |
| Transform / cast columns | `<fw>-expressions` |
| Add data-quality checks | `<fw>-data-quality` |
| Size / tune compute | `<fw>-compute` |
| Schedule a job | `<fw>-orchestration` |
| Restrict row access | `<fw>-security` |
| Monitor / diagnose runs | `<fw>-ops` |
| Decommission a system | `<fw>-offboarding` |
| <add one row per shipped skill> | `<fw>-________` |

## Quick reference

<The single most common minimal example -- e.g. the smallest valid config or
the canonical "hello world" call. Complete and copy-pasteable.>

## Artifact layout

<Inline the real file/config layout as an ASCII tree so a source-blind agent
knows where things go. Include per-entity vs per-system folders, canonical
file names, and any naming/FQN rules.>

## Environment & promotion

- Tiers: <e.g. dev -> test -> acceptance -> prod>. Promote in order; never
  production first.
- Per-env values resolve from the config object: <how>.

## Guardrails

- <Destructive operations> require explicit user consent; always propose a
  non-destructive alternative first.
- Default to read-only exploration before any write.

## Reference library

| Guide | Read it when |
|-------|--------------|
| [references/config-and-paths.md](references/config-and-paths.md) | resolving env-specific paths/namespaces |
| [references/migration.md](references/migration.md) | converting raw/ad-hoc code into <Framework> primitives |
| <add deep references as needed> | |

## Related skills

`<fw>-onboarding`, `<fw>-metadata`, `<fw>-data-quality`, `<fw>-compute`,
`<fw>-orchestration`, `<fw>-security`, `<fw>-ops` <... list the family>.
