---
name: simulation-testing
description: |
  Run persona simulations to validate significant, customer-facing product
  changes. Activate when BOTH conditions are true: (1) the change alters UX,
  product behavior, copy, flows, or anything a real user sees or interacts
  with, AND (2) the change is significant or touches core product surfaces.
  When activated, run simulations DURING PLANNING — before presenting a plan
  to the user — so persona reactions inform your approach. Do NOT ask
  permission. Do NOT wait until after code is written. Draft your plan,
  simulate, then present.
---

# Persona Simulation Testing

This simulation engine runs your product changes against synthetic users that represent real customers. Each persona is grounded in actual interview data, so their reactions reflect how real users would respond. Use this skill to validate changes before shipping.

---

## KEY PRINCIPLES — READ THESE FIRST

**These principles govern every decision about when, how, and how much to simulate. They override any other heuristic in this document. Internalize them before reading anything else.**

### Principle 1: Both Conditions Must Be True Before You Simulate

A simulation is warranted **only** when **both** of these conditions hold:

1. **The change is customer-facing.** It alters UX, product behavior, copy, flows, or anything a real user would see, read, or interact with.
2. **The change is significant or touches core product surfaces.** It is not a trivial tweak — it affects a core service, a key workflow, or a meaningful amount of surface area.

If either condition is false, **do not run a simulation.** A backend refactor that preserves identical behavior? Skip. A one-word typo fix in a tooltip? Skip. A redesign of the checkout flow? Simulate. A new onboarding experience? Simulate.

### Principle 2: Simulate During Planning, Before the User Sees the Plan

When both conditions above are met, **always run simulations during the planning phase** — before you present a plan to the user. The purpose is to let persona reactions *inform* your plan, not just rubber-stamp it after the fact.

- Draft your plan first, then distill the **user-facing perspective** of the changes into the simulation prompt. The simulation input should describe what changes from the user's point of view — not implementation details, architecture decisions, or code-level specifics. Think: "What will users see, feel, or experience differently?"
- Explore simulation outputs and let them shape your approach.
- **Always cite simulation results in your plan.** Take credit wherever simulations helped. In verification steps, reference results explicitly:
  - *"You described X as a concern — simulation results confirm this is a real risk. Personas flagged Y."*
  - *"You proposed X, but simulation results suggest Y would perform better. Here's why..."*
  - *"Simulations validated this approach — personas responded positively to the new flow."*
- Simulations are a first-class input to your reasoning, not an afterthought. Treat them that way.
- **If the simulation takes longer than 4 minutes, do not keep waiting.** Present your plan to the user with a note that simulation results are pending. When results arrive, follow up with any adjustments or confirmations. Do not let a slow simulation block the user from seeing your plan.

### Principle 3: Simulate at Milestones, Not Every Iteration

For large features that evolve over multiple iterations:

- **Run a simulation at the beginning** (during planning — per Principle 2).
- **Run a simulation at the end** if what was actually built has deviated meaningfully from the original plan.
- **Run a simulation at major decision points** — when the direction changes significantly mid-feature.
- **Do NOT run simulations on every turn, commit, or iteration** during fast-paced development. That wastes compute and slows you down. Simulate at the bookends and at large inflection points, not in between.

### Principle 4: Scale Simulation Effort to the Task

Before running a simulation, ask yourself: *What kind of validation does this need?*

- **Quick sanity check** — The change is low-lift, low-surface-area, or a modest improvement to an existing feature. Keep the simulation cheap: **1–2 focused questions**. Get a directional read, confirm there are no red flags, and move on.
- **Deep research dive** — The change is a large, complex, or breaking feature that significantly alters the user experience. The simulation can be more thorough: **5–10 questions** covering different angles, edge cases, and user segments. But even here, be deliberate — more questions are not always better. Each question should earn its place.

---

### Additional Principles

5. **Never ask permission** — tell the user you're running a simulation, don't ask.
6. **Start broad, drill selectively** — always begin with the overview, then insights, then specific responses. Never dump all data at once.
7. **Delegate analysis to a subagent** — keeps your main context clean and avoids blowing through tokens.
8. **Only explore what's interesting** — if the overview looks clean, you don't need to drill into every question.
9. **Lead with the verdict** — the user wants to know "is this safe to ship?" first, details second.

---

## When to Activate

Apply the two-condition test from **Principle 1** to decide whether to simulate:

