# Fund Research Workspace — A Concrete Implementation

This document describes a specific, ready-to-use workspace configuration built using the Research Workspace Framework — a four-tab environment designed for deep fund research.

---

## The Workspace

A fund research analyst opens this workspace with one goal: understand a fund deeply enough to write something meaningful about it. The workspace is designed around that single workflow from start to finish.

```
┌─────────────────────┬─────────────────────┐
│   Portfolio         │   Holdings          │
│   Overview          │   Detail            │
│   [SHARED]          │   [SHARED]          │
├─────────────────────┼─────────────────────┤
│   Notes             │   Research          │
│   Browser           │   Publisher         │
│   [PROMPT]          │   [INDEPENDENT]     │
└─────────────────────┴─────────────────────┘
```

Four tabs. One fund. Everything connected.

---

## How It Works

### Step 1 — Enter a Fund

The analyst types a fund name, model portfolio, or benchmark into the workspace context bar at the top. That single entry becomes the context that flows through the workspace.

```
Analyst types: "Global Technology Growth Fund"
        ↓
Context published to all tabs
        ↓
Portfolio Overview → loads fund summary, AUM, strategy, key facts
Holdings Detail   → loads current holdings, weights, sector allocation
Notes Browser     → prompts: "Show notes for Global Technology Growth Fund?"
Publisher         → stays on current draft (INDEPENDENT)
```

### Step 2 — Review the Fund

The analyst reviews the Portfolio Overview — strategy, mandate, benchmark, key characteristics — alongside the Holdings Detail showing current positions, weights, and sector breakdowns. Both tabs are in SHARED mode and update simultaneously when the fund context is set.

### Step 3 — Surface Prior Research

The Notes Browser prompts the analyst to load relevant prior research. One click surfaces all previous notes, commentary, and meeting records related to the active fund — giving the analyst the institutional context before they write anything new.

### Step 4 — Write and Publish Findings

The Research Publisher opens as a draft editor. The analyst writes their findings — a note, a commentary, a model update, a meeting summary. The Publisher is in INDEPENDENT mode — it will not update or discard the draft if the analyst briefly changes the workspace context to check another fund.

When the draft is complete, the analyst publishes directly from the workspace. The published note is stored in the research store and immediately surfaces in the Notes Browser for the relevant fund — closing the loop.

---

## Saving This as a Named Workspace

After setting up this configuration, the analyst saves it as a named workspace:

**Workspace name:** "Fund Deep Dive"

From this point, opening "Fund Deep Dive" restores:
- The four-tab layout exactly as configured
- Each tab's propagation mode (SHARED / PROMPT / INDEPENDENT)
- The last active fund context
- Any in-progress draft in the Publisher

The analyst never rebuilds this setup again — they open the workspace, type a fund name, and the environment is ready.

---

## Context Passing Between Tabs

The context passing in this workspace follows a specific pattern worth documenting:

```
CONTEXT SOURCE          PROPAGATES TO           MODE
─────────────────────────────────────────────────────
Workspace Context Bar → Portfolio Overview      SHARED (auto)
Workspace Context Bar → Holdings Detail         SHARED (auto)
Workspace Context Bar → Notes Browser           PROMPT (confirm)
Workspace Context Bar → Research Publisher      INDEPENDENT (ignore)

Holdings Detail       → Workspace Context Bar   When analyst clicks a holding
                      → Portfolio Overview      AUTO via workspace context
                      → Notes Browser           PROMPT via workspace context

Notes Browser         → Research Publisher      When analyst clicks "Start note on this fund"
                                               Publisher opens with fund pre-filled
```

The last pattern is particularly useful: an analyst reading a prior note on a fund can click "Write follow-up" and the Publisher opens pre-filled with that fund's context — no manual entry, no risk of writing a note linked to the wrong fund.

---

## Why This Workspace Pattern Matters

The fund research workflow has a consistent shape across investment management firms:
1. Orient to the fund (what does it own, how has it performed)
2. Review what has already been written (avoid duplicating prior analysis)
3. Add something new (a note, a view, a finding)
4. Share it with the team or publish it

Most research platforms require the analyst to navigate between four separate tools to complete this workflow. Switching between tools breaks concentration, introduces the risk of losing context, and wastes time that should be spent on analysis.

This workspace completes the full workflow without the analyst leaving a single environment.

---

## Planned Additions to This Workspace

The following tabs are in development for inclusion in this workspace template:

- **Returns & Attribution tab** — performance context alongside holdings
- **Corporate Events tab** — upcoming earnings and events for holdings in context
- **Peer Comparison tab** — benchmark the active fund against selected peers

Contributions and suggestions for additional tabs welcome — see [CONTRIBUTING.md](../../CONTRIBUTING.md).
