---
description: Run QA and Security agents together to review existing code before deployment.
argument-hint: "<task or code to review>"
---

Run both the `qa` and `security` agents on the following task or code.

First, use the `qa` agent to:
- Run specialized analysis via pr-review-toolkit agents (code review, error handling, test coverage, type design, comment quality)
- Perform integration and regression checks
- Write missing E2E tests and validate test coverage
- Validate acceptance criteria and build traceability matrix
- Produce `specs/testing.md` with unified, attributed findings

Then, use the `security` agent to:
- Perform threat modeling on the changes
- Review code for security vulnerabilities
- Audit dependencies and configuration

Combine the findings into a unified review report with a clear PASS/FAIL verdict and any required fixes before deployment.

Task: $ARGUMENTS
