# Micro-App: Content Publisher

## Purpose
The Content Publisher is where analysts draft, review, and publish research content — notes, commentary, client-facing reports, and model updates. It is the output end of the research workflow, connected to the research store and the firm's publishing and distribution systems.

## Core Capabilities
- Rich text editor for research drafting
- Draft autosave — no work is lost if the session ends
- Structured templates for common content types (earnings note, initiation report, model update, sector commentary)
- Review and approval workflow integration
- Publish to research store and optionally to downstream distribution systems
- Link published content to active assignments — auto-closes the assignment on publication

## Context Behaviour
**Default mode: INDEPENDENT**

The Content Publisher maintains its own context — the fund or security the analyst is writing about. This is intentional: an analyst writing a note on Fund A should not have their draft context overwritten if they briefly check Fund B in another app. The Publisher always shows which entity the draft is linked to and alerts if the workspace context differs.

## Draft Schema
```json
{
  "draft_id": "uuid",
  "analyst_id": "uuid",
  "entity_id": "string",
  "content_type": "EARNINGS_NOTE | INITIATION | MODEL_UPDATE | SECTOR_COMMENTARY | MEETING_NOTE",
  "title": "string",
  "body": "rich text",
  "status": "DRAFT | UNDER_REVIEW | APPROVED | PUBLISHED",
  "linked_assignment_id": "uuid or null",
  "created_at": "ISO8601",
  "last_saved": "ISO8601"
}
```

---

# Micro-App: Portfolio Context View

## Purpose
The Portfolio Context View shows the current state of the active fund — holdings, weights, sector and geographic allocation, cash position, and key portfolio characteristics. It is the foundational reference view that orients all other research in the workspace.

## Core Capabilities
- Full holdings list with position weights and market values
- Sector, geography, and asset class allocation breakdowns
- Top and bottom contributors (configurable period)
- Portfolio-level characteristics (P/E, duration, yield depending on asset class)
- Comparison to benchmark weights with active weight highlighting

## Context Behaviour
**Default mode: SHARED**

The Portfolio Context View is the most natural SHARED mode app — it always shows the active workspace context. When context changes, it is the first app to update, anchoring the workspace's visual orientation to the new fund.

## API Contract (Summary)
```
GET /api/v1/portfolio/{entity_id}/holdings
  Query params: as_of_date
  Response: { holdings: [{security_id, name, weight, market_value, sector, geography}] }

GET /api/v1/portfolio/{entity_id}/allocation
  Query params: dimension (SECTOR | GEOGRAPHY | ASSET_CLASS), as_of_date
  Response: { allocations: [{category, weight, benchmark_weight, active_weight}] }
```

---

# Micro-App: Returns & Performance Analyzer

## Purpose
The Returns Analyzer provides performance attribution and return decomposition for the active fund context — showing what drove returns over a selected period, how the fund performed against its benchmark, and where alpha was generated or lost.

## Core Capabilities
- Total return over configurable date ranges
- Attribution by sector, security, factor
- Benchmark comparison with active return decomposition
- Contribution to return by position
- Rolling return windows (1M, 3M, 6M, YTD, 1Y, 3Y, 5Y)
- Drawdown analysis and recovery periods

## Context Behaviour
**Default mode: SHARED**

Updates automatically when workspace context changes. Date range is retained across context changes — an analyst comparing two funds over the same period does not need to re-enter dates.

## API Contract (Summary)
```
GET /api/v1/performance/{entity_id}/returns
  Query params: start_date, end_date, benchmark_id, frequency (DAILY | MONTHLY)
  Response: { returns: [{date, fund_return, benchmark_return, active_return}] }

GET /api/v1/performance/{entity_id}/attribution
  Query params: start_date, end_date, dimension (SECTOR | SECURITY | FACTOR)
  Response: { attribution: [{category, allocation_effect, selection_effect, total}] }
```
