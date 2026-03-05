---
name: find-free-time
description: Query free/busy status across calendars to find available meeting slots.
---

# Find Free Time

Check free/busy availability for one or more people to find a time that works for everyone.

## Before you start

Run this check before proceeding:

```bash
gws auth list
```

- If `gws` is not found, or no accounts are listed, or the output indicates no active session, **stop and run the setup-gws skill first**.
- If an account is listed and active, continue below.

## Query free/busy for yourself

```bash
gws calendar freebusy query --json '{
  "timeMin": "2026-03-05T08:00:00-08:00",
  "timeMax": "2026-03-05T18:00:00-08:00",
  "items": [{"id": "primary"}]
}'
```

## Query multiple people

```bash
gws calendar freebusy query --json '{
  "timeMin": "2026-03-05T08:00:00-08:00",
  "timeMax": "2026-03-05T18:00:00-08:00",
  "items": [
    {"id": "alice@company.com"},
    {"id": "bob@company.com"},
    {"id": "primary"}
  ]
}'
```

## Workflow

1. Ask the user who needs to attend and the preferred date range.
2. Run the freebusy query for all participants.
3. Parse the response to identify overlapping free windows.
4. Present the available slots to the user.
5. Once a slot is chosen, use the **create-event** skill to book it.

## Reading the response

The response contains a `calendars` object keyed by email. Each entry has a `busy` array of `{start, end}` time ranges. Any time not covered by a busy block is free.

```json
{
  "calendars": {
    "alice@company.com": {
      "busy": [
        {"start": "2026-03-05T09:00:00-08:00", "end": "2026-03-05T10:00:00-08:00"},
        {"start": "2026-03-05T14:00:00-08:00", "end": "2026-03-05T15:00:00-08:00"}
      ]
    }
  }
}
```

## Tips

- Free/busy data is limited by each user's sharing settings — if someone hasn't shared their calendar, you'll get an empty result.
- The `timeMin` and `timeMax` window should be reasonable (1-7 days) to keep responses manageable.
- This is read-only — it never modifies any calendar.
