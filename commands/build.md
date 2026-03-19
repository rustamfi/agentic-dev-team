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
3. **Spec first** — Check if specs exist in `specs/`. If not, route to `product-manager` (and `architect` / `ux-researcher` / `ui-designer` as needed) to produce them before any implementation begins.
4. Analyze the task and determine which agents are needed
5. Dynamically invoke agents based on the evolving task state
6. **Human approval gate** — After specs are ready, present a summary of requirements (with REQ-xxx IDs) and key design decisions. Ask the user to approve before starting implementation.
7. Enforce quality gates: QA and Security must review before completion
8. **Cross-validate QA/Security findings** — Before applying fixes from QA or Security reviews, verify findings against project docs (`AGENTS.md`) and the actual framework version. Do not blindly apply fixes that contradict project-level instructions.
9. Iterate if validated issues are found — route back to implementing agents with findings
10. **Runtime validation** — After build passes, run a smoke test: start the dev server (`npm run dev` or equivalent), wait for it to be ready, then curl key routes and check for errors in server output. A passing build is necessary but NOT sufficient — runtime errors (Edge Runtime incompatibilities, missing environment variables, import errors) only manifest when the app actually runs.
11. **Final approval gate** — After QA, Security, and runtime validation pass, present the traceability summary and ask the user to confirm completion.
12. Declare complete only when all acceptance criteria are verified AND runtime smoke test passes

Provide status updates between agent handoffs so the user can follow progress.

Task: $ARGUMENTS
