# DevOps Pipeline Skill

## Purpose

Plan CI/CD pipelines, containerization, and deployment strategies.

## When to Use

Invoke this skill when:
- Setting up new services
- Planning infrastructure
- Configuring deployment pipelines
- Planning containerization
- Setting up environment configurations

## Inputs

- Solution design
- Security requirements
- Deployment targets (cloud, on-prem, hybrid)
- Scale requirements

## Process

### 1. Container Strategy

- **Dockerfile**: Multi-stage builds for smaller images
- Base image selection (alpine, slim, distroless)
- Dependency management
- Security scanning (Trivy, Snyk)

### 2. Orchestration

- Docker Compose for local development
- Kubernetes manifests (if applicable)
- Service discovery
- Configuration management

### 3. CI/CD Pipeline

- **Build**: Compile, lint, test
- **Test**: Unit, integration, e2e
- **Security**: Scan for vulnerabilities
- **Deploy**: Staged rollout (dev → staging → prod)

### 4. Environment Configuration

- Environment variables management
- Secrets management (Vault, AWS Secrets Manager)
- ConfigMaps for non-sensitive data

### 5. Deployment Strategy

- Blue-green deployment
- Canary releases
- Rolling updates
- Rollback procedures

### 6. Monitoring & Observability

- Logging (structured JSON)
- Metrics (Prometheus, DataDog)
- Tracing (OpenTelemetry)
- Alerting

## Outputs

1. **Dockerfile**: Production-ready container definition
2. **Docker Compose**: Local development setup
3. **CI/CD Config**: GitHub Actions, GitLab CI, or similar
4. **Deployment Manifests**: Kubernetes or cloud-specific
5. **Environment Config**: Template for environment variables

## Log Format

```
[HH:MM:SS] SKILL: devops-pipeline — INVOKED
> Planning: <deployment-target>...
> Outputs: <list>
> Output: DevOps plan
```

## Example

```
Dockerfile:
- Multi-stage build (builder → runtime)
- Node 20 Alpine base
- Non-root user
- Health check included

Docker Compose:
- Service + Redis + Postgres
- Volume for data persistence
- Network isolation

CI/CD (GitHub Actions):
- Lint → Test → Build → Deploy
- Environment-specific variables
- Slack notifications

Deployment:
- Rolling update strategy
- Health check grace period: 30s
- Rollback: docker-compose pull && restart
```
