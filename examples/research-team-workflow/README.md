# Example: Research Team Collaboration Workflow

This example shows the Team Collaboration Layer in action for a seven-person equity research team covering large-cap US technology stocks.

---

## Team Structure

| Name | Tier | Coverage |
|---|---|---|
| Research Director | Director | Full sector oversight |
| Sector Head — Technology | Senior | Large-cap tech mega-caps |
| Senior Analyst — Semiconductors | Senior | Semiconductor sub-sector |
| Analyst A | Analyst | Cloud and SaaS |
| Analyst B | Analyst | Consumer technology |
| Research Associate A | Associate | Support for Analyst A |
| Research Associate B | Associate | Support for Analyst B |

---

## Monday Morning Research Meeting — Workflow

**8:45 AM — Director opens the weekly priority fund**

The Director types "Global Technology ETF" into the workspace context bar and broadcasts to the team.

Every team member's workspace shows a banner: *"[Director] has broadcast: Global Technology ETF — follow?"*

Six of seven team members accept. Analyst B dismisses — they are finishing a time-sensitive earnings note from Friday and cannot context-switch yet.

**8:47 AM — Team is aligned**

Six workspaces now show the same fund context simultaneously. The Director begins presenting — each analyst can follow along in their own workspace, zooming into the micro-apps most relevant to their coverage sub-sector.

The Sector Head navigates to the Peer Comparator in INDEPENDENT mode to pull up the fund alongside the benchmark. This does not affect the team context — their comparator is pinned independently while the shared workspace context remains on the fund.

**9:15 AM — Director assigns new research**

The Director creates three assignments directly from the Assignment Manager:
- Semiconductor sub-sector update → Senior Analyst — Semiconductors, due Thursday
- Cloud exposure analysis → Analyst A, due Wednesday
- Consumer tech holdings review → Analyst B, due Friday

All three analysts receive notification. Their Assignment Managers update automatically — the new assignments appear at the top of their queues.

---

## Earnings Event — Real-Time Collaboration

**Tuesday 4:05 PM — A major holding reports earnings**

The earnings surprise is significant. The Sector Head broadcasts the security context to the team immediately.

**4:06 PM — Team response**

Analyst A and Research Associate A both accept the broadcast context. Their workspaces orient to the security simultaneously.

Research Associate A begins drafting an earnings note. Analyst A reviews the Returns Analyzer and Risk Metrics simultaneously. Neither is writing over the other's work — they are operating in parallel in their own workspaces, both with the same context.

**4:45 PM — Research Associate A submits draft**

Research Associate A submits the earnings note for review with EARNINGS priority — 4 hour SLA.

Analyst A receives notification and opens the draft in their Notes Browser. They approve with minor edits. The note moves to APPROVED status.

**5:10 PM — Publication**

Analyst A publishes the note. All seven team members receive notification: *"New research published: [Security Name] earnings — [Analyst A and Research Associate A]"*

Every team member's Notes Browser, when in that security context, now surfaces the note automatically.

---

## Junior Analyst Development — Review Workflow

Research Associate B has been at the firm for three months. Their drafts go through a structured two-stage review: Analyst B reviews first, then the Sector Head approves before publication.

**The workflow:**

```
Research Associate B drafts note
        ↓
Submits to Analyst B (Stage 1 review, 48hr SLA)
        ↓
Analyst B returns with comments (twice in first month)
        ↓
Research Associate B revises
        ↓
Analyst B approves → escalates to Sector Head (Stage 2, 24hr SLA)
        ↓
Sector Head approves → Published to team
```

The Audit Log captures every step. At the monthly team review, the Director can see Research Associate B's revision cycle improving — average revisions before approval dropping from 2.3 to 1.1 over three months.

This is not just operational workflow documentation. It is institutional research quality management made visible.

---

## Team Template — Earnings Season

During earnings season (four weeks per quarter), the Director deploys the Earnings Season Template to all team members.

**Template configuration:**
- Corporate Events Calendar: full width, prominent, filtered to holdings in the team's coverage universe
- Returns Analyzer: pre-set to 1-quarter comparison vs benchmark
- Notes Browser: filtered to earnings-related notes from the last 12 months
- Assignment Manager: filtered to earnings-related assignments
- Content Publisher: available but minimised — prioritise reading over writing during peak

**Deployment:**
The Director creates the template on the Sunday before earnings season begins. All team members receive a Sunday evening notification: *"Earnings Season Template available — apply to your workspace?"*

Five of seven team members apply it directly. Two create it as a new named workspace alongside their standard configuration so they can switch between earnings-mode and standard-mode easily.

---

## Measuring Team Collaboration Health

Using the Workspace Analytics layer, the Director tracks these team metrics monthly:

| Metric | Target | Current |
|---|---|---|
| Context broadcast acceptance rate | > 80% | 87% |
| Average review SLA compliance | > 90% | 94% |
| Team notes published per analyst per month | > 4 | 5.2 |
| Average revisions before approval (Associates) | Decreasing | 1.4 (down from 2.8) |
| Notes surfaced via collaboration vs. direct search | > 30% | 38% |

The last metric — notes surfaced via the collaboration layer rather than direct search — is the proxy for whether the team knowledge-sharing is actually working. If analysts are finding relevant team research automatically through context propagation rather than having to know to search for it, the collaboration architecture is doing its job.
