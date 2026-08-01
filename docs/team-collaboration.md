# Team Collaboration Layer

The Research Workspace Framework was initially designed for individual analysts building personal workspaces. This document extends the framework to teams — defining how multiple analysts share context, collaborate on research, and manage a structured publishing workflow within the same workspace ecosystem.

---

## Why Team Collaboration Needs Its Own Design

Individual workspaces work because the analyst has full control: they set the context, configure the apps, decide what updates and what stays independent. Team collaboration introduces tension with that model.

When a research director broadcasts a fund context during a morning meeting, every team member's workspace should be able to follow — but nobody's in-progress draft should be silently overwritten. When a junior analyst publishes a research note, it should be visible to the team — but not before a senior analyst has reviewed it. When two analysts are covering the same fund simultaneously, their notes should be visible to each other — but clearly attributed so nobody mistakes one person's view for another's.

The Team Collaboration Layer solves these tensions explicitly rather than leaving them to convention.

---

## Team Structure and Permission Tiers

Every team in the framework has a defined structure with four permission tiers:

| Tier | Role | Key Capabilities |
|---|---|---|
| **Director** | Research Director / Head of Research | Broadcast context to team, deploy team templates, approve publishing, manage team membership |
| **Senior** | Senior Analyst / Sector Head | Review and approve junior content, create shared watchlists, broadcast context within sector |
| **Analyst** | Research Analyst | Publish to team notes, create personal watchlists, submit drafts for review |
| **Associate** | Research Associate / Junior Analyst | Draft research, submit for review, cannot publish independently |

Permission tiers are assigned per team, not per user globally. An analyst may be a Senior on one team and an Analyst on another.

---

## Context Broadcasting

Context broadcasting extends the individual workspace context model to the team level. A Director or Senior analyst can set a context and broadcast it to all team members simultaneously.

### How It Works

```
Director sets context: "Global Technology Fund"
Director clicks: Broadcast to team
        ↓
Context Bus publishes: team.context.broadcast event
        ↓
Each team member's workspace receives the broadcast
        ↓
Each member sees a banner:
"[Director Name] has broadcast: Global Technology Fund — follow?"
        ↓
Member accepts → workspace updates to Global Technology Fund
Member dismisses → workspace stays on current context
```

The broadcast is always a **PROMPT**, never a forced update. No team member's active research is disrupted without their consent. A team member writing a note on a different fund when the broadcast arrives can finish their draft before accepting the team context.

### Broadcast Event Schema

```json
{
  "event_type": "team.context.broadcast",
  "broadcast_id": "uuid",
  "team_id": "uuid",
  "broadcast_by": {
    "analyst_id": "uuid",
    "name": "string",
    "tier": "DIRECTOR | SENIOR"
  },
  "context": {
    "entity_type": "FUND | SECURITY | SECTOR | BENCHMARK",
    "entity_id": "string",
    "entity_name": "string",
    "asset_class": "string",
    "date_range_start": "ISO8601 date",
    "date_range_end": "ISO8601 date"
  },
  "message": "string or null (optional note from broadcaster)",
  "expires_at": "ISO8601 datetime (broadcast expires after 4 hours by default)",
  "timestamp": "ISO8601 datetime"
}
```

### Use Cases for Context Broadcasting

**Morning research meeting:**
The director opens the week's priority fund at the start of the meeting. One broadcast orients the entire team's workspace simultaneously — everyone is looking at the same context while the director presents.

**Earnings event:**
A company in the coverage universe reports earnings. A senior analyst broadcasts the security context immediately — all team members can pull up their relevant apps without re-entering the ticker.

**Sector rotation discussion:**
The team is discussing a shift from technology to healthcare. The director broadcasts each sector in turn — team members follow the discussion in their workspaces in real time.

---

## Shared Research Notes

Within the individual workspace framework, notes are private by default. The Team Collaboration Layer adds a visibility dimension to every note.

### Note Visibility Levels

| Level | Visibility | Use Case |
|---|---|---|
| **Private** | Author only | Working notes, rough analysis, personal reference |
| **Team** | All team members | Completed research ready for team review |
| **Published** | Approved, full distribution | Final research distributed to stakeholders |

Analysts set visibility when saving a note. The default is configurable per team — some teams default to Team visibility to encourage sharing; others default to Private to protect work-in-progress.

### Attribution Rules

Team-visible notes always display:
- Author name and tier
- Date and time written
- Fund or security context it was written in
- Current status (Draft / Under Review / Published)
- Edit history — if a note was revised, the revision history is visible

