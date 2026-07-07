# Requirements

**Bring (required):**

- A laptop.
- An agent: **Cursor / Claude Code** (to generate the DQX skill).
- A **Databricks workspace** you can run against (to test the skill).

**To run generated code:** paste it into a **Databricks notebook** on your
cluster. That is enough for the workshop — zero local Spark setup.

## Optional — run locally (Databricks Connect)

Want to run generated code from your laptop instead of a notebook? Set up
**Databricks Connect** — no local Spark/Java; borrow Spark from a cluster.

1. Install the **Databricks** VS Code / Cursor extension, **sign in**, pick a
   cluster → it writes `.databricks/.databricks.env`.
2. Install **`databricks-connect` matching your cluster's DBR**
   (find the version in **Compute → your cluster**):

```bash
uv venv --python 3.12                          # Spark 3.5 needs Python <= 3.12
uv pip install "databricks-connect==16.4.*"    # change 16.4 -> YOUR cluster's DBR
```

Don't also install `pyspark` — it conflicts. No `uv`? On Mac:
`brew install uv` (or `curl -LsSf https://astral.sh/uv/install.sh | sh`).

## Optional — verify local Spark

**From the repo root**, load the env and verify in one shot:

```bash
set -a; source .databricks/.databricks.env; set +a   # export host/cluster/auth
uv run python -c "from databricks.connect import DatabricksSession; \
print(DatabricksSession.builder.getOrCreate().range(3).collect())"
```

Expect `[Row(id=0), Row(id=1), Row(id=2)]` → Spark works.

In your own scripts:

```python
from databricks.connect import DatabricksSession
spark = DatabricksSession.builder.getOrCreate()       # runs on the cluster
```

## Optional — prompt for usage of local spark

When prompting add this prompt to acomplish env injection

```text
Spark setup (skip the source line if your shell already loaded the env):
- Load env from repo root: set -a; source .databricks/.databricks.env; set +a
- Create Spark: from databricks.connect import DatabricksSession;
  spark = DatabricksSession.builder.getOrCreate()
```
