---
marp: true
theme: default
paginate: false
---

<!-- markdownlint-disable MD025 MD041 -->

# Hands-on: rebuild DQX's skills from the wheel

1. **Install the wheel** - `pip install databricks-labs-dqx`
2. **Get the source** - `git clone https://github.com/databrickslabs/dqx.git`
3. **Delete its `skills/` folder** - that is the answer key; you rebuild it
   (`git stash` it to compare later)
4. **Build it yourself** - this is the creative part: take what the deck taught
   about the problem space (source-blind consumer, an activating description,
   structure/process/proof) and have your agent author the DQX skill family
   straight from the repo, your way
5. **Test (the real check)** - new agent with **only your skills + the wheel**:
   *"add a check that drops rows where price < 0 and split good vs bad"* -
   it must produce real DQX and never ask for the source
6. **Deploy** - copy the skill folders into `.assistant/skills/` in Genie Code
7. **Try it** - in Genie Code, ask for a real DQX task and watch your skill
   drive it

---
<!-- 20 minute workshop; once finishes -->
---

# ...oh, now you want to do it at scale?

You just hand-built **one** skill family. Now picture every framework your
teams touch - ingestion, orchestration, ML, governance. Hand-crafting each one?

**Check out this repo:** [`framework-skill-authoring`](https://github.com/grusin-db/framework-skill-authoring) - a
**meta-skill that builds skill families**. Point it at any framework you have the
source for; it does the four phases for you: **analyze -> map -> generate ->
validate**.

```
use framework-skill-authoring, and based on it and the files in the repo,
build me skills for <your framework>
```

Same rules you just learned by hand - now automated, repeatable, at scale.
