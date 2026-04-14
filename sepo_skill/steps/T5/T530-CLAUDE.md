# CLAUDE.md — T530 — Create Checkpoint AP-Code-DB PRD (L3: Step)

> **Level:** L3 — Downloaded when user starts step T530.
> **Download:** Into project folder, overrides project CLAUDE.md.
> **Activate:** Injected into all prompts while executing step T530.

---

## Objective
Luu checkpoint production: version commits, DB schema, docs.

## Expert Team
Technical Writer + DevOps Engineer

## Preconditions (MUST be true before starting)
- Production deployed va stable

## Output Contract (MUST deliver these)
- PRD_VersionCommit.md
- PRD_DB_Schema.md

## Validation Checklist
- [ ] Files exist and accurate

## Failure Cases (watch out)
- Export fail

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
- Checkpoint files saved

## Next Step
- Done! Hoac quay lai T1 cho version tiep theo.

## Prompt Preview (see SKILL file for full prompt)
> Tổng hợp tất cả các commit git/repo vào file PRD_VersionCommit.md với tên thêm hậu tố vxx.date (v3_190326)
> Export ra DB schema ra file : PRD_DB_Schema_{version}_{date}...

## Global Rules
Read `sepo_skill/CLAUDE.md` (L1) for Iron Rules, Git conventions, Tech Stack, SEPO API reference.
