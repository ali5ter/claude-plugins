# CLAUDE.md — ali5ter/claude-plugins

## Project Status

Live at <https://github.com/ali5ter/claude-plugins>. Tested and working locally.
Both plugins install successfully via `/plugin install`.

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
| `over-50s-health` | `ali5ter/over-50s-health-advisor` | 3.1.0 |
| `obsidian-project-documentation` | `ali5ter/obsidian-project-assistant` | 3.1.0 |

## Install Commands

```text
/plugin marketplace add ali5ter/claude-plugins
/plugin install over-50s-health@ali5ter
/plugin install obsidian-project-documentation@ali5ter
```

## Adding a New Plugin

1. Ensure the plugin's repo has `.claude-plugin/plugin.json` at its root.
2. Remove `.claude-plugin/marketplace.json` from the plugin repo if present (no longer needed).
3. Add an entry to `.claude-plugin/marketplace.json` in this repo following the existing schema.
4. Update the plugin table in `README.md` and this file.
5. Commit and push.
6. Run `/plugin marketplace update ali5ter` in Claude Code before installing — the plugin
   installer uses a cached copy of the catalog and will not see new or renamed plugins without
   an explicit update.

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

`~/.claude/plugins/marketplaces/ali5ter` is symlinked to the local project directory
for development. When Claude Code clones the marketplace from GitHub, it will replace
the symlink with a real clone.
