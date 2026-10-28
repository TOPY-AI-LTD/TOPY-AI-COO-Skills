---
name: topy-coo-server-health
description: Reads disk (every mount and its inodes), memory, swap and load on the founder's servers and the OneMan host, says what is near its limit and how soon a growing disk fills, and names who should act — without deleting or restarting anything. Use when the user says things like "how are my servers?"; "is any disk full?"; "check server disk and memory"; "server health"; "why is the server slow?"; "how long until the database disk fills?". Uses TOPY when it is connected.
license: MIT
metadata:
  category: incidents
  tier: connected
  version: 0.1.1
  docs: https://github.com/TOPY-AI-LTD/TOPY-AI-COO-Skills/blob/main/plugins/topy-ai-coo-incidents/skills/topy-coo-server-health/SKILL.md
---
# Server health

A disk that fills up takes the database, the logs and every Routine down with it, usually at night. A good COO knows
each server's headroom and how fast it shrinks, and says so before it becomes an outage — without touching the server.

## When to use
- The daily check or weekly report shows a resource note, or "is any disk full?", "why is the server slow?".
- Not for: a process that is down or restarting (`coo_server_jobs` shows it; `topy-coo-failed-runs` for failed Routines)
  or the scheduler not running at all (`topy-coo-scheduler-down`).

## Before you start
- The servers are the ones the founder added and consented to (read-only) in the TOPY dashboard's Security setup; a
  server without consent is listed as skipped, never read.
- Without TOPY, on a server you may run commands on: `df -P`, `df -Pi`, `free`, `nproc`, `/proc/loadavg`, and
  `ps -eo rss,comm --sort=-rss | head` give the same readings (read-only).

## Workflow
1. `coo_resources` (optionally `hosts`): per server, every mount's `used_percent`, `free_gb`, `inodes_percent`,
   `growth_per_day` and `days_to_full`; `memory_percent` (page cache counts as free), `swap_percent`, `load` and `cores`;
   `top_memory`; and the `attention` list (`urgent: true` first).
2. Read it the way the COO's daily check does:
   - **urgent** — a disk or inodes at 90% or more, a disk full within 7 days at its current growth, memory at 90% or
     more, load over 5 minutes above twice the cores, or a server that could not be read;
   - **noted** — 80% or more, full within 30 days, load above the cores, swap at 50% or more (memory was short at
     some point, even if it is fine now).
3. For each finding, the likely cause and the person to act: a database or log mount growing (the CTO: log rotation,
   retention, a bigger volume), memory held by named processes (the CTO: a leak or too many workers), high load with
   `coo_server_jobs` showing restarts (the CTO: a crash loop). Use the readings; don't guess at file names.
4. If nothing is urgent or noted, say so in one line with the fullest disk across the servers.

## Output
| Server | Fullest disk | Free | Grows | Full in | Memory | Swap | Load / cores |
|---|---|---|---|---|---|---|---|

Then: **Urgent** (each with who acts), **Noted**, and one line on when the next reading is (every morning at 07:47).

## Quality bar
- Every number comes from `coo_resources`, with the server and mount; growth only when the tool gives it (it needs
  readings over at least two days).
- Each urgent finding has an owner and a concrete next check.

## Guardrails
- Read-only: never offer to delete files, clear logs, kill processes or restart services; say what a person should
  check and where.
- Only servers with the founder's consent are read; never ask for SSH keys, passwords or IP addresses in chat.
- Never print or ask for keys, tokens or passwords.

## Related skills
- `topy-coo-daily-check` — where a resource note first shows up.
- `topy-coo-scheduler-down` — when a full disk or memory has stopped the scheduler.
- `topy-coo-failed-runs` — Routines that failed because a server was short of space.
