# CLAUDE.md — T412 — Deploy & SelfTest tai Local Host (L3: Step)

> **Level:** L3 — Downloaded when user starts step T412.
> **Download:** Into project folder, overrides project CLAUDE.md.
> **Activate:** Injected into all prompts while executing step T412.

---

## Objective
Deploy app len local, chay smoke test de verify basic functionality.

## Expert Team
DevOps Engineer + QA Engineer

## Preconditions (MUST be true before starting)
- Migration done (T411)
- Code da build

## Output Contract (MUST deliver these)
- App running tai localhost
- Health check pass
- Basic flows work

## Validation Checklist
- [ ] Health check 200
- [ ] Login works
- [ ] Main pages load

## Failure Cases (watch out)
- Server crash
- DB connection error at runtime

## Branching Logic
- Neu server fail start → check logs, fix
- Neu smoke test fail → fix code, re-deploy

## SEPO Integration

### Before executing this step:
1. Login SEPO: `POST /api/dev/quick-login`
2. Check required docs exist locally (see skill file for list)
3. If missing: `GET /api/docs?projectId=X` → filter → download

### After completing this step:
1. Ask user: "Upload output to SEPO? (y/n)"
2. If yes: `POST /api/docs` → `POST /api/docs/{id}/versions`
3. Update task-item status: `PATCH /api/task-items/{id}` → doneStatus="Done"

## Completion Criteria (step DONE when)
- App running
- Smoke test pass

## Next Step
- T413 (Push Git)

## Prompt Preview (see SKILL file for full prompt)
> Step 1: Migration:
>  "You are a senior database architect specialized in safe production migrations (MySQL 8.0).
> Your task is to generate a database migration script.
> INPUT:
> 1. Architectural Pack : [architecturepack_version_date]
> 2. IncrementalStepPlan: [IncrementalStepPlan_version_date]
> 3. Vault fil...

## Global Rules
Read `sepo_skill/CLAUDE.md` (L1) for Iron Rules, Git conventions, Tech Stack, SEPO API reference.
