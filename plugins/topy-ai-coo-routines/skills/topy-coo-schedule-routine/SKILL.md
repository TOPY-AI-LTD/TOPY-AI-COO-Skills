---
name: topy-coo-schedule-routine
description: Picks a good schedule for a new Routine — daily or weekly, off the hour, clear of the other Routines, within the daily cap — and checks it before it goes on the scheduler. Use when the user says things like "schedule this routine"; "run this every morning"; "when should this run?"; "set up a weekly routine"; "add a cron for this"; "move this routine's time". Uses TOPY when it is connected.
license: MIT
metadata:
  category: routines
  tier: connected
  version: 0.1.0
  docs: https://github.com/TOPY-AI-LTD/TOPY-AI-COO-Skills/blob/main/plugins/topy-ai-coo-routines/skills/topy-coo-schedule-routine/SKILL.md
---
# Schedule a new Routine

Routines are daily or weekly work that runs with nobody watching. Where they sit in the day decides whether they collide, wait on the work they need, or eat the daily budget. This skill picks the time with the rules that kept the CFO's and CEO's Routines clean.

## When to use
- A new Routine, or "move this to the afternoon".
- Not for: checking every schedule at once (`topy-coo-schedule-check`).

## Before you start
- What the Routine reads and what must run before it (e.g. a report after the data it reports on).
- `coo_routines`: what already runs, and when.

## Workflow
1. Pick the rhythm: daily or weekly. Minute-level watching belongs to monitoring, not to a Routine (more than four runs a
   day is refused).
2. Pick an uncommon minute off the hour (07:47, 16:17 — not :00, and preferably not :15/:30/:45): schedulers bunch on the hour.
3. Put it after what it depends on, and at least 10 minutes from any other Routine's start on the same day.
4. `coo_schedule_plan` with the cron and time zone: the next three runs and any clash. Fix and try again until clean.
5. `coo_schedule_check`: the whole schedule with it in place, against the daily cap.
6. Hand the cron to whoever owns the Routine file; the role's Routines run only once the role is on the scheduler and its
   inputs are set.

## Output
```
Cron: <m h dom mon dow> (<time zone>) · next: <t1>, <t2>, <t3>
Checks: off the hour ✅ · clear of other Routines ✅ · within the daily cap ✅ · reports at the end ✅
```

## Quality bar
- The time is off the hour and at least 10 minutes from other starts.
- The Routine ends with a report to the founder or the CEO.

## Guardrails
- Writes take two calls: call without `confirm` for a preview, show it, and only after the Founder agrees call again with `confirm: true` and the `confirmation`. Never confirm for the Founder.
- Never approve a Routine step for the Founder: approvals are the Founder's own one-click links (or their yes in their own client).
- Never print or ask for keys, tokens or passwords; settings are changed on the server by the Founder or the CTO.

## Related skills
- `topy-coo-schedule-check` — the whole schedule.
- `topy-coo-run-routine` — try it once now.
