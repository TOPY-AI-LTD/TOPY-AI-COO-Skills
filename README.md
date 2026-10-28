<div align="center">
  <p>
    <img src="https://www.topy.ai/long_logo.svg" alt="TOPY AI COO" width="420" />
  </p>
  <h1>TOPY AI COO Skills</h1>
  <p><strong>14 skills that make your coding agent a small company's AI COO: every AI cofounder's Routines on time, failures never silent, approvals that don't pile up, and a weekly operations report to the founder and the CEO — on the TOPY.AI COO MCP server.</strong></p>
  <p>
    <a href="https://www.npmjs.com/package/@topy-ai/coo-skills"><img alt="npm" src="https://img.shields.io/npm/v/@topy-ai/coo-skills?style=for-the-badge&color=ff1212" /></a>
    <a href="https://github.com/TOPY-AI-LTD/TOPY-AI-COO-Skills/blob/main/LICENSE"><img alt="MIT License" src="https://img.shields.io/badge/License-MIT-111827?style=for-the-badge" /></a>
    <a href="https://topy.ai/developers"><img alt="MCP" src="https://img.shields.io/badge/MCP-topy.ai%2Fmcp%2Fcoo-0f172a?style=for-the-badge" /></a>
  </p>
</div>

---

The AI COO is goal-first. It keeps four standing goals — **on time** (the scheduler ticks every 5 minutes), **no silent
failure** (every failed run known within 24 hours), **approvals flowing** (nothing waits on the founder over a day) and
**less founder time per outcome** — and every skill below exists to move one of them.

- **Core** (`topy-ai-coo`): connect, the daily operations check, the COO's goals.
- **Routines**: schedule a new Routine (daily or weekly, off the hour, clear of the others), check every schedule, run a Routine now.
- **Incidents**: failed runs and **safe retries only** (a failure on a read with nothing sent or written outside, once a day;
  permission and token errors go to a person), and what to do when the scheduler itself stops.
- **Approvals**: what waits on the founder, and fresh one-click links for the ones that waited too long.
- **Reporting**: the weekly operations report to the founder and the CEO, efficiency, an automation sweep, tool and vendor evaluation.

The operating rules come from running the AI CEO's and AI CFO's Routines in production, and from Maggie Ops' autopilot:
every run is recorded, a job runs once per period, retries never repeat something that may already have taken effect,
and every Routine ends in a report. Skills never approve a step for the founder (approvals are the founder's own
one-click links), never confirm a change for you, and never handle keys or passwords.

## Install

```sh
npx @topy-ai/coo-skills install                  # this project, for the agents set up here
npx @topy-ai/coo-skills install -g --mcp         # your user, plus the MCP server
npx @topy-ai/coo-skills install --category incidents,approvals
npx @topy-ai/coo-skills list
```

Claude Code plugins: `/plugin marketplace add TOPY-AI-LTD/TOPY-AI-COO-Skills`, then `/plugin install topy-ai-coo-all@topy-ai-coo`.
Codex: `codex plugin marketplace add TOPY-AI-LTD/TOPY-AI-COO-Skills`. Pi: `pi install npm:@topy-ai/coo-skills`.
See the [installer README](packages/skills/README.md).

## Connect TOPY.AI

`https://topy.ai/mcp/coo` — OAuth with your TOPY.AI account (the same one as the AI CEO and AI CFO). Only the accounts
that own the OneMan operation see its Routines and runs.

| Client | How |
|---|---|
| Claude, ChatGPT (web, desktop) | custom connector `https://topy.ai/mcp/coo` |
| Claude Code | `claude mcp add --transport http topy-coo https://topy.ai/mcp/coo` |
| Codex | `codex mcp add topy-coo --url https://topy.ai/mcp/coo` |
| Clients that only start a command | `npx -y @topy-ai/coo-mcp` after `npx @topy-ai/coo-mcp login` |
| Terminal and scripts | `npm i -g @topy-ai/coo-cli && coo-cli login`; `coo-cli health`, `coo-cli waiting`, `coo-cli runs` |

## How the skills fit together

```
topy-coo-connect ─▶ topy-coo-goals
        │
   daily: topy-coo-daily-check ─▶ topy-coo-failed-runs (safe retries) · topy-coo-waiting-approvals
          topy-coo-approval-reminders (17:13)   ·   watchdog alert ─▶ topy-coo-scheduler-down
        │
   weekly: topy-coo-weekly-report ─▶ topy-coo-efficiency · topy-coo-schedule-check
        │
   change: topy-coo-automation-sweep ─▶ topy-coo-schedule-routine ─▶ topy-coo-run-routine
           topy-coo-vendor-evaluation
```

Try: *"Did anything fail last night?"*, *"What's waiting on me?"*, *"Schedule this every Monday morning"*,
*"Send the weekly operations report"*, *"The routines stopped"*

## Packages

