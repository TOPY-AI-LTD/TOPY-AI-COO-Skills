---
name: topy-coo-failed-runs
description: Looks at failed Routine runs, reruns once the ones that are safe (failed on a read, nothing sent or written outside, not a permission or token error) and hands the rest to a person with the reason. Use when the user says things like "a routine failed"; "why did the run fail?"; "retry the failed routines"; "fix last night's failures"; "what broke?"; "rerun what failed". Uses TOPY when it is connected.
license: MIT
metadata:
  category: incidents
  tier: connected
  version: 0.1.0
  docs: https://github.com/TOPY-AI-LTD/TOPY-AI-COO-Skills/blob/main/plugins/topy-ai-coo-incidents/skills/topy-coo-failed-runs/SKILL.md
---
# Failed runs and safe retries

A failure is fine; a silent one isn't, and a careless retry is worse: it can post, email or write twice. This skill follows the Ops Autopilot rule — retry only what is known not to have taken effect, once.

## When to use
- After the daily check reports a failure, or "why did the CFO's run fail?".
- Not for: the scheduler itself being down (`topy-coo-scheduler-down`).

## Before you start
- `coo_runs` with `state: failed` gives each failed run's steps and errors.

## Workflow
1. `coo_runs` (`state: failed`, `days`): group by Routine and failed step.
2. Classify each, as `coo_retry_failed` does:
   - **safe to retry** — failed on a read step, and no step that already succeeded sent a message or wrote outside;
   - **needs a person** — a permission, token, 401/403 or "not configured" error (a key or connection to renew); a failed
     write; or an earlier step already sent or wrote (a rerun would repeat it).
3. `coo_retry_failed` (preview shows exactly what reruns); confirm after the founder agrees. Each Routine reruns at most
   once a day.
4. For each "needs a person": the step, the error in plain words, who fixes it (a connection in the TOPY dashboard, a
   server setting for the CTO, a decision for the founder), and `coo_routine_run` once it is fixed.

## Output
| Routine | Failed step | Error | Retried | Needs | Who |
|---|---|---|---|---|---|

## Quality bar
- No retry of a run that may have sent or written anything.
- Every failure ends either retried or with a named owner.

## Guardrails
- Writes take two calls: call without `confirm` for a preview, show it, and only after the Founder agrees call again with `confirm: true` and the `confirmation`. Never confirm for the Founder.
- Never approve a Routine step for the Founder: approvals are the Founder's own one-click links (or their yes in their own client).
- Never print or ask for keys, tokens or passwords; settings are changed on the server by the Founder or the CTO.
- A permission or token error is never retried: it becomes a notice for a person.

## Related skills
- `topy-coo-daily-check` — where failures are first seen.
- `topy-coo-run-routine` — rerun after the fix.
- `topy-coo-scheduler-down` — when nothing runs at all.
