# Doc Impact Review Agent

A Copilot CLI custom agent that reviews Jira tickets and identifies customer-facing
documentation on docs.progress.com that needs corrections or additions.

## What It Does

1. **Prompts for a Bug/Story ID** and retrieves Jira details via MCP to understand the change
2. **Verifies** (optionally) that the change was implemented in GitHub repos under `Progress-OpenEdge`
3. **Searches** docs.progress.com across affected OpenEdge versions (12.2, 12.8, 13.0)
4. **Generates** Agile stories for Information Developers in the format:
   > "As an Information Developer, I want to update **{topic}** to cover the customer-facing details for **{feature/bug}**."

## Setup

Copy the instructions file to one of these locations:

### Option A — Single Repository
```
your-repo/.github/instructions/doc-impact-reviewer.instructions.md
```

### Option B — All Repositories (Global)
```
%USERPROFILE%\.copilot\instructions\doc-impact-reviewer.instructions.md
```
On macOS/Linux: `~/.copilot/instructions/doc-impact-reviewer.instructions.md`

## Usage

1. Launch Copilot CLI in a directory where the instructions file is accessible:
   ```
   copilot
   ```

2. Verify the agent is loaded:
   ```
   /instructions
   ```
   You should see `doc-impact-reviewer` listed and enabled.

3. Enter the Jira ticket ID when prompted:

   ```
   Review this Jira ticket for documentation impact.
   Bug ID: OCTA-12345
   ```

   The agent retrieves ticket data from Jira via MCP. If fields are missing, it asks only
   for the missing values.

4. The agent will:
   - Analyze the ticket and extract keywords
   - Ask if you want to verify the implementation in GitHub
   - Search documentation for each affected version
   - Output stories for the information development team

## Example Output

```
## Ticket Analysis
OCTA-12345 adds FIPS 140-2 mode support to PAS for OpenEdge...

## Documentation Search Results
### OpenEdge 13.0
- Found: PAS Configuration Reference — no mention of FIPS mode
- Found: Security Guide — covers TLS but not FIPS compliance
### OpenEdge 12.8
- Found: PAS Configuration Reference — no mention of FIPS mode

## Documentation Impact Assessment

### Stories for Information Development

**Story**: As an Information Developer, I want to update the **PAS for OpenEdge
Configuration Reference** topic to cover the customer-facing details for FIPS 140-2
mode support.

- **Jira Ticket**: OCTA-12345 — Add FIPS mode support to PAS for OpenEdge
- **Doc URL**: https://docs.progress.com/bundle/openedge-pas-13/page/Configuration-Reference.html
- **Version(s)**: 13.0
- **Action**: Add Section
- **Details**: Add documentation for the new `server.fipsMode` configuration property,
  including valid values, default behavior, JVM flag requirements, and the FIPS-specific
  error messages that may appear when non-compliant cipher suites are used.

## Summary
- Total stories generated: 3
- Versions affected: 12.8, 13.0
- Documentation areas impacted: PAS Configuration, Security Guide, Upgrade Guide
```

## Supported OpenEdge Versions

| Version | Documentation Hub |
|---|---|
| 12.2 | docs.progress.com/category/openedge122 |
| 12.8 | docs.progress.com/category/openedge-information-hub |
| 13.0 | docs.progress.com/category/openedge |

## Adding New Versions

Edit the version mapping table in the `.instructions.md` file to add new OpenEdge
versions as they are released.

## Future Enhancements

- **Source-level doc search**: If documentation source files are in a Git repo, search
  those directly for more precise matching
- **Batch mode**: Process multiple tickets at once and deduplicate documentation stories