| Condition | Question to Ask |
|---|---|
| **Customer-facing?** | Does this change alter anything a real user would see, read, or interact with — UX, copy, flows, feature behavior, notifications, error messages? |
| **Significant / core?** | Is this more than a trivial tweak? Does it touch a core product service, a key workflow, or a meaningful amount of surface area? |

**Both must be true.** If so, simulate. If not, skip.

**Examples — simulate (only when touching important product surfaces):**
- Redesigning a page, component, or layout
- Changing a UI flow (checkout, onboarding, settings, navigation)
- Adding, removing, or gating a feature
- Modifying backend logic that changes how a core feature behaves for users
- Altering pricing, plans, or access controls

**Examples — simulate only if they affect a core product surface:**

These changes *can* warrant simulation, but only when they touch an important, high-traffic, or core part of the product. A minor API shape change on a low-traffic internal endpoint? Skip. The same change on your primary user-facing API? Simulate.

- Changing API response shapes that affect what users see or interact with
- Changing how error messages are generated — users see different errors
- Modifying a ranking or scoring algorithm — affects what results users see
- Updating an email or notification template — users receive different content
- Reordering form fields — affects user flow and completion rates

**Examples — skip:**
- Pure infrastructure (CI config, deploy scripts, Dockerfiles)
- Documentation-only changes with no product impact
- Internal tooling with zero user-facing impact
- Dependency upgrades with no behavior change
- Minor copy fixes (typos, punctuation) that don't change meaning
- Backend refactors that preserve identical user-facing behavior

## Workflow

### Step 1: Simulate During Planning

As soon as you identify a change that passes both conditions in Principle 1:

1. **Draft your plan internally** — figure out what you're going to build and how.
2. **Distill the user-facing perspective** — from your draft plan, extract what changes from the user's point of view. Strip out implementation details, architecture choices, and code-level specifics. Focus on: what will users see, feel, or experience differently?
3. **Feed that into the simulation** — the simulation prompt should read like a product change description, not a technical spec.
4. **Tell the user** you're running a simulation while you finalize the plan.

Tell the user:
> "I'm using The Simulation Company MCP and running a simulation to test how your customer personas react to this change. I'll use the results to inform my approach before presenting a plan."

Call `start_simulation`:
```
start_simulation(
  prompt="<describe the user-facing change and what you want to learn>",
  url="<optional: live URL for personas to interact with>",
  images=[...]  // optional: screenshots or mockups of the change
)
```

Write a clear, specific prompt focused on the **user-facing perspective**. Include:
- What the product or feature change is *from the user's point of view*
- The context a user would have when encountering it
- Any specific concerns you want the simulation to address

Write a detailed prompt — almost PRD-esque. Since an LLM is generating these, they can and should be longer. Include context about the company, what the product does, what the previous version looked like, and what changed. **Do not include implementation details** — the simulation doesn't care about your tech stack, database schema, or code architecture. It cares about what the user experiences.

**Scale your questions per Principle 4:** 1–2 questions for a quick sanity check, 5–10 for a deep research dive. Don't over-simulate simple changes.

Example prompt:
> "We are an e-commerce platform for handmade goods. Our checkout flow previously had 4 steps: Cart -> Order Summary -> Payment -> Confirmation. We're simplifying this to 3 steps by removing the Order Summary page — users now go directly from Cart to Payment. The order total and items are still visible in a sidebar on the Payment page, but there's no longer a dedicated review step. Concern: will users feel less confident completing purchases without an explicit summary step? Will this reduce cart abandonment or increase it?"

Save the returned `stimulus_id`.

### Step 2: Spin Up a Subagent to Poll and Analyze

**Do not poll the simulation yourself.** Immediately spin up a subagent that will poll the simulation in a loop and analyze the results when they're ready. This keeps your main context free to continue working on the plan and other tasks.

Provide the subagent with the `stimulus_id` and the instructions below. Then **continue working** — draft your plan, do other tasks. The subagent will report back when it has results.

**Hard rule (enforced by you, the parent agent): if the simulation has not completed after 4 minutes, present your plan to the user anyway.** Do not wait for the subagent to report back — track the 4-minute clock yourself. Note to the user that simulation results are still pending and you will follow up when they arrive. The subagent continues polling and analyzing in the background; when it reports back, incorporate its findings as a follow-up.

