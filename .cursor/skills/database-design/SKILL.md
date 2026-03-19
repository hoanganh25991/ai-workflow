# Database Design Skill

## Purpose

Design database schemas, plan migrations, set indexes for optimal performance.

## When to Use

Invoke this skill when:
- Creating new data storage
- Modifying existing schemas
- Optimizing query performance
- Planning data migrations

## Inputs

- Solution design from previous skill
- Entity definitions
- Query patterns (read/write ratio, frequency)

## Process

### 1. Entity Analysis

- Identify data entities and their attributes
- Define relationships (1:1, 1:N, N:N)
- Determine cardinality
- Identify primary keys and foreign keys

### 2. Schema Design

- Normalize to appropriate level (typically 3NF)
- Denormalize for read-heavy queries where beneficial
- Choose appropriate data types
- Set constraints (NOT NULL, UNIQUE, CHECK)

### 3. Index Strategy

- Primary indexes (clustered)
- Secondary indexes for query patterns
- Composite indexes for multi-column queries
- Consider index selectivity

### 4. Migration Planning

- Create migration scripts (up and down)
- Plan for zero-downtime migrations
- Backup strategies
- Rollback procedures

### 5. Performance Considerations

- Partitioning strategies for large tables
- Caching requirements
- Connection pooling

## Outputs

1. **Schema Definition**: DDL for create/alter statements
2. **Migration Scripts**: Versioned migration files
3. **Index Plan**: All indexes with justification
4. **Entity Relationships**: Diagram or description

## Log Format

```
[HH:MM:SS] SKILL: database-design — INVOKED
> Designing schema for <domain>...
> Entities: <entity-list>
> Output: database implementation plan
```

## Example

```
Entities:
- TokenRecord (id, group_id, type, value, created_at, expires_at)
- TokenConfig (id, group_id, encoding, length, created_at)

Indexes:
- TokenRecord: PRIMARY(id), UNIQUE(group_id, type, internal_id)
- TokenRecord: INDEX(expires_at) — for cleanup job

Migration: 001_add_token_tables.sql
```
