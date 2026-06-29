# Example: Multi-Asset Research Team

**Team:** 6 analysts covering equities, fixed income, and alternatives
**Challenge:** One platform must serve analysts with completely different metric requirements, coverage universes, and workflow patterns

---

## The Core Problem

A fixed income analyst and an equity analyst have almost nothing in common in their daily research workflow. P/E ratios are irrelevant to a credit analyst. Duration and DV01 are irrelevant to a growth equity analyst. A platform built for one fails the other.

---

## Asset Class Specific Workspace Configurations

### Fixed Income Analyst

```
Characteristics Explorer: Duration, YTM, YTW, Spread, Credit Quality
Corporate Events Calendar: Coupon dates, maturities, rating reviews
Risk Metrics: Interest rate sensitivity, spread duration, DV01
Returns Analyzer: Total return, excess return over duration-matched treasury
```

Context behaviour note: Fixed income analysts often monitor an issuer across multiple instruments. The Peer Comparator is configured in INDEPENDENT mode with specific bond issues pinned for comparison, while the workspace context moves between issuers.

### Alternatives Analyst

```
Characteristics Explorer: Custom metrics (IRR, TVPI, DPI, vintage year)
Research Timeline: Important for tracking long-dated investments
Notes Browser: Meeting notes with GP management teams
Assignment Manager: Due diligence tracking
```

The Alternatives analyst rarely uses the Returns Analyzer or Risk Metrics Dashboard in standard configurations — these are removed from their workspace. The modularity of the framework means removing unused apps is a configuration choice, not a platform limitation.

---

## Shared Team Context

The multi-asset team uses one additional feature: **team context broadcasting**.

When the research director sets a context at the team level — for example, during a Monday morning research meeting reviewing the portfolio — that context can be broadcast to all team members' workspaces simultaneously. Each analyst's workspace receives the broadcast as a PROMPT — they can accept it to follow the meeting discussion in their own workspace, or dismiss it to continue their own work.

This transforms the Research Workspace Framework from a personal tool into a collaborative research environment — without removing individual control.
