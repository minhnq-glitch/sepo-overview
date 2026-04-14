# CLAUDE.md — T415 — Create Checkpoint AP-Code-DB STG (L3: Step)

> **Level:** L3 — Downloaded when user starts step T415.
> **Download:** Into project folder, overrides project CLAUDE.md.
> **Activate:** Injected into all prompts while executing step T415.

---

## Objective
Luu checkpoint: version commits, DB schema snapshot, docs cho staging version.

## Expert Team
Technical Writer + DevOps Engineer

## Preconditions (MUST be true before starting)
- Staging deployed va verified

## Output Contract (MUST deliver these)
- STG_VersionCommit.md
- STG_DB_Schema.md
- Checkpoint doc

## Validation Checklist
- [ ] Checkpoint files exist
- [ ] Git log accurate
- [ ] Schema matches

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
- T420 (UAT Staging)

## Prompt Preview (see SKILL file for full prompt)
> Tổng hợp tất cả các commit git/repo vào file STG_VersionCommit.md với tên thêm hậu tố {version}_{date} (v3_190326)
> Export ra DB schema ra file : STG_DB_Schema_{version}_{date}...

## Global Rules
Read `sepo_skill/CLAUDE.md` (L1) for Iron Rules, Git conventions, Tech Stack, SEPO API reference.
