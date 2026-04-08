---
name: doc-story-generator
description: "Generate Agile stories for Information Developers from a documentation gap analysis. Includes impact scoring, effort estimation, and Jira update recommendations. Use when producing doc work items from an impact assessment."
---

# Documentation Story Generator

## When to Use

Activate this skill when you need to:
- Generate Agile stories from a documentation gap analysis
- Score and prioritize documentation work items
- Estimate effort for documentation updates
- Recommend Jira update actions (comment vs. subtask)

## Page Classification

For each documentation page assessed, classify it as one of:
- **Needs update**: Content is incomplete, outdated, or incorrect
- **Needs addition**: Page covers the topic area but is missing content about the change
- **Is correct**: Page already reflects the change (no action needed)
- **New topic needed**: No existing page covers the change

For pages needing action, identify the exact change target:
- Guide name
- Topic/page title
- Section heading (or nearest anchor)
- Page URL

## Story Format

For each documentation gap, output a story in this exact format:

---

**Title**: DOC: {concise one-line summary of the documentation change}

**Story**: As an Information Developer, I want to update the **{topic name}** topic to
cover the customer-facing details for {brief description of the feature/bug}.

- **Jira Ticket**: {ticket ID} — {ticket title}
- **Doc URL**: {link to the affected page, or "NEW TOPIC" if none exists}
- **Version(s)**: {which version(s) need this update}
- **Action**: {Update | Add Section | New Topic | Correct}
- **GitHub Reference**: {link to PR or commit, if available; otherwise omit}
- **Details**: {2-3 sentences on what specifically needs to change, referencing technical
  details from the ticket}
- **Priority**: {P1 | P2 | P3} (score: {N}/10)
- **Effort**: {estimated hours}

---

## Impact Scoring

Score = Customer Impact + Release Risk + Surface Area + Correctness Risk

| Dimension | 0 | 1 | 2 | 3 |
|---|---|---|---|---|
| **Customer Impact** | Niche/internal | Low user visibility | Common admin/dev workflows | Broad/high-frequency |
| **Release Risk** | Informational only | Mild confusion | Setup/config failure likely | Security/compliance/availability |
| **Surface Area** | One page, one section | Multiple sections, one guide | Multiple guides | — |
| **Correctness Risk** | Wording clarity only | Behavior mismatch possible | Known ticket vs. impl discrepancy | — |

**Priority bands**:
- **P1**: 7-10
- **P2**: 4-6
- **P3**: 0-3

## Effort Estimation Framework

Default breakdown (adjust per scope):

| Activity | Hours |
|---|---|
| Analysis confirmation | 0.5 – 1.0 |
| Draft updated wording (per 2 versions/pages) | 1.5 – 2.5 |
| Technical validation with SME/engineering | 1.0 – 2.0 |
| Apply edits in docs source + peer review | 1.0 – 2.0 |
| Final QA pass + Jira updates | 0.5 – 1.0 |

**Totals**:
- Best case: 4.5 hrs
- Typical: 6.0 – 7.0 hrs
- With slow feedback: 1-2 business days elapsed
- Planning placeholder: 6.5 hrs / 1 working day

## Jira Update Strategy

### Description cleanup (all ticket types)

Before applying any updates, review the parent ticket's Description field:

1. Check whether the description contains raw HTML tags (e.g., `<p>`, `<ul>`, `<li>`,
   `<br>`, `<a>`, `<strong>`, `<em>`, `<h1>`–`<h6>`, `<table>`, `<div>`, or similar
   HTML markup).
2. **If HTML tags are present**:
   - Copy the original description verbatim into a new comment on the ticket with the
     heading: *"Original description (archived before cleanup)"*
   - Rewrite the Description field with the same content but with all HTML markup
     removed — convert to clean, readable Markdown (preserve headings, lists, links,
     and formatting intent).
3. **If the description is already clean** (no HTML tags found): skip this step entirely.

### CONT- tickets (comment only)
- Add a structured comment to the parent ticket containing the full documentation
  impact assessment (stories, effort, versions, change targets).
- Do NOT create subtasks for CONT- tickets.

### OCTA- tickets (subtask)
- Find the existing **"Documentation updates"** subtask on the parent ticket.
  - If it exists, update it.
  - If it does not exist, create a new subtask.
- Rename (or title) the subtask: **DOC: {concise title describing the doc fix}**
  (e.g., `DOC: Add FIPS mode configuration to PAS guide`).
- Place the effort estimate at the top of the subtask description.
- Include the full documentation impact assessment in the subtask description:
  - Effort estimate
  - Versions affected
  - Ticket analysis
  - Documentation impact assessment
  - Proposed change targets
  - Priority score

## Grouping Rules

- If multiple pages in the same guide need updates for the same reason, combine into a
  single story with multiple pages listed.
- Always separate stories by version when content differs across versions.

## Exclusions

- `openedge-product-notes` (Product Notes / Known Issues) is NOT maintained by
  Information Development. List these pages under "No Action Needed" but do NOT generate
  stories for them.
