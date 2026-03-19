# AI Engineering Workflow

Modern AI-assisted development isn't just about code completion. This repo demonstrates how to turn Cursor into a structured workflow engine for software engineering using four powerful primitives:

| Primitive    | Purpose                         | Example                                     |
| :----------- | :------------------------------ | :------------------------------------------ |
| **Rules**    | Enforce standards automatically | Coding standards, logging requirements      |
| **Skills**   | Reusable domain expertise       | Solution Design, Database, Security, DevOps |
| **Agents**   | Orchestrate multi-step work     | Coordinate skills with sub-agents           |
| **Commands** | One-click workflow entry points | `/plan`, `/execute`, `/review`              |

## The Workflow Advantage

Complete observability: Every workflow action is logged to `logs/workflow.md` — trace every decision, skill invocation, and orchestration step. See exactly what the AI did, in what order, and why.

## How the Workflow Works

**The Engineering Loop**

```
    +----------------+         +----------------+         +----------------+
    |     /plan      |         |   /execute     |         |    /review     |
    |  Design        | ------> |  Implement     | ------> |  Audit         |
    |  Tech          |         |  Build         |         |  Standards     |
    |  Secure        |         |  Test          |         |  Security      |
    +----------------+         +----------------+         +----------------+
            |                           |                           |
            +---------------------------+---------------------------+
                                        |
                                        v
                            +---------------------------+
                            |   logs/workflow.md        |
                            |   (every step logs here)  |
                            +---------------------------+
```

---

## /plan — Workflow Step 1: Analyze & Design

The plan command orchestrates multiple skills in sequence:

| Step | Skill           | What It Does                                               |
| :--- | :-------------- | :--------------------------------------------------------- |
| 1    | Solution Design | Breaks requirements into components, identifies boundaries |
| 2    | Database Design | Designs schema, plans migrations, sets indexes             |
| 3    | Security Review | Threat models the design, defines auth strategy            |
| 4    | DevOps Pipeline | Plans CI/CD, containerization, deployment                  |

Each step logs its start, inputs, decisions, and outputs to `logs/workflow.md`. When the plan is complete, the full plan is saved to `plans/plan-<ID>.md` (ID = plan-YYYYMMDD-HHMMSS) and the path is logged so you can run `/execute plans/plan-<ID>`.

## /execute — Workflow Step 2: Build It

Implement a specific plan by ID, or the latest plan:

- **`/execute plans/plan-<ID>`** — Implement the plan stored at `plans/plan-<ID>.md` (e.g. `/execute plans/plan-20260319-103000`).
- **`/execute`** — Implement the latest plan (resolved from `logs/workflow.md`).

Gets the plan from step 1:

- Generates code following the architecture decisions
- Creates database migrations from the schema design
- Implements security controls from the threat model
- Sets up CI/CD from the DevOps plan

## /review — Workflow Step 3: Audit Everything

Reviews the implementation against all standards:

- **Code quality:** Adherence to coding standards rules
- **Architecture alignment:** Verifying if the code matches the initial plan
- **Security compliance:** Ensuring all identified threats are mitigated
- **Test coverage:** Checking if critical paths are tested
- **DevOps readiness:** Confirming if the project is ready for deployment

---

## Observability

The centerpiece of this workflow is `logs/workflow.md` — an append-only log that traces every action.

### What Gets Logged

| Event             | Example                                                       |
| :---------------- | :------------------------------------------------------------ |
| Command start/end | `[COMMAND:plan] Started planning: "Build task API"`           |
| Skill invocation  | `[SKILL:solution-design] Invoked - analyzing 5 requirements`  |
| Skill completion  | `[SKILL:solution-design] Complete - 3 components identified`  |
| Decision made     | `[DECISION] Tech stack: Express + TypeScript + PostgreSQL`    |
| Sub-agent spawn   | `[AGENT:orchestrator] Spawning sub-agent for database-design` |
| Sub-agent result  | `[AGENT:sub-01] Complete - schema with 4 tables designed`     |
| Rule applied      | `[RULE:coding-standards] Validated: ESLint config present`    |
| Warning/Issue     | `[WARN] No rate limiting detected in API design`              |

### Example Log Entry

