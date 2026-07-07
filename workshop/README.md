# Workshop

Hands-on workshop: rebuild a real library's Agent Skills family from **only the
installed wheel**, using [DQX](https://github.com/databrickslabs/dqx)
(`databricks-labs-dqx`) as the demo framework.

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
3. **Hands-on** — put [`exercises.md`](exercises.md) on screen and work through
   the three exercises: watch Genie Code fail at DQX, drop in a tiny skill, then
   build a real skill family.
4. **Test & deploy** — validate the skill with source-blind sub-agents
   ([`testing-skills-with-subagents.md`](testing-skills-with-subagents.md)), then
   deploy it to Genie Code.
