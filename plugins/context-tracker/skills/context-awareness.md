---
description: Check context window usage. Use when working on large task lists, long sessions, or autonomous workflows to know when to prepare handoff.
---

# Context Window Awareness

Run `claude-context-pct` to get current context window usage as an integer (0-100).

## Returns
- `0-100`: percentage of context window used
- `-1`: no data available (session just started, or statusline hasn't rendered yet)

## Gotchas
- Value can **decrease** after auto-compaction — don't assume it's monotonically increasing
- Subagents get their own session ID; the statusline writes data for the parent session only, so subagents will get `-1`
- Data updates on each statusline render (after every agent turn), not in real-time

## Autonomous workflow pattern
When processing a large task list, check context usage periodically. As usage approaches or exceeds a threshold (e.g. 30%), prepare a structured handoff: summarize completed work, remaining tasks, and any state the next session needs.
