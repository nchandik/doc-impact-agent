# Doc Impact Process Plan

## 1. Purpose
Establish a repeatable process to assess documentation impact from Jira tickets by comparing ticket details/comments with customer-facing documentation and producing actionable stories for Information Developers.

## 2. Scope
- In scope: Jira Story/Bug analysis, review of ticket comments/feedback, customer-facing docs gap analysis (with primary focus on OpenEdge Information Hub), optional implementation verification in GitHub, story creation.
- Out of scope: direct doc publishing, Jira automation, release governance.

## 2.1 Trusted Documentation Source
- Treat OpenEdge Information Hub as a trusted source for customer-facing documentation:
  - https://docs.progress.com/category/openedge-information-hub
- Treat docs.progress.com OpenEdge bundles discovered from this hub as trusted for documentation impact review.
- Start documentation discovery from the Information Hub, then follow version/category links (12.8, 13.0, 12.2) to identify exact impacted pages.

## 3. Inputs and Outputs
### Required Inputs
- Ticket ID (Bug/Story ID, for example CONT-27092)

### Batch Input Option
- The process can also accept a list of ticket IDs.
- If a list is provided:
  - Deduplicate the ticket IDs first.
  - Retrieve Jira details for all tickets in one pass when possible.
  - Triage and prioritize the batch before creating subtasks.

### Strongly Recommended Inputs
- Acceptance Criteria
- Labels
- Related PR/commit links (if available)

Note: Ticket fields are retrieved from Jira via MCP using the Ticket ID. Missing fields
are requested from the user only when Jira data is incomplete.

### Outputs
- Ticket analysis summary
- Comment impact determination (whether feedback requires doc changes)
- Optional implementation verification summary
- Documentation search results by version
- Current page status for referenced URLs (valid page, redirected topic, or retired/404)
- Documentation impact assessment
- Exact change targets (guide/topic/section/page URL)
- Agile stories for Information Development
- Effort estimate (hours and elapsed-time range)
- Jira update recommendation (comment only vs DOC subtask)
- No-action-needed list
- Roll-up summary (counts, versions, impacted doc areas)

## 4. Process Workflow (Leverage Existing Agent)
This workflow mirrors the existing doc-impact-reviewer agent behavior.

1. Ticket analysis
- Prompt for Bug/Story ID and retrieve Jira details via MCP.
- Extract change intent, technical scope, customer impact, and explicit customer feedback from comments/description.
- Generate 5-10 search keywords.

2. Documentation discovery
- Start with customer-facing docs in OpenEdge Information Hub:
  - https://docs.progress.com/category/openedge-information-hub
- Treat this site and linked OpenEdge documentation bundles as trusted inputs for the assessment.
- Search docs.progress.com for each Fix Version and Affects Version.
- Use keyword and component-based searches.
- Broaden scope if version-specific results are sparse.

2.1 Referenced URL validation
- Validate the exact page URL referenced in the Jira ticket when one is provided.
- Classify the referenced URL as one of:
  - Current valid page
  - Redirect or successor topic found
  - Retired or 404 page
- If the referenced page is retired or 404:
  - Identify the best current replacement topic in docs.progress.com.
  - Record the dead link as part of documentation impact.

3. Documentation page assessment
- For each relevant page, classify as:
  - Needs update
  - Needs addition
  - Is correct
  - New topic needed
- For each page that needs action, identify where to change content:
  - Guide name
  - Topic/page title
  - Section heading (or nearest anchor)
  - Page URL

4. Story generation
- Generate one or more Information Development stories using the standard format.
- Include versions and concrete page/topic targets.

5. Effort estimation
- Estimate execution effort for the proposed documentation work.
- Record both:
  - Capacity estimate (hours)
  - Scheduling estimate (elapsed time including review/feedback)
- Include a short assumptions note (versions/pages in scope, expected SME review depth).

6. Jira update strategy
- Update the parent ticket with a structured comment containing:
  - Effort estimate
  - Versions affected
  - Ticket analysis
  - Documentation impact assessment
  - Proposed change targets
  - Priority score
- Default subtask rule:
  - P1 items: create or update a DOC subtask with the effort estimate at the top of the description.
  - P2 and P3 items: update the parent ticket only unless the user requests subtasks.
- If a DOC subtask already exists, update it instead of creating a new one.

7. Implementation verification (optional, do last)
- Run this step after story generation, when GitHub coverage is available.
- Ask whether to include GitHub verification.
- Search Progress-OpenEdge by ticket ID first, then keywords.
- Record repo/PR/commit evidence and discrepancies.

