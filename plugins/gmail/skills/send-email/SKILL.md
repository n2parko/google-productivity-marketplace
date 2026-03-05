---
name: send-email
description: Compose and send an email via Gmail using the gws CLI.
---

# Send Email

Compose and send a plain-text email. For HTML or attachments, fall back to the raw Gmail API.

## Before you start

Run this check before proceeding:

```bash
gws auth list
```

- If `gws` is not found, or no accounts are listed, or the output indicates no active session, **stop and run the setup-gws skill first**.
- If an account is listed and active, continue below.

## Quick send

```bash
gws gmail +send --to <EMAIL> --subject '<SUBJECT>' --body '<BODY>'
```

## Steps

1. **Confirm recipients, subject, and body** with the user before sending.
2. **Dry-run first** for any message the user hasn't explicitly approved:
   ```bash
   gws gmail +send --to alice@example.com --subject 'Hello' --body 'Hi Alice!' --dry-run
   ```
3. **Send** once confirmed:
   ```bash
   gws gmail +send --to alice@example.com --subject 'Hello' --body 'Hi Alice!'
   ```

## Sending HTML or adding CC/BCC/attachments

Use the raw messages API when you need more than plain text:

```bash
gws gmail users messages send --json '{
  "raw": "<base64-encoded RFC 2822 message>"
}'
```

Build the RFC 2822 message with proper MIME headers for CC, BCC, HTML body, or attachments, then base64url-encode it.

## Tips

- The `+send` helper handles RFC 2822 formatting and base64 encoding automatically for plain text.
- Always confirm with the user before executing — this is a write operation.
- Use `gws schema gmail.users.messages.send` to inspect the full request schema.