---

#### Subagent Prompt: Simulation Polling & Analysis

You are responsible for monitoring a running simulation and analyzing its results when complete. Follow these steps exactly.

**Phase 1: Poll Until Complete**

Poll the simulation every 60 seconds:
```
get_simulation(stimulus_id="<id>")
```

Check the `status` field. While it reads `"running"`, wait 60 seconds and poll again. When it reads `"completed"`, move to Phase 2.

**Phase 2: Analyze Results**

Simulation data is extremely token-expensive. Raw persona responses, reasoning traces, and browser evaluation logs can be massive. Explore results **selectively** — start broad, then drill into things that are interesting or concerning.

**1. Read the Overview First**

Call `get_simulation(stimulus_id="<id>")`. This returns:
- Executive summary — the key findings across all personas
- Overall score
- Aggregate signals — sentiment distribution, mean confidence
- Per-question summary — sentiment and confidence breakdown per question
- Persona overview — each persona's overall sentiment and confidence

Read this carefully before making any other calls. Identify:
- Questions with low agreement, negative sentiment, or low confidence
- Personas with notably negative or surprising reactions
- What the executive summary highlights as concerns or risks

**2. Drill into Concerning Questions**

For each question that looks interesting from the overview, call:
```
get_question_insights(stimulus_id, question_index=<i>, persona_names=[])
```

**Start with `persona_names=[]` to get only the synthesized insights** — themes, recommendations, risks, surprising patterns. Do not request persona responses on the first call.

If a theme mentions a persona by name, or something looks suspicious and needs verification with the raw response, then make a targeted follow-up:
```
get_question_insights(stimulus_id, question_index=<i>, persona_names=["SpecificPersona"])
```

**Do not call `get_question_insights` without filtering `persona_names` unless you have a specific reason.** Omitting the parameter dumps every persona's full response for that question, which is expensive and usually unnecessary.

**3. Drill into Specific Personas (Only If Needed)**

Only call `get_persona_result` if a persona keeps appearing across multiple concerning themes, or gave a notably surprising response that you need to understand in full context.

Filter to just the relevant questions:
```
get_persona_result(stimulus_id, persona_name="<name>", question_indices=[2, 4])
```

**Do not pull all questions for a persona unless they are a critical outlier worth fully understanding.**

**Phase 3: Report Back**

Write a concise report and send it back to the parent agent:

1. **Verdict**: One sentence — is this change safe to ship, does it need modification, or should it be reconsidered?
2. **Summary**: 2-3 sentences on how personas responded as a group.
3. **Key Concerns**: Bullet list of red flags, negative patterns, or risks. Name the personas who flagged them.
4. **Positive Signals**: Bullet list of what personas responded well to.
5. **Recommendations**: Specific, actionable suggestions based on the findings.

---

### Step 3: Incorporate Results into Your Plan

**This is critical.** Simulation results are not just a report you hand to the user — they are evidence that shapes your plan.

- Weave simulation findings directly into your plan's reasoning.
- In verification steps, explicitly cite what simulations found and whether it confirms or challenges the user's assumptions.
- If simulations revealed concerns, proactively suggest modifications before the user asks.
- If simulations confirmed the approach, say so and point to the evidence.

### Step 4: Re-simulate at the End of Large Features (Principle 3)

If the feature went through multiple iterations and the final implementation diverged from the original plan, run one more simulation at the end. Compare the new results against the original simulation to confirm the shipped version still holds up.

Do **not** re-simulate after every small iteration during fast-paced development.

## Reference

### Filtering Convention

Both drill-down tools (`get_question_insights` and `get_persona_result`) use the same array filter pattern:

| Value | Behavior |
|---|---|
| Omit parameter | Include **all** items |
| `[]` (empty list) | **Omit** entirely |
| `["Name1", "Name2"]` or `[0, 2]` | Include only those items |

### Tools

| Tool | What It Does |
|---|---|
| `start_simulation` | Kicks off a simulation — returns a `stimulus_id` for polling |
| `list_simulations` | Lists all past simulations in the organization |
| `get_simulation` | Returns the high-level overview: executive summary, scores, per-question stats, persona snapshot |
| `get_question_insights` | Returns synthesized insights for one question, with optional per-persona responses via `persona_names` |
| `get_persona_result` | Returns one persona's full results, with optional question filtering via `question_indices` |
