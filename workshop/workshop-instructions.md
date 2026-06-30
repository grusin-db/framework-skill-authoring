---
marp: true
theme: default
paginate: false
---

<!-- markdownlint-disable MD025 MD041 -->

# Hands-on: rebuild DQX's skills from the wheel

1. **Install the wheel** - `pip install databricks-labs-dqx`
2. **Get the source as a ZIP, not git** - `curl -L https://github.com/databrickslabs/dqx/archive/refs/heads/main.zip -o dqx.zip && unzip dqx.zip`
   (no `git clone` - git history lets the agent recover the answer key and cheat)
3. **Delete its `skills/` folder** - the answer key
4. **Build it yourself** - apply the deck's rules (source-blind consumer,
   activating description, structure/process/proof); author the DQX skill family
   into `.agents/skills/` (the OSS standard) so your local agent finds it
5. **Test** - fresh agent, **only your skills + the wheel**:
   *"add a check that drops rows where price < 0 and split good vs bad"* -
   must produce real DQX, never ask for the source
6. **Deploy** - copy the skill folders to `.assistant/skills/` in Genie Code
7. **Try it** - in Genie Code, run a real DQX task; watch your skill drive it

---
<!-- 20 minute workshop; once finishes -->
---

# ...oh, now you want to do it at scale?

You hand-built **one** family. Now every framework your teams touch -
ingestion, orchestration, ML, governance. Hand-craft each one?

**Check out this repo:** [`framework-skill-authoring`](https://github.com/grusin-db/framework-skill-authoring) -
a **meta-skill that builds skill families**. Point it at any framework you have
the source for; it runs the four phases: **analyze -> map -> generate -> validate**.

```
use framework-skill-authoring, and based on it and the files in the repo,
build me skills for <your framework>
```

Same rules, now automated and repeatable at scale.
