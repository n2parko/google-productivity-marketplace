# Productivity Marketplace

A Cursor Team Marketplace with Google Workspace plugins powered by the [gws CLI](https://github.com/googleworkspace/cli).

## Plugins

| Plugin | Description |
|--------|-------------|
| [Gmail](plugins/gmail/) | Send, read, triage, and manage email |
| [Google Calendar](plugins/google-calendar/) | View agenda, create events, find free time, and manage your schedule |

## Setup

1. Install the gws CLI: `npm install -g @googleworkspace/cli`
2. Authenticate: `gws auth setup`
3. Install plugins from this marketplace in Cursor

## Repository structure

```
.cursor-plugin/marketplace.json          # marketplace manifest
plugins/gmail/                           # Gmail plugin
  .cursor-plugin/plugin.json             # plugin manifest
  .mcp.json                              # MCP server config
  skills/                                # 7 skills (setup, send, triage, search, labels, drafts, filters)
  agents/inbox-manager.md                # inbox management agent
  assets/logo.svg                        # plugin logo
plugins/google-calendar/                 # Google Calendar plugin
  .cursor-plugin/plugin.json             # plugin manifest
  .mcp.json                              # MCP server config
  skills/                                # 6 skills (setup, agenda, create, free time, reschedule, recurring)
  agents/schedule-manager.md             # schedule management agent
  assets/logo.svg                        # plugin logo
```
