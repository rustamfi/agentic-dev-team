---
name: build
description: >
  Run the full development team pipeline on a task. The orchestrator dynamically routes
  work to PM, Designer, Architect, Backend, Frontend, QA, and Security agents,
  iterating until the job is done.
argument-hint: "<task description>"
---

Run the full development team pipeline for the given task.

Use the `orchestrator` agent to manage this workflow. The orchestrator should:

1. **Read project context first** — Read `AGENTS.md` (or equivalent project-level instructions) before doing anything else. Pass relevant warnings and framework notes to every agent.
2. **Set up project structure** — Create `specs/` and `tasks/active/` directories if they don't exist. If the project already has equivalent directories, use those.
3. **Spec first** — Check if specs exist in `specs/`. If not, route to `product-manager` (and `architect` as needed) to produce them before any implementation begins.
4. **UX/UI design (for UI tasks)** — If the task involves any user-facing UI, route to `ux-researcher` → `ui-designer` to produce `specs/ux-research.md` and `specs/ui-design.md`. Do not skip this step even if the task seems visually straightforward.
5. **Human approval gate** — After all specs (including UX/UI for UI tasks) are ready, present a summary of requirements (with REQ-xxx IDs) and key design decisions. Ask the user to approve before starting implementation.
6. **Prototype validation (for UI tasks)** — Route to `frontend` to build a clickable prototype. Present to user for approval before full implementation.
7. Dynamically invoke implementing agents based on the evolving task state
8. Enforce quality gates: QA and Security must review before completion
9. **Cross-validate QA/Security findings** — Before applying fixes from QA or Security reviews, verify findings against project docs (`AGENTS.md`) and the actual framework version. Do not blindly apply fixes that contradict project-level instructions.
10. Iterate if validated issues are found — route back to implementing agents with findings
11. **Runtime validation** — After build passes, run a smoke test: start the dev server (`npm run dev` or equivalent), wait for it to be ready, then curl key routes and check for errors in server output. A passing build is necessary but NOT sufficient — runtime errors (Edge Runtime incompatibilities, missing environment variables, import errors) only manifest when the app actually runs.
12. **Final approval gate** — After QA, Security, and runtime validation pass, present the traceability summary and ask the user to confirm completion.
13. Declare complete only when all acceptance criteria are verified AND runtime smoke test passes

Provide status updates between agent handoffs so the user can follow progress.

Task: $ARGUMENTS
