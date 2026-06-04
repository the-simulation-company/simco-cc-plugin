---
name: using-simulations
user-invocable: true
description: |
  STANDING ORDER: For any code change that touches the product — UI,
  features, copy, flows, APIs, backend behavior, errors, configs,
  permissions, pricing, notifications — consult the `simulation-testing`
  skill. The skill decides whether a persona simulation is warranted; its
  default is SKIP, so most consults end without firing anything. Do NOT
  ask permission. Skip silently when the skill says skip, fire when it
  says fire.
---

# Using Simulations

Background skill that ensures the simulation-testing skill is invoked for significant, customer-facing changes. The description above is the operative instruction — it is loaded into context on every turn so the standing order is always in view.
