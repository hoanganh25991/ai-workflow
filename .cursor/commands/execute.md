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

Log to `logs/workflow.md` in README format (see `01-workflow-logging.mdc`):

```
### [HH:MM:SS] COMMAND:execute - START
> Input: "plans/plan-<ID>" (or "latest")
> Reading plan from plans/plan-<ID>.md
> Implementing: <component-list>

### [HH:MM:SS] COMMAND:execute - COMPLETE
> Implementation complete
```

## Example

```bash
/execute plans/plan-20260319-103000
```

Log output:

```
### [10:31:01] COMMAND:execute - START
> Input: "plans/plan-20260319-103000"
> Reading plan from plans/plan-20260319-103000.md
> Implementing: UserService, AuthMiddleware, /login route

### [10:31:45] COMMAND:execute - COMPLETE
> Implementation complete
```

## Next Step

After execution, run `/review` to audit the implementation.
