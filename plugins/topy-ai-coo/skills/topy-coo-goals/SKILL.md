---
name: topy-coo-goals
description: The AI COO's four standing goals — Routines run on time, no silent failure, approvals flowing, less founder time per outcome — measured from the scheduler's records, with the status the COO reports to the CEO. Use when the user says things like "what is the COO for?"; "how are operations doing?"; "COO goals"; "COO status for the CEO"; "are we running reliably?"; "operations KPIs". Needs the TOPY MCP server.
license: MIT
metadata:
  category: core
  tier: core
  version: 0.1.0
  docs: https://github.com/TOPY-AI-LTD/TOPY-AI-COO-Skills/blob/main/plugins/topy-ai-coo/skills/topy-coo-goals/SKILL.md
---
# The COO's goals

The COO is goal-first: every Routine, retry and reminder exists to move one of four goals. This skill shows them with today's numbers.

## When to use
- "How are operations doing?", the CEO asking for the COO's status, or setting next quarter's operations objectives.
- Not for: one failure (`topy-coo-failed-runs`).

## Before you start
- Goals and measures:
  - **On time** — the scheduler ticks every 5 minutes (stale after 15): `coo_health`.
  - **No silent failure** — every failed run known within 24 hours: `coo_health`, `coo_efficiency`.
  - **Approvals flowing** — nothing waits on the founder over 24 hours: `coo_waiting`.
  - **Founder time per outcome** — fewer approvals per outcome: `coo_status_report`.

## Workflow
1. `coo_health` and `coo_efficiency` (`days: 7`): success rate (completed or waiting on the founder), runs and failures by
   cofounder, the steps that fail most.
2. `coo_waiting`: how many approvals, the oldest.
3. Health per goal: green (met), amber (one miss this week), red (the scheduler down, a failure unreported for over 24 hours,
   an approval over 48 hours, or success below 90%).
4. For the CEO: `coo_status_report` (KPIs with source and as-of, blockers, asks) and pass it on unchanged.

## Output
| Goal | Measure | Now | Health | Source |
|---|---|---|---|---|

## Quality bar
- Every number has a source tool; a missing number says why instead of a guess.
- Success rate counts runs waiting on the founder as not failed.

## Guardrails
- Writes take two calls: call without `confirm` for a preview, show it, and only after the Founder agrees call again with `confirm: true` and the `confirmation`. Never confirm for the Founder.
- Never approve a Routine step for the Founder: approvals are the Founder's own one-click links (or their yes in their own client).
- Never print or ask for keys, tokens or passwords; settings are changed on the server by the Founder or the CTO.

## Related skills
- `topy-coo-daily-check` — the daily look.
- `topy-coo-weekly-report` — the week for the founder and the CEO.
