---
name: jira-ticket-analyzer
description: "Parse and analyze a Jira ticket (story or bug) to extract change intent, technical scope, customer impact, and search keywords. Use when reviewing a Jira ticket for any downstream workflow."
---

# Jira Ticket Analyzer

## When to Use

Activate this skill when you need to:
- Parse a Jira ticket for structured analysis
- Extract search keywords from a ticket
- Determine customer impact and technical scope
- Classify a ticket for downstream workflows (doc impact, release notes, test planning)

## Input

Prompt the user for a **Bug/Story ID** (e.g., `OCTA-12345`).

If a ticket ID is provided, retrieve ticket details from Jira via MCP tools. If MCP is
unavailable or the fetch is incomplete, ask the user to provide only the missing fields.

## Required Fields

| Field | Description |
|---|---|
| **Ticket ID** | e.g., OCTA-12345 |
| **Type** | Story or Bug |
| **Title/Summary** | One-line summary |
| **Description** | Full description of the change or defect |
| **Acceptance Criteria** | What defines "done" (if present) |
| **Fix Version(s)** | Version(s) where the fix/feature ships |
| **Affects Version(s)** | Version(s) impacted by the bug |
| **Component(s)** | e.g., ABL Compiler, PAS for OpenEdge, Database, OECC |
| **Labels** | Any relevant labels |

If critical fields are missing (especially versions and description), ask the user before
proceeding.

## Analysis Steps

1. **What changed**: Identify the specific feature, fix, or behavioral change.
2. **Technical scope**: Determine which components, APIs, configurations, or behaviors
   are affected.
3. **Customer impact**: Assess how the change affects end users, developers, or
   administrators.
4. **Comment/feedback review**: Extract explicit customer feedback from ticket comments
   and description. Determine whether feedback maps to documentation changes.
5. **Keyword extraction**: Generate 5-10 specific technical keywords for downstream
   search (API names, configuration parameters, error codes, ABL statements, feature
   names).

## Batch Mode

When given a list of ticket IDs:
1. Deduplicate the list first.
2. Retrieve Jira details for all tickets in one pass when possible.
3. Analyze each ticket individually.
4. Return structured results that can be triaged and prioritized.

## Output Format

Return a structured "Ticket Analysis" section containing:

```
## Ticket Analysis
- **Ticket**: {ID} — {title}
- **Type**: {Story|Bug}
- **What changed**: {description}
- **Technical scope**: {components, APIs, configs}
- **Customer impact**: {impact assessment}
- **Customer feedback**: {summary of relevant comments, or "None"}
- **Fix Version(s)**: {versions}
- **Affects Version(s)**: {versions}
- **Keywords**: {comma-separated list}
```
