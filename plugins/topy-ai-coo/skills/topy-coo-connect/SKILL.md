---
name: topy-coo-connect
description: Connects the founder's AI COO — the TOPY COO MCP server on the same TOPY account as the AI CEO and CFO — and checks that OneMan's Routine scheduler, approval links and Founder email are set up. Use when the user says things like "connect my COO"; "set up the AI COO"; "is my operations agent connected?"; "use topy as my COO"; "are my routines running?"; "check the COO setup". Needs the TOPY MCP server.
license: MIT
metadata:
  category: core
  tier: core
  version: 0.1.0
  docs: https://github.com/TOPY-AI-LTD/TOPY-AI-COO-Skills/blob/main/plugins/topy-ai-coo/skills/topy-coo-connect/SKILL.md
---
# Connect the AI COO

The AI COO runs operations for every AI cofounder: their Routines on the OneMan scheduler, the runs, the failures and the approvals waiting on the Founder. This skill connects it once and shows that it can see the operation.

## When to use
- First contact with the AI COO, or "are my routines running at all?".
- After a sign-in change, or when another account should (or should not) see the operation.
- Not for: the daily check itself (`topy-coo-daily-check`).

## Before you start
- The COO MCP server is `https://topy.ai/mcp/coo` (remote) or `npx @topy-ai/coo-mcp` (local); the CLI is `npx @topy-ai/coo-cli`.
  All sign in with the founder's TOPY account; `account_get` confirms who is signed in.
- Only the accounts that own the OneMan operation see it. Another account gets "not the owner": that is by design.

## Workflow
1. `account_get`: who is signed in.
2. `coo_health`: the scheduler's last tick, runs and failures in 24 hours, approvals waiting, and setup issues
   (approval links, Founder email). An error saying the account doesn't own this operation → stop and say which
   account to sign in with.
3. `coo_routines`: every cofounder's active Routines, which roles the scheduler runs, the next run of each, and
   inputs that are missing.
4. List what is missing and who fixes it: inputs and `ONEMAN_SCHEDULE_ROLES` on the server (the Founder or the
   CTO), approval links and email (server settings). Use `dashboard_link` for anything done in the TOPY dashboard.

## Output
```
Connected as <email> · scheduler: <ok | attention | down> (last tick <n> min ago)
Scheduled cofounders: <cfo, coo, ceo> · Routines: <n> active, next: <routine> at <time>
Missing: <input / setting> — fixed by <who>
Next: topy-coo-daily-check (every morning) · topy-coo-goals
```

## Quality bar
- Every status comes from `coo_health` and `coo_routines`, not from memory.
- The founder knows which cofounders run on the scheduler and which don't yet.

## Guardrails
- Writes take two calls: call without `confirm` for a preview, show it, and only after the Founder agrees call again with `confirm: true` and the `confirmation`. Never confirm for the Founder.
- Never approve a Routine step for the Founder: approvals are the Founder's own one-click links (or their yes in their own client).
- Never print or ask for keys, tokens or passwords; settings are changed on the server by the Founder or the CTO.

## Related skills
- `topy-coo-daily-check` — the morning check once connected.
- `topy-coo-goals` — what the COO is measured on.
