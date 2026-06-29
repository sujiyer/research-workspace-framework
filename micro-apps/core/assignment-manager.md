# Micro-App: Assignment Manager

## Purpose
The Assignment Manager is the task and coverage management micro-app. It surfaces what research an analyst needs to produce, by when, and for which fund or security — scoped to the active workspace context.

## Core Capabilities
- View all open research assignments for the active context
- Filter by status: Open, In Progress, Under Review, Published
- See assignment details: due date, requestor, priority, linked fund or security
- Update assignment status as work progresses
- Create new ad-hoc assignments from within the workspace

## Context Behaviour
**Default mode: SHARED**

When the workspace context changes to a new fund, the Assignment Manager automatically filters to show assignments related to that fund. The analyst can override to show all assignments across their coverage universe by toggling the context filter off.

## API Contract (Summary)

```
GET /api/v1/assignments
  Query params: entity_id, status, analyst_id, due_before, due_after
  Response: { assignments: [{id, title, entity, due_date, status, priority}] }

PATCH /api/v1/assignments/{id}
  Request: { status, notes }
  Response: { updated_assignment }

POST /api/v1/assignments
  Request: { title, entity_id, due_date, priority, notes }
  Response: { created_assignment }
```

## Context-Driven Event
When an analyst opens an assignment, the Assignment Manager publishes:
```json
{
  "event_type": "context.changed",
  "trigger": "APP_DRIVEN",
  "new_context": {
    "entity_type": "FUND | SECURITY",
    "entity_id": "string from assignment"
  }
}
```
This drives the workspace context to the fund or security linked to the opened assignment — allowing one click to orient the entire workspace around the work at hand.
