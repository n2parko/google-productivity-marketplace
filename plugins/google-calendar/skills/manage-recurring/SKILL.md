---
name: manage-recurring
description: Create, view, and modify recurring calendar events.
---

# Manage Recurring Events

Create events that repeat on a schedule, view individual instances, and modify the series or single occurrences.

## Before you start

Run this check before proceeding:

```bash
gws auth list
```

- If `gws` is not found, or no accounts are listed, or the output indicates no active session, **stop and run the setup-gws skill first**.
- If an account is listed and active, continue below.

## Create a recurring event

```bash
gws calendar events insert --params '{"calendarId": "primary"}' --json '{
  "summary": "Weekly Team Sync",
  "start": {"dateTime": "2026-03-09T09:00:00-08:00", "timeZone": "America/Los_Angeles"},
  "end": {"dateTime": "2026-03-09T09:30:00-08:00", "timeZone": "America/Los_Angeles"},
  "recurrence": ["RRULE:FREQ=WEEKLY;BYDAY=MO;COUNT=12"],
  "attendees": [
    {"email": "alice@company.com"},
    {"email": "bob@company.com"}
  ]
}'
```

## Common RRULE patterns

| Pattern | RRULE |
|---------|-------|
| Every weekday | `RRULE:FREQ=WEEKLY;BYDAY=MO,TU,WE,TH,FR` |
| Every Monday | `RRULE:FREQ=WEEKLY;BYDAY=MO` |
| Every 2 weeks on Tuesday | `RRULE:FREQ=WEEKLY;INTERVAL=2;BYDAY=TU` |
| Monthly on the 1st | `RRULE:FREQ=MONTHLY;BYMONTHDAY=1` |
| Monthly, first Wednesday | `RRULE:FREQ=MONTHLY;BYDAY=1WE` |
| Daily for 10 occurrences | `RRULE:FREQ=DAILY;COUNT=10` |
| Weekly until a date | `RRULE:FREQ=WEEKLY;BYDAY=FR;UNTIL=20261231T000000Z` |

## List instances of a recurring event

```bash
gws calendar events instances --params '{
  "calendarId": "primary",
  "eventId": "<RECURRING_EVENT_ID>",
  "timeMin": "2026-03-01T00:00:00Z",
  "timeMax": "2026-04-01T00:00:00Z"
}'
```

## Modify a single instance

Fetch the specific instance ID from the instances list, then patch it:

```bash
gws calendar events patch --params '{
  "calendarId": "primary",
  "eventId": "<INSTANCE_EVENT_ID>",
  "sendUpdates": "all"
}' --json '{
  "summary": "Team Sync (moved)",
  "start": {"dateTime": "2026-03-16T10:00:00-08:00", "timeZone": "America/Los_Angeles"},
  "end": {"dateTime": "2026-03-16T10:30:00-08:00", "timeZone": "America/Los_Angeles"}
}'
```

## Modify the entire series

Patch the parent recurring event ID (not an instance ID) to change all future occurrences:

```bash
gws calendar events patch --params '{
  "calendarId": "primary",
  "eventId": "<RECURRING_EVENT_ID>",
  "sendUpdates": "all"
}' --json '{
  "summary": "Team Sync (updated)",
  "recurrence": ["RRULE:FREQ=WEEKLY;BYDAY=TU;COUNT=12"]
}'
```

## Delete a recurring event

```bash
# Delete entire series
gws calendar events delete --params '{
  "calendarId": "primary",
  "eventId": "<RECURRING_EVENT_ID>",
  "sendUpdates": "all"
}'

# Delete single instance
gws calendar events delete --params '{
  "calendarId": "primary",
  "eventId": "<INSTANCE_EVENT_ID>",
  "sendUpdates": "all"
}'
```

## Tips

- RRULE follows the [iCalendar RFC 5545](https://datatracker.ietf.org/doc/html/rfc5545#section-3.3.10) spec.
- Patching a single instance creates an "exception" — the rest of the series stays unchanged.
- Always confirm changes with the user, especially for series modifications that affect multiple people.
