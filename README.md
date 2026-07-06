# framework-skill-authoring

A **meta-skill**: a skill that builds skills.

Point it at a framework you have the **source** for, and it generates a
self-contained Agent Skills family that another agent can use later with only
the **installed package** (wheel/jar) — no source, no repo, no docs.

![How to create a skill: 1. draw some circles, 2. draw the rest of the owl](misc/meme.png)

Most "how to make a skill" guides stop at step 1. This repo is step 2.

It's geared mainly toward **data engineering** frameworks, but works just as
well for ML/data-science libraries or anything you'd typically run on
Databricks. The capability taxonomy is broad enough to map most code
frameworks.

```mermaid
flowchart TB
  subgraph AUTHOR[Author time -- has source]
    direction LR
    SRC[Framework source<br/>code, tests, docs] --> AGENT[Author agent +<br/>this meta-skill]
  end

  AGENT -->|analyze, map,<br/>generate, validate| FAMILY[Skill family<br/>entry router + capability skills<br/>+ workspace instructions]

  subgraph RUNTIME[Run time -- no source]
    direction LR
    PKG[Installed package<br/>wheel / jar] --> CONSUMER[Consumer agent]
  end

  FAMILY --> CONSUMER

  classDef artifact fill:#fef3c7,stroke:#d97706,stroke-width:2px,color:#1f2937;
  classDef agent fill:#dbeafe,stroke:#2563eb,color:#1f2937;
  class FAMILY artifact
  class AGENT,CONSUMER agent
```

## This repository

```
framework-skill-authoring/
├── SKILL.md              # meta-skill entry point
├── references/           # phase-by-phase playbook
└── assets/templates/     # ready-to-fill skeletons
```

Install this meta-skill by placing `framework-skill-authoring/` in your agent's
skills directory (e.g. `~/.agents/skills/`).

## Usage

Open the target framework's codebase and run:

```
use framework-skill-authoring, and based on it, and available files in the
repo build me skills for <your framework> framework
```

Then verify:

```
verify correctness of everything you built vs the code/docs — no
hallucinations, invalid references, or typos in function/attribute names
```

The agent must have the framework **source**; it can't author from a black box.

**Recommended setup:** run the main agent on Claude Opus 4.8 Thinking at the
highest reasoning level you can afford for the best results, and let cheaper/
faster models handle scanning and subagent exploration. This works best in
Cursor, which auto-spawns Composer for the exploratory work.

## How it works — four phases

1. **Analyze** the source — public API, config object, declarative models, CLI,
   runtime doc APIs, critical user journeys, decision points.
2. **Map** to a universal capability taxonomy; emit only domains the framework has.
3. **Generate** the workspace-instructions file, an entry router, and one skill
   per capability.
4. **Validate** — lint + a no-source consumer simulation of each journey.

Once generation is done, tune further with your own domain knowledge — add or
remove capability domains, split or merge skills, and sharpen examples to match
how your teams actually use the framework.

**Golden rule:** everything the consumer needs lives inside the skills — no
source paths, copy-pasteable examples, cross-link by skill name, no org leakage.

## Example output

A skill family for a framework with prefix `<fw>` (e.g. its import name):

```
<deploy-root>/
├── workspace-instructions.md         # always-injected: "read <fw> first" + recovery
└── skills/
    ├── <fw>/                          # entry router: decision tree + skill index
    │   ├── SKILL.md
    │   ├── references/                # config/paths, migration, deep refs
    │   └── assets/templates/          # starter configs the consumer copies
    ├── <fw>-onboarding/               # one skill per in-scope capability
    │   └── SKILL.md
    ├── <fw>-data-quality/
    │   └── SKILL.md
    ├── <fw>-orchestration/
    │   ├── SKILL.md
    │   └── reference.md               # sub-guide (progressive disclosure)
    └── <fw>-governance/
        └── SKILL.md
```

Only the domains the framework actually has get emitted.

## Installing the generated skill family

The output is a folder of skills. Drop it where your consumer agent reads skills:

- **Local agent (Claude, Cursor, etc.):** copy each generated skill folder into
  `~/.agents/skills/` (or the project's `.agents/skills/`).
- **Databricks Genie Code:** put each skill folder under `.assistant/skills/` —
  `Workspace/.assistant/skills/` for workspace-wide, or
  `/Users/{username}/.assistant/skills/` for personal. Each skill needs its own
  folder with a `SKILL.md`. Genie Code loads them in Agent mode; start a new
  thread after edits. See
  [Extend Genie Code with agent skills](https://docs.databricks.com/aws/en/genie-code/skills).

## Databricks: install the framework on compute

The skills teach an agent to *use* a framework; the framework's in-scope
libraries still need to be installed on the compute that runs the code, on both
clusters and serverless.

- **Clusters:** trivial — add the framework and its in-scope libraries as
  [cluster libraries](https://docs.databricks.com/aws/en/libraries/cluster-libraries)
  so every notebook and job on the cluster can import them.
- **Serverless:** have an admin set up a workspace-wide
  [base environment](https://docs.databricks.com/aws/en/admin/workspace-settings/base-environment)
  with those libraries. When serverless is selected it loads automatically, so
  users avoid the runtime errors they'd hit running serverless without the
  custom libs.

## Workshop

Want to run this as a workshop (or try it yourself)? The [`workshop/`](workshop/)
folder has ready-to-present material that rebuilds a real library's skill family
from only the installed wheel, using [DQX](https://github.com/databrickslabs/dqx)
(`databricks-labs-dqx`) as the demo. See [`workshop/README.md`](workshop/README.md)
for the files and flow:

- [`workshop/requirements.md`](workshop/requirements.md) — what to prepare
  **before** the workshop: a laptop, an agent (Cursor / Claude Code), and a
  Databricks workspace. Local Databricks Connect is optional; a notebook is
  enough to run generated code.
- [`workshop/skill-authoring-deck.md`](workshop/skill-authoring-deck.md) — a
  6-slide [Marp](https://marp.app/) deck: 3 theory slides + the same 3 points
  applied to DQX.
- [`workshop/exercises.md`](workshop/exercises.md) — the hands-on exercises:
  (1) watch Genie Code fail at DQX with no skill, (2) drop in a tiny skill and
  watch it get smart, (3) rebuild DQX's skill family from the wheel and deploy to
  Genie Code.
- [`workshop/testing-skills-with-subagents.md`](workshop/testing-skills-with-subagents.md) —
  a deck on proving a skill works: spawn source-blind sub-agents that must produce
  runnable code from the skill alone (rubric + execution gates).

Render the slides with:

```
npx @marp-team/marp-cli@latest workshop/skill-authoring-deck.md -o deck.pdf
```
