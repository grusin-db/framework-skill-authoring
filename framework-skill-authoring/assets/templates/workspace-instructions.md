<!--
TEMPLATE: the always-injected workspace-instructions file.

Place this where your consumer platform always loads it into every agent
context (the exact location/filename is platform-specific -- keep it
configurable). Its job: make sure the agent reads the entry router first, and
recover from the one failure that blocks everything (the package not being
importable). Keep it short -- it is injected on every turn.

Replace <fw>, <Framework>, <import-name>, and the install recipe with the
framework's real values. Delete this comment.
-->

# Workspace Instructions

## <Framework>: read the `<fw>` skill first

For any task touching <Framework> (<list the main topics/primitives: e.g.
ingestion, pipelines, data products, scheduling, quality, onboarding,
governance>), **read the `<fw>` skill before anything else**.

It contains the decision tree, quick references, skill index, and the
config/paths rules. Do not answer from memory or write raw, hand-rolled code
before reading it.

## `ModuleNotFoundError` / import failure for <import-name>

Only run this if an import actually fails. **Do not run it preemptively** --
an already-prepared session has the libraries installed.

Fix it, do not work around it:

```
<the framework's real install/restore recipe, e.g.>
<install command for the framework's environment>
<restart the runtime if required>
```

Then retry.

- Do not work around a missing framework lib with an unrelated ad-hoc
  install; restore the intended environment instead.
- If a library is genuinely absent from the intended environment, report the
  gap rather than pinning a one-off install in user code.
