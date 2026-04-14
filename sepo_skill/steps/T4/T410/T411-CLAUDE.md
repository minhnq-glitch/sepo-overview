# CLAUDE.md — T411 — Run Migration (L3: Step)

> **Level:** L3 — Downloaded when user starts step T411.
> **Download:** Into project folder, overrides project CLAUDE.md.
> **Activate:** Injected into all prompts while executing step T411.

---

## Objective
Chay database migration scripts tren target environment.

## Expert Team
Database Architect + DevOps Engineer

## Preconditions (MUST be true before starting)
- Migration SQL da co (tu T211)
- Target DB accessible

## Output Contract (MUST deliver these)
- Migration executed successfully
- Schema verified
- No data loss

## Validation Checklist
- [ ] Schema dung sau migration
- [ ] No data loss
- [ ] Critical queries work

## Failure Cases (watch out)
- Migration fail giua chung → partial state

## Branching Logic
- Neu migration fail → rollback tu backup
- Neu data migration can → chay rieng

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
- Migration done
- Schema verified

## Next Step
- T412 (Deploy & SelfTest)

## Prompt Preview (see SKILL file for full prompt)
> Step 1 Migration:
> 
>  "You are a senior database architect specialized in safe production migrations (MySQL 8.0).
> 
> Your task is to generate a database migration script.
> 
> INPUT:
> 
> 1. Architectural Pack : architecturepack_TEP230_V4.3_25032026
> 
> 2. IncrementalStepPlan: IncrementalStepPlan-TEP230V4.3_260320...

## Global Rules
Read `sepo_skill/CLAUDE.md` (L1) for Iron Rules, Git conventions, Tech Stack, SEPO API reference.
