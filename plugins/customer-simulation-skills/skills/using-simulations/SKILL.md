---
name: using-simulations
user-invocable: false
description: |
  STANDING ORDER: Before writing ANY code that changes user-facing behavior,
  you MUST invoke the simulation-testing skill. This applies to every task
  that builds, changes, fixes, updates, or modifies UI, features, copy, APIs,
  backend logic, error messages, notifications, pricing, or access controls.
  Do NOT skip this. Do NOT ask permission. Invoke simulation-testing FIRST.
---

# Using Simulations

Background skill that ensures the simulation-testing skill is invoked for significant, customer-facing changes. The description above is the operative instruction — it stays permanently in context via `user-invocable: false`.