## 5. Version Coverage Rules
- 12.2: docs.progress.com/category/openedge122
- 12.8: docs.progress.com/category/openedge128
- 13.0: docs.progress.com/category/openedge

Rule: assess each listed version independently; do not assume parity across versions.

## 6. Impact Scoring (Prioritization)
Assign each story a priority score to sequence work.

Score = Customer Impact + Release Risk + Surface Area + Correctness Risk

- Customer Impact (0-3)
  - 0: niche/internal
  - 1: low user visibility
  - 2: common admin/developer workflows
  - 3: broad/high-frequency workflows

- Release Risk (0-3)
  - 0: informational only
  - 1: mild confusion risk
  - 2: setup/configuration failure likely
  - 3: security/compliance/availability risk

- Surface Area (0-2)
  - 0: one page, single section
  - 1: multiple sections in one guide
  - 2: multiple guides/areas

- Correctness Risk (0-2)
  - 0: wording clarity only
  - 1: behavior mismatch possible
  - 2: known ticket vs implementation discrepancy

Priority bands
- P1: 7-10
- P2: 4-6
- P3: 0-3

## 6.1 Effort Estimation Framework
Use this default breakdown when planning doc updates. Adjust ranges based on scope.

- Analysis confirmation: 0.5-1.0 hr
- Draft updated wording (per 2 versions/pages): 1.5-2.5 hrs
- Technical validation with SME/engineering: 1.0-2.0 hrs
- Apply edits in docs source + peer review: 1.0-2.0 hrs
- Final QA pass + Jira updates: 0.5-1.0 hr

Total guidance:
- Best case: 4.5 hrs
- Typical: 6.0-7.0 hrs
- If feedback loops are slow: 1-2 business days elapsed

Planning recommendation:
- Capacity planning placeholder: 6.5 hrs
- Scheduling placeholder: 1 working day

## 7. Definition of Done
A ticket is complete when:
- Required ticket fields are present (or missing fields are explicitly called out).
- Ticket comments/feedback are evaluated and mapped to documentation impact (change/no change).
- All Fix/Affects versions were searched and documented.
- Referenced documentation URLs are validated and any retired/404 links are called out.
- Every relevant page has one of the 4 classifications.
- Every required update includes a concrete target location (guide/topic/section or URL).
- At least one story exists for each identified gap.
- Effort estimate and assumptions are documented in the Jira subtask/comment.
- The parent Jira ticket has a structured impact comment.
- For P1 items, a DOC subtask exists unless the user explicitly opted out.
- Summary includes story count, versions, and impacted doc areas.
- If verification was enabled: evidence links and discrepancy notes are included.

## 8. Working Template
Use this per ticket:

## Effort Estimate (put this at top of Jira subtask summary)
- Analysis confirmation:
- Draft updated wording:
- Technical validation:
- Apply edits + peer review:
- Final QA + Jira updates:
- Total estimate (best/typical):
- Scheduling estimate (elapsed time):
- Assumptions:

## Versions Affected
- Fix Version(s):
- Affects Version(s):

## Referenced URL Status
- Jira URL:
- Status: Current valid page | Redirected/successor topic found | Retired/404
- Current replacement topic (if applicable):

## Ticket Analysis
- What changed:
- Technical scope:
- Customer impact:
- Customer comment/feedback summary:
- Keywords:

## Implementation Verification (Optional)
- Included: Yes/No
- Evidence:
- Discrepancies:

## Documentation Search Results
### Version X
- Searches run:
- Relevant pages:

## Documentation Impact Assessment
### Stories for Information Development
- Story:
- Jira Ticket:
- Doc URL:
- Target location (guide/topic/section):
- Version(s):
- Action:
- GitHub Reference:
- Details:
- Priority Score:

### No Action Needed
- Page:
- Why:

## Jira Update Plan
- Parent ticket comment: Yes/No
- DOC subtask action: Create | Update existing | No subtask
- DOC subtask key:

## Summary
- Total stories generated:
- Versions affected:
- Documentation areas impacted:
- Implementation verified:

## 9. Pilot Plan (Next Step)
Run 1 pilot ticket end-to-end using this process and collect:
- Time to complete
- Number of pages reviewed
- Number of stories generated
- Number of comment-to-doc mappings validated
- Number of retired/404 referenced URLs found
- Number of P1 items requiring DOC subtasks
- Any ambiguity in scoring or template fields

Then refine sections 6-8 based on pilot findings.
