# CLAUDE.md — Task T411: Run Database Migration

## Task Identity
- **Code:** T411 | **Phase:** T4 — Deploy & UAT | **Type:** B | **Team:** DB Architect + DevOps

## Objective
Chay database migration scripts tren staging environment.

## Preconditions
- Migration SQL da co (tu T211) | Target DB accessible

## Input Docs
- **migration.sql** — Migration scripts | **vault.md** — DB credentials

## Output Contract
- Migration executed successfully | Schema verified | No data loss

## Instructions
You are a senior database architect specialized in safe production migrations.
1. Read vault.md for database connection info
2. Read migration.sql and ArchitecturePack for all migration steps needed
3. Compare new requirements with current schema
4. Execute migration with rollback plan ready
5. Verify schema matches expected state

## References
- @docs/migration.sql — load always | @docs/vault.md — load for credentials
