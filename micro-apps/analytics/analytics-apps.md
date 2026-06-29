# Analytics Micro-Apps

---

## Characteristics & Metrics Explorer

### Purpose
The Characteristics & Metrics Explorer surfaces the quantitative characteristics of the active fund context — asset class specific metrics that define what the fund owns and how it is positioned relative to history and peers.

### Why Asset Class Matters
Metrics are not universal. The right characteristics for a fixed income fund are completely different from those for an equity fund. This micro-app is configurable by asset class — the analyst selects which metrics matter for their coverage universe.

**Equity metrics:** P/E, Forward P/E, EV/EBITDA, Price/Book, Price/Sales, Earnings Growth, Dividend Yield, ROE, ROA, Debt/Equity

**Fixed income metrics:** Duration, Modified Duration, Yield to Maturity, Yield to Worst, Spread to Treasury, Credit Quality Distribution, Convexity, DV01

**Multi-asset metrics:** Asset Class Allocation, Factor Exposures, Correlation Matrix, Beta to Equity, Interest Rate Sensitivity

### Context Behaviour
**Default mode: SHARED** — updates to active workspace context automatically. Metric configuration (which metrics to show) is saved per analyst and persists across context changes.

### API Contract (Summary)
```
GET /api/v1/characteristics/{entity_id}
  Query params: asset_class, metrics[], as_of_date
  Response: { characteristics: [{metric_name, value, benchmark_value, percentile_rank}] }

GET /api/v1/characteristics/{entity_id}/history
  Query params: metric, start_date, end_date, frequency
  Response: { history: [{date, value, benchmark_value}] }
```

---

## Peer & Benchmark Comparator

### Purpose
The Peer Comparator enables side-by-side comparison of the active fund against selected peers, indices, and benchmarks. It is the primary tool for competitive positioning analysis and benchmark-relative assessment.

### Core Capabilities
- Select up to 5 comparison entities (peers, benchmarks, indices)
- Compare across any metric available in the Characteristics Explorer
- Returns comparison over configurable periods
- Risk-adjusted return comparison (Sharpe, Information Ratio, Sortino)
- Factor exposure comparison

### Context Behaviour
**Default mode: INDEPENDENT**

The Peer Comparator maintains its own comparison set independently of workspace context. The analyst explicitly selects the primary fund and comparators — these are not overwritten by workspace context changes. This is intentional: a comparison in progress should not be disrupted by the analyst checking another fund in a different app.

When the workspace context changes, the Comparator shows a non-intrusive prompt: *"Add [New Context Fund] to comparison?"*

### API Contract (Summary)
```
GET /api/v1/compare
  Query params: primary_entity_id, comparison_entity_ids[], metrics[], start_date, end_date
  Response: { comparison: [{entity_id, entity_name, metrics: {metric: value}, returns: {period: return}} ]}
```

---

## Risk Metrics Dashboard

### Purpose
The Risk Metrics Dashboard provides a comprehensive view of portfolio risk for the active fund context — quantitative risk measures that complement the returns analysis in the Returns Analyzer.

### Core Capabilities
- Volatility (realised and implied)
- Value at Risk (VaR) — parametric and historical, configurable confidence interval
- Tracking Error vs benchmark
- Sharpe Ratio, Sortino Ratio, Information Ratio
- Maximum Drawdown and drawdown duration
- Factor risk decomposition
- Tail risk metrics (CVaR, Expected Shortfall)

### Context Behaviour
**Default mode: SHARED** — updates automatically with workspace context.

**Context linking:** The Risk Metrics Dashboard supports context linking with the Returns Analyzer. When linked, date range changes in Returns Analyzer automatically propagate to Risk Metrics — ensuring the analyst is always comparing risk and return over the same period.

### API Contract (Summary)
```
GET /api/v1/risk/{entity_id}/metrics
  Query params: start_date, end_date, benchmark_id, var_confidence, var_method
  Response: { 
    volatility, tracking_error, sharpe_ratio, information_ratio,
    max_drawdown, var, cvar, beta
  }

GET /api/v1/risk/{entity_id}/factor-decomposition
  Query params: start_date, end_date, factor_model
  Response: { factors: [{factor_name, exposure, contribution_to_risk}] }
```
