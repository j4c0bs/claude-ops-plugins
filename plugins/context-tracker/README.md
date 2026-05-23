# context-tracker

Exposes Claude Code's context window usage percentage to agents via `claude-context-pct`.

## Setup

This plugin has two parts: a **consumer** (the `bin/` script, installed automatically with the plugin) and a **producer** (a snippet you add to your statusline script).

### Statusline Configuration

Add the following block to your statusline script (e.g. `~/.claude/statusline.sh`). It writes a small JSON file on each render that `claude-context-pct` reads.

```bash
# --- BEGIN context-tracker (claude-ops-plugins) ---
_ctx_dir="/private/tmp/claude-$(id -u)/context"
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
claude-context-pct        # returns 0-100, or -1 if unavailable
claude-context-pct <id>   # query a specific session ID
```

Define thresholds and actions in your project's CLAUDE.md.
