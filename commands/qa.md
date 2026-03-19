---
name: qa
description: Run the QA Engineer agent to review code, write tests, and validate acceptance criteria.
argument-hint: "<task description>"
---

Use the `qa` agent to review the following code or feature. Perform a thorough code review, identify edge cases and bugs, write missing tests, and validate against acceptance criteria.

IMPORTANT: Before flagging any framework convention as a bug, the QA agent MUST verify against the project's actual framework version docs and `AGENTS.md`. Do not rely on training data for framework conventions — they change between versions.

Task: $ARGUMENTS
