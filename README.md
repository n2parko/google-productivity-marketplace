# Team Marketplace

A Cursor Team Marketplace for sharing plugins across the team.

## Getting started

Add a new plugin by following the guide in [docs/add-a-plugin.md](docs/add-a-plugin.md).

## Repository structure

- `.cursor-plugin/marketplace.json` — marketplace manifest and plugin registry
- `plugins/<plugin-name>/.cursor-plugin/plugin.json` — per-plugin metadata
- `plugins/<plugin-name>/rules/` — rule files (`.mdc`)
- `plugins/<plugin-name>/skills/` — skill folders with `SKILL.md`
- `plugins/<plugin-name>/agents/` — subagent definitions
- `plugins/<plugin-name>/mcp.json` — MCP server configuration

## Validate changes

```bash
node scripts/validate-template.mjs
```

## Adding a plugin

1. Create `plugins/<your-plugin>/` with a `.cursor-plugin/plugin.json` manifest
2. Add skills, rules, agents, or MCP configs as needed
3. Register the plugin in `.cursor-plugin/marketplace.json`
4. Run the validation script to verify everything is wired up correctly