**No anonymous contributions.** Every note is attributed. Every edit is logged. This is both a governance requirement and a team culture decision — research quality is tied to authorship accountability.

### Surfacing Team Notes in the Workspace

When a team member opens the Notes Browser in their workspace, they see:

1. Their own notes (all visibility levels)
2. Team-visible notes from colleagues, filtered to the active context
3. Published notes from the full team

Team notes from colleagues appear with a visual distinction — a different colour indicator or a "Team" badge — so analysts always know whether they are reading their own analysis or a colleague's.

---

## Collaborative Publishing Workflow

The publishing workflow defines how research moves from draft to team visibility to full publication — with review gates that match the team's permission structure.

### Standard Publishing Flow

```
ASSOCIATE / ANALYST drafts a note
        ↓
Submits for review → assigned to designated reviewer
        ↓
Reviewer receives notification: "New draft awaiting review: [Fund Name] — [Author]"
        ↓
Reviewer opens draft in their workspace
        ↓
REVIEWER DECISION:
  ┌────────────────┬─────────────────┬──────────────────┐
  │ APPROVE        │ RETURN          │ PUBLISH DIRECTLY │
  │                │                 │ (Senior/Director)│
  └───────┬────────┴────────┬────────┴────────┬─────────┘
          │                 │                  │
    Status → APPROVED  Returns to author  Status → PUBLISHED
    Author notified    with comments      Team notified
    Can publish        Revision cycle     Full distribution
```

### Review SLAs

Teams configure review SLAs that are tracked and reported in the Workspace Analytics layer:

| Draft Priority | Review SLA |
|---|---|
| Standard | 48 hours |
| Earnings / Time-Sensitive | 4 hours |
| Director-Requested | 2 hours |

SLA breaches generate escalation notifications — first to the reviewer, then to the team director if unresolved.

### Publishing Workflow API

```
POST /api/v1/notes/{note_id}/submit-for-review
  Request: { reviewer_id, priority, deadline }
  Response: { review_id, status: PENDING_REVIEW, sla_due_by }

POST /api/v1/reviews/{review_id}/decision
  Request: { decision: APPROVE | RETURN | PUBLISH, comments }
  Response: { updated note status, notification_ids }

GET /api/v1/team/{team_id}/review-queue
  Response: { pending_reviews: [{note_id, author, fund_context, submitted_at, sla_due_by, priority}] }
```

---

## Team Workspace Templates

Directors can create named workspace templates and deploy them to team members as a starting configuration.

### Template Types

**Morning Review Template:**
Pre-configured for the team's daily standup — Assignment Manager, Corporate Events Calendar, Research Timeline, top team notes. Deployed to all team members at the start of each week.

**Earnings Season Template:**
During earnings season, every analyst needs quick access to the same set of tools. The director deploys an earnings-focused template — Corporate Events Calendar prominent, Returns Analyzer pre-set to 1-quarter view, Notes Browser filtered to earnings-related notes.

**Deep Dive Template:**
For a specific fund under intensive coverage. Pre-configured with the fund context set, all analytics apps in SHARED mode, Content Publisher open.

### Template Deployment

```
Director creates template → names it, configures apps and layouts
Director deploys to team → members receive notification
Members receive template as suggestion, not forced installation
Members can accept template (replaces current workspace) or
  save as new named workspace (keeps current + adds template)
```

---

## Team Notifications

The Team Collaboration Layer introduces a notification system that connects team activity to individual workspaces.

**Notifications an analyst receives:**
- A colleague has published a note on a fund in your coverage universe
- Your draft has been approved or returned for revision
- A new assignment has been created in your name
- A team broadcast context has been set
- A review in your queue is approaching SLA

**Notification delivery:**
- In-workspace notification centre (all notifications)
- Email digest (configurable — immediate, hourly, or daily summary)
- Mobile push (configurable per notification type)

Notifications link directly to the relevant content — clicking a "colleague published a note" notification opens that note in the analyst's Notes Browser in the correct fund context.

---

## Governance and Audit

All team collaboration actions are logged to the shared Audit Log:

| Action | Logged Fields |
|---|---|
| Context broadcast | Broadcaster, team, context, acceptance rate |
| Note visibility change | Author, previous visibility, new visibility, timestamp |
| Review submission | Submitter, reviewer assigned, priority, deadline |
| Review decision | Reviewer, decision, comments, timestamp |
| Template deployment | Director, template name, team members notified |
| Note publication | Author, reviewer, publication timestamp, distribution list |

This audit trail supports two purposes: regulatory examination readiness (who published what research and when, and who approved it) and team performance management (review queue throughput, SLA compliance, research output by analyst).
