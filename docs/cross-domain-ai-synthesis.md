# Multi-Source AI Synthesis: Uses in Investment, Banking, and Fintech

The AI Research Synthesis pattern is not limited to investment research. Its underlying setup — several different inputs, a tailored prompt, grounded RAG-driven synthesis, citations, and human oversight — works in any setting where professionals need to combine complex information before they synthesise or advise on a high-stakes choice.

This document applies the framework to three financial services areas and lays out the source inputs, prompt styles, and resulting output formats for each one.

---

## Domain 1: Investment Research

**Who uses it:** Research analysts, portfolio managers, investment associates

**The core workflow:**
An analyst is working on a fund review. They have a pitchbook from a management presentation, an earnings transcript from last quarter, their own OneNote notes from a management meeting, a colleague's published sector commentary, and a focused analytical question about the fund's technology concentration.

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
After analyst review in the Content Publisher, the synthesis draft becomes a published research note.

---

## Domain 2: Banking — Credit and Lending Analysis

**Who uses it:** Loan officers, credit analysts, underwriters, relationship managers

**The core workflow:**
A commercial lending officer is assessing a mid-size business loan application. They have the business's audited financials covering the last three years, a management-prepared business plan, the borrower's bank statements from six months of the institution's own records, a credit bureau report, and a property appraisal for the collateral. They need a credit memo that brings all of this together into a structured lending recommendation.

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
The draft credit memo goes into the loan officer's review workflow. Once compliance and the credit officer approve it, it becomes the formal credit recommendation.

**Domain-specific governance:**
Credit decisions that use AI-assisted synthesis are governed by ECOA and fair lending requirements. The synthesis output must not mention or indirectly stand in for protected class characteristics. Before any decision is communicated to the borrower, every credit recommendation produced by this tool must be reviewed by a qualified credit officer.

---

## Domain 3: Fintech — Merchant Onboarding and Risk Assessment

**Who uses it:** Risk analysts, compliance officers, merchant onboarding teams

**The core workflow:**
A payments platform is bringing on a new merchant. It has the merchant's business registration documents, a website review, six months of processing history from a prior processor, the merchant's bank statements, a fraud risk score from the platform's risk engine, and notes from a sales call about the merchant's business model. The risk analyst needs a structured onboarding recommendation.

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
The onboarding recommendation is routed into the compliance officer review queue. After approval, it is attached to the merchant record and sets the pricing, volume limits, and monitoring conditions for the account.

**Domain-specific governance:**
Merchant onboarding decisions must satisfy BSA/AML requirements. AI-generated risk assessments serve as advisory input to a human decision, not the decision itself. Every onboarding recommendation must be reviewed by a qualified risk or compliance officer before it is actioned.

---

## Domain 4: Open Banking and Consumer Financial Health (Emerging)

**Who uses it:** Financial advisors, consumer lending officers, financial wellness platforms

**The core workflow:**
A financial advisor is preparing a financial health review for a client. With the client's permission under CFPB Section 1033, they can access the client's account data from three linked financial institutions, their investment account holdings, uploaded tax returns, and the client's own notes about their financial goals shared through the advisor's portal.

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
The advisor reviews the financial health summary before it is shared with the client. In consumer-facing settings, plain language standards and CFPB disclosure requirements apply.

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

The issue this architecture addresses is the same no matter the domain: practitioners have more relevant information available than they can combine manually, and that manual synthesis is both time-intensive and unevenly recorded.

In investment research, the effect of uneven synthesis is uneven research quality. In credit analysis, it becomes uneven credit decisions that introduce fair lending risk. In merchant onboarding, it leads to uneven risk assessments that increase fraud exposure. In consumer financial health, it produces uneven advice that does not serve the people it is intended to help.

A grounded, cited, auditable AI synthesis layer does not substitute for the practitioner's judgment. Instead, it provides a more organised starting point so that judgment can focus on what matters: assessing the conclusions, closing the gaps, and making the decision that only a human with full context can make.

That is the purpose this framework is meant to serve.
