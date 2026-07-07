# Workshop

Hands-on workshop: rebuild a real library's Agent Skills family from **only the
installed wheel**, using [DQX](https://github.com/databrickslabs/dqx)
(`databricks-labs-dqx`) as the demo framework.

**A workshop in a repo is usually about that repo. This one isn't.**

The point is learning how to build a skill system **from scratch** — by hand, so
you understand every piece. Only the **last exercise** reaches for this repo's
[`framework-skill-authoring`](../README.md) meta-skill, which then does the whole
thing for you.

> Give a person a fish and they eat for a day; teach them to fish and they eat
> for a lifetime.


## Files

| File | What it is | When it's used |
| --- | --- | --- |
| [`requirements.md`](requirements.md) | Setup everyone should prepare **before** the workshop (laptop, an agent like Cursor/Claude Code, a Databricks workspace). Local Databricks Connect is **optional** — a notebook is enough to run code. | Send ahead of time so people arrive ready. |
| [`skill-authoring-deck.md`](skill-authoring-deck.md) | **Theory + practice** — how to author a skill (source-blind consumer, activating description, structure/process/proof), applied to DQX. | Presented first, ~10 minutes. |
| [`exercises.md`](exercises.md) | **Hands-on exercises**: (1) watch Genie Code fail at DQX with no skill, (2) drop in a tiny skill and watch it get smart, (3) build a real skill family from the wheel. | Kept on screen the whole time while people work hands-on. |
| [`testing-skills-with-subagents.md`](testing-skills-with-subagents.md) | **How to prove a skill works**: spawn source-blind sub-agents that must produce runnable code from the skill alone (rubric + execution gates). | The "test" step — how to validate what you built. |

## Flow

1. **Before** — attendees complete [`requirements.md`](requirements.md) so their
   laptop, agent, and Databricks workspace are ready. Notebook-based testing is
   the default; Databricks Connect is optional for local runs.
2. **Theory + practice (~10 min)** — present
   [`skill-authoring-deck.md`](skill-authoring-deck.md).
3. **Hands-on (~35 min)** — put [`exercises.md`](exercises.md) on screen and work
   through the **core Exercises 1–3**: watch Genie Code fail at DQX, drop in a tiny
   skill, then build a real skill family. **Go exercise by exercise as a group** —
   present each one, let everyone do it, then **pause and wait until the whole room
   has finished before moving to the next.** Exercises 4–5 are optional take-home
   extras. Use the per-exercise time estimates to pace the room.

