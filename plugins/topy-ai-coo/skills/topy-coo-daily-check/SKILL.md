---
name: topy-coo-daily-check
description: 'The founder''s morning operations check: is the scheduler alive, what failed overnight (safe ones retried once), what waits on the founder — quiet when all is well. Use when the user says things like "daily ops check"; "did anything fail last night?"; "are the routines running?"; "anything waiting on me?"; "COO morning check"; "operations update". Needs the TOPY MCP server.'
license: MIT
metadata:
  category: core
  tier: core
  version: 0.1.1
  docs: https://github.com/TOPY-AI-LTD/TOPY-AI-COO-Skills/blob/main/plugins/topy-ai-coo/skills/topy-coo-daily-check/SKILL.md
---
# Daily operations check

A good COO looks at the operation every morning and bothers the founder only when something needs them. This skill reads the scheduler's own records and turns them into a few lines.

## When to use
- Every morning (the OneMan Routine `coo-daily-ops` does this at 07:47 London time), or "did anything fail?".
- Not for: a week's view (`topy-coo-weekly-report`) or a scheduler that has stopped (`topy-coo-scheduler-down`).

## Before you start
- `coo_health` does most of the work: last tick, runs and failures in 24 hours, approvals waiting, setup issues.

## Workflow
1. `coo_health`. Verdict `down` (no tick for 15 minutes or more) → switch to `topy-coo-scheduler-down`.
2. Failed runs in 24 hours → `coo_runs` with `state: failed`, `days: 1` for the steps and errors. Then `coo_retry_failed`
   (preview first): it reruns only failures that are safe — failed on a read, nothing sent or written outside, not a
   permission or token error — once a day each. Show the preview; confirm only after the founder agrees.
3. `coo_waiting`: approvals waiting, oldest first, with when each link expires.
   Then `coo_resources`: an `urgent` item (a disk or memory at 90%, a disk full within 7 days, a server not read) is
   news; for detail use `topy-coo-server-health`.
4. Decide what is news: the scheduler down, a failure that needs a person, an approval waiting over 24 hours or whose link
   expires within a day, a setup issue. Nothing → one line: "All Routines ran; nothing needs you."
5. Otherwise up to five lines, most important first, and one action tied to a COO goal. To email it, `coo_report_send`
   with `kind: daily` (preview, then confirm).

## Output
```
Operation: <ok | attention | down> · <n> runs in 24h, <n> failed · <n> waiting on you
<news line> — source
Do: <one action> (goal: <on time | no silent failure | approvals flowing | founder time>)
```

## Quality bar
- Every figure is from a COO tool; nothing estimated.
- No more than five lines; never an action without a goal.

## Guardrails
- Writes take two calls: call without `confirm` for a preview, show it, and only after the Founder agrees call again with `confirm: true` and the `confirmation`. Never confirm for the Founder.
- Never approve a Routine step for the Founder: approvals are the Founder's own one-click links (or their yes in their own client).
- Never print or ask for keys, tokens or passwords; settings are changed on the server by the Founder or the CTO.
- Retries are only the safe kind (`coo_retry_failed` decides); never rerun something that may already have sent or written.

## Related skills
- `topy-coo-failed-runs` — a failure in detail.
- `topy-coo-waiting-approvals` — approvals piling up.
- `topy-coo-scheduler-down` — when the scheduler stopped.
- `topy-coo-server-health` — a disk, memory or load note in detail.
