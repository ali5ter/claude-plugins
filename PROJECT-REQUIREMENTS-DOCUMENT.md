# Project Requirements: ali5ter/claude-plugins Marketplace

## Goal

Set up `~/Documents/Projects/claude-plugins` as a GitHub repository that serves
as a Claude Code plugin marketplace for all ali5ter plugins. Once pushed to
GitHub as `ali5ter/claude-plugins`, users (including the repo owner) will install
plugins with:

```text
/plugin marketplace add ali5ter/claude-plugins
/plugin install over-50s-health-advisor@ali5ter
/plugin install obsidian-project-documentation-assistant@ali5ter
```

## Background: How the Claude Code Plugin Marketplace Works

Investigated from `~/.claude/plugins/marketplaces/claude-plugins-official/`:

- A marketplace is a GitHub repo cloned locally to
  `~/.claude/plugins/marketplaces/{marketplace-name}/`
- The marketplace declares its plugins via a `.claude-plugin/marketplace.json`
  catalog file at the repo root
- Each entry in `marketplace.json` defines a plugin name, description, version,
  author, category, and **source** — where the plugin content lives
- The `/plugin marketplace add user/repo` command:
  - Registers the marketplace under the name derived from the repo owner/name
  - Clones the repo locally
- The `/plugin install plugin-name@marketplace-name` command:
  - Reads `.claude-plugin/marketplace.json` from the cloned marketplace
  - Resolves the plugin source and fetches the content
  - Caches the plugin under `~/.claude/plugins/cache/{marketplace}/{plugin}/{version}/`

### Supported Plugin Source Types

Confirmed from the official `marketplace.json`:

| Type | Schema | Use case |
|---|---|---|
| Relative path | `"./plugins/plugin-name"` | Plugin content lives in the marketplace repo |
| GitHub | `{"source": "github", "repo": "owner/repo"}` | Plugin content is the root of a GitHub repo |
| URL | `{"source": "url", "url": "https://github.com/owner/repo.git"}` | Plugin content from any Git URL |
| URL + path | `{"source": "url", "url": "...", "path": "subdir"}` | Plugin in a subdirectory of an external repo |
| git-subdir | `{"source": "git-subdir", "url": "...", "path": "subdir"}` | Plugin in a subdirectory of an external Git repo |

## Plugins to Include

### over-50s-health-advisor

- Source repo: `ali5ter/over-50s-health-advisor`
  (`~/Documents/Projects/over-50s-health-advisor`)
- Plugin content at repo root:
  - `.claude-plugin/plugin.json`
  - `agents/over-50s-health-advisor.md`
  - `context/templates/` (5 context template files)

### obsidian-project-documentation-assistant

- Source repo: `ali5ter/obsidian-project-assistant`
- Plugin content cached at:
  `~/.claude/plugins/cache/ali5ter/obsidian-project-documentation-assistant/3.0.1/`
  (`agents/`, `skills/`, `.claude-plugin/plugin.json`)

## Architecture Decision: Thin Catalog with GitHub Sources

**Decision: thin catalog repo.** The `ali5ter/claude-plugins` repo contains only
`marketplace.json` (and supporting docs). Each plugin references its own GitHub
repo via the `github` source type.

**Rationale:**

- Each plugin repo remains the single source of truth — no content duplication
- No git submodules — the plugin system resolves sources directly from `marketplace.json`
- Confirmed supported: the official marketplace uses `"source": "github"` in
  production (e.g., `browserbase/agent-browse`)
- Adding a new plugin requires only a new entry in `marketplace.json`

**Source entries for this marketplace:**

```json
{
  "name": "over-50s-health-advisor",
  "source": { "source": "github", "repo": "ali5ter/over-50s-health-advisor" }
},
{
  "name": "obsidian-project-documentation-assistant",
  "source": { "source": "github", "repo": "ali5ter/obsidian-project-assistant" }
}
```

**Prerequisite:** Each source repo must have `.claude-plugin/plugin.json` at its
root and the plugin content (agents/, skills/, etc.) accessible from that root.
Verify this before writing `marketplace.json`.

## Required Repository Structure

```text
claude-plugins/
├── README.md
├── CLAUDE.md
├── .gitignore
└── .claude-plugin/
    └── marketplace.json
```

## Tasks

1. **Verify source repos**: Confirm `ali5ter/over-50s-health-advisor` and
   `ali5ter/obsidian-project-assistant` each have `.claude-plugin/plugin.json`
   at their root. Note the current `version` field from each `plugin.json` for
   use in `marketplace.json`.

2. **Initialise the repo**: `git init`, create `.gitignore`, initial commit.

3. **Write `.claude-plugin/marketplace.json`**: Catalog file referencing both
   plugins via GitHub sources. Follow the official schema (`$schema`,
   `name`, `description`, `owner`, `plugins[]` with `name`, `description`,
   `version`, `author`, `source`, `category`).

4. **Write `README.md`**: Document the marketplace, how to add it, and how to
   install each plugin. Follow markdownlint standards (120-char line limit,
   fenced code blocks with language identifiers, blank lines around elements).

5. **Write `CLAUDE.md`**: Project status, architecture decisions, install
   commands, and notes on the thin catalog approach.

6. **Test locally before pushing**: Register the marketplace locally by symlinking
   or copying the repo to `~/.claude/plugins/marketplaces/ali5ter/` and updating
   `~/.claude/plugins/known_marketplaces.json`, then run
   `/plugin install over-50s-health-advisor@ali5ter` inside Claude Code to verify
   it works. If the `github` source type does not resolve correctly, fall back to
   `"source": "url"` with the full `.git` URL.

7. **Update `~/.claude/settings.json`**: Change `extraKnownMarketplaces.ali5ter`
   to point to `ali5ter/claude-plugins` (from `ali5ter/over-50s-health-advisor`).

8. **Create the GitHub repo** `ali5ter/claude-plugins` and push.

9. **Update downstream projects**: Once working, update
   `~/Documents/Projects/over-50s-health-advisor/README.md` and `CLAUDE.md`
   to change the marketplace install command from:

   ```text
   /plugin marketplace add ali5ter/over-50s-health-advisor
   ```

   to:

   ```text
   /plugin marketplace add ali5ter/claude-plugins
   ```

## Current Broken State

- `~/.claude/plugins/marketplaces/ali5ter/` does **not** exist (clone never succeeded)
- `~/.claude/settings.json` (`extraKnownMarketplaces`) currently points
  `ali5ter` → `ali5ter/over-50s-health-advisor` — must be updated to
  `ali5ter/claude-plugins` once the new repo is ready
- The obsidian plugin (`obsidian-project-documentation-assistant@ali5ter`) is
  installed and cached but its marketplace source is broken; it must remain
  functional after this work

## Constraints

- Follow the global `~/.claude/CLAUDE.md` development principles (markdownlint,
  bash style guide, professional docs tone, no manual steps — automate what can
  be automated)
- Do not push to GitHub until local install testing passes
- Preserve the obsidian plugin's functionality throughout
