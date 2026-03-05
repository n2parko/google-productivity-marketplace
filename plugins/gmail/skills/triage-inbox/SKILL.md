---
name: triage-inbox
description: Show an unread inbox summary and help prioritize email.
---

# Triage Inbox

Surface unread messages so the user can quickly see what needs attention.

## Before you start

Run this check before proceeding:

```bash
gws auth list
```

- If `gws` is not found, or no accounts are listed, or the output indicates no active session, **stop and run the setup-gws skill first**.
- If an account is listed and active, continue below.

## Show unread messages

```bash
gws gmail +triage
```

## Common variations

| Goal | Command |
|------|---------|
| Top 5 unread | `gws gmail +triage --max 5` |
| From a specific sender | `gws gmail +triage --query 'from:boss@company.com'` |
| With label info | `gws gmail +triage --labels` |
| Starred and unread | `gws gmail +triage --query 'is:starred is:unread'` |
| JSON for processing | `gws gmail +triage --format json` |

## Workflow

1. Run `gws gmail +triage` to show the inbox summary.
2. Ask the user which messages to act on.
3. Use the **draft-and-reply** skill to respond, or **manage-labels** to organize.

## Tips

- This is read-only — it never modifies the mailbox.
- Defaults to table format for quick scanning; use `--format json` when piping to other tools.
- Combine with `--query` to use any Gmail search operator (e.g., `has:attachment`, `newer_than:1d`, `label:important`).
