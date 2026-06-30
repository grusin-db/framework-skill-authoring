# Workshop

Hands-on workshop: rebuild a real library's Agent Skills family from **only the
installed wheel**, using [DQX](https://github.com/databrickslabs/dqx)
(`databricks-labs-dqx`) as the demo framework.

## Files

| File | What it is | When it's used |
| --- | --- | --- |
| [`requirements.md`](requirements.md) | Setup everyone should prepare **before** the workshop (laptop, an agent like Cursor/Claude Code, a Databricks workspace, and the Spark/Databricks Connect setup). | Send ahead of time so people arrive ready. |
| [`skill-authoring-deck.md`](skill-authoring-deck.md) | The **theory + practice** deck — how to author a skill (source-blind consumer, activating description, structure/process/proof), applied to DQX. | Presented first, ~10 minutes. |
| [`workshop-instructions.md`](workshop-instructions.md) | The **hands-on runbook** deck: install the wheel, get the source as a ZIP, delete its shipped `skills/`, rebuild, test source-blind, deploy to Genie Code. | Kept on screen the whole time while people work hands-on. |

## Flow

1. **Before** — attendees complete [`requirements.md`](requirements.md) so their
   laptop, agent, and Databricks workspace are ready.
2. **Theory + practice (~10 min)** — present
   [`skill-authoring-deck.md`](skill-authoring-deck.md).
3. **Hands-on** — put [`workshop-instructions.md`](workshop-instructions.md) on
   screen and let people build the DQX skill family, then test and deploy it.

## Rendering the decks

All three files are [Marp](https://marp.app/) slides. Render any of them to PDF:

```bash
npx @marp-team/marp-cli@latest workshop/skill-authoring-deck.md -o deck.pdf
```
