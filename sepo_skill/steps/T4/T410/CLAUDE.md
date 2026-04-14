# CLAUDE.md — T410: Deploy Staging (L2: Sub-phase)

> **Level:** L2 — Deploy sub-phase of T4.
> **Steps:** T411→T412→T413→T414→T415 (strictly sequential)

## Flow
```
T411: Run migration (backup first!)
  ↓
T412: Deploy & test on localhost
  ↓
T413: Push to git (no secrets!)
  ↓
T414: Deploy to staging server
  ↓
T415: Create checkpoint docs (STG_VersionCommit, STG_DB_Schema)
```

## Rule: If ANY step fails → stop, fix, restart from T411.
