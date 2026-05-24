# context-tracker

Exposes Claude Code's context window usage percentage to agents via `claude-context-pct`.

**Supported platforms**: macOS, Linux. Windows is not supported — Windows users are encouraged to point their agent at this repo and build their own solution.

## Setup

This plugin has two parts: a **consumer** (the `bin/` script, installed automatically with the plugin) and a **producer** (a snippet you add to your statusline script).

### Statusline Configuration

Add the following block to your statusline script (e.g. `~/.claude/statusline.sh`). It writes a small JSON file on each render that `claude-context-pct` reads.

```bash
# --- BEGIN context-tracker (claude-ops-plugins) ---
_ctx_dir="/tmp/claude-$(id -u)/context"
_ctx_session_id=$(echo "$input" | jq -r '.session_id // empty')
if [ -n "$pct" ] && [ -n "$_ctx_session_id" ]; then
    mkdir -p "$_ctx_dir" 2>/dev/null
    chmod 700 "$_ctx_dir" 2>/dev/null
    _ctx_tmp=$(mktemp "$_ctx_dir/.ctx.XXXXXX")
    printf '{"session_id":"%s","used_pct":%s,"cwd":"%s","updated_at":"%s"}\n' \
        "$_ctx_session_id" "${pct:-0}" "$cwd" "$(date -u +%Y-%m-%dT%H:%M:%SZ)" \
        > "$_ctx_tmp"
    mv -f "$_ctx_tmp" "$_ctx_dir/${_ctx_session_id}.json"
fi
# --- END context-tracker (claude-ops-plugins) ---
```

**Requirements**: The snippet expects two variables from your statusline script:
- `$pct` — the context window percentage (integer), extracted from `.context_window.used_percentage`
- `$cwd` — the workspace directory, extracted from `.workspace.current_dir`

If your statusline uses different variable names, adjust accordingly.

## Usage

```bash
claude-context-pct        # returns 0%-100%, or -1 if unavailable
claude-context-pct <id>   # query a specific session ID
```

## Configuring agent behavior

The plugin gives agents *visibility* into context usage but does not define what to do with it. It is up to you to instruct your agent on how to respond to context levels — for example, in a project's `CLAUDE.md` or via persistent memory.

Some common patterns:

- **Handoff preparation**: "When context usage reaches 70%, wrap up the current task, commit progress, and prepare a handoff summary for the next session."
- **Periodic check-ins**: "Run `claude-context-pct` after completing each major task to decide whether to continue or hand off."
- **Conservative pacing**: "If context exceeds 50%, avoid spawning new subagents and prefer smaller, focused tool calls."

Choose thresholds and actions that match your workflow — there are no universal defaults.
