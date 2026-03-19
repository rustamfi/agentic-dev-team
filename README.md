# dev-team

A Claude Code plugin that simulates a full software development team with dynamically orchestrated agents.

## What It Does

Instead of writing code as a single generalist, `dev-team` breaks work into specialized roles — each with deep domain expertise, structured outputs, and quality standards. An orchestrator agent dynamically routes tasks to the right specialists, enforces quality gates, and iterates until the job is truly done.

## Agents

| Agent | Slash Command | Role |
|-------|---------------|------|
| **Orchestrator** | (used by `/dev-team:build`) | Routes tasks, manages state, enforces quality gates |
| **Product Manager** | `/dev-team:pm` | Requirements, user stories, acceptance criteria |
| **UX Researcher** | `/dev-team:ux` | User personas, journeys, information architecture |
| **UI Designer** | `/dev-team:design` | Component specs, design tokens, accessibility |
| **Architect** | `/dev-team:architect` | System design, API contracts, tech decisions |
| **Backend Developer** | `/dev-team:backend` | Server-side implementation, APIs, data layer |
| **Frontend Developer** | `/dev-team:frontend` | Client-side implementation, UI components |
| **QA Engineer** | `/dev-team:qa` | Testing, code review, edge case analysis |
| **Security Engineer** | `/dev-team:security` | Threat modeling, vulnerability review, compliance |

Additional commands:

| Command | Purpose |
|---------|---------|
| `/dev-team:prototype` | Build a clickable prototype to validate design direction |
| `/dev-team:review` | Run combined QA + Security review on existing code |

## Quick Start

### Install

```bash
# From marketplace (once published)
/plugin install dev-team@your-marketplace

# Or load locally for development
claude --plugin-dir ./dev-team
```

### Usage

**Full pipeline** — orchestrator dynamically routes to all needed agents:
```
/dev-team:build Add a user authentication system with email/password and OAuth2 Google login
```

**Individual agents** — invoke a specific role directly:
```
/dev-team:pm Define requirements for a notification preferences page
/dev-team:architect Design the API for a real-time chat feature
/dev-team:backend Implement the REST endpoints for user profile management
/dev-team:qa Review the authentication module for edge cases and vulnerabilities
/dev-team:review Run QA + Security review on the payment processing code
/dev-team:prototype Design a dashboard for real-time analytics
```

## How the Orchestrator Works

The orchestrator uses dynamic routing, not a fixed pipeline:

1. **Assess** — Analyzes the task to determine which agents are needed
2. **Route** — Sends work to the right agent(s) based on current task state
3. **Parallelize** — Runs independent agents simultaneously (e.g., backend + frontend after architecture is defined)
4. **Gate** — Enforces mandatory checkpoints:
   - Requirements must exist before implementation
   - QA + Security must review before deployment
   - All acceptance criteria must pass before completion
5. **Iterate** — If QA or Security find issues, routes back to implementing agents with specific findings
6. **Complete** — Declares done only when all quality gates pass

## Project Structure

```
dev-team/
├── .claude-plugin/
│   └── plugin.json          # Plugin manifest
├── agents/
│   ├── orchestrator.md      # Task routing and lifecycle management
│   ├── product-manager.md   # Requirements and acceptance criteria
│   ├── ux-researcher.md     # UX research, personas, and journeys
│   ├── ui-designer.md       # UI component specs and visual design
│   ├── architect.md         # System design and API contracts
│   ├── backend.md           # Server-side implementation
│   ├── frontend.md          # Client-side implementation
│   ├── qa.md                # Testing and code review
│   └── security.md          # Threat modeling and vulnerability review
├── skills/
│   └── team-workflow/
│       └── SKILL.md         # Auto-invoked context for team collaboration
├── commands/
│   ├── build.md             # Full orchestrated pipeline
│   ├── pm.md                # Product Manager
│   ├── ux.md                # UX Researcher
│   ├── design.md            # UI Designer
│   ├── architect.md         # Architect
│   ├── backend.md           # Backend Developer
│   ├── frontend.md          # Frontend Developer
│   ├── qa.md                # QA Engineer
│   ├── security.md          # Security Engineer
│   └── review.md            # Combined QA + Security review
└── README.md
```

## Customization

### Adjusting Agent Models

Model assignments are centralized in the orchestrator agent (`agents/orchestrator.md`) under the **Agent Model Assignment** section. The orchestrator passes the `model` parameter when spawning each agent via the Agent tool. To change which model an agent uses, update the table there.

### Adding Domain-Specific Context

Add skills in `skills/` for your specific domain. For example, if your team works on a fintech product, add `skills/fintech-context.md` with domain rules, compliance requirements, and architectural patterns.

### Extending with MCP Servers

Add a `.mcp.json` at the plugin root to give agents access to external tools:

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": { "GITHUB_PERSONAL_ACCESS_TOKEN": "<token>" }
    }
  }
}
```

## License

MIT
