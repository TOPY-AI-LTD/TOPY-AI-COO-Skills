---
name: topy-coo-vendor-evaluation
description: Compares tools, vendors or contractors on weighted criteria and value for money, and recommends one with the reasons — a method that works with or without TOPY. Use when the user says things like "which tool should we use?"; "compare these vendors"; "evaluate contractors"; "best value option"; "pick a supplier"; "tool comparison". Uses TOPY when it is connected.
license: MIT
metadata:
  category: reporting
  tier: connected
  version: 0.1.0
  docs: https://github.com/TOPY-AI-LTD/TOPY-AI-COO-Skills/blob/main/plugins/topy-ai-coo-reporting/skills/topy-coo-vendor-evaluation/SKILL.md
---
# Tool and vendor evaluation

Operations choices (a monitoring service, an email provider, a contractor) are best made on the same scale. This skill weights the criteria, scores each option and divides by cost.

## When to use
- "Which of these should we use?", or a renewal coming up.
- Not for: legal review of a contract (that is a lawyer).

## Before you start
- Agree the criteria and weights with the founder first (e.g. reliability 3, price 2, support 1); scores 0–10 from facts
  (trials, documentation, references), not impressions.

## Workflow
1. List options with monthly cost and a 0–10 score per criterion, each with the fact behind it.
2. `coo_evaluate` with the criteria and options: weighted fit, and value for money (fit ÷ cost) when costs are given.
3. Check the pick against any hard requirement (data location, an integration that must exist); a miss rules it out.
4. Recommend one, with the runner-up and what would change the choice.

## Output
| Option | Fit | Cost / month | Value for money | Notes |
|---|---|---|---|---|
Pick: <option> — because …

## Quality bar
- Every score has a fact behind it.
- Hard requirements are checked before ranking.

## Guardrails
- Writes take two calls: call without `confirm` for a preview, show it, and only after the Founder agrees call again with `confirm: true` and the `confirmation`. Never confirm for the Founder.
- Never approve a Routine step for the Founder: approvals are the Founder's own one-click links (or their yes in their own client).
- Never print or ask for keys, tokens or passwords; settings are changed on the server by the Founder or the CTO.
- A recommendation, not a purchase: buying and contracts stay with the founder.

## Related skills
- `topy-coo-automation-sweep` — when the choice is build a Routine or buy a tool.
