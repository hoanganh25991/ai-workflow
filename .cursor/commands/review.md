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

Log to `logs/workflow.md` in README format (see `01-workflow-logging.mdc`):

```
### [HH:MM:SS] COMMAND:review - START
> Auditing implementation

### [HH:MM:SS] COMMAND:review - COMPLETE
> Review complete: PASS/FAIL

### SUMMARY
- **Command**: /review
- **Result**: PASS or FAIL
- **Key points**: <overall verdict>, critical issues if any, components checked, readiness for deployment>
```

## Actions

If review finds issues:
1. List all issues with severity
2. Prioritize fixes
3. Use `/execute` to apply fixes if requested
4. Re-run `/review` to verify

If review passes:
- Implementation is ready for deployment
