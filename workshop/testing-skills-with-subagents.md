# Testing Skills with Sub-Agents

How do you know a **skill** actually works?

Spawn agents that only know the *problem* — and make them run real code.

Similar idea here: https://agentskills.io/skill-creation/evaluating-skills

## The problem

- A skill is docs for an agent, not a library.
- "It looks right" ≠ "an agent can use it."
- We don't want to test the **library** — we want to test the **skill**.

> Does the skill alone get an agent to correct, runnable code?

## Core idea: source-blind sub-agents

- Spawn a fresh sub-agent per scenario.
- Give it **only** the problem statement + the `skills/` folder.
- **Forbid**: source tree, docs, web search, existing answers.
- The skill is the *only* bridge from problem → solution.

If the agent succeeds, the skill carries its own weight.

## The setup (generic)

- **One scenario = one prompt** (a real task in plain English).
- **One sub-agent per scenario**, run in parallel.
- A small, real environment to execute against (here: a live cluster + sample tables).
- Match the runtime to the target (e.g. client version == cluster version).

## The loop

```
read skills  ->  write code  ->  RUN it  ->  fix from skills only  ->  repeat
```

- Errors must be fixed using **skill knowledge**, not by reading source.
- Cap the attempts (e.g. ~6) so a bad skill fails fast.

## Don't just eyeball it — run it

Static review catches *shape*. Execution catches *truth*:

- Invented function names that don't exist.
- Columns that aren't in the table.
- APIs that changed or never existed.
- Code that "looks right" but throws at runtime.

Running against real data is what surfaces these.

## What "pass" means

Two gates:

1. **Rubric** — required APIs present, forbidden/cheating signals absent.
2. **Execution** — the code runs and prints real evidence (counts, rules, …).

A scenario passes only if the skill produced **runnable, correct** code.

## Reading the results

For each sub-agent, collect:

- **Files read** — proves it stayed source-blind.
- **Final script** — what the skill led to.
- **Execution output** — the proof it ran.
- **Notes** — gaps/ambiguities → the skill backlog.

Failures are skill bugs, not code bugs. Fix the **skill**.

## Why this works

- Tests the skill the way it's actually used: by an agent, cold.
- Catches drift between docs and reality automatically.
- Parallel + cheap + repeatable on every skill change.

## Takeaways

- Test **skills**, not the library.
- Keep the agent **source-blind**.
- **Run** the code — don't trust the look.
- Two gates: rubric **and** execution.
- Every failure is a skill to improve.
