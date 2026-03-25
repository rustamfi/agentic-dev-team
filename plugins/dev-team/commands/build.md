---
description: >
  Run the full development team pipeline on a task. Orchestrates PM, Designer, Architect,
  Backend, Frontend, QA, and Security agents in phases, with user approval gates.
argument-hint: "<task description>"
---

Run the full development team pipeline for the given task.

You orchestrate this workflow directly — do NOT delegate the entire pipeline to a single agent. Execute each phase sequentially, spawning the appropriate agent(s) per phase, and pausing at user-selected approval gates.

## Step 0 — Setup & Gate Selection

1. **Read project context** — Read `AGENTS.md` (or equivalent project-level instructions) before doing anything else. Pass relevant warnings and framework notes to every agent.
2. **Set up project structure** — Create `specs/`, `tasks/active/`, and `tasks/completed/` directories if they don't exist.
3. **Create the task plan** — Write a `tasks/active/<task-slug>.md` file with the task description, timestamp, and selected gates. This file tracks progress through the pipeline and is moved to `tasks/completed/` when done.
4. **Ask the user which approval gates they want.** Use `AskUserQuestion` with this prompt:

   > Which checkpoints do you want to review before I continue?
   >
   > 1. **Specs/features** (requirements & architecture) ← selected by default
   > 2. UX/UI design
   > 3. Prototype
   > 4. Implementation
   > 5. QA/Security review
   >
   > Reply with numbers (e.g. "1,3"), "all", or "none".

   Default (if user just confirms or says nothing specific): gate 1 only.

Store the selected gates and respect them throughout the workflow.

## Phase 1 — Discovery & Specs

1. Spawn `product-manager` agent (model: `sonnet`) to produce `specs/prd.md` with requirement IDs.
2. If the task involves a new system or complex feature, also spawn `architect` agent (model: `opus`) to produce `specs/architecture.md`.
3. Present a summary of requirements (with REQ-xxx IDs) and key design decisions.

**If gate 1 is selected:** Use `AskUserQuestion` to ask the user to approve specs before continuing. If they request changes, re-run the relevant agent(s) with their feedback.

## Phase 2 — UX/UI Design (skip if task has no UI)

1. Spawn `ux-researcher` agent (model: `sonnet`) to produce `specs/ux-research.md`.
2. Then spawn `ui-designer` agent (model: `sonnet`) to produce `specs/ui-design.md`.
3. Present a summary of personas, key flows, aesthetic direction, and component specs.

**If gate 2 is selected:** Use `AskUserQuestion` for approval. Iterate if needed.

## Phase 3 — Prototype (skip if task has no UI)

1. Spawn `frontend` agent (model: `sonnet`) to build a clickable prototype in `prototypes/<feature>/`.
2. Present the prototype to the user.

**If gate 3 is selected:** Use `AskUserQuestion` for approval. Allow up to 3 iterations.

## Phase 4 — Implementation

1. Determine which agents are needed (`backend`, `frontend`, or both).
2. If API contracts are defined in `specs/architecture.md`, spawn `backend` (model: `sonnet`) and `frontend` (model: `sonnet`) in parallel. Otherwise, run them sequentially.
3. Verify test files were created (`*.test.*`, `*.spec.*`, or `__tests__/`). If missing, route back to the implementing agent: "Implementation is incomplete without tests. Write unit tests before proceeding."
4. Present a summary of what was implemented and artifacts created.

**If gate 4 is selected:** Use `AskUserQuestion` for approval.

## Phase 5 — Quality & Security

1. Spawn `qa` agent (model: `opus`) and `security` agent (model: `sonnet`) — can run in parallel.
2. **Cross-validate findings** before applying fixes:
   - Compare findings against `AGENTS.md` and project documentation
   - Compare against the framework version's actual docs
   - Compare against `specs/architecture.md` — if the architect intentionally made a choice that QA flags, defer to the spec unless QA provides evidence the spec is wrong
   - If a finding is ambiguous, escalate to the user
3. If validated issues are found, route back to implementing agents with findings. Maximum 3 iterations.
4. Present QA/Security summary.

**If gate 5 is selected:** Use `AskUserQuestion` for approval.

## Phase 6 — Runtime Validation & Completion

1. **Runtime smoke test**: Start the dev server (`npm run dev` or equivalent), wait for it to be ready, then curl key routes and check for errors in server output. A passing build is NOT sufficient — runtime errors only manifest when the app actually runs.
2. If runtime errors are found, route back to implementing agents to fix.
3. Move task plan from `tasks/active/` to `tasks/completed/`.
4. Present the final traceability summary:

```
TASK COMPLETE
━━━━━━━━━━━━━━━
Summary: <what was accomplished>
Artifacts:
   - <file1>: <description>
   - <file2>: <description>
Traceability:
   - REQ-001 → <implementation file> → TEST-001
   - REQ-002 → <implementation file> → TEST-002
Quality gates passed:
   - [x] Requirements defined
   - [x] UX research complete (if applicable)
   - [x] UI design reviewed (if applicable)
   - [x] Architecture defined
   - [x] Prototype validated (if applicable)
   - [x] Implementation complete
   - [x] Tests written and passing
   - [x] Security reviewed
   - [x] Runtime smoke test passed
```

## General Rules

- **Specs before code.** Never start implementation without defined requirements.
- **Never skip QA.** Even for "simple" changes, at minimum do a quick review pass.
- **Never skip Security** for anything touching auth, user data, APIs, or infrastructure.
- **Always validate against acceptance criteria** before marking complete.
- **Maintain traceability** — every requirement should trace to implementation and tests.
- **If uncertain about routing, ask the user.**
- **Keep the user informed** with status updates between agent handoffs.
- **Preserve context** — when routing to an agent, include relevant outputs from prior agents.
- **Build passing ≠ app working.** Always run runtime validation.
- **Review the review.** Cross-validate QA/Security findings against project docs and framework version.

## Agent Model Assignment

| Agent | Model | Reason |
|-------|-------|--------|
| `product-manager` | `sonnet` | Structured output, requirements gathering |
| `ux-researcher` | `sonnet` | Structured output, research synthesis |
| `ui-designer` | `sonnet` | Structured output, design specs |
| `backend` | `sonnet` | Focused implementation |
| `frontend` | `sonnet` | Focused implementation |
| `security` | `sonnet` | Checklist-driven review |
| `architect` | `opus` | Complex system design, trade-off analysis |
| `qa` | `opus` | Deep reasoning for edge cases, cross-validation |

Task: $ARGUMENTS
