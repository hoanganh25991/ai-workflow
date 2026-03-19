# /review Command

## Purpose

Audit the implementation against all standards and verify compliance with the plan.

## Usage

```
/review
```

## Prerequisites

Must have previously run `/execute` (or `/execute plans/plan-<ID>`) so there is an implementation to audit.

## Process

### 1. Code Quality Review

Check against `02-coding-standards.mdc`:

- [ ] TypeScript strict mode compliance
- [ ] No `any` types without justification
- [ ] Proper error handling
- [ ] Async/await usage
- [ ] Naming conventions followed
- [ ] Documentation on public APIs

### 2. Architecture Alignment

Verify implementation matches plan:

- [ ] All planned components implemented
- [ ] Component responsibilities honored
- [ ] Interfaces match specifications
- [ ] Dependencies correctly managed

### 3. Security Compliance

Verify all Security Review mitigations implemented:

- [ ] Input validation in place
- [ ] Authentication working
- [ ] Authorization enforced
- [ ] Error messages sanitized
- [ ] Rate limiting if required

### 4. Test Coverage

Check testing:

- [ ] Unit tests for core logic
- [ ] Integration tests for API endpoints
- [ ] Critical paths covered
- [ ] Test coverage report available

### 5. DevOps Readiness

Verify deployment readiness:

- [ ] Dockerfile builds successfully
- [ ] Docker Compose works locally
- [ ] CI/CD pipeline configured
- [ ] Environment variables documented
- [ ] Health check endpoints present

## Output Format

```
=== Review Report ===

Code Quality: PASS/FAIL
- Issues found: <n>
- Critical: <n>

Architecture Alignment: PASS/FAIL
- Components implemented: <n>/<n>
- Gaps: <list>

Security Compliance: PASS/FAIL
- Mitigations implemented: <n>/<n>
- Gaps: <list>

Test Coverage: <percentage>%
- Critical paths covered: YES/NO

DevOps Readiness: PASS/FAIL
- Ready for deployment: YES/NO

=== OVERALL: PASS/FAIL ===
```

## Logging

Each phase logs to `logs/workflow.md` using the format in `01-workflow-logging.mdc` (match README "What Gets Logged"):

- Start each run with `## Workflow Run: <ISO timestamp>`, **Task**, **Command**: /review
- Use `### [HH:MM:SS] COMMAND:review - START` and `PHASE:<name> - INVOKED` / `- Complete` for each phase (code-quality, architecture, security, test-coverage, devops-readiness), then `COMMAND:review - COMPLETE`
- Log decisions, phase outputs (PASS/FAIL, issues), and completion
- Append a **SUMMARY** block (see `01-workflow-logging.mdc`): Command, Result (PASS or FAIL), Key points (overall verdict, critical issues if any, components checked, readiness for deployment)

```
---
## Workflow Run: YYYY-MM-DDTHH:MM:SSZ
**Task**: Audit implementation
**Command**: /review

### [HH:MM:SS] COMMAND:review - START
> Auditing implementation

### [HH:MM:SS] PHASE:code-quality - INVOKED
> Checking against coding standards

### [HH:MM:SS] PHASE:code-quality - Complete
> <PASS/FAIL>, issues if any
...
### [HH:MM:SS] PHASE:architecture - INVOKED
> Verifying alignment with plan

### [HH:MM:SS] PHASE:architecture - Complete
> <PASS/FAIL>, components implemented, gaps if any

### [HH:MM:SS] PHASE:security - INVOKED
> Verifying security mitigations

### [HH:MM:SS] PHASE:security - Complete
> <PASS/FAIL>, mitigations, gaps if any

### [HH:MM:SS] PHASE:test-coverage - INVOKED
> Checking tests

### [HH:MM:SS] PHASE:test-coverage - Complete
> <percentage>%, critical paths, gaps if any

### [HH:MM:SS] PHASE:devops-readiness - INVOKED
> Verifying deployment readiness

### [HH:MM:SS] PHASE:devops-readiness - Complete
> <PASS/FAIL>, ready for deployment

### [HH:MM:SS] COMMAND:review - COMPLETE
> Review complete: PASS/FAIL

### SUMMARY
- **Command**: /review
- **Result**: PASS or FAIL
- **Key points**: <overall verdict>, critical issues if any, components checked, readiness for deployment>
```

## Example

```bash
/review
```

Log output (in README format):

```
---
## Workflow Run: 2026-03-19T10:32:00Z
**Task**: Audit implementation
**Command**: /review

### [10:32:01] COMMAND:review - START
> Auditing implementation

### [10:32:02] PHASE:code-quality - INVOKED
> Checking against coding standards

### [10:32:05] PHASE:code-quality - Complete
> PASS, 0 critical issues

### [10:32:06] PHASE:architecture - INVOKED
> Verifying alignment with plan

### [10:32:08] PHASE:architecture - Complete
> PASS, 3/3 components implemented

### [10:32:09] PHASE:security - INVOKED
> Verifying security mitigations

### [10:32:11] PHASE:security - Complete
> PASS, mitigations in place

### [10:32:12] PHASE:test-coverage - INVOKED
> Checking tests

### [10:32:14] PHASE:test-coverage - Complete
> 85%, critical paths covered

### [10:32:15] PHASE:devops-readiness - INVOKED
> Verifying deployment readiness

### [10:32:17] PHASE:devops-readiness - Complete
> PASS, ready for deployment

### [10:32:17] COMMAND:review - COMPLETE
> Review complete: PASS

### SUMMARY
- **Command**: /review
- **Result**: PASS
- **Key points**: All phases passed. Code quality, architecture, security, test coverage, devops readiness verified. Ready for deployment.
```

## Actions

If review finds issues:
1. List all issues with severity
2. Prioritize fixes
3. Use `/execute` to apply fixes if requested
4. Re-run `/review` to verify

If review passes:
- Implementation is ready for deployment
