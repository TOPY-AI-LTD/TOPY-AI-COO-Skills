---
name: topy-coo-run-routine
description: Runs one Routine now through the scheduler (the same policy gate and approvals as a scheduled run) and reads back where it stopped — for a first try, or to catch up after a fix. Use when the user says things like "run this routine now"; "try the routine once"; "rerun the daily cash check"; "kick off the weekly review"; "test the new routine"; "run it again now". Uses TOPY when it is connected.
license: MIT
metadata:
  category: routines
  tier: connected
  version: 0.1.0
  docs: https://github.com/TOPY-AI-LTD/TOPY-AI-COO-Skills/blob/main/plugins/topy-ai-coo-routines/skills/topy-coo-run-routine/SKILL.md
---
# Run a Routine now

Sometimes a Routine should run now rather than at its next time: a first try, a catch-up after a fix. This skill starts it the way the scheduler does, so the same checks and approvals apply.

## When to use
- "Run the weekly review now", after fixing a failed step, or to test a new Routine.
- Not for: retrying all of last night's failures (`topy-coo-failed-runs`).

## Before you start
- `coo_routines`: the exact Routine id and whether its inputs are set.
- Say what the Routine does when it runs (what it reads, what it may write or send) before starting it.

## Workflow
1. `coo_routines`: confirm the Routine is active and its inputs are set; missing inputs → stop and say which.
2. `coo_routine_run` with the Routine id: the preview says what will run; confirm only after the founder agrees.
3. Read the result: completed; awaiting approval (which step — the founder gets the one-click email); failed (which step,
   the error).
4. `coo_runs` with the Routine to show this run beside the previous ones.

## Output
```
<routine>: <completed | waiting on you: <step> | failed at <step>: <error>>
Steps: <id ✅ …> · approval email: <sent | not needed>
```

## Quality bar
- The run's state comes from the tool result, not from expectations.
- A waiting step is named, with the reminder that only the founder approves it.

## Guardrails
- Writes take two calls: call without `confirm` for a preview, show it, and only after the Founder agrees call again with `confirm: true` and the `confirmation`. Never confirm for the Founder.
- Never approve a Routine step for the Founder: approvals are the Founder's own one-click links (or their yes in their own client).
- Never print or ask for keys, tokens or passwords; settings are changed on the server by the Founder or the CTO.
- A Routine whose steps write or send does so again when run again; say so in the preview.

## Related skills
- `topy-coo-failed-runs` — when the run fails.
- `topy-coo-waiting-approvals` — when it waits on the founder.
