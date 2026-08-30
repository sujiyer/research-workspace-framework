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

# Multi-Source AI Synthesis: Uses in Investment, Banking, and Fintech

The AI Research Synthesis pattern is not limited to investment research. Its underlying setup — several different input types, a tailored prompt, grounded RAG-based synthesis, citations, and human review — works in any setting where professionals need to combine complex evidence before making, or advising on, an important decision.

This document applies the framework to three financial services areas and lays out the domain-specific input sources, prompt styles, and output formats for each.

---

## Domain 1: Investment Research

**Who uses it:** Research analysts, portfolio managers, investment associates

**The core workflow:**
An analyst is putting together a fund review. On hand are a pitchbook from a management presentation, an earnings transcript from the previous quarter, their own OneNote notes from a management meeting, a colleague’s published sector commentary, and a focused question about the fund’s technology concentration.

**Input sources in this domain:**
- Pitchbooks and investor presentations (PDF, PPTX)
- Annual reports and 10-K, 10-Q filings
- Earnings call transcripts
- Analyst meeting notes (OneNote, Drive)
- Published research notes from the team
- Live portfolio data: holdings, weights, returns, risk metrics
- Corporate events calendar data

**Effective prompt examples:**
- "What is the fund's current exposure to interest rate risk and how has the manager described their approach to managing it across the last three earnings calls?"
- "Based on the uploaded pitchbook and my meeting notes, draft an investment thesis and identify the top three risks to the thesis."
- "Compare the fund's stated ESG commitments in the pitchbook against the actual portfolio holdings. Flag any inconsistencies."

**Output use:**
The synthesis draft is sent to the Content Publisher for analyst review. Once approved, it becomes a published research note.

---

## Domain 2: Banking — Credit and Lending Analysis

**Who uses it:** Loan officers, credit analysts, underwriters, relationship managers

**The core workflow:**
A commercial lending officer is assessing a mid-size business loan application. They have the business’s audited financials covering the last three years, a management-prepared business plan, the borrower’s bank statements from six months of the institution’s own records, a credit bureau report, and a property appraisal for the collateral. They need a credit memo that turns all of this into a structured credit recommendation.

**Input sources in this domain:**
- Audited financial statements (PDF, XLSX)
- Business plans and projections
- Bank statements (internal system data or uploaded PDFs)
- Credit bureau reports
- Property appraisals and valuation reports
- Industry reports and benchmarks
- Prior relationship notes from the CRM
- Regulatory compliance checks (KYC, sanctions screening results)

**Effective prompt examples:**
- "Based on the uploaded financials and bank statements, assess the borrower's debt service coverage ratio trend over the past three years and flag any periods of stress."
- "Compare the business plan projections against the historical financial performance. Identify where management is being optimistic versus conservative."
- "Summarise the key credit risks across all provided documents and suggest conditions that would mitigate each risk."
- "Draft a credit memo executive summary recommending approval or decline with the top three supporting reasons."

**Output use:**
The credit memo draft enters the loan officer’s review process. After compliance and credit officer approval, it becomes the official credit recommendation.

**Domain-specific governance:**
AI-assisted synthesis used in credit decisions is governed by ECOA and fair lending requirements. The synthesis output must not mention, or indirectly stand in for, protected class characteristics. Before any decision is shared with the borrower, every credit recommendation produced by this tool must be reviewed by a qualified credit officer.

---

## Domain 3: Fintech — Merchant Onboarding and Risk Assessment

**Who uses it:** Risk analysts, compliance officers, merchant onboarding teams

**The core workflow:**
A payments platform is bringing on a new merchant. Available materials include the merchant’s business registration documents, a website review, six months of processing history from a previous processor, the merchant’s bank statements, a fraud risk score from the platform’s risk engine, and notes from a sales call about the merchant’s business model. The risk analyst needs a structured onboarding recommendation.

