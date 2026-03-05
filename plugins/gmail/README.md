# Gmail Plugin

Send, read, triage, and manage email using the [gws CLI](https://github.com/googleworkspace/cli).

## Setup

1. Install the CLI: `npm install -g @googleworkspace/cli`
2. Authenticate: `gws auth login`
3. Verify: `gws gmail +triage`

## Skills

| Skill | Description |
|-------|-------------|
| **setup-gws** | Install and configure the gws CLI for Gmail access |
| **send-email** | Compose and send plain-text email |
| **triage-inbox** | Show unread inbox summary and prioritize |
| **search-and-read** | Search messages by query and read full content |
| **manage-labels** | Create, apply, and remove Gmail labels |
| **draft-and-reply** | Create drafts and reply within threads |
| **create-filter** | Set up filters to auto-label, archive, or forward mail |

## Agent

**inbox-manager** — Triages your inbox, drafts replies, and keeps email organized. Start a conversation and ask it to check your inbox.
