---
name: topy-coo-waiting-approvals
description: Shows every Routine paused on the founder — what it waits for, what it found so far, since when, when the link expires — so the founder can decide in one sitting. Use when the user says things like "what's waiting on me?"; "pending approvals"; "which routines are paused?"; "do I need to approve anything?"; "approval queue"; "what did the CFO ask me?". Uses TOPY when it is connected.
license: MIT
metadata:
  category: approvals
  tier: connected
  version: 0.1.0
  docs: https://github.com/TOPY-AI-LTD/TOPY-AI-COO-Skills/blob/main/plugins/topy-ai-coo-approvals/skills/topy-coo-waiting-approvals/SKILL.md
---
# Approvals waiting

Routines stop before anything that spends money, writes to the founder's records or reaches the outside, and wait for the founder's yes. A good COO keeps that queue short and clear; it never answers for the founder.

## When to use
- "Anything waiting on me?", or before the founder's weekly review.
- Not for: sending reminders (`topy-coo-approval-reminders`).

## Before you start
- Each waiting run was emailed to the founder with a one-click approve link (it shows what will happen before anything does).

## Workflow
1. `coo_waiting`: each paused run — Routine, the step it waits on, since when, hours waiting, hours until the link expires,
   what was approved earlier in the run.
2. For each, `coo_runs` with the Routine for what the run found before it stopped.
3. Order: links expiring within a day first, then the oldest.
4. Say what approving does (e.g. "books September's Stripe journal in QuickBooks") and what declining or waiting does (the run
   stays paused; nothing changes). The founder approves from the email link or their own client — not this agent.
5. Expired links → offer `topy-coo-approval-reminders` (a fresh link) or `topy-coo-run-routine` to start over.

## Output
| Routine | Waiting for | Since | Link expires | Approving will… |
|---|---|---|---|---|

## Quality bar
- Every row from `coo_waiting`; nothing marked approved unless a later run shows it.
- The founder knows nothing happens until they approve.

## Guardrails
- Writes take two calls: call without `confirm` for a preview, show it, and only after the Founder agrees call again with `confirm: true` and the `confirmation`. Never confirm for the Founder.
- Never approve a Routine step for the Founder: approvals are the Founder's own one-click links (or their yes in their own client).
- Never print or ask for keys, tokens or passwords; settings are changed on the server by the Founder or the CTO.

## Related skills
- `topy-coo-approval-reminders` — a fresh link for old ones.
- `topy-coo-run-routine` — start again after a link expired.
