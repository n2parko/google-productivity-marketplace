---
name: draft-and-reply
description: Create drafts and send replies to existing Gmail threads.
---

# Draft and Reply

Create email drafts for review and send replies within existing threads.

## Before you start

Run this check before proceeding:

```bash
gws auth list
```

- If `gws` is not found, or no accounts are listed, or the output indicates no active session, **stop and run the setup-gws skill first**.
- If an account is listed and active, continue below.

## Create a draft

```bash
gws gmail users drafts create --params '{"userId": "me"}' --json '{
  "message": {
    "raw": "<base64url-encoded RFC 2822 message>"
  }
}'
```

## List drafts

```bash
gws gmail users drafts list --params '{"userId": "me", "maxResults": 10}'
```

## Send a draft

```bash
gws gmail users drafts send --params '{"userId": "me"}' --json '{
  "id": "<DRAFT_ID>"
}'
```

## Reply to a message

To reply in-thread, include the `threadId` and appropriate `In-Reply-To` / `References` headers in the raw RFC 2822 message:

```bash
gws gmail users messages send --params '{"userId": "me"}' --json '{
  "threadId": "<THREAD_ID>",
  "raw": "<base64url-encoded reply message>"
}'
```

## Workflow — draft-first approach

1. **Read the original message** using the search-and-read skill.
2. **Compose a draft reply** and present it to the user for review.
3. **Create the draft** via the API so the user can see it in Gmail.
4. **Send the draft** only after the user approves.

## Building the raw message

Construct an RFC 2822 message with these headers:

```
From: me
To: <recipient>
Subject: Re: <original subject>
In-Reply-To: <original Message-ID header>
References: <original Message-ID header>
Content-Type: text/plain; charset="UTF-8"

<reply body>
```

Then base64url-encode the entire string (replace `+` with `-`, `/` with `_`, strip `=` padding).

## Tips

- Always use a draft-first workflow for important messages — it lets the user review before sending.
- The `threadId` field groups the reply into the original conversation.
- Confirm with the user before sending — these are write operations.
