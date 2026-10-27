---
name: topy-coo-schedule-check
description: Checks every cofounder's scheduled Routines against the scheduling rules — runs a day, on-the-hour starts, clashes, the daily cap, missing inputs and Routines that never report — and proposes fixes. Use when the user says things like "check the schedules"; "are my routines clashing?"; "schedule review"; "too many routines?"; "which routines are missing inputs?"; "is the schedule healthy?". Uses TOPY when it is connected.
license: MIT
metadata:
  category: routines
  tier: connected
  version: 0.1.0
  docs: https://github.com/TOPY-AI-LTD/TOPY-AI-COO-Skills/blob/main/plugins/topy-ai-coo-routines/skills/topy-coo-schedule-check/SKILL.md
---
# Schedule check

A schedule decays as Routines are added. This skill runs the rules every Routine was scheduled with (ported from Maggie Ops' routines check) and turns the findings into concrete moves.

## When to use
- Monthly, after adding Routines, or before putting a cofounder on the scheduler.
- Not for: placing one new Routine (`topy-coo-schedule-routine`).

## Before you start
- `coo_schedule_check` checks; `coo_routines` shows which roles are on the scheduler (missing inputs only block those).

## Workflow
1. `coo_schedule_check` (all cofounders, or one with `role`).
2. Errors first — they stop runs or overload the day: more than four runs a day; missing inputs for a scheduled cofounder;
   the busiest day over the daily cap.
3. Then warnings: starts on the hour; two starts within 10 minutes on the same day; a Routine that never reports; missing
   inputs for a cofounder not yet scheduled.
4. For each: the fix (a new minute, a later hour after its dependency, an input to set, a report step to add) and who owns it.

## Output
| Routine | Issue | Level | Fix | Owner |
|---|---|---|---|---|

## Quality bar
- Errors are listed before warnings; nothing is silently dropped.
- Every fix keeps the Routine after what it depends on.

## Guardrails
- Writes take two calls: call without `confirm` for a preview, show it, and only after the Founder agrees call again with `confirm: true` and the `confirmation`. Never confirm for the Founder.
- Never approve a Routine step for the Founder: approvals are the Founder's own one-click links (or their yes in their own client).
- Never print or ask for keys, tokens or passwords; settings are changed on the server by the Founder or the CTO.

## Related skills
- `topy-coo-schedule-routine` — choose a new time.
- `topy-coo-weekly-report` — the weekly view includes the schedule check.
