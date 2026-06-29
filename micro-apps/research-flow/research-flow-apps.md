# Research Flow Micro-Apps

---

## Corporate Events Calendar

### Purpose
The Corporate Events Calendar surfaces upcoming and recent corporate events relevant to the holdings in the active fund context — earnings announcements, dividend dates, corporate actions, rating events, and management meetings.

### Why Context Matters Here
Without context awareness, a corporate events calendar is just a firehose of events across the entire market. With context, it becomes a curated view of what matters for the analyst's active fund — only events for holdings in the portfolio, weighted by position size.

### Core Capabilities
- Calendar view of upcoming events filtered to active fund holdings
- Event types: earnings dates, dividend dates, ex-dividend dates, coupon payments, bond maturities, rating agency reviews, management guidance events, annual general meetings
- Position size weighting — larger holdings surface more prominently
- Click-through to create an assignment or note directly from a calendar event
- Historical events with linked research notes where they exist

### Context Behaviour
**Default mode: SHARED** — calendar automatically updates to show events for holdings in the active workspace context. The analyst can manually add events for securities outside the current portfolio.

### API Contract (Summary)
```
GET /api/v1/events
  Query params: entity_id, event_types[], date_from, date_to, holdings_only (boolean)
  Response: { events: [{event_id, event_type, security_id, security_name,
                        event_date, position_weight, linked_notes_count}] }

POST /api/v1/events/{event_id}/link-note
  Request: { note_id }
  Response: { updated event }
```

---

## Research Timeline

### Purpose
The Research Timeline provides a chronological history of all research activity on the active context — every note written, every model updated, every report published, every meeting held, and every corporate event that occurred — in a single unified view.

### The Problem It Solves
Understanding the history of a fund or security requires piecing together information from multiple sources: the notes browser, the assignment history, the events calendar, the published research store. The Research Timeline assembles this picture automatically in context.

### Core Capabilities
- Unified timeline: research notes, published reports, model updates, corporate events, meeting notes, assignment completions
- Filter by event type to focus on one category
- Zoom to any date range
- Click any timeline item to open the full content
- Identify research gaps — periods with no research activity on a fund are visually highlighted

### Context Behaviour
**Default mode: SHARED** — always shows the timeline for the active workspace context.

### API Contract (Summary)
```
GET /api/v1/timeline/{entity_id}
  Query params: start_date, end_date, event_types[], analyst_id (optional)
  Response: { events: [{date, event_type, title, author, entity_id, linked_content_id}] }
```

---

## Watchlist & Coverage Manager

### Purpose
The Watchlist & Coverage Manager is the analyst's personal tracking universe — funds and securities they are monitoring, covering, or watching for research opportunities. It is the map of the analyst's professional responsibility and interest.

### Core Capabilities
- Personal watchlists: named lists of funds and securities the analyst is monitoring
- Coverage list: the analyst's formally assigned coverage universe
- Alert configuration: price movement, earnings surprise, rating change, news volume alerts
- Coverage status: active, on-hold, under initiation, dropped
- Quick-switch: clicking any item in the watchlist sets it as the active workspace context instantly

### Context Behaviour
**Default mode: INDEPENDENT** — the Watchlist does not update based on workspace context. It is always showing the analyst's full monitoring universe, not filtered to any single fund.

**Context publishing:** Clicking a fund or security in the Watchlist publishes a context change event — making the Watchlist a navigation tool for the workspace. The analyst clicks a fund in their Watchlist and the entire workspace orients to that fund.

### API Contract (Summary)
```
GET /api/v1/watchlists
  Query params: analyst_id
  Response: { watchlists: [{id, name, entities: [{entity_id, entity_name, alert_config}]}] }

POST /api/v1/watchlists/{id}/entities
  Request: { entity_id, entity_type, alert_config }
  Response: { updated watchlist }

GET /api/v1/coverage
  Query params: analyst_id
  Response: { coverage: [{entity_id, entity_name, status, since_date, assigned_by}] }
```
