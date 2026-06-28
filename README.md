# Research Workspace Framework

**An open-source framework for building context-aware, personalised research workspaces for investment analysts**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![NIST AI RMF](https://img.shields.io/badge/NIST%20AI%20RMF-Aligned-green)](https://airc.nist.gov/Home)

---

## The Core Idea

A research analyst opens their workspace. They are covering a technology fund. They type the fund name once — and every app in their workspace immediately has that context. Their assignment list shows tasks related to that fund. Their notes browser surfaces relevant prior research. Their portfolio view shows current holdings. Their returns analyzer shows performance attribution. Their corporate events calendar highlights upcoming earnings for the fund's top holdings.

One context entry. Everything connected.

This is the central design principle of the Research Workspace Framework: **context-aware micro-app architecture** where a shared research context flows through a personalised collection of focused tools — each independently useful, together forming a complete research environment.

---

## What Makes This Different

Traditional investment research platforms are monolithic. A single application tries to do everything — and does everything adequately, nothing exceptionally. Research analysts adapt their workflows to the platform's structure rather than the platform adapting to their workflow.

This framework inverts that model:

**Workspace first.** Each research analyst builds their own workspace — choosing which micro-apps they need, arranging them to match their workflow, and configuring each app to their preferences. A fixed-income analyst's workspace looks nothing like an equities analyst's workspace. Both are built from the same micro-app library.

**Context as the connective tissue.** When a fund or security is set as the active context, that context propagates through the workspace. Apps that are configured to share context update automatically. Apps that need independent context — an analyst comparing two funds side by side — can hold their own context independently. The workspace remembers each app's context when the analyst switches between views.

**Micro-apps, not modules.** Each tool in the workspace is a fully independent micro-app: its own data, its own deployment, its own API contract. An update to the Returns Analyzer does not affect the Notes Browser. A failure in the Corporate Events Calendar does not disrupt the Portfolio View.

---

## The Research Context Model

The fundamental unit of this framework is the **research context** — the fund, security, sector, or benchmark that an analyst is actively researching.

```
Research Context
    │
    ├── Fund ID / Security ID / Benchmark ID
    ├── Asset Class
    ├── Coverage Universe
    ├── Date Range
    └── Analyst ID (workspace owner)
```

When an analyst sets a research context, it is published to the **Context Bus** — a lightweight event stream that all micro-apps in the workspace subscribe to. Each micro-app decides independently how to respond to a new context:

- **Auto-update:** The app immediately refreshes its data for the new context
- **Prompt-update:** The app highlights that a new context is available and waits for analyst confirmation before updating
- **Independent:** The app maintains its own context and ignores the workspace context

This flexibility is what makes the workspace genuinely personalised rather than rigidly connected.

---

## Micro-App Library

### Core Research Apps
| Micro-App | Purpose |
|---|---|
| [Assignment Manager](micro-apps/core/assignment-manager.md) | View, manage, and track research assignments by fund or coverage |
| [Notes & Commentary Browser](micro-apps/core/notes-browser.md) | Search and surface prior research notes in active context |
| [Content Publisher](micro-apps/core/content-publisher.md) | Draft, review, and publish research content and commentary |
| [Portfolio Context View](micro-apps/core/portfolio-context-view.md) | Holdings, weights, sector allocation for active fund context |
| [Returns & Performance Analyzer](micro-apps/core/returns-analyzer.md) | Performance attribution, benchmark comparison, return decomposition |

### Analytics Apps
| Micro-App | Purpose |
|---|---|
| [Characteristics & Metrics Explorer](micro-apps/analytics/characteristics-metrics-explorer.md) | Asset class specific metrics — P/E, duration, yield, factor exposures |
| [Peer & Benchmark Comparator](micro-apps/analytics/peer-benchmark-comparator.md) | Side-by-side comparison against peers, indices, and benchmarks |
| [Risk Metrics Dashboard](micro-apps/analytics/risk-metrics-dashboard.md) | VaR, tracking error, Sharpe ratio, drawdown for active context |

### Research Flow Apps
| Micro-App | Purpose |
|---|---|
| [Corporate Events Calendar](micro-apps/research-flow/corporate-events-calendar.md) | Earnings, dividends, corporate actions for holdings in active context |
| [Research Timeline](micro-apps/research-flow/research-timeline.md) | Chronological history of all research published on active context |
| [Watchlist & Coverage Manager](micro-apps/research-flow/watchlist-coverage-manager.md) | Personal watchlists, coverage alerts, tracking universe |

---

## Repository Structure

```
research-workspace-framework/
├── docs/
│   ├── architecture-overview.md     # Workspace architecture and context bus
│   ├── workspace-design.md          # How to design and personalise a workspace
│   ├── context-propagation.md       # Context model and propagation patterns
│   ├── ai-integration.md            # AI governance in research micro-apps
│   └── use-cases.md                 # Analyst workflow scenarios
├── micro-apps/
│   ├── core/                        # Five core research micro-apps
│   ├── analytics/                   # Three analytics micro-apps
│   └── research-flow/               # Three research flow micro-apps
├── framework/
│   ├── context-model.md             # Context schema and lifecycle
│   ├── workspace-personalization.md # Workspace configuration patterns
│   └── api-contracts.md             # Inter-app API standards
└── examples/
    ├── equity-research-desk/        # Equity analyst workspace example
    └── multi-asset-team/            # Multi-asset team workspace example
```

---

## Who This Framework Is For

- **Asset managers** building or modernising internal research platforms
- **RIAs** wanting structured research tooling without enterprise platform cost
- **Fintech platforms** building research products for institutional clients
- **Quantitative research teams** looking for composable, context-aware analytics environments

---

## Validated Outcomes

The architectural patterns in this framework have been validated in production investment research environments:

- Approximately 40% reduction in platform latency through micro-app isolation and parallel data loading
- Approximately 34% increase in platform adoption among research analysts — attributed to workspace personalisation reducing tool-switching friction
- Approximately 156% increase in mobile adoption — enabled by selective micro-app delivery to mobile surfaces

---

## Contributing

Contributions from investment research practitioners, quantitative analysts, and platform engineers are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

---

## License

MIT License. See [LICENSE](LICENSE).

*This framework is derived from architectural patterns validated in production investment research environments. All proprietary, employer-specific, and confidential details have been removed. The framework represents generalised architectural knowledge for broad industry use.*

---

## Author

**Sujatha Gopalakrishnan Iyer**
Financial Technology Product Professional | AI Governance | Investment Platform Systems
