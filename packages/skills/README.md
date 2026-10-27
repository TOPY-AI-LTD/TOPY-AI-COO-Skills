# @topy-ai/coo-skills

The [TOPY.AI](https://topy.ai) AI COO skills for **Claude Code, Codex, Pi, Cursor, Gemini CLI, OpenCode, GitHub Copilot, Windsurf** and other coding agents: 14 skills that run a small company's operations the way a senior COO would — every AI cofounder's Routines on time, failures never silent (safe ones retried once, the rest handed to a person), approvals that don't pile up, and a weekly operations report to the founder and the CEO — powered by the TOPY.AI COO MCP server.

```sh
npx @topy-ai/coo-skills install                  # this project, for the agents set up here
npx @topy-ai/coo-skills install -g               # your user: every project
npx @topy-ai/coo-skills install -g -a codex,pi   # pick the agents
npx @topy-ai/coo-skills install --mcp            # also add the COO MCP server to .mcp.json (Claude Code)
```

Pi users can also install it as a Pi package: `pi install npm:@topy-ai/coo-skills`.

## Agents

| Agent | Skills | MCP server |
|---|---|---|
| **Claude Code** | `npx @topy-ai/coo-skills install -a claude`, or the plugin marketplace | `install --mcp` writes `.mcp.json`, or the `topy-ai-coo` plugin |
| **Codex** | `npx @topy-ai/coo-skills install -a codex`, or `codex plugin marketplace add TOPY-AI-LTD/TOPY-AI-COO-Skills` | `codex mcp add topy-coo --url https://topy.ai/mcp/coo` |
| **Pi** | `pi install npm:@topy-ai/coo-skills` | `npx -y @topy-ai/coo-mcp` |
| **Cursor, Gemini CLI, OpenCode, Copilot, Windsurf and others** | `npx @topy-ai/coo-skills install -a <agent>` | Remote URL `https://topy.ai/mcp/coo` |
| **Claude.ai, Claude Desktop, ChatGPT** | Upload a skill folder as a zip | Custom connector `https://topy.ai/mcp/coo` |

`npx @topy-ai/coo-skills agents` prints the supported agents and marks the ones found on your machine. The install record lives in `<project>/.topy-ai/coo-skills-install.json`, or `~/.config/topy/coo-skills-install.json` for the user scope (`TOPY_HOME`). Commit the project's skill folders and `.topy-ai/` to share the skills with your team.

## Choose skills

```sh
npx @topy-ai/coo-skills list                                              # every skill, by category
npx @topy-ai/coo-skills install --category incidents,approvals            # categories (or plugin names like topy-ai-coo-incidents)
npx @topy-ai/coo-skills install topy-coo-failed-runs topy-coo-weekly-report   # single skills
```

Categories: `core` (connect, daily check, goals — always installed), `routines`, `incidents`, `approvals`, `reporting`.

## The MCP server

`https://topy.ai/mcp/coo` — the same TOPY.AI account and sign-in (OAuth) as the AI CEO and AI CFO; only the accounts that own the OneMan operation see it. For scheduled agents, an API key as `TOPY_API_KEY`.

```sh
npx @topy-ai/coo-skills mcp -g     # prints the commands for Claude Code, Codex, Gemini CLI, Cursor and Pi
```

## Update and remove

```sh
npx @topy-ai/coo-skills@latest update                      # this project (-g for your user)
npx @topy-ai/coo-skills remove topy-coo-vendor-evaluation  # or --all
npx @topy-ai/coo-skills doctor
```

Files you edited are kept on update and remove unless you pass `--force`. Every command accepts `--dry-run`. The CEO, CFO and COO skill packages install side by side; each keeps its own record.

## Without the installer

```sh
git clone --depth 1 https://github.com/TOPY-AI-LTD/TOPY-AI-COO-Skills.git
mkdir -p ~/.agents/skills
cp -R TOPY-AI-COO-Skills/plugins/*/skills/* ~/.agents/skills/
```

Or `npx skills add TOPY-AI-LTD/TOPY-AI-COO-Skills -a <agent>`. Claude Code plugins: `/plugin marketplace add TOPY-AI-LTD/TOPY-AI-COO-Skills`, then `/plugin install topy-ai-coo-all@topy-ai-coo`.

Source, skills index and changelog: [TOPY-AI-LTD/TOPY-AI-COO-Skills](https://github.com/TOPY-AI-LTD/TOPY-AI-COO-Skills).