```markdown
---
## Workflow Run: 2026-03-18T10:30:00Z
**Task**: Build Task API with CRUD and due-date filtering
**Command**: /plan

### [10:30:01] COMMAND:plan - START
> Input: "Build task API with @README.md spec — create, list, update status, filter by project"

### [10:30:02] SKILL:solution-design - INVOKED
> Analyzing requirements...
> - Identified 4 functional requirements (create task, list tasks, update status, filter by project)
> - Identified 2 non-functional requirements (response time < 200ms, pagination)
```

---

## Project Structure

The workflow engine is built on Cursor's primitives with automatic logging: The `01-workflow-logging.mdc` rule enables complete workflow observability in `logs/workflow.md`, tracking every decision, skill invocation, and orchestration step.

```
ai-workflow/
├── .cursor/
│   ├── rules/
│   │   ├── 01-workflow-logging.mdc    # Core: every action writes to log
│   │   ├── 02-coding-standards.mdc    # TypeScript/Node.js standards
│   │   └── 03-agent-tracing.mdc       # Sub-agent orchestration tracking
│   │
│   ├── skills/                        # Reusable domain expertise
│   │   ├── solution-design/SKILL.md   # Requirements → tech stack & components
│   │   ├── database-design/SKILL.md   # Schema, migrations, optimization
│   │   ├── security-review/SKILL.md   # Threat model, auth, vulnerabilities
│   │   └── devops-pipeline/SKILL.md   # CI/CD, containers, deployment
│   │
│   ├── agents/                        # Multi-agent orchestration
│   │   └── orchestrator.md            # Coordinates skills via sub-agents
│   │
│   └── commands/                      # Workflow entry points
│       ├── plan.md                    # /plan — full analysis pipeline
│       ├── execute.md                 # /execute — implement plans/plan-<ID>
│       └── review.md                  # /review — audit against standards
│
├── plans/                             # Plan artifacts (plan-YYYYMMDD-HHMMSS.md)
│
├── logs/                              # Observable workflow traces
│   └── workflow.md                    # Active workflow log (append-only)
│
└── README.md                          # You are here
```

### Workflow Rules & Enforcement

| Rule             | File                      | Purpose                                                  |
| :--------------- | :------------------------ | :------------------------------------------------------- |
| Workflow Logging | `01-workflow-logging.mdc` | Every skill, command, and agent action writes to the log |
| Coding Standards | `02-coding-standards.mdc` | TypeScript/Node.js standards, linting, formatting        |
| Agent Tracing    | `03-agent-tracing.mdc`    | Sub-agent orchestration tracking with parent-child IDs   |

---

## How Rules Enforce Workflow Observability

Rules are **always-on**. You don't invoke them — they activate automatically. The logging rule intercepts every skill invocation and command execution, ensuring nothing happens silently.

- **Agent starts working**
  - → Rule 01 activates: "Log this action"
- **Agent invokes Skill**
  - → Rule 01 activates: "Log skill start"
  - → Skill completes
  - → Rule 01 activates: "Log skill result"
- **Agent spawns sub-agent**
  - → Rule 03 activates: "Log agent spawn with trace ID"
  - → Sub-agent works
  - → Rule 03 activates: "Log agent result"

---

## Quick Start

### 1. Set Up the Workflow

```bash
# Clone the repo
git clone <repo-url> && cd ai-workflow
# Open in Cursor
cursor .
```

### 2. Plan, Execute, Then Review

1. **Plan** — In Cursor chat: `/plan <task>`. All decisions are logged to `logs/workflow.md`, and the plan is saved to `plans/plan-<ID>.md`.
2. **Execute** — Run `/execute plans/plan-<ID>` (use the ID from the plan completion log, e.g. `plan-20260319-103000`). Implementation is logged to `logs/workflow.md`.
3. **Review** — Run `/review` to audit the implementation against code quality, architecture, security, tests, and DevOps readiness.

---

## Extend the Workflow

Build custom workflows for your domain:

1. **Add Skills:** Create domain-specific expertise (e.g., API design, testing strategies)
2. **Create Commands:** Build workflow entry points for your team's processes
3. **Define Rules:** Enforce project-specific standards automatically
4. **Include Logging:** All workflow additions must log to `logs/workflow.md` (non-negotiable)
5. **Test & Share:** Submit PRs with your workflow logs showing the complete trace

---

## License

MIT
