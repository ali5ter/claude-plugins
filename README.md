# ali5ter Claude Code Plugin Marketplace

Personal [Claude Code](https://claude.ai/code) plugin marketplace for plugins maintained by
[ali5ter](https://github.com/ali5ter).

## Available Plugins

| Plugin | Version | Description |
|---|---|---|
| [cli-ux-tester](https://github.com/ali5ter/claude-cli-ux-skill) | 3.0.0 | Expert UX evaluator for command-line interfaces, CLIs, terminal tools, shell scripts, and developer APIs |
| [obsidian-project-documentation](https://github.com/ali5ter/obsidian-project-assistant) | 3.1.0 | Automatically documents technical projects in Obsidian vaults during Claude Code sessions |
| [over-50s-health](https://github.com/ali5ter/over-50s-health-advisor) | 3.1.0 | Evidence-based health, fitness, nutrition, and longevity guidance for adults 50+ |
| [pair-programmer](https://github.com/ali5ter/pair-programmer) | 1.0.0 | Graduated assistance framework to prevent skill atrophy when coding with AI |

## Usage

### Add this marketplace

```text
/plugin marketplace add ali5ter/claude-plugins
```

### Install plugins

```text
/plugin install cli-ux-tester@ali5ter
/plugin install obsidian-project-documentation@ali5ter
/plugin install over-50s-health@ali5ter
/plugin install pair-programmer@ali5ter
```

### Update plugins

```text
/plugin update cli-ux-tester@ali5ter
/plugin update obsidian-project-documentation@ali5ter
/plugin update over-50s-health@ali5ter
/plugin update pair-programmer@ali5ter
```

## Architecture

This is a thin catalog marketplace. The repository contains only a
`.claude-plugin/marketplace.json` catalog that references each plugin's own GitHub
repository as its source. Each plugin repo remains the single source of truth.
