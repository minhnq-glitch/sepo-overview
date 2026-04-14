# CLAUDE.md — T320-001 — Chay Test Prompt — Verify step vua build (L3: Step)

> **Level:** L3 — Downloaded when user starts step T320-001.
> **Download:** Into project folder, overrides project CLAUDE.md.
> **Activate:** Injected into all prompts while executing step T320-001.

---

## Objective
Kiem tra ky luong code vua build: happy path, edge cases, error handling, security.

## Expert Team
QA Engineer + Security Tester

## Preconditions (MUST be true before starting)
- T310 da complete (code + tests exist)
- Build passes

## Output Contract (MUST deliver these)
- Test report: pass/fail cho moi test case
- Edge cases tested
- Known limitations documented
- Security issues found (neu co)

## Validation Checklist
- [ ] ALL test cases executed
- [ ] Pass rate >= 90%
- [ ] Critical bugs = 0
- [ ] Known limitations documented

## Failure Cases (watch out)
- Critical bug found → block deployment
- Test infrastructure fail

## Branching Logic
- Neu critical bug found → quay lai T310 de fix
- Neu chi minor issues → document va continue

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
- ALL tests executed
- Results documented
- No critical bugs

## Next Step
- T310-001 (next ISP step) hoac T380 (hoan thanh ISP)

## Prompt Preview (see SKILL file for full prompt)
> You are acting as a disciplined senior engineer practicing strict incremental development.
> 
> The step description above [Step.....] is the SINGLE source of truth.
> 
> Your task is to implement this step using automated tests and a tight feedback loop.
> 
> GLOBAL RULES:
> - One step only; do not implement fut...

## Global Rules
Read `sepo_skill/CLAUDE.md` (L1) for Iron Rules, Git conventions, Tech Stack, SEPO API reference.
