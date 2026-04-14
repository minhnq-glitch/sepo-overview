# CLAUDE.md — T421 — Chuan bi data va testcase (L3: Step)

> **Level:** L3 — Downloaded when user starts step T421.
> **Download:** Into project folder, overrides project CLAUDE.md.
> **Activate:** Injected into all prompts while executing step T421.

---

## Objective
Chuan bi test data va viet test cases tu AP specs cho UAT.

## Expert Team
QA Lead + Test Data Engineer

## Preconditions (MUST be true before starting)
- Staging deployed
- AP da co

## Output Contract (MUST deliver these)
- Test cases document
- Test data ready on staging
- Test accounts created

## Validation Checklist
- [ ] Moi feature co test case
- [ ] Test data ton tai tren staging

## Failure Cases (watch out)
- Test data corrupt
- Staging down

## Branching Logic
- Neu staging khong co data → seed data truoc

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
- Test cases written
- Data ready

## Next Step
- T422 (UAT va feedback)

## Global Rules
Read `sepo_skill/CLAUDE.md` (L1) for Iron Rules, Git conventions, Tech Stack, SEPO API reference.
