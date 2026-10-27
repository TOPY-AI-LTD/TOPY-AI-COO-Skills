---
name: topy-coo-scheduler-down
description: 'When the Routine scheduler stops ticking (the routine-watchdog email, or the COO''s health check says down): confirm it, say what is not running, and walk whoever has server access through the checks — without guessing a fix. Use when the user says things like "the routines stopped"; "the scheduler is down"; "I got a routines have stopped email"; "nothing has run today"; "is the scheduler alive?"; "watchdog alert". Uses TOPY when it is connected.'
license: MIT
metadata:
  category: incidents
  tier: connected
  version: 0.1.0
  docs: https://github.com/TOPY-AI-LTD/TOPY-AI-COO-Skills/blob/main/plugins/topy-ai-coo-incidents/skills/topy-coo-scheduler-down/SKILL.md
---
# Scheduler down

Every Routine depends on one scheduler ticking every five minutes on the OneMan server. If it stops, nothing on that server can notice; the routine-watchdog service checks it from outside every 10 minutes and emails the founder. This skill turns that alert into a short, safe recovery.

## When to use
- The "OneMan Routines have stopped" email, or `coo_health` verdict `down`.
- Not for: one Routine failing while the scheduler runs (`topy-coo-failed-runs`).

## Before you start
- Recovery happens on the server; it needs the person with server access (the founder or the CTO). Never ask for server
  passwords or keys in chat.

## Workflow
1. `coo_health`: last tick and how long ago. Ticking again (under 15 minutes) → it recovered: say so and continue with step 4.
2. If the COO can't be reached at all, the server itself may be down: say that, and that the watchdog keeps checking.
3. Checks for the person on the server, in order: the cron entry for the scheduler (`crontab -l`); the scheduler log
   (`~/oneman-scheduler.log`) for the last error; the last-tick file (`~/.oneman/scheduler-last-tick`); a manual
   `oneman_scheduler.py tick`. Report what they find; don't guess a fix.
4. After recovery: `coo_routines` for what should have run while it was down. Runs older than 6 hours are not replayed —
   list the missed ones and offer `topy-coo-run-routine` for those that matter (e.g. the daily cash check).
5. `coo_runs` the next hour to confirm Routines run again.

## Output
```
Scheduler: <down since … | running again since …> · missed: <routines>
Checks: cron <ok/missing> · log <last error> · manual tick <ok/fails>
Next: <rerun list> · watchdog will email when it recovers
```

## Quality bar
- Status comes from `coo_health` and the watchdog, never assumed.
- Every missed Routine is listed; none rerun without the founder's yes.

## Guardrails
- Writes take two calls: call without `confirm` for a preview, show it, and only after the Founder agrees call again with `confirm: true` and the `confirmation`. Never confirm for the Founder.
- Never approve a Routine step for the Founder: approvals are the Founder's own one-click links (or their yes in their own client).
- Never print or ask for keys, tokens or passwords; settings are changed on the server by the Founder or the CTO.

## Related skills
- `topy-coo-run-routine` — catch up what was missed.
- `topy-coo-failed-runs` — failures after recovery.
