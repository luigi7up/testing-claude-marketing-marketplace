# Claude SMB Marketplace

This repository is to be used as a Claude Cowork marketplace to enable plugins and skills that automate key SMB workflows

## Local development

If you're contributing or want to test changes without pushing to the GitHub repository, install it in CLaude from your local clone.

### Prerequisites

- [Claude Code CLI](https://docs.claude.com/en/docs/claude-code)
- Verify with `claude --version`
- Git

### Setup

Clone this repo

Open a terminal, navigate to the repo folder and start a Claude Code session:

```bash
cd <path to this local repo>
claude
```

Register your local clone as a marketplace and install the plugin from it:

```
/plugin marketplace add ./.
/plugin install marketing-planner-v1@claude-smb-marketplace
/reload-plugins
```

### Run the plugin

1. Invoke the plugin to test 
    ```
    /marketing-planner-v1:claude-smb-marketplace
    ```
2. After making local changes, reload:
   ```
   /reload-plugins
   ```


For changes to a skill's `description:` frontmatter (which controls auto-triggering), restart the session entirely — descriptions are evaluated at session start.

## Structure

- `catalog/plugins.json` - plugin catalog used to list and discover all plugins
- `plugins/<plugin-id>/.claude-plugin/plugin.json` - plugin manifest
- `plugins/<plugin-id>/skills/<skill-name>/SKILL.md` - plugin skills
- `plugins/<plugin-id>/references/` - plugin reference docs

## Included Plugins

- `marketing-planner-v1` - Guided 5-phase marketing planning plugin for SMBs (v1)

## How To Add A New Plugin

1. Create a new folder under `plugins/` using a unique plugin id.
2. Add a `plugin.json` file with metadata.
3. Add implementation assets (for example `SKILL.md`, prompts, references).
4. Add the plugin entry to `catalog/plugins.json`.

This keeps the marketplace consistent and makes plugins easy to discover and maintain.
