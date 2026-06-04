---
name: using-simulations
user-invocable: true
description: Use at the start of any conversation that may lead to a code change on a product surface — UI, features, copy, flows, APIs, backend behavior, errors, configs, permissions, pricing, notifications. Establishes the rule to invoke simulation-testing before planning, clarifying, or exploring.
---

<SUBAGENT-STOP>
If you were dispatched as a subagent for a narrow technical task (search, file reads, single-file edits, status checks), skip this skill.
</SUBAGENT-STOP>

<EXTREMELY-IMPORTANT>
For ANY message that may lead to a product-touching code change, you MUST invoke the `simulation-testing` skill BEFORE writing code, presenting a plan, clarifying requirements, or exploring the codebase.

You do not decide whether a simulation should fire — `simulation-testing` decides. Your job is to make sure it is consulted. Its built-in default is SKIP, so most consults end immediately with no simulation and no user-visible delay. The cost of consulting is essentially zero; the cost of skipping a needed simulation is shipping a bad change.

You cannot rationalize your way out of this. If you are uncertain whether a change touches the product, invoke `simulation-testing` anyway and let it decide.
</EXTREMELY-IMPORTANT>

## The Rule

When you receive a message that may lead to a code change, your **first action** is to invoke `simulation-testing` via the Skill tool. Not after exploring. Not after clarifying. First.

`simulation-testing` is the decision authority:
- If its gates clear, it will fire a persona simulation during planning and surface findings while you continue working.
- If its gates do not clear, it will silently skip and you proceed normally with no mention of simulations to the user.

You never make the simulate/skip judgment yourself. You always delegate.

## Red Flags — These Thoughts Mean STOP and Invoke

| Thought | Reality |
|---|---|
| "This is just a small change" | `simulation-testing` decides what's small. Invoke it. |
| "This is a bugfix, sims don't apply" | `simulation-testing` has explicit SKIP rules for bugfixes. Let it apply them. |
| "I should explore the codebase first" | Skill check comes BEFORE exploration. |
| "Let me clarify the requirements first" | Skill check comes BEFORE clarifying questions. |
| "I'll plan it out and then decide whether to sim" | Wrong order. Invoke `simulation-testing` AT plan time — it runs the sim during planning. |
| "I already know how users will react" | That bias is exactly what persona simulations exist to correct. |
| "This is internal tooling, doesn't count" | `simulation-testing` handles internal-tooling skip logic. Let it. |
| "The user just wants the code, not a sim" | Sims run in the background and don't block the plan. Invoke. |

## Workflow

1. User message arrives.
2. If it could lead to a product-touching code change, invoke `simulation-testing` via the Skill tool.
3. `simulation-testing` decides: fire or skip. Trust its decision.
4. If fire: it kicks off a simulation during planning, presents the plan immediately, and surfaces findings when ready.
5. If skip: proceed silently with the work, no mention of simulations to the user.

## When You Can Skip This Skill

- Pure read-only questions ("explain this code", "what does this file do", "where is X defined").
- Pure infrastructure with zero product surface (CI configs, Dockerfiles, deploy scripts with no behavior change).
- Documentation-only edits.
- Narrow subagent tasks (see SUBAGENT-STOP).

For anything else, invoke `simulation-testing`. Let it decide.
