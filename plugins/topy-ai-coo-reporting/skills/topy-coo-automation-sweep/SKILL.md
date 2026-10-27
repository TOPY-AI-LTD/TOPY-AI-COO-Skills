---
name: topy-coo-automation-sweep
description: Finds repeated manual work (done three times or more) worth turning into a Routine or a service, drafts the Routine's steps and schedule, and leaves the go-ahead to the founder. Use when the user says things like "what can we automate?"; "find repetitive work"; "turn this into a routine"; "automation ideas"; "stop doing this by hand"; "automation sweep". Uses TOPY when it is connected.
license: MIT
metadata:
  category: reporting
  tier: connected
  version: 0.1.0
  docs: https://github.com/TOPY-AI-LTD/TOPY-AI-COO-Skills/blob/main/plugins/topy-ai-coo-reporting/skills/topy-coo-automation-sweep/SKILL.md
---
# Automation sweep

Work done by hand three times is a Routine waiting to be written. This skill finds it, drafts it and checks its schedule — the founder decides whether it goes live.

## When to use
- A monthly sweep, or "I keep doing this by hand".
- Not for: changing an existing Routine's time (`topy-coo-schedule-routine`).

## Before you start
- Collect the tasks: name, when, who, how long. Ask one question at a time.
- `coo_routines`: is something already doing it?

## Workflow
1. `coo_find_automation` with the tasks: anything done three times or more is a candidate.
2. Drop candidates an existing Routine already covers (`coo_routines`).
3. For the top candidate: the cofounder that owns it, the steps (reads first, then any write behind an approval step), what it
   reports at the end, and the time saved per month.
4. `coo_schedule_plan` for a proposed time (daily or weekly, off the hour, clear of others).
5. Hand the draft to the founder; it goes live only with their yes and once it is added to the owning cofounder's Routines.

## Output
| Candidate | Times | Owner | Saves / month | Next |
|---|---|---|---|---|
Then the top draft: steps, approval points, report, cron.

## Quality bar
- Every candidate comes from real occurrences, not guesses.
- Any write in the draft sits behind an approval step.

## Guardrails
- Writes take two calls: call without `confirm` for a preview, show it, and only after the Founder agrees call again with `confirm: true` and the `confirmation`. Never confirm for the Founder.
- Never approve a Routine step for the Founder: approvals are the Founder's own one-click links (or their yes in their own client).
- Never print or ask for keys, tokens or passwords; settings are changed on the server by the Founder or the CTO.

## Related skills
- `topy-coo-schedule-routine` — place it in the day.
- `topy-coo-efficiency` — measure it once it runs.