**Input sources in this domain:**
- Business registration documents (PDF)
- Website and business model review notes
- Prior processor statements (PDF, XLSX)
- Bank statements
- KYC verification results
- Fraud and risk score outputs from the risk engine
- Sales call notes (OneNote, CRM)
- Industry chargeback benchmark data
- Watchlist screening results

**Effective prompt examples:**
- "Based on the processing history and bank statements, assess whether the merchant's transaction volume and average ticket size are consistent with the stated business model."
- "Summarise the key risk indicators across all provided documents. Rate each as low, medium, or high and explain the rating."
- "Compare the merchant's stated business model from the sales call notes against what the website and processing history actually show. Flag any inconsistencies."
- "Draft an onboarding recommendation with conditions or monitoring requirements if applicable."

**Output use:**
The onboarding recommendation enters the compliance officer review queue. After approval, it is attached to the merchant record and sets the pricing, volume limits, and monitoring conditions for the account.

**Domain-specific governance:**
Merchant onboarding decisions must meet BSA/AML requirements. AI-generated risk assessments are advisory inputs to a human decision, not the decision itself. Every onboarding recommendation must be reviewed by a qualified risk or compliance officer before action is taken.

---

## Domain 4: Open Banking and Consumer Financial Health (Emerging)

**Who uses it:** Financial advisors, consumer lending officers, financial wellness platforms

**The core workflow:**
A financial advisor is preparing a financial health review for a client. With the client’s permission under CFPB Section 1033, they can access account data from three linked financial institutions, the client’s investment account holdings, uploaded tax returns, and the client’s own notes about financial goals shared through the advisor’s portal.

**Input sources in this domain:**
- Consumer-permissioned bank account data (CFPB 1033)
- Investment account holdings and returns
- Tax return documents (uploaded PDF)
- Insurance policy summaries
- Client-provided financial goals and notes
- Debt obligation documents

**Effective prompt examples:**
- "Based on all linked accounts and the uploaded tax returns, summarise the client's current financial position including net worth, cash flow, and debt coverage."
- "Identify the top three gaps between the client's stated financial goals and their current financial position."
- "Draft a plain language financial health summary suitable for sharing with the client directly."

**Output use:**
The financial health summary is reviewed by the advisor before it is shared with the client. In consumer-facing settings, plain language standards and CFPB disclosure requirements apply.

---

## Cross-Domain Architecture Summary

The same five-layer architecture is used across all domains. What varies is the domain-specific input source setup and the governance obligations:

```
INPUT LAYER
  Document Upload → domain specific formats
  Published/Stored Notes → domain specific note store
  Connected Notes → OneNote, Drive, CRM notes
  System Data → live data from the domain platform
  Custom Prompt → analyst defined analytical question

RETRIEVAL LAYER
  Relevant sections retrieved per source
  Structured data parsed separately from narrative
  Source tagged with type, date, author

SYNTHESIS LAYER
  Grounded generation from retrieved content only
  No training memory for factual claims
  Gap detection where inputs are insufficient

OUTPUT LAYER
  Structured summary with section headings
  Citation per claim: source, section, confidence
  Gaps identified
  Follow-up suggestions

GOVERNANCE LAYER
  Human review required before any output is actioned
  AI-assisted label permanent and irremovable
  Audit log: inputs, prompt, output, reviewer, decision
  Domain-specific compliance requirements applied
```

---

## Why This Pattern Matters Across All Domains

The issue this architecture addresses is the same no matter the domain: practitioners have more relevant information available than they can synthesize manually, and the manual synthesis process is both time-intensive and unevenly documented.

In investment research, the result of inconsistent synthesis is uneven research quality. In credit analysis, it becomes inconsistent credit decisions that introduce fair lending risk. In merchant onboarding, it produces uneven risk assessments that increase fraud exposure. In consumer financial health, it leads to inconsistent advice that falls short for the people it is supposed to help.

A grounded, cited, auditable AI synthesis layer does not substitute for the practitioner’s judgment. Instead, it offers a more orderly starting point so judgment can focus on what matters: assessing the conclusions, closing the gaps, and making the decision that only a human with full context can make.

That is the purpose this framework is meant to serve.
