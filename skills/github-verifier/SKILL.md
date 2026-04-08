---
name: github-verifier
description: "Verify that changes described in a Jira ticket were implemented in GitHub repositories. Searches commits, PRs, and branches in a specified GitHub organization. Use when verifying implementation evidence for a ticket."
---

# GitHub Implementation Verifier

## When to Use

Activate this skill when you need to:
- Verify that a Jira ticket's changes were implemented in source code
- Find commits, PRs, or branches related to a ticket ID
- Compare what was planned (ticket) vs. what was built (code)

## Eligibility

This skill only applies to tickets that have GitHub traceability:

| Ticket Prefix | Eligible? | Reason |
|---|---|---|
| **OCTA-** | Yes | References Progress-OpenEdge GitHub repos |
| **CONT-** | No | No Git location in ticket |
| Other prefixes | No | Assume no Git traceability unless confirmed otherwise |

If the ticket is not eligible, skip this skill entirely and note:
*"GitHub verification skipped — not available for {prefix} tickets."*

**Access note**: Not all team members have access to the Progress-OpenEdge GitHub
repositories. If a search fails due to permissions, report the error clearly and
allow the workflow to continue without verification.

## GitHub Organization

Default target: **Progress-OpenEdge** on GitHub.com (`github.com/Progress-OpenEdge`).

## Search Strategy

Try these approaches in order until matching results are found:

### 1. Search by Ticket ID

Search the organization for the ticket ID (e.g., `OCTA-12345`):
- **Commits**: Look for the ticket ID in commit messages
- **Pull requests**: Look for the ticket ID in PR titles or descriptions
- **Code**: Look for references in comments or changelogs

### 2. Search by Keywords

If the ticket ID yields no results:
- Search for key technical terms in recently merged PRs
- Search for API names, class names, or config parameter names in the codebase
- Narrow by repository if the component suggests a specific repo

### 3. Search by Branch Name

Look for branches containing the ticket ID as part of the branch name.

## Reporting Format

Return a structured "Implementation Verification" section:

```
## Implementation Verification
- **Included**: Yes
- **Search method**: {ticket ID | keywords | branch name}
- **Repository(ies)**: {repo names where changes were found}
- **Evidence**:
  - {PR link or commit hash — brief description}
  - {additional evidence}
- **Code change summary**: {brief description of actual changes}
- **Discrepancies**: {differences between ticket description and implementation, or "None"}
```

### When Nothing Is Found

If no matching implementation is found after all three search strategies:
- State this clearly as a risk
- Report which searches were attempted
- Note that documentation should not be written until implementation is confirmed

## Discrepancy Handling

If the implementation differs from the Jira ticket description:
- Flag this prominently
- Document both what the ticket says and what the code does
- Note that documentation should reflect what was actually built, not what was planned
