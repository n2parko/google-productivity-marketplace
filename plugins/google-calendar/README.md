# Google Calendar Plugin

View agenda, create events, find free time, and manage your schedule using the [gws CLI](https://github.com/googleworkspace/cli).

## Setup

1. Install the CLI: `npm install -g @googleworkspace/cli`
2. Authenticate: `gws auth login`
3. Verify: `gws calendar +agenda --today`

## Skills

| Skill | Description |
|-------|-------------|
| **setup-gws** | Install and configure the gws CLI for Google Calendar access |
| **view-agenda** | View upcoming events for today, this week, or a custom range |
| **create-event** | Create events with attendees, location, description, and Meet links |
| **find-free-time** | Query free/busy status across calendars to find available slots |
| **reschedule-event** | Move events to a new time and notify attendees |
| **manage-recurring** | Create and modify recurring events with flexible RRULE patterns |

## Agent

**schedule-manager** — Manages your calendar end-to-end. Ask it to check your day, find time for a meeting, or reschedule something.

## MCP

This plugin exposes Google Calendar as an MCP server via `gws mcp -s calendar -e`, giving the agent direct tool access to all Calendar API methods.
