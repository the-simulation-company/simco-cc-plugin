---
name: using-simulations
user-invocable: false
description: |
  STANDING ORDER: When a code change introduces a meaningful new user-facing
  decision or capability, the `simulation-testing` skill decides whether to
  run a persona simulation. The skill's default is SKIP for bugfixes,
  refactors, error-handling tweaks, and small additive controls that reuse
  an existing flow. Do not simulate the same surface twice within 24 hours.
  Do NOT ask permission — consult the skill, decide, and act. When the skill
  says fire, fire. When it says skip, skip silently and keep working.
---

# Using Simulations

Background skill that ensures the simulation-testing skill is invoked for significant, customer-facing changes. The description above is the operative instruction — it stays permanently in context via `user-invocable: false`.
