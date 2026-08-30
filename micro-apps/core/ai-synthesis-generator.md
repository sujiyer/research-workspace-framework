# Micro-App: Multi-Source AI Research Synthesis

## Purpose

The AI Research Synthesis micro-app collects several heterogeneous inputs and turns them into a structured, grounded research summary for the current workspace context. In contrast to general-purpose AI assistants, it is deliberately grounded: every conclusion it states can be traced back to a particular source document or data point supplied by the analyst. It does not rely on its own training memory. It synthesises only from the material you provide.

---

## The Core Problem This Solves

Across investment, banking, and fintech, research analysts spend a substantial amount of time bringing together information that already exists in multiple disconnected places: a pitchbook uploaded last week, a fund report from the investment platform, handwritten notes from a management meeting in OneNote, a colleague’s published commentary from six months earlier, and a specific analytical question the analyst wants answered.

Today, this is usually done by manually gathering material and then having a person synthesise it. That process takes hours. It also creates inconsistency: two analysts reviewing the same inputs may emphasise sources differently and arrive at different conclusions without recording why.

The AI Research Synthesis micro-app speeds this up while making the result consistent and fully auditable.

---

## Input Sources

The micro-app draws from five input channels at the same time:

### 1. Document Upload
Analysts can upload documents directly into the synthesis session. Supported formats:

| Format | Use Case Examples |
|---|---|
| PDF | Pitchbooks, fund factsheets, annual reports, research reports, regulatory filings |
| PPTX | Presentation decks, investor day materials, strategy slides |
| XLSX | Financial models, data extracts, portfolio analytics exports |
| DOCX | Written reports, internal memos, draft research notes |

At upload, documents are processed: text is extracted, tables are parsed, and the content is indexed against the active fund or security context. The analyst does not have to identify the relevant portion of a document manually — the synthesis engine automatically retrieves the pertinent sections based on the custom prompt.

### 2. Published Notes Selector
Analysts choose from earlier published research notes kept in the research store, filtered to match the active workspace context. The selector exposes:
- Notes by fund or security context
- Notes by date range
- Notes by author (own notes, team notes, or all)
- Notes by type (earnings notes, management meeting notes, model updates, sector commentary)

The selected notes are brought in as structured inputs together with uploaded documents.

### 3. Connected Notes (OneNote and Google Drive)
Analysts link their personal note repositories — Microsoft OneNote notebooks or Google Drive folders — where informal research notes, meeting notes, and working documents are stored outside the formal research store.

This connection is read-only and permissioned for each session. The analyst decides which notebook sections or Drive folders to include. Connected note content is indexed alongside uploaded documents and published notes.

**Privacy control:** Connected note content is used only for the current synthesis session. It is neither stored in the research store nor shared with team members unless the analyst takes explicit action.

### 4. Workspace Context Data
The micro-app automatically retrieves current data from the active workspace context:
- Portfolio holdings and weights (from Portfolio Context View)
- Recent returns and attribution (from Returns Analyzer)
- Current risk metrics (from Risk Metrics Dashboard)
- Upcoming corporate events (from Corporate Events Calendar)

Because this live data is current, it grounds the synthesis in present reality rather than depending only on uploaded documents that may be outdated.

### 5. Custom Prompt
The analyst provides a plain-language prompt describing the intended focus of the synthesis. This guidance layer is what makes the output specific and useful instead of generic.

**Examples of effective custom prompts:**
- "Summarise the fund's technology exposure and how it has changed over the past two quarters. Flag any concentration risks."
- "Compare the management team's guidance from the last three earnings calls against actual results. Identify where they were consistently optimistic or conservative."
- "Summarise the key risks identified across all provided documents and rank them by potential portfolio impact."
- "Based on the uploaded pitchbook and my OneNote meeting notes, write a first draft investment thesis for this fund."

---

## How the Synthesis Works

The synthesis uses a four-stage pipeline designed to preserve grounding and make the reasoning explainable:

```
STAGE 1: DOCUMENT INDEXING
All input sources are processed and indexed.
Tables, charts, and structured data are parsed separately from narrative text.
Each input source is tagged with type, date, and author.

STAGE 2: CONTEXT-AWARE RETRIEVAL
The custom prompt is used to retrieve the most relevant sections
from each indexed input source.
Retrieval is per-source — the engine pulls relevant sections
from the pitchbook, the OneNote notes, and the published notes separately
before combining them.

STAGE 3: GROUNDED SYNTHESIS
The AI generates the summary using ONLY the retrieved sections.
It does not draw on its training memory for factual claims.
Every factual claim is linked to the source section it came from.

STAGE 4: CITATION AND CONFIDENCE
The output includes a citation for every claim:
which document, which section, and a confidence indicator
showing how directly the claim is supported by the source.
```

---

## Output Structure

```json
{
  "synthesis_id": "uuid",
  "workspace_context": {
    "entity_id": "string",
    "entity_name": "string",
    "date_range": "string"
  },
  "custom_prompt": "string",
  "summary": {
    "headline": "string — one sentence summary",
    "sections": [
      {
        "section_title": "string",
        "content": "string — narrative synthesis",
        "citations": [
          {
            "source_type": "UPLOAD | PUBLISHED_NOTE | CONNECTED_NOTE | WORKSPACE_DATA",
            "source_name": "string",
            "source_section": "string",
            "quote_or_data": "string — the specific text or data point supporting this claim",
            "confidence": "HIGH | MEDIUM | LOW"
          }
        ]
      }
    ],
    "gaps_identified": [
      "string — questions the synthesis could not answer from provided inputs"
    ],
    "suggested_follow_up": [
      "string — additional sources or analyses that would strengthen the synthesis"
    ]
  },
  "inputs_used": {
    "uploaded_documents": ["string"],
    "published_notes": ["note_id"],
    "connected_note_sections": ["string"],
    "workspace_data_points": ["string"]
  },
  "ai_assisted": true,
  "requires_human_review": true,
  "generated_at": "ISO8601",
  "model_version": "string"
}
```

---

## Governance Controls

**Human review required**  
No AI-generated synthesis is published to the research store without human review. The synthesis output is only a draft — it is passed into the Content Publisher micro-app for analyst review, editing, and approval before becoming a research note.

**AI-assisted labelling**  
Every summary permanently carries a label showing that it was AI-assisted, which input sources were used, and which model version produced it. If the summary is published, the label remains and cannot be removed.

**Audit trail**  
Each synthesis session is logged: who executed it, which inputs were supplied, what prompt was used, what output was created, and whether it was reviewed and published. The log is retained in line with the institution's data retention policy.

**Hallucination prevention**  
The synthesis engine is limited to retrieved source content. If the custom prompt requests information that is absent from the provided inputs, the output marks that explicitly in the "gaps identified" section instead of inventing unsupported content.

---

## Context Behaviour

**Default mode: SHARED**

The synthesis micro-app runs inside the active workspace context. If the context changes, the micro-app clears the current session and asks the analyst to begin a new synthesis session for the new context. Sessions in progress are saved and can be resumed.

---

## API Contract (Summary)

```
POST /api/v1/synthesis/initiate
  Request: {
    entity_id, date_range,
    uploaded_document_ids: [],
    published_note_ids: [],
    connected_note_references: [],
    include_workspace_data: boolean,
    custom_prompt: string
  }
  Response: { synthesis_id, status: PROCESSING, estimated_seconds }

GET /api/v1/synthesis/{synthesis_id}/result
  Response: { full synthesis output per schema above }

POST /api/v1/synthesis/{synthesis_id}/send-to-publisher
  Request: { analyst_id, reviewer_id }
  Response: { draft_id in Content Publisher }
```
