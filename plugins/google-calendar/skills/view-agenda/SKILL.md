---
name: view-agenda
description: View upcoming events across all calendars for today, this week, or a custom range.
---

# View Agenda

Check what's on the schedule — today, tomorrow, this week, or any number of days ahead.

## Before you start

Run this check before proceeding:

```bash
gws auth list
```

- If `gws` is not found, or no accounts are listed, or the output indicates no active session, **stop and run the setup-gws skill first**.
- If an account is listed and active, continue below.

## Show upcoming events

```bash
gws calendar +agenda
```

## Common variations

| Goal | Command |
|------|---------|
| Today's events | `gws calendar +agenda --today` |
| Tomorrow | `gws calendar +agenda --tomorrow` |
| This week | `gws calendar +agenda --week` |
| Next 3 days | `gws calendar +agenda --days 3` |
| Specific calendar | `gws calendar +agenda --today --calendar 'Work'` |
| Table format | `gws calendar +agenda --week --format table` |
| JSON for processing | `gws calendar +agenda --today --format json` |

## Detailed event listing via API

For more control (e.g., filtering by time range or showing attendees):

```bash
gws calendar events list --params '{
  "calendarId": "primary",
  "timeMin": "2026-03-04T00:00:00Z",
  "timeMax": "2026-03-05T00:00:00Z",
  "singleEvents": true,
  "orderBy": "startTime"
}'
```

## Tips

- This is read-only — it never modifies events.
- Queries all calendars by default; use `--calendar` to narrow it down.
- Use `--format json` and pipe through `jq` for structured extraction.
- Combine with the find-free-time skill to spot gaps in the schedule.
