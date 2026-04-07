# Doc Impact Agent

This repository contains Copilot customization files for the **Doc Impact Review Agent** —
a VS Code Copilot agent that reviews Jira tickets and identifies customer-facing
documentation on docs.progress.com that needs corrections or additions.

## Repository Convention

- **Instruction files** (`.github/instructions/`) define agent workflows and are
  auto-attached by Copilot based on the YAML `description` field.
- **Skill files** (`skills/*/SKILL.md`) contain reusable domain knowledge that
  instructions reference at specific workflow steps.
- **Plan files** (`plans/`) are human-readable process documentation — rationale, risks,
  checklists, and pilot guidance. They are NOT consumed by Copilot automatically.

## Coding Conventions

- When writing Copilot instruction/skill files, use YAML frontmatter with a `description`
  field for instructions and `name` + `description` for skills.
- Keep instruction files focused on workflow orchestration; delegate domain data to skills.
- Use Markdown tables for structured reference data (version mappings, scoring matrices).
