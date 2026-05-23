---
description: Check context window usage percentage. Run `claude-context-pct` to get an integer 0-100 (or -1 if unavailable). Refer to project instructions for thresholds and actions.
---

# Check Context Window Usage

Run `claude-context-pct` to get current context window usage as an integer (0-100).

## Returns
- `0-100`: percentage of context window used
- `-1`: no data available (session just started, or statusline hasn't rendered yet)

## Behavior
- Value can **decrease** after auto-compaction — it is not monotonically increasing
- Subagents get their own session ID; the statusline writes data for the parent session only, so subagents will get `-1`
- Data updates on each statusline render (after every agent turn), not in real-time
- Thresholds and actions to take are defined by the user in project instructions (e.g. CLAUDE.md) — do not assume defaults
