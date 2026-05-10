# Agent Orchestrator

A pipeline agent for Cursor that turns an idea into a fully validated feature through a managed pipeline: PRD → approval → implementation → validation → visual QA.

## Why

A common problem when working with AI agents in an IDE: code gets written before scope is locked down. The result — drift, rework, features nobody asked for. Agent Orchestrator solves this with a strict rule: **PRD and plan first, code second**. Not a single line appears in the repository until a human explicitly approves.

## How It Works

The pipeline has two phases:

**Phase 1 — Plan (manual gate)**
The user provides an idea, description, or plan at any level of detail. The orchestrator generates a PRD with a numbered implementation plan (`docs/requirements/[feature-name].md`) and stops. No code is created. Implementation begins only after explicit user approval.

**Phase 2 — Execution (automatic)**
After approval, an automatic cycle runs: implementation → validation (MODE 2) → mismatch remediation → re-verification, until the result passes all acceptance criteria. For UI features, in-browser verification with a recorded QA execution record is mandatory.

## Pipeline

| Step | Agent | Output |
|------|-------|--------|
| 1 | `prd-requirements-engineer` (MODE 1) | `docs/requirements/[feature-name].md` — PRD + implementation plan |
| 2 | `feature-implementer` | Code fulfilling PRD criteria |
| 3 | `prd-requirements-engineer` (MODE 2) | `docs/requirements/[feature-name].validation.md` — acceptance |
| 4 | `visual-qa-engineer` | Visual QA against reference (if one was provided) |

## Key Rules

- **PRD before code** — every feature starts with a requirements document, even if the request sounds like "just add a button"
- **Only a human approves the plan** — an assistant, child agent, or Task cannot substitute for user confirmation
- **Structured plan is mandatory** — every PRD contains: goal, reconnaissance, milestones, execution checklist, verification matrix
- **Browser QA for UI** — `type-check` and `build` are necessary but not sufficient; UI is verified in a real browser via MCP
- **Remediation loop** — mismatch with PRD → fix → full re-verification; pipeline progression is blocked while any defect remains open

## Accepted Inputs

- A detailed plan with sections and criteria
- Brief scope and constraint notes
- A one-sentence idea
- A visual reference (link or image) — persisted in the PRD and used for visual QA

## When the PRD Can Be Skipped

Only if the user explicitly opts out (`skip PRD`, `hotfix only`) **and** the work is a single, obvious fix (typo, one-line bug, dependency bump), not a new feature.

## File Structure

```
docs/requirements/
├── [feature-name].md              # PRD + implementation plan
├── [feature-name].validation.md   # Validation results + QA execution record
├── [feature-name].visual-test.md  # Visual QA (when a reference exists)
└── [feature-name].handoff.json    # Pipeline state
docs/designs/
└── [feature-name].png             # Visual reference (optional)
```

## Usage

Point to this agent when describing a feature in Cursor:

```
@agent-orchestrator create a todo app where you can mark tasks as done
```

The orchestrator will generate a PRD, wait for your approval, and automatically drive the feature through the entire pipeline.
