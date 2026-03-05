---
name: reschedule-event
description: Move an existing calendar event to a new time and notify attendees.
---

# Reschedule Event

Change the time of an existing event and optionally notify all attendees.

## Before you start

Run this check before proceeding:

```bash
gws auth list
```

- If `gws` is not found, or no accounts are listed, or the output indicates no active session, **stop and run the setup-gws skill first**.
- If an account is listed and active, continue below.

## Find the event to reschedule

First, list events to find the event ID:

```bash
gws calendar events list --params '{
  "calendarId": "primary",
  "timeMin": "2026-03-04T00:00:00Z",
  "timeMax": "2026-03-08T00:00:00Z",
  "singleEvents": true,
  "orderBy": "startTime",
  "q": "standup"
}'
```

## Update the event time

```bash
gws calendar events patch --params '{
  "calendarId": "primary",
  "eventId": "<EVENT_ID>",
  "sendUpdates": "all"
}' --json '{
  "start": {"dateTime": "2026-03-05T10:00:00-08:00", "timeZone": "America/Los_Angeles"},
  "end": {"dateTime": "2026-03-05T10:30:00-08:00", "timeZone": "America/Los_Angeles"}
}'
```

## Cancel and notify

To cancel an event entirely and notify attendees:

```bash
gws calendar events delete --params '{
  "calendarId": "primary",
  "eventId": "<EVENT_ID>",
  "sendUpdates": "all"
}'
```

## sendUpdates options

| Value | Behavior |
|-------|----------|
| `all` | Notify all attendees of the change |
| `externalOnly` | Notify only non-Google Calendar attendees |
| `none` | No notifications sent |

## Workflow

1. Ask the user which event to reschedule.
2. Search for the event using the events list API.
3. Show the current event details and confirm the new time.
4. Use `events patch` with `sendUpdates: "all"` to move it.
5. Confirm the change was successful.

## Tips

- Use `patch` instead of `update` — it only changes the fields you specify.
- Always confirm the new time with the user before executing.
- Set `sendUpdates: "all"` so attendees get notified automatically.
- Use `--dry-run` to preview the change before applying.
