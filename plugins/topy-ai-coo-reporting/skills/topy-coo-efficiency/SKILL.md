---
name: topy-coo-efficiency
description: Measures how well the Routines run — success rate, retries, cost and failures by cofounder and step — and finds the few changes that would raise reliability or cut founder time. Use when the user says things like "how efficient are our routines?"; "success rate"; "which routine fails most?"; "operations efficiency"; "where do we lose time?"; "reliability review". Uses TOPY when it is connected.
license: MIT
metadata:
  category: reporting
  tier: connected
  version: 0.1.0
  docs: https://github.com/TOPY-AI-LTD/TOPY-AI-COO-Skills/blob/main/plugins/topy-ai-coo-reporting/skills/topy-coo-efficiency/SKILL.md
---
# Efficiency review

Reliability is a number: runs that completed or reached the founder's decision, out of all runs. This skill finds what moves it.

## When to use
- A monthly review, a success rate under 90%, or "which routine fails most?".
- Not for: one failure (`topy-coo-failed-runs`).

## Before you start
- Compare the same period length (e.g. 7 days against the 7 before) to see a trend.

## Workflow
1. `coo_efficiency` for the period, and again for the period before.
2. The top failing steps: `coo_runs` for each to read the errors; group by cause (a connection or key, a missing input, a
   timeout in a service, a bug).
3. `coo_waiting`: approvals that wait long are founder time; too many approvals per outcome suggests a Routine to merge or a
   step to drop.
4. Propose at most three changes, each with the expected effect (e.g. "set the missing input: removes 4 failures a week").

## Output
| Measure | This period | Before | Note |
|---|---|---|---|
Then the top causes with their fixes.

## Quality bar
- The comparison uses the same period length.
- Each proposed change names the failures or minutes it removes.

## Guardrails
- Writes take two calls: call without `confirm` for a preview, show it, and only after the Founder agrees call again with `confirm: true` and the `confirmation`. Never confirm for the Founder.
- Never approve a Routine step for the Founder: approvals are the Founder's own one-click links (or their yes in their own client).
- Never print or ask for keys, tokens or passwords; settings are changed on the server by the Founder or the CTO.

## Related skills
- `topy-coo-weekly-report` — where these numbers go weekly.
- `topy-coo-automation-sweep` — turning repeated manual work into Routines.
