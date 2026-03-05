---
name: create-event
description: Create calendar events with attendees, location, and description.
---

# Create Event

Schedule new events on Google Calendar — one-off meetings, all-day events, or quick-add from natural language.

## Before you start

Run this check before proceeding:

```bash
gws auth list
```

- If `gws` is not found, or no accounts are listed, or the output indicates no active session, **stop and run the setup-gws skill first**.
- If an account is listed and active, continue below.

## Quick create

```bash
gws calendar +insert \
  --summary 'Team Standup' \
  --start '2026-03-05T09:00:00-08:00' \
  --end '2026-03-05T09:30:00-08:00'
```

## With attendees and location

```bash
gws calendar +insert \
  --summary 'Design Review' \
  --start '2026-03-06T14:00:00-08:00' \
  --end '2026-03-06T15:00:00-08:00' \
  --location 'Conference Room B' \
  --description 'Review Q2 design proposals' \
  --attendee alice@company.com \
  --attendee bob@company.com
```

## Quick-add from natural language

```bash
gws calendar events quickAdd --params '{
  "calendarId": "primary",
  "text": "Lunch with Sarah tomorrow at noon"
}'
```

## All-day event

```bash
gws calendar events insert --params '{"calendarId": "primary"}' --json '{
  "summary": "Company Offsite",
  "start": {"date": "2026-03-10"},
  "end": {"date": "2026-03-11"}
}'
```

## Event with a Google Meet link

```bash
gws calendar events insert --params '{"calendarId": "primary", "conferenceDataVersion": 1}' --json '{
  "summary": "Remote Sync",
  "start": {"dateTime": "2026-03-05T10:00:00-08:00"},
  "end": {"dateTime": "2026-03-05T10:30:00-08:00"},
  "conferenceData": {
    "createRequest": {"requestId": "unique-id-123"}
  }
}'
```

## Tips

- Use RFC 3339 format for times (e.g., `2026-03-05T09:00:00-08:00`).
- For all-day events, use `date` instead of `dateTime` in start/end.
- Always confirm event details with the user before creating — this is a write operation.
- Use `--dry-run` to preview the API request without actually creating the event.
