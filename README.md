# GitHub Cloud Marketplace (Internal)

This repository is organized as an internal marketplace for company plugins.

## Structure

- `catalog/plugins.json` - plugin catalog used to list and discover all plugins
- `plugins/<plugin-id>/.claude-plugin/plugin.json` - plugin manifest
- `plugins/<plugin-id>/skills/<skill-name>/SKILL.md` - plugin skills
- `plugins/<plugin-id>/references/` - plugin reference docs

## Included Plugins

- `marketing-planner` - guided marketing planning workflow for SMB customers

## How To Add A New Plugin

1. Create a new folder under `plugins/` using a unique plugin id.
2. Add a `plugin.json` file with metadata.
3. Add implementation assets (for example `SKILL.md`, prompts, references).
4. Add the plugin entry to `catalog/plugins.json`.

This keeps the marketplace consistent and makes plugins easy to discover and maintain.
