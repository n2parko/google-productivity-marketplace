---
name: manage-labels
description: Create, list, and apply Gmail labels to organize messages.
---

# Manage Labels

Organize email with Gmail labels — list existing labels, create new ones, and apply or remove them from messages.

## Before you start

Run this check before proceeding:

```bash
gws auth list
```

- If `gws` is not found, or no accounts are listed, or the output indicates no active session, **stop and run the setup-gws skill first**.
- If an account is listed and active, continue below.

## List labels

```bash
gws gmail users labels list --params '{"userId": "me"}'
```

## Create a label

```bash
gws gmail users labels create --params '{"userId": "me"}' --json '{
  "name": "Project/Alpha",
  "labelListVisibility": "labelShow",
  "messageListVisibility": "show"
}'
```

## Apply labels to a message

```bash
gws gmail users messages modify --params '{
  "userId": "me",
  "id": "<MESSAGE_ID>"
}' --json '{
  "addLabelIds": ["<LABEL_ID>"]
}'
```

## Remove labels from a message

```bash
gws gmail users messages modify --params '{
  "userId": "me",
  "id": "<MESSAGE_ID>"
}' --json '{
  "removeLabelIds": ["<LABEL_ID>"]
}'
```

## Archive a message (remove from inbox)

```bash
gws gmail users messages modify --params '{
  "userId": "me",
  "id": "<MESSAGE_ID>"
}' --json '{
  "removeLabelIds": ["INBOX"]
}'
```

## Workflow

1. List labels to find existing label IDs.
2. Create new labels if the desired category doesn't exist.
3. Apply/remove labels on target messages.
4. Confirm label changes with the user before executing — these are write operations.

## Tips

- Nested labels use `/` separators (e.g., `Project/Alpha`).
- System labels like `INBOX`, `UNREAD`, `STARRED`, `IMPORTANT` can be applied/removed but not deleted.
- Use `gws schema gmail.users.labels.create` to inspect the full label schema.
