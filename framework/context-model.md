# Context Model

The context model defines the data structure, lifecycle, and governance of research contexts across the workspace framework.

---

## Context Schema

```json
{
  "context_id": "uuid — unique per context instance",
  "workspace_id": "uuid — the workspace this context belongs to",
  "analyst_id": "uuid — the analyst who set this context",
  
  "primary_entity": {
    "entity_type": "FUND | SECURITY | SECTOR | BENCHMARK | INDEX | CUSTOM",
    "entity_id": "string — internal or vendor identifier",
    "entity_name": "string — human readable name",
    "ticker": "string or null",
    "asset_class": "EQUITY | FIXED_INCOME | MULTI_ASSET | ALTERNATIVES | CASH",
    "currency": "ISO 4217 currency code"
  },
  
  "date_range": {
    "start": "ISO8601 date",
    "end": "ISO8601 date",
    "preset": "1M | 3M | 6M | YTD | 1Y | 3Y | 5Y | CUSTOM or null"
  },
  
  "coverage_universe_filter": "string or null — filter to a specific coverage universe",
  "benchmark_id": "string or null — benchmark to use in comparative calculations",
  
  "propagation_overrides": {
    "app_id": "SHARED | PROMPT | INDEPENDENT"
  },
  
  "pinned_contexts": {
    "app_id": {
      "entity_id": "string",
      "entity_name": "string",
      "pinned_at": "ISO8601 datetime"
    }
  },
  
  "set_at": "ISO8601 datetime",
  "set_by": "ANALYST | SYSTEM | LINKED_APP",
  "source_app_id": "string or null — if set by an app"
}
```

---

## Context Lifecycle

```
CREATED (analyst or app initiates a context change)
    ↓
VALIDATING (entity ID resolved, data availability confirmed)
    ↓
PUBLISHING (context.changed event sent to Context Bus)
    ↓
PROPAGATING (each micro-app evaluates its propagation mode)
    ↓
ACTIVE (all apps have responded — some updated, some prompted, some unchanged)
    ↓
SUPERSEDED (a new context is set — this context stored in history)
```

---

## Context History

The last 10 contexts set in each workspace are stored and accessible for quick re-selection. Context history is displayed in the workspace context bar as a dropdown.

```
GET /api/v1/workspace/{workspace_id}/context-history
  Response: { history: [{context_id, entity_name, set_at, set_by}] }
```

---

## Context Governance

**Who can set context:**
Any analyst who owns or has been granted access to the workspace. Context changes are logged to the Audit Log with analyst ID and timestamp.

**Context data retention:**
Active context is retained indefinitely per workspace. Context history (last 10) is retained per session. Full context audit log is retained per the firm's data retention policy.

**Context isolation:**
Context is workspace-scoped. An analyst's context change in Workspace A does not affect their Workspace B, and never affects another analyst's workspace.
