# Agent Orchestrator

A Cursor AI agent rule that enforces a **PRD-first development pipeline**. It prevents the most common failure mode of AI coding assistants — jumping straight to implementation and shipping scope that was never agreed on.

## How it works

The orchestrator owns the full feature lifecycle through three strict phases:

### 1. Plan
Every feature request — from a one-liner to a detailed brief — starts with a **PRD and a structured implementation plan**. The plan includes numbered steps, milestones, and verification criteria. No code is written on the first turn, ever.

### 2. Build
Implementation begins **only after the user explicitly approves the plan**. The orchestrator delegates work to specialized sub-agents (PRD engineer, developer) and tracks progress against the approved spec.

### 3. Verify
Once implementation is complete, the orchestrator cross-checks the result against the original plan and design references to catch scope drift before delivery.

## Core principles

- **PRD gate is non-negotiable.** Even if the user says "build", "create", or "implement" — that's a briefing, not a waiver. The plan comes first.
- **Structured plans always.** Every plan follows a mandatory scaffold: numbered steps, milestones, verification criteria. The format never disappears, regardless of how the idea is described.
- **Human approval checkpoint.** No code ships without explicit user sign-off on the spec.
- **Narrow opt-out.** The PRD can only be skipped for trivial fixes (typos, one-line bugs, dependency bumps) when the user explicitly requests it.

## Usage

Place `agent-orchestrator.md` in your `.cursor/agents/` directory. The orchestrator activates automatically when:

- A user describes new user-facing behavior, UI, flows, or data
- A user gives a problem, user story, or idea without an existing spec
- The user mentions the orchestrator or says "pipeline", "PRD", or "requirements"
- The request is multi-step or multi-file by nature

## Project structure

```
.cursor/agents/
├── agent-orchestrator.md          # This orchestrator rule
├── prd-requirements-engineer.md   # PRD engineer sub-agent
docs/requirements/
└── [feature-name].md              # Generated PRDs and implementation plans
```

## Anti-patterns the orchestrator prevents

1. Coding on the first response to a feature request
2. Skipping the PRD because the request "sounds simple"
3. Starting implementation because the repo is empty
4. Shipping a PRD without a filled implementation plan
5. Asking "do you want the pipeline?" instead of defaulting to it
