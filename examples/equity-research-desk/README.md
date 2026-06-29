# Example: Equity Research Desk

**Team:** 8 equity research analysts, 2 sector heads, 1 research director
**Coverage:** Large-cap US equities, technology and healthcare sectors
**Goal:** Replace a monolithic research platform with a workspace framework that each analyst can configure for their specific coverage and workflow

---

## The Problem With the Old Platform

The team's existing platform was configured once by IT and deployed to all analysts identically. An analyst covering 5 technology mega-caps had the same interface as an analyst covering 30 mid-cap healthcare companies. Corporate events calendars showed the entire market, not just their coverage. Notes searches returned all research, not research on their stocks. Every analyst had learned to mentally filter out 90% of what the platform showed them.

---

## Workspace Configuration by Role

### Equity Analyst — Standard Workspace

```
┌─────────────────────┬───────────────────────┐
│  Assignment Manager │  Corporate Events Cal │
│  [SHARED]           │  [SHARED]             │
├─────────────────────┼───────────────────────┤
│  Portfolio Context  │  Returns & Perf.      │
│  View [SHARED]      │  Analyzer [SHARED]    │
├─────────────────────┴───────────────────────┤
│  Characteristics & Metrics Explorer         │
│  (Equity config: P/E, EV/EBITDA, Growth)    │
│  [SHARED]                                   │
└─────────────────────────────────────────────┘
Content Publisher available as separate workspace
```

### Senior Analyst / Sector Head — Comparative Workspace

```
┌──────────────────────────────────────────────┐
│  Peer & Benchmark Comparator [INDEPENDENT]   │
│  (Pinned: Tech sector fund vs. S&P 500)      │
├───────────────────┬──────────────────────────┤
│  Risk Metrics     │  Returns Analyzer        │
│  Dashboard        │  [SHARED]                │
│  [CONTEXT LINKED] │                          │
├───────────────────┴──────────────────────────┤
│  Notes Browser [PROMPT]                      │
└──────────────────────────────────────────────┘
```

### Research Associate — Output-Focused Workspace

```
┌──────────────────────┬───────────────────────┐
│  Assignment Manager  │  Research Timeline    │
│  [SHARED]            │  [SHARED]             │
├──────────────────────┴───────────────────────┤
│  Content Publisher [INDEPENDENT]             │
│  (Always writing — context stays on draft)   │
├───────────────────────────────────────────────┤
│  Notes Browser [PROMPT]                       │
└───────────────────────────────────────────────┘
```

---

## Context Propagation in Practice

**Morning workflow example:**

1. Analyst opens workspace — last active context loads (Microsoft Fund, as of yesterday)
2. Analyst checks Assignment Manager — sees "Earnings note due: AAPL — 2 days"
3. Analyst clicks the assignment → Assignment Manager publishes context change to AAPL
4. Portfolio View, Characteristics Explorer, Corporate Events Calendar update to AAPL
5. Notes Browser prompts: "Context updated to AAPL — refresh notes?"
6. Analyst confirms — Notes Browser shows all prior AAPL research
7. Analyst opens Content Publisher in separate workspace — begins drafting earnings note

The analyst navigated from a fund to a specific security and oriented their entire workspace with one click — without re-entering data in five separate apps.

---

## Observed Outcomes

- **Tool-switching friction:** Significantly reduced — analysts report spending more time on analysis and less on navigating between tools
- **Research gap identification:** Research Timeline highlighted funds with no published notes in over 6 months — 3 coverage gaps identified in first week
- **Morning briefing preparation:** Analysts report faster morning setup — workspace loads previous session state, context history allows quick navigation to the day's priority names
- **New analyst onboarding:** Research Associate template provided a sensible starting workspace; analysts customised within 2 weeks to match their specific workflow
