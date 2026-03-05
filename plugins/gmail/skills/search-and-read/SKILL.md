---
name: search-and-read
description: Search Gmail messages by query and read their full content.
---

# Search and Read Emails

Find messages matching a query and retrieve their full content.

## Before you start

Run this check before proceeding:

```bash
gws auth list
```

- If `gws` is not found, or no accounts are listed, or the output indicates no active session, **stop and run the setup-gws skill first**.
- If an account is listed and active, continue below.

## Search messages

```bash
gws gmail users messages list --params '{
  "userId": "me",
  "q": "<GMAIL_SEARCH_QUERY>",
  "maxResults": 10
}'
```

## Read a specific message

```bash
gws gmail users messages get --params '{
  "userId": "me",
  "id": "<MESSAGE_ID>",
  "format": "full"
}'
```

## Read a thread

```bash
gws gmail users threads get --params '{
  "userId": "me",
  "id": "<THREAD_ID>",
  "format": "full"
}'
```

## Common search queries

| Goal | Query |
|------|-------|
| From a person | `from:alice@example.com` |
| With attachment | `has:attachment` |
| Recent (last 7 days) | `newer_than:7d` |
| In a date range | `after:2026/01/01 before:2026/02/01` |
| Subject contains | `subject:quarterly report` |
| Unread and important | `is:unread is:important` |
| Larger than 5MB | `larger:5M` |

## Workflow

1. Ask the user what they're looking for.
2. Build an appropriate Gmail search query.
3. List matching messages and present a summary (sender, subject, date).
4. Let the user pick a message, then fetch the full content.

## Tips

- Use `--format json` and pipe through `jq` for structured extraction.
- The `format` param on `messages get` controls detail: `full`, `metadata`, `minimal`, `raw`.
- For paginated results, add `--page-all` to stream all pages.
