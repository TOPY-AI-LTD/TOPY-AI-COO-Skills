# Changelog

## 0.1.1 — 2026-10-28

New skill `topy-coo-server-health` (incidents): disk (every mount and inodes), memory, swap and load on the founder's
servers and the OneMan host, how soon a growing disk fills, and who should act — read-only, never deletes or restarts.
The daily check now includes the servers' resources. New tools on `https://topy.ai/mcp/coo`: `coo_resources`,
`coo_server_jobs`, `coo_publishing`; `@topy-ai/coo-cli` 0.1.1 adds `resources`, `servers` and `publishing`.

## 0.1.0 — 2026-10-27

First release: 14 skills in five plugins (core, routines, incidents, approvals, reporting) on
`https://topy.ai/mcp/coo` — the daily operations check, Routine scheduling and the schedule check, failed runs with safe
retries only, the scheduler-down runbook (with the routine-watchdog alert), approvals and reminders, the weekly
operations report, efficiency, automation sweep and tool evaluation. Packages: `@topy-ai/coo-skills`,
`@topy-ai/coo-mcp`, `@topy-ai/coo-cli` 0.1.0.
