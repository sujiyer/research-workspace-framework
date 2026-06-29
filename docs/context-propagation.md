# Context Propagation

Context propagation is the central architectural innovation of this framework. This document defines how research context is created, published, propagated, and managed across a personalised workspace.

---

## What Is a Research Context

A research context is the active focus of an analyst's research session. It answers the question: *what are we looking at right now?*

A context can be:
- A specific fund ("Technology Growth Fund")
- A specific security ("AAPL")
- A sector or theme ("Semiconductor sector")
- A benchmark ("S&P 500")
- A comparison set ("Technology Growth Fund vs. benchmark vs. peer")

The context is not just an ID. It carries metadata that every micro-app uses to correctly scope its data: asset class, date range, coverage universe, and the analyst who set it.

---

## How Context Is Set

An analyst sets context in one of three ways:

**1. Direct entry**
The analyst types a fund name, ticker, or entity into the workspace context bar at the top of their workspace. The context is published immediately to the Context Bus.

**2. App-driven context**
An analyst clicks a fund name in their Assignment Manager. The Assignment Manager publishes a context change event — the workspace updates its active context and propagates to all subscribed apps.

**3. Saved context**
The analyst opens a saved workspace that remembers its last context. The workspace restores the last known context for each app on load.

---

## How Context Propagates

When a context change event is published to the Context Bus:

```
Analyst sets context: "Technology Growth Fund"
              ↓
Context Bus publishes: context.changed event
              ↓
Each micro-app evaluates its propagation mode:

  SHARED mode apps:
    → Automatically load data for "Technology Growth Fund"
    → Update immediately without analyst confirmation

  PROMPT mode apps:
    → Display banner: "Context updated to Technology Growth Fund — refresh?"
    → Wait for analyst to confirm or dismiss
    → Retain current data until confirmed

  INDEPENDENT mode apps:
    → Ignore the event
    → Retain current context
    → No visual change
```

---

## Context Event Schema

```json
{
  "event_type": "context.changed",
  "event_id": "uuid",
  "workspace_id": "uuid",
  "analyst_id": "uuid",
  "previous_context": {
    "entity_id": "string or null",
    "entity_name": "string or null"
  },
  "new_context": {
    "entity_type": "FUND | SECURITY | SECTOR | BENCHMARK",
    "entity_id": "string",
    "entity_name": "string",
    "asset_class": "string",
    "date_range_start": "ISO8601 date",
    "date_range_end": "ISO8601 date"
  },
  "trigger": "DIRECT_ENTRY | APP_DRIVEN | WORKSPACE_LOAD | LINKED_CONTEXT",
  "timestamp": "ISO8601 datetime"
}
```

---

## Multi-Context Workspaces

An analyst comparing two funds needs both in their workspace simultaneously. The framework supports this through **pinned contexts**:

- The workspace has one **active context** (what was most recently set)
- Each micro-app can also have a **pinned context** — a context it holds regardless of active workspace context

Example: An analyst pins "Technology Growth Fund" in the Peer Comparator, while setting "Healthcare Innovation Fund" as the active workspace context. The Peer Comparator continues showing Technology Growth Fund data while all other apps update to Healthcare Innovation Fund.

Pinned contexts are displayed as a badge on the micro-app header — a visual signal to the analyst that the app is not following the workspace context.

---

## Context Persistence

Every context state is persisted:

- The active workspace context when the analyst ends their session
- Each micro-app's last context (active or pinned) when the session ends
- Context history — the last 10 contexts set in each workspace, accessible for quick re-selection

On session restore:
1. The workspace loads each micro-app's last known context
2. Apps in SHARED mode check if the workspace active context differs from their last known context
3. If it differs, they display a "Context updated since last session" prompt before loading
4. This prevents stale context being silently loaded without analyst awareness

---

## Context Linking

Advanced workspaces can link context between specific apps — not broadcasting to all apps but creating directed relationships:

Example: The Returns Analyzer and the Risk Metrics Dashboard are context-linked. When the analyst changes the date range in the Returns Analyzer, the Risk Metrics Dashboard automatically updates to the same date range — even if other apps do not.

Context links are directional:
- **Source app:** publishes context updates when its context changes
- **Target app:** subscribes to updates from the source app, not from the general workspace context

This enables sophisticated comparative workflows without disrupting the analyst's other open apps.
