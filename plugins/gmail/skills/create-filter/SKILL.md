---
name: create-filter
description: Create Gmail filters to automatically label, archive, or forward incoming messages.
---

# Create Filter

Set up Gmail filters that automatically process incoming messages based on criteria.

## Before you start

Run this check before proceeding:

```bash
gws auth list
```

- If `gws` is not found, or no accounts are listed, or the output indicates no active session, **stop and run the setup-gws skill first**.
- If an account is listed and active, continue below.

## List existing filters

```bash
gws gmail users settings filters list --params '{"userId": "me"}'
```

## Create a filter

```bash
gws gmail users settings filters create --params '{"userId": "me"}' --json '{
  "criteria": {
    "from": "notifications@github.com",
    "subject": "[repo-name]"
  },
  "action": {
    "addLabelIds": ["<LABEL_ID>"],
    "removeLabelIds": ["INBOX"]
  }
}'
```

## Filter criteria options

| Field | Description | Example |
|-------|-------------|---------|
| `from` | Sender address or pattern | `noreply@service.com` |
| `to` | Recipient address | `team-alias@company.com` |
| `subject` | Subject contains | `[alerts]` |
| `query` | Full Gmail search query | `has:attachment larger:5M` |
| `negatedQuery` | Exclude matching messages | `from:me` |
| `hasAttachment` | Has attachment (boolean) | `true` |

## Filter action options

| Field | Description |
|-------|-------------|
| `addLabelIds` | Apply these labels |
| `removeLabelIds` | Remove these labels (use `["INBOX"]` to archive) |
| `forward` | Forward to another address |
| `star` | Add a star (set `addLabelIds: ["STARRED"]`) |

## Common filter recipes

**Auto-label GitHub notifications:**
```json
{
  "criteria": { "from": "notifications@github.com" },
  "action": { "addLabelIds": ["<GITHUB_LABEL_ID>"], "removeLabelIds": ["INBOX"] }
}
```

**Star messages from VIPs:**
```json
{
  "criteria": { "from": "ceo@company.com" },
  "action": { "addLabelIds": ["STARRED", "IMPORTANT"] }
}
```

## Delete a filter

```bash
gws gmail users settings filters delete --params '{
  "userId": "me",
  "id": "<FILTER_ID>"
}'
```

## Tips

- List labels first (manage-labels skill) to get the label IDs needed for filter actions.
- Filters only apply to new incoming messages, not existing ones.
- Confirm with the user before creating — filters permanently change how mail is processed.
