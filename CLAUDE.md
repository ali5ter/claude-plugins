# CLAUDE.md — ali5ter/claude-plugins

## Project Status

Active. The marketplace is a thin catalog repo referencing plugin source repos
via `.claude-plugin/marketplace.json`.

## Purpose

`ali5ter/claude-plugins` is a Claude Code plugin marketplace hosting personal plugins
maintained by ali5ter. Users register it with `/plugin marketplace add ali5ter/claude-plugins`
and install individual plugins by name.

## Architecture: Thin Catalog

The marketplace repo contains only:

- `.claude-plugin/marketplace.json` — the plugin catalog
- `README.md` — usage documentation
- `CLAUDE.md` — this file

Each plugin entry in `marketplace.json` uses `{"source": "github", "repo": "owner/repo"}`
to reference the plugin's own GitHub repository. No plugin content is stored here.

**Why:** Each plugin repo is the single source of truth. This avoids content duplication,
eliminates git submodules, and makes adding new plugins a one-line change in `marketplace.json`.

**Confirmed supported:** The official `anthropics/claude-plugins-official` marketplace uses
the `github` source type in production (e.g., `browserbase/agent-browse`).

## Registered Plugins

| Plugin | Source Repo | Version |
|---|---|---|
| `over-50s-health-advisor` | `ali5ter/over-50s-health-advisor` | 3.0.0 |
| `obsidian-project-documentation-assistant` | `ali5ter/obsidian-project-assistant` | 3.0.1 |

## Install Commands

```text
/plugin marketplace add ali5ter/claude-plugins
/plugin install over-50s-health-advisor@ali5ter
/plugin install obsidian-project-documentation-assistant@ali5ter
```

## Adding a New Plugin

1. Ensure the plugin's repo has `.claude-plugin/plugin.json` at its root.
2. Add an entry to `.claude-plugin/marketplace.json` following the existing schema.
3. Update the plugin table in `README.md` and this file.
4. Commit and push.

## Local Settings

`~/.claude/settings.json` registers this marketplace via `extraKnownMarketplaces`:

```json
{
  "extraKnownMarketplaces": {
    "ali5ter": {
      "source": { "source": "github", "repo": "ali5ter/claude-plugins" }
    }
  }
}
```
