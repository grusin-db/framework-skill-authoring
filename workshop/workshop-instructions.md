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
4. **Build** - in your agent, inside the repo:
   *"use framework-skill-authoring and build an Agent Skills family for DQX"*
5. **Test (the real check)** - new agent with **only your skills + the wheel**:
   *"add a check that drops rows where price < 0 and split good vs bad"* -
   it must produce real DQX and never ask for the source
6. **Deploy** - copy the skill folders into `.assistant/skills/` in Genie Code
7. **Try it** - in Genie Code, ask for a real DQX task and watch your skill
   drive it