| Package | What it is |
|---|---|
| [`@topy-ai/coo-skills`](https://www.npmjs.com/package/@topy-ai/coo-skills) | This installer (Claude Code, Codex, Pi, Cursor, Gemini CLI, …) |
| [`@topy-ai/coo-mcp`](https://www.npmjs.com/package/@topy-ai/coo-mcp) | The COO MCP server as a local stdio server (forwards to `https://topy.ai/mcp/coo`) |
| [`@topy-ai/coo-cli`](https://www.npmjs.com/package/@topy-ai/coo-cli) | The AI COO on the command line |

## Repository structure

```
plugins/
  topy-ai-coo/               core skills + .mcp.json (Claude Code) and mcp.json (Codex)
  topy-ai-coo-routines/      topy-ai-coo-incidents/   topy-ai-coo-approvals/
  topy-ai-coo-reporting/
  topy-ai-coo-all/           bundle that depends on all five
.claude-plugin/marketplace.json     Claude Code marketplace
.agents/plugins/marketplace.json    Codex marketplace
skills.json                         machine-readable catalogue
packages/skills/                    the @topy-ai/coo-skills installer
```

The skills are generated from a source catalogue kept next to TOPY.AI's MCP server; the build checks every tool a
skill names against the COO server's tool list. Please open an issue rather than editing a `SKILL.md`.

## License

MIT © TOPY AI LTD. Privacy: [topy.ai/privacy](https://topy.ai/privacy).

## Skills

<!-- skills:start -->

**15 skills in 5 plugins**, plus the `topy-ai-coo-all` bundle. Generated from the source catalogue: please open an issue rather than editing a `SKILL.md`.

### AI COO core (MCP) · `topy-ai-coo`

| Skill | Tier | What it does |
|---|---|---|
| [`topy-coo-connect`](plugins/topy-ai-coo/skills/topy-coo-connect/SKILL.md) | core | Connects the founder's AI COO — the TOPY COO MCP server on the same TOPY account as the AI CEO and CFO — and checks that OneMan's Routine scheduler, approval links and Founder email are set up. |
| [`topy-coo-daily-check`](plugins/topy-ai-coo/skills/topy-coo-daily-check/SKILL.md) | core | The founder's morning operations check: is the scheduler alive, what failed overnight (safe ones retried once), what waits on the founder — quiet when all is well. |
| [`topy-coo-goals`](plugins/topy-ai-coo/skills/topy-coo-goals/SKILL.md) | core | The AI COO's four standing goals — Routines run on time, no silent failure, approvals flowing, less founder time per outcome — measured from the scheduler's records, with the status the COO reports to the CEO. |

### Routines & schedules · `topy-ai-coo-routines`

| Skill | Tier | What it does |
|---|---|---|
| [`topy-coo-run-routine`](plugins/topy-ai-coo-routines/skills/topy-coo-run-routine/SKILL.md) | connected | Runs one Routine now through the scheduler (the same policy gate and approvals as a scheduled run) and reads back where it stopped — for a first try, or to catch up after a fix. |
| [`topy-coo-schedule-check`](plugins/topy-ai-coo-routines/skills/topy-coo-schedule-check/SKILL.md) | connected | Checks every cofounder's scheduled Routines against the scheduling rules — runs a day, on-the-hour starts, clashes, the daily cap, missing inputs and Routines that never report — and proposes fixes. |
| [`topy-coo-schedule-routine`](plugins/topy-ai-coo-routines/skills/topy-coo-schedule-routine/SKILL.md) | connected | Picks a good schedule for a new Routine — daily or weekly, off the hour, clear of the other Routines, within the daily cap — and checks it before it goes on the scheduler. |

### Incidents & retries · `topy-ai-coo-incidents`

| Skill | Tier | What it does |
|---|---|---|
| [`topy-coo-failed-runs`](plugins/topy-ai-coo-incidents/skills/topy-coo-failed-runs/SKILL.md) | connected | Looks at failed Routine runs, reruns once the ones that are safe (failed on a read, nothing sent or written outside, not a permission or token error) and hands the rest to a person with the reason. |
| [`topy-coo-scheduler-down`](plugins/topy-ai-coo-incidents/skills/topy-coo-scheduler-down/SKILL.md) | connected | When the Routine scheduler stops ticking (the routine-watchdog email, or the COO's health check says down): confirm it, say what is not running, and walk whoever has server access through the checks — without guessing a fix. |
| [`topy-coo-server-health`](plugins/topy-ai-coo-incidents/skills/topy-coo-server-health/SKILL.md) | connected | Reads disk (every mount and its inodes), memory, swap and load on the founder's servers and the OneMan host, says what is near its limit and how soon a growing disk fills, and names who should act — without deleting or restarting anything. |

### Approvals · `topy-ai-coo-approvals`

| Skill | Tier | What it does |
|---|---|---|
| [`topy-coo-approval-reminders`](plugins/topy-ai-coo-approvals/skills/topy-coo-approval-reminders/SKILL.md) | connected | Sends the founder a fresh one-click approval email for runs waiting longer than a threshold (default 24 hours) — once a day per run, never more. |
| [`topy-coo-waiting-approvals`](plugins/topy-ai-coo-approvals/skills/topy-coo-waiting-approvals/SKILL.md) | connected | Shows every Routine paused on the founder — what it waits for, what it found so far, since when, when the link expires — so the founder can decide in one sitting. |

### Reporting & improvement · `topy-ai-coo-reporting`

| Skill | Tier | What it does |
|---|---|---|
| [`topy-coo-automation-sweep`](plugins/topy-ai-coo-reporting/skills/topy-coo-automation-sweep/SKILL.md) | connected | Finds repeated manual work (done three times or more) worth turning into a Routine or a service, drafts the Routine's steps and schedule, and leaves the go-ahead to the founder. |
| [`topy-coo-efficiency`](plugins/topy-ai-coo-reporting/skills/topy-coo-efficiency/SKILL.md) | connected | Measures how well the Routines run — success rate, retries, cost and failures by cofounder and step — and finds the few changes that would raise reliability or cut founder time. |
| [`topy-coo-vendor-evaluation`](plugins/topy-ai-coo-reporting/skills/topy-coo-vendor-evaluation/SKILL.md) | connected | Compares tools, vendors or contractors on weighted criteria and value for money, and recommends one with the reasons — a method that works with or without TOPY. |
| [`topy-coo-weekly-report`](plugins/topy-ai-coo-reporting/skills/topy-coo-weekly-report/SKILL.md) | connected | The week's operations for the founder (branded email) and the CEO: runs by cofounder, success rate, what fails most, schedule problems and what waits on the founder. |

<!-- skills:end -->
