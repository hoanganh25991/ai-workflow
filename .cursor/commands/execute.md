# /execute Command

## Purpose

Implement the plan created by `/plan` by coordinating code generation across all designed components.

## Usage

```
/execute [plans/plan-<ID>]
```

- **`/execute plans/plan-<ID>`** — Implement the plan stored at `plans/plan-<ID>.md` (recommended).
- **`/execute`** — Implement the latest plan: read `logs/workflow.md`, find the most recent `Plan saved to plans/plan-<ID>.md`, then load `plans/plan-<ID>.md`. If no plan path is in the log, infer the plan from the latest workflow run in the log.

Example: `/execute plans/plan-20260319-103000`

## Prerequisites

- For `/execute plans/plan-<ID>`: the file `plans/plan-<ID>.md` must exist (created by a previous `/plan`).
- For `/execute` with no argument: at least one `/plan` run must have completed and written a plan artifact.

## Process

### 1. Read the Plan

1. If a plan path was given: read `plans/plan-<ID>.md`.
2. If no argument: from `logs/workflow.md` find the latest line `Plan saved to plans/plan-<ID>.md` and read that file; or reconstruct the plan from the latest `/plan` run in the log.
3. Extract all design decisions, components, and requirements from the plan.
4. Identify the implementation order based on dependencies.

### 2. Implementation Sequence

Implement components in dependency order:

1. **Database Migrations**
   - Run migrations created by Database Design skill
   - Verify schema creation

2. **Core Business Logic**
   - Implement services/components from Solution Design
   - Follow coding standards (Rule 02)

3. **Security Controls**
   - Implement mitigations from Security Review
   - Add authentication/authorization
   - Add input validation

4. **API Routes/Endpoints**
   - Implement routes from Solution Design
   - Integrate middleware
   - Add error handling

5. **DevOps Setup**
   - Create Dockerfile
   - Set up docker-compose
   - Configure CI/CD pipeline

### 3. Code Generation Guidelines

When generating code:
- Follow `02-coding-standards.mdc` rules
- Use TypeScript strict mode
- Add JSDoc for public APIs
- Include error handling
- Log significant actions

### 4. Validation

After implementation:
- Verify all planned components exist
- Check security controls are in place
- Ensure code compiles without errors

## Logging

Each phase logs to `logs/workflow.md` using the format in `01-workflow-logging.mdc` (match README "What Gets Logged"):

- Start each run with `## Workflow Run: <ISO timestamp>`, **Task**, **Command**: /execute
- Use `### [HH:MM:SS] COMMAND:execute - START` and `PHASE:<name> - INVOKED` / `- Complete` for each phase (read-plan, implement, build, test, validation), then `COMMAND:execute - COMPLETE`
- Log decisions, phase outputs, and completion
- Append a **SUMMARY** block (see `01-workflow-logging.mdc`): Command, Result (implementation complete), Key points (plan path, phases run, components, key files, notable decisions or issues)

```
---
## Workflow Run: YYYY-MM-DDTHH:MM:SSZ
**Task**: Implement plans/plan-<ID>
**Command**: /execute

### [HH:MM:SS] COMMAND:execute - START
> Input: "plans/plan-<ID>" (or "latest")

### [HH:MM:SS] PHASE:read-plan - INVOKED
> Reading plan from plans/plan-<ID>.md

### [HH:MM:SS] PHASE:read-plan - Complete
> Plan loaded; components: <component-list>

### [HH:MM:SS] PHASE:implement - INVOKED
> Implementing: migrations, business logic, security, API, devops

### [HH:MM:SS] PHASE:implement - Complete
> Output: <key files created, components implemented>
...
### [HH:MM:SS] PHASE:build - INVOKED
> Building...

### [HH:MM:SS] PHASE:build - Complete
> <build outcome>

### [HH:MM:SS] PHASE:test - INVOKED
> Running tests...

### [HH:MM:SS] PHASE:test - Complete
> <test outcome>

### [HH:MM:SS] PHASE:validation - INVOKED
> Verifying components, security, compile

### [HH:MM:SS] PHASE:validation - Complete
> <validation outcome>

### [HH:MM:SS] COMMAND:execute - COMPLETE
> Implementation complete

### SUMMARY
- **Command**: /execute
- **Result**: Implementation complete
- **Key points**: <plan path>, phases (read-plan, implement, build, test, validation), components implemented, key files created, any notable decisions or issues>
```

## Example

```bash
/execute plans/plan-20260319-103000
```

Log output (in README format):

```
---
## Workflow Run: 2026-03-19T10:31:00Z
**Task**: Implement plans/plan-20260319-103000
**Command**: /execute

### [10:31:01] COMMAND:execute - START
> Input: "plans/plan-20260319-103000"

### [10:31:02] PHASE:read-plan - INVOKED
> Reading plan from plans/plan-20260319-103000.md

### [10:31:05] PHASE:read-plan - Complete
> Plan loaded; components: UserService, AuthMiddleware, /login route

### [10:31:06] PHASE:implement - INVOKED
> Implementing: migrations, business logic, security, API, devops

### [10:31:40] PHASE:implement - Complete
> Output: src/services/user.ts, src/middleware/auth.ts, src/routes/auth.ts

### [10:31:41] PHASE:build - INVOKED
> Building...

### [10:31:43] PHASE:build - Complete
> Build succeeded

### [10:31:44] PHASE:test - INVOKED
> Running tests...

### [10:31:45] PHASE:test - Complete
> All tests passed

### [10:31:45] PHASE:validation - INVOKED
> Verifying components, security, compile

### [10:31:46] PHASE:validation - Complete
> Validation passed

### [10:31:46] COMMAND:execute - COMPLETE
> Implementation complete

### SUMMARY
- **Command**: /execute
- **Result**: Implementation complete
- **Key points**: plan-20260319-103000. Phases: read-plan, implement, build, test, validation. UserService, AuthMiddleware, /login route. Key files: src/services/user.ts, src/middleware/auth.ts.
```

## Next Step

After execution, run `/review` to audit the implementation.
