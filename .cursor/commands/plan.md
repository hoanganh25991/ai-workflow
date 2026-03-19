# /plan Command

## Purpose

Analyze requirements and create a comprehensive implementation plan by orchestrating multiple skills in sequence.

## Usage

```
/plan <task description>
```

## Process

The `/plan` command orchestrates the following skills in sequence:

### Step 1: Solution Design

1. Read and analyze the user's task description
2. Reference any files mentioned with `@`
3. Invoke the **Solution Design** skill:
   - Break requirements into components
   - Identify boundaries and interfaces
   - Document architectural decisions

### Step 2: Database Design

1. Review the solution design output
2. Invoke the **Database Design** skill:
   - Design entity schemas
   - Plan indexes for query patterns
   - Create migration strategy

### Step 3: Security Review

1. Review solution and database designs
2. Invoke the **Security Review** skill:
   - Threat model the design
   - Identify vulnerabilities
   - Create mitigation strategy

### Step 4: DevOps Pipeline

1. Review all previous outputs
2. Invoke the **DevOps Pipeline** skill:
   - Plan CI/CD pipeline
   - Design containerization
   - Plan deployment strategy

## Logging

Each step logs to `logs/workflow.md` using the format in `01-workflow-logging.mdc` (match README "What Gets Logged"):

- Start each run with `## Workflow Run: <ISO timestamp>`, **Task**, **Command**: /plan
- Use `### [HH:MM:SS] COMMAND:plan - START` and `SKILL:<name> - INVOKED` / `- Complete`
- Log decisions, skill outputs, and completion

## Plan artifact

When the plan is complete:

1. Generate a plan ID: `plan-YYYYMMDD-HHMMSS` (e.g. `plan-20260319-103000`).
2. Write the full plan (task, components, schema, security mitigations, DevOps steps) to `plans/plan-<ID>.md`.
3. Log to `logs/workflow.md`: `COMMAND:plan - COMPLETE` with `> Plan saved to plans/plan-<ID>.md`.
4. Append a **SUMMARY** block (see `01-workflow-logging.mdc`): Command, Result (plan path), Key points (skills run, main decisions/components).

This allows executing a specific plan later: `/execute plans/plan-<ID>`.

## Example

```bash
/plan Add user authentication to the API with JWT tokens
```

Log output (in README format):

```
---
## Workflow Run: 2026-03-19T10:30:00Z
**Task**: Add user authentication to the API with JWT tokens
**Command**: /plan

### [10:30:01] COMMAND:plan - START
> Input: "Add user authentication to the API with JWT tokens"

### [10:30:02] SKILL:solution-design - INVOKED
> Analyzing requirements...

### [10:30:08] SKILL:solution-design - Complete
> Output: 3 components identified (auth service, middleware, routes)
...
### [10:30:30] COMMAND:plan - COMPLETE
> Plan saved to plans/plan-20260319-103000.md

### SUMMARY
- **Command**: /plan
- **Result**: Plan saved to plans/plan-20260319-103000.md
- **Key points**: solution-design (3 components), database-design, security-review, devops-pipeline. Auth service, middleware, routes.
```

## Next Step

After planning is complete, run `/execute plans/plan-<ID>` to implement that plan, then `/review` to audit.
