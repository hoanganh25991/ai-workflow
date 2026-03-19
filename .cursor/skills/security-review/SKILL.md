# Security Review Skill

## Purpose

Threat model designs, identify vulnerabilities, and create mitigation strategies.

## When to Use

Invoke this skill when:
- Designing new API endpoints
- Handling sensitive data
- Implementing authentication/authorization
- Processing user input
- Integrating with external services

## Inputs

- Solution design
- Database schema
- API specifications
- User input handling points

## Process

### 1. Surface Analysis

- Map all attack surfaces (endpoints, inputs, integrations)
- Identify trust boundaries
- Document data flows

### 2. Threat Modeling

Apply STRIDE or similar framework:

- **S**poofing: Can attackers impersonate users?
- **T**ampering: Can data be modified in transit/storage?
- **R**epudiation: Can users deny their actions?
- **I**nformation Disclosure: Can sensitive data be leaked?
- **D**enial of Service: Can availability be compromised?
- **E**levation of Privilege: Can attackers gain unauthorized access?

### 3. Vulnerability Identification

- Injection risks (SQL, NoSQL, Command, XSS, CSRF)
- Authentication weaknesses
- Authorization gaps
- Data exposure
- Cryptographic issues

### 4. Risk Assessment

- Severity: Critical/High/Medium/Low
- Likelihood: High/Medium/Low
- Priority: Based on severity × likelihood

### 5. Mitigation Strategy

For each threat, define:
- Prevention mechanism
- Detection mechanism
- Response procedure

## Outputs

1. **Threat Model**: Documented attack surfaces and threats
2. **Risk Matrix**: Severity × likelihood for each threat
3. **Mitigation Checklist**: Specific controls to implement
4. **Security Requirements**: Non-negotiable security needs

## Log Format

```
[HH:MM:SS] SKILL: security-review — INVOKED
> Threat modeling the <system-area>...
> Threats identified: <n> (<severity-breakdown>)
> Mitigations: <list>
> Output: Security controls checklist
```

## Example

```
Threats Identified:
1. SQL Injection (HIGH) - User input in query
2. Token Enumeration (HIGH) - Sequential token generation
3. Rate Limiting Absent (MEDIUM) - No abuse prevention

Mitigations:
1. Parameterized queries, input validation
2. Cryptographically random tokens, error message sanitization
3. Rate limiting middleware, IP-based throttling

Security Requirements:
- All tokens must be > 128 bits entropy
- Passwords hashed with bcrypt (cost factor 12)
- HTTPS required for all endpoints
```
