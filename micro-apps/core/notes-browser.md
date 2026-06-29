# Micro-App: Notes & Commentary Browser

## Purpose
The Notes Browser surfaces prior research — notes, commentary, model updates, meeting records — relevant to the active research context. It is the institutional memory of the research desk, made searchable and context-aware.

## Core Capabilities
- Full-text search across all research notes in the firm's research store
- Context-filtered view: notes related to active fund or security surface automatically
- Filter by author, date range, note type, and publication status
- Preview notes inline without leaving the workspace
- Link notes to active assignments

## Context Behaviour
**Default mode: PROMPT**

When the workspace context changes, the Notes Browser shows a banner: *"Context updated to [Fund Name] — show notes for this fund?"* The analyst confirms before the browser refreshes. This prevents accidental loss of a search in progress when context changes elsewhere in the workspace.

## Note Types Surfaced
- Research notes (analyst-authored, unpublished)
- Published commentary (approved, client-distributed)
- Meeting notes (earnings calls, management meetings)
- Model update notes (when a financial model is revised)
- Corporate action notes (event-specific research)

## API Contract (Summary)

```
GET /api/v1/notes
  Query params: entity_id, author_id, note_type, date_from, date_to, search_query
  Response: { notes: [{id, title, author, date, type, preview, entity}] }

GET /api/v1/notes/{id}
  Response: { full note content, metadata, linked_assignments }
```
