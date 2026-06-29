# Architecture Overview

## System Layers

The Research Workspace Framework is organised across four layers that together deliver a personalised, context-aware research environment.

```
┌─────────────────────────────────────────────────────────────────┐
│                    WORKSPACE LAYER                              │
│    Analyst-specific workspace: chosen apps, layout, settings    │
│                                                                 │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐         │
│  │Assignment│ │  Notes   │ │Portfolio │ │ Returns  │  ...     │
│  │ Manager  │ │ Browser  │ │  View    │ │Analyzer  │         │
│  └────┬─────┘ └────┬─────┘ └────┬─────┘ └────┬─────┘         │
└───────┼────────────┼────────────┼────────────┼─────────────────┘
        │            │            │            │
┌───────▼────────────▼────────────▼────────────▼─────────────────┐
│                    CONTEXT BUS LAYER                            │
│         Research Context: Fund ID │ Asset Class │ Date Range    │
│         Context Mode: SHARED │ INDEPENDENT │ PROMPT            │
└──────────────────────────────────┬──────────────────────────────┘
                                   │
┌──────────────────────────────────▼──────────────────────────────┐
│                   MICRO-APP SERVICE LAYER                       │
│  Each micro-app: own database, own API, own deployment pipeline │
└──────────────────────────────────┬──────────────────────────────┘
                                   │
┌──────────────────────────────────▼──────────────────────────────┐
│                      DATA LAYER                                 │
│   Market Data │ Research Store │ Portfolio Data │ Audit Log     │
└─────────────────────────────────────────────────────────────────┘
```

---

## The Workspace Layer

The workspace is the analyst's personal environment. It is not a pre-configured dashboard — it is a composable surface where each analyst selects the micro-apps they need, arranges them to match their workflow, and configures each app independently.

**Workspace characteristics:**

- **User-owned:** Each analyst has one or more named workspaces. Workspaces are not shared by default.
- **Composable:** Any combination of micro-apps from the library can be added to a workspace.
- **Persistent:** The workspace remembers the last state of every micro-app — active context, scroll position, applied filters — across sessions.
- **Switchable:** Analysts can maintain multiple workspaces for different research tasks and switch between them without losing state.

---

## The Context Bus Layer

The Context Bus is the architectural innovation that connects micro-apps without coupling them. It is a lightweight publish-subscribe channel that carries the active research context — the fund, security, or benchmark the analyst is currently focused on.

**Context propagation modes — set per micro-app:**

| Mode | Behaviour |
|---|---|
| SHARED | App automatically updates when workspace context changes |
| PROMPT | App highlights that context has changed and waits for analyst to confirm update |
| INDEPENDENT | App maintains its own context regardless of workspace context changes |

This three-mode design is what allows genuinely flexible workspaces. An analyst can have their Portfolio View in SHARED mode (always showing the active fund), their Notes Browser in PROMPT mode (suggests updating but lets the analyst decide), and their Peer Comparator in INDEPENDENT mode (always showing two specific funds for comparison regardless of active context).

**Context schema:**

```json
{
  "context_id": "uuid",
  "workspace_id": "uuid",
  "analyst_id": "uuid",
  "primary_entity": {
    "entity_type": "FUND | SECURITY | SECTOR | BENCHMARK | INDEX",
    "entity_id": "string",
    "entity_name": "string",
    "asset_class": "EQUITY | FIXED_INCOME | MULTI_ASSET | ALTERNATIVES"
  },
  "date_range": {
    "start": "ISO8601 date",
    "end": "ISO8601 date"
  },
  "coverage_universe": "string or null",
  "set_at": "ISO8601 datetime",
  "set_by": "ANALYST | SYSTEM | LINKED_APP"
}
```

---

## The Micro-App Service Layer

Each micro-app is an independently deployed service with:

- Its own database — no shared schemas between micro-apps
- Its own API contract — versioned, documented, stable
- Its own deployment pipeline — updates deploy without affecting other apps
- Its own failure boundary — a failed micro-app does not cascade to others

Micro-apps communicate with the workspace through two channels:

1. **Context Bus subscription** — receiving context updates and responding per configured mode
2. **API contract calls** — when one micro-app needs data from another, it calls through the API contract, never directly to the database

---

## The Data Layer

**Market Data Store**
Real-time and historical market data consumed by analytics micro-apps. Not owned by any single micro-app — accessed through a shared data service with standard API contracts.

**Research Store**
Stores all research content: notes, published commentary, assignment records, research timelines. Owned collectively by the research flow micro-apps through their individual schemas.

**Portfolio Data**
Holdings, weights, transactions, and valuations. The Portfolio Context View micro-app owns the interface to this data — other micro-apps that need portfolio data request it through the Portfolio Context View API contract.

**Audit Log**
Append-only log of every research action: context changes, content published, assignments updated, AI inferences produced. Required for regulatory examination readiness and NIST AI RMF accountability.

---

## Workspace State Machine

Each workspace moves through the following states during an analyst session:

```
INITIALISING
     ↓
LOADING (micro-apps loading last known state)
     ↓
READY (analyst can interact)
     ↓
CONTEXT_UPDATING (analyst has set a new context)
     ↓        ↘
  SYNCING    PROMPTING (apps in PROMPT mode waiting for confirmation)
     ↓           ↓
  READY ← ANALYST_CONFIRMED
```

The PROMPTING state is important — it is the moment where the workspace asks the analyst to confirm whether certain apps should update to the new context. This respects analyst intent rather than automatically overwriting independent research in progress.
