# Workspace Design

This document covers how research workspaces are designed, configured, and personalised by individual analysts.

---

## The Workspace as a Personal Tool

The workspace is not a shared dashboard configured by IT and deployed to all users. It is a personal research environment — as individual as the analyst's coverage universe and workflow.

Two analysts at the same firm covering different asset classes will have fundamentally different workspaces:

**Fixed Income Analyst workspace:**
- Characteristics & Metrics Explorer (configured for duration, yield, credit metrics)
- Corporate Events Calendar (bond maturities, coupon dates, rating events)
- Risk Metrics Dashboard (interest rate sensitivity, spread duration, DV01)
- Notes Browser
- Returns Analyzer

**Equity Analyst workspace:**
- Characteristics & Metrics Explorer (configured for P/E, EV/EBITDA, earnings growth)
- Corporate Events Calendar (earnings dates, dividend dates, guidance events)
- Assignment Manager (coverage list, model update schedule)
- Peer & Benchmark Comparator
- Content Publisher

Same micro-app library. Completely different workspaces.

---

## Workspace Configuration

Each workspace has a configuration that defines:

```json
{
  "workspace_id": "uuid",
  "analyst_id": "uuid",
  "workspace_name": "string",
  "asset_class_focus": "EQUITY | FIXED_INCOME | MULTI_ASSET | ALTERNATIVES | ALL",
  "micro_apps": [
    {
      "app_id": "assignment-manager",
      "position": {"row": 1, "col": 1, "width": 2, "height": 1},
      "context_mode": "SHARED | PROMPT | INDEPENDENT",
      "app_config": {}
    }
  ],
  "default_date_range_days": 90,
  "coverage_universe_filter": "string or null",
  "created_at": "ISO8601 datetime",
  "last_modified": "ISO8601 datetime"
}
```

---

## Layout Principles

Workspaces use a grid layout where micro-apps occupy cells. Each micro-app has a minimum and maximum size — small apps (Assignment Manager, Watchlist) work well in a 1x1 grid cell; analytics apps (Returns Analyzer, Risk Metrics Dashboard) need more space and work best at 2x2 or wider.

**Recommended layout patterns:**

**Research Morning Workspace** — used at session start:
```
┌────────────────┬────────────────┐
│  Assignment    │  Corporate     │
│  Manager       │  Events Cal.   │
├────────────────┼────────────────┤
│  Research      │  Notes         │
│  Timeline      │  Browser       │
└────────────────┴────────────────┘
```

**Deep Analysis Workspace** — used for active fund research:
```
┌──────────────────────┬───────────┐
│   Portfolio Context  │  Returns  │
│   View               │  Analyzer │
├──────────────────────┴───────────┤
│   Characteristics & Metrics      │
│   Explorer                       │
├──────────────┬───────────────────┤
│  Notes       │  Risk Metrics     │
│  Browser     │  Dashboard        │
└──────────────┴───────────────────┘
```

**Publishing Workspace** — used when writing research:
```
┌────────────────────────────────┐
│   Content Publisher            │
│   (full width)                 │
├────────────────┬───────────────┤
│  Notes Browser │  Research     │
│                │  Timeline     │
└────────────────┴───────────────┘
```

---

## Multiple Workspaces Per Analyst

Analysts can maintain multiple named workspaces and switch between them:

- **Morning Review** — assignment manager, calendar, timeline
- **Deep Dive** — analytics, portfolio view, risk metrics
- **Publishing** — content publisher, notes browser, research timeline
- **Coverage Compare** — peer comparator, characteristics explorer, returns analyzer

Switching between workspaces is instant — the previous workspace state is preserved in full, including each app's active context and any in-progress content in the Publisher.

---

## Workspace Templates

Firms can define workspace templates for common analyst roles — a starting point that analysts then personalise:

| Template | Intended Role | Pre-configured Apps |
|---|---|---|
| Equity Research | Equity analyst | Assignment Manager, Portfolio View, Characteristics Explorer, Peer Comparator, Content Publisher |
| Fixed Income | Credit analyst | Characteristics Explorer (FI config), Risk Metrics, Corporate Events, Returns Analyzer |
| Portfolio Manager | PM overseeing multiple funds | Portfolio View, Risk Metrics, Returns Analyzer, Watchlist Manager |
| Research Associate | Junior analyst | Assignment Manager, Notes Browser, Research Timeline, Content Publisher |

Templates are starting points only — analysts modify them freely after initial setup.
