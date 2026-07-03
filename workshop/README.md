# Workshop

Hands-on workshop: rebuild a real library's Agent Skills family from **only the
installed wheel**, using [DQX](https://github.com/databrickslabs/dqx)
(`databricks-labs-dqx`) as the demo framework.

## Files

| File | What it is | When it's used |
| --- | --- | --- |
| [`requirements.md`](requirements.md) | Setup everyone should prepare **before** the workshop (laptop, an agent like Cursor/Claude Code, a Databricks workspace, and the Spark/Databricks Connect setup). | Send ahead of time so people arrive ready. |
| [`skill-authoring-deck.md`](skill-authoring-deck.md) | The **theory + practice** deck — how to author a skill (source-blind consumer, activating description, structure/process/proof), applied to DQX. | Presented first, ~10 minutes. |
| [`exercises.md`](exercises.md) | The **hands-on exercises** deck: (1) watch Genie Code fail at DQX with no skill, (2) drop in a tiny skill and watch it get smart, (3) build a real skill family from the wheel. | Kept on screen the whole time while people work hands-on. |
| [`testing-skills-with-subagents.md`](testing-skills-with-subagents.md) | A deck on **how to prove a skill works**: spawn source-blind sub-agents that must produce runnable code from the skill alone (rubric + execution gates). | The "test" step — how to validate what you built. |

## Flow

1. **Before** — attendees complete [`requirements.md`](requirements.md) so their
   laptop, agent, and Databricks workspace are ready.
2. **Theory + practice (~10 min)** — present
   [`skill-authoring-deck.md`](skill-authoring-deck.md).
3. **Hands-on** — put [`exercises.md`](exercises.md) on screen and work through
   the three exercises: watch Genie Code fail at DQX, drop in a tiny skill, then
   build a real skill family.
4. **Test & deploy** — validate the skill with source-blind sub-agents
   ([`testing-skills-with-subagents.md`](testing-skills-with-subagents.md)), then
   deploy it to Genie Code.

## Rendering the decks

All of these files are [Marp](https://marp.app/) slides. Render any of them to PDF:

```bash
npx @marp-team/marp-cli@latest workshop/skill-authoring-deck.md -o deck.pdf
```
