# Solution Design Skill

## Purpose

Break requirements into components, identify boundaries, interfaces, and architectural decisions.

## When to Use

Invoke this skill when starting a new feature, service, or significant code change that requires architectural thinking.

## Inputs

- User requirements (from `/plan` command)
- Reference files (@ symbol references)

## Process

### 1. Analyze Requirements

- Parse the user request for functional requirements
- Identify non-functional requirements (performance, scalability, reliability)
- Extract constraints (tech stack, deadlines, existing systems)

### 2. Identify Components

Break down the system into logical components:

- **Services/APIs**: What endpoints or interfaces are needed?
- **Data**: What entities and relationships exist?
- **Business Logic**: What are the core transformations or rules?
- **External Integrations**: What other systems need to communicate?

### 3. Define Boundaries

- Component responsibilities
- Public vs private interfaces
- Integration points
- Data flow

### 4. Document Architecture

- Component diagram (text-based)
- Key decisions with rationale
- Trade-offs considered

## Outputs

1. **Component List**: Enumerated components with responsibilities
2. **Interface Definitions**: API contracts, function signatures
3. **Architecture Decisions**: Key choices and why
4. **Dependencies**: External libraries, services, configurations

## Log Format

```
[HH:MM:SS] SKILL: solution-design — INVOKED
> Analyzing requirements...
> Components identified: <n>
> Output: solution design complete
```

## Example

```
Components:
1. UserService - User CRUD operations, authentication
2. TokenService - JWT generation and validation
3. CacheService - Redis caching layer

Interfaces:
- UserService.getUser(id: string): Promise<User>
- TokenService.generateToken(user: User): string
- TokenService.validateToken(token: string): boolean

Architecture Decisions:
- Use repository pattern for data access
- JWT for stateless authentication
- Redis for session caching
```
