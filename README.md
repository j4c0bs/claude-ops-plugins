# claude-ops-plugins

A Claude Code plugin marketplace for operational and runtime utilities.

## Plugins

| Plugin | Description |
|--------|-------------|
| [context-tracker](plugins/context-tracker/) | Exposes context window usage percentage to agents via `claude-context-pct` |

## Skills

| Skill | Plugin | Description |
|-------|--------|-------------|
| check-context-window-usage | context-tracker | Check what % of the context window is used in the current session |

## Installation

Add this marketplace to your Claude Code settings:

```json
{
  "extraKnownMarketplaces": {
    "claude-ops-plugins": {
      "source": {
        "source": "github",
        "repo": "j4c0bs/claude-ops-plugins"
      }
    }
  }
}
```

Then install plugins via `/plugin` in Claude Code.
