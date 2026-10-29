# Lab: One Ticket, Two JSON Designs

## 1. Referenced Design

```json
{
  "users": [
    {
      "user_id": 103,
      "display_name": "Requester 103"
    },
    {
      "user_id": 201,
      "display_name": "Agent 201"
    }
  ],
  "tickets": [
    {
      "ticket_id": 1003,
      "subject": "Low water pressure",
      "status": "resolved",
      "requester_id": 103,
      "assignee_id": 201
    }
  ],
  "events": [
    {
      "event_id": 5005,
      "ticket_id": 1003,
      "actor_id": 103,
      "event_type": "created",
      "event_at": "2026-02-03T12:05:00Z"
    },
    {
      "event_id": 5006,
      "ticket_id": 1003,
      "actor_id": 201,
      "event_type": "status_changed",
      "event_at": "2026-02-03T14:25:00Z"
    },
    {
      "event_id": 5007,
      "ticket_id": 1003,
      "actor_id": 201,
      "event_type": "status_changed",
      "event_at": "2026-02-03T19:40:00Z"
    }
  ]
}
```

## 2. Embedded/Hybrid Design

```json
{
  "ticket_id": 1003,
  "subject": "Low water pressure",
  "status": "resolved",
  "requester": {
    "user_id": 103,
    "display_name": "Requester 103"
  },
  "assignee": {
    "user_id": 201,
    "display_name": "Agent 201"
  },
  "events": [
    {
      "event_id": 5005,
      "event_type": "created",
      "event_at": "2026-02-03T12:05:00Z"
    },
    {
      "event_id": 5006,
      "event_type": "status_changed",
      "event_at": "2026-02-03T14:25:00Z"
    },
    {
      "event_id": 5007,
      "event_type": "status_changed",
      "event_at": "2026-02-03T19:40:00Z"
    }
  ]
}
```

## 3. Tradeoff

When the application opens ticket 1003 in the referenced design, it finds the ticket in the tickets array, uses the requester and assignee IDs to find their names in the users array, and uses the ticket ID to find the related events in the events array. In the embedded design, the application finds the requester and assignee names and all three events directly inside the ticket document, making the ticket page easier to read. If the requester changes their display name, the referenced design only needs the user record updated, while the embedded design may require updating the name in multiple ticket documents. The events array in the embedded design could also grow very large over many years.
