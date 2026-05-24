---
description: Check context window usage percentage. Run `claude-context-pct` to get a value like "42%" (or -1 if unavailable). Only the main/parent agent can use this — subagents and team members will get inaccurate data. Refer to project instructions for thresholds and actions.
---

# Check Context Window Usage

Run `claude-context-pct` to get current context window usage.

## Returns
- `0%-100%`: percentage of context window used
- `-1`: no data available (session just started, or statusline hasn't rendered yet)

## Important: main agent only
This skill is only accurate for the **main/parent agent**. Do NOT use from subagents, agent team members, or background agents — they will receive the parent's context percentage instead of their own, which is misleading. If you are not the main agent, treat the result as unavailable (-1).

## Behavior
- Value can **decrease** after auto-compaction — it is not monotonically increasing
- Data updates on each statusline render (after every agent turn), not in real-time
- Thresholds and actions to take are defined by the user in project instructions (e.g. CLAUDE.md) — do not assume defaults
