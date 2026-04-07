# Contributing to Doc Impact Agent

Thank you for your interest in improving the Doc Impact Review Agent.

## Repository Structure

```
doc-impact-agent/
├── .github/
│   ├── copilot-instructions.md              # Repo-level Copilot context
│   └── instructions/
│       └── doc-impact-reviewer.instructions.md  # Orchestrator instruction
├── skills/
│   ├── openedge-docs/SKILL.md               # Version URLs, bundle patterns, search strategy
│   ├── jira-ticket-analyzer/SKILL.md        # Ticket parsing and keyword extraction
│   ├── github-verifier/SKILL.md             # Implementation verification in GitHub
│   └── doc-story-generator/SKILL.md         # Story generation, scoring, effort estimation
├── plans/
│   └── DOC_IMPACT_PROCESS_PLAN.md           # Human-readable process plan
├── CONTRIBUTING.md
├── LICENSE
└── README.md
```

## How to Contribute

### Improving Skills

Each skill in `skills/` is a self-contained unit of domain knowledge. To improve a skill:

1. Edit the `SKILL.md` file in the relevant skill folder.
2. Keep the YAML frontmatter (`name`, `description`) accurate — this controls when
   Copilot activates the skill.
3. Test by running the doc-impact-reviewer workflow end-to-end against a sample ticket.

### Improving the Orchestrator

The instruction file in `.github/instructions/` defines the 5-step workflow:

1. It should only contain workflow logic and output structure.
2. Domain knowledge (version URLs, scoring matrices, story templates) belongs in skills.
3. If you add a new step, reference the skill it delegates to.

### Adding New Skills

1. Create a new folder under `skills/` with a descriptive name.
2. Add a `SKILL.md` with YAML frontmatter (`name`, `description`).
3. Reference the new skill from the orchestrator instruction if needed.
4. Update this file's structure diagram.

### Updating the Process Plan

The plan in `plans/DOC_IMPACT_PROCESS_PLAN.md` is human documentation. Update it to
reflect any workflow changes, but remember: the plan is reference material for people;
the instruction and skill files are what Copilot actually follows.

## Testing

1. Open this repo in VS Code with GitHub Copilot enabled.
2. Verify the instruction appears in Copilot's instruction list.
3. Run a doc impact review against a test Jira ticket.
4. Verify all 5 steps execute and the output matches the expected structure.

## Style Guidelines

- Use Markdown tables for structured reference data.
- Keep instruction files under 150 lines — delegate to skills.
- Write skills as imperative rules ("Apply Courier New for..."), not explanatory prose.
- Use consistent heading levels: `##` for sections, `###` for subsections.

## Pull Requests

- One logical change per PR.
- Describe what you changed and why.
- If you modified a skill, note which workflow steps are affected.
- If you modified scoring or effort estimation, include before/after examples.
