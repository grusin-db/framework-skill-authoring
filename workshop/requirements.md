---
marp: true
theme: default
paginate: true
---

<!-- markdownlint-disable MD025 MD041 -->

# Requirements

**Bring:**

- A laptop.
- An agent: **Cursor / Claude Code** (to generate the DQX skill).
- A **Databricks workspace** you can run against (to test the skill).

---

# Spark setup (Databricks Connect)

No local Spark/Java — borrow Spark from a cluster.

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

---

# Run Spark code

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
