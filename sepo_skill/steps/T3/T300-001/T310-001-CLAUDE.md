# CLAUDE.md — T310-001 — Chay Build Prompt — Implement tung step (L3: Step)

> **Level:** L3 — Downloaded when user starts step T310-001.
> **Download:** Into project folder, overrides project CLAUDE.md.
> **Activate:** Injected into all prompts while executing step T310-001.

---

## Objective
Implement code cho 1 step trong ISP: viet tests truoc (TDD), implement, verify.

## Expert Team
Senior Engineer + Code Reviewer

## Preconditions (MUST be true before starting)
- ISP Audited da co
- Previous steps da complete
- Dev environment ready

## Output Contract (MUST deliver these)
- Test cases written va passing
- Code implemented theo dung scope cua step
- Build passes (npm run check)
- No regressions (existing tests still pass)

## Validation Checklist
- [ ] ALL new tests pass
- [ ] ALL existing tests still pass
- [ ] npm run check thanh cong
- [ ] Code changes chi trong scope cua step

## Failure Cases (watch out)
- Tests fail va khong fix duoc → co the ISP step scope sai
- TypeScript errors → import/type issues
- Regression → code moi break code cu

## Branching Logic
- Neu test fail sau 3 lan fix → bao cao va hoi user
- Neu step qua lon → de xuat tach thanh sub-steps
- Neu phat hien bug o step truoc → tao bug report, khong fix (stay in scope)

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
- ALL tests pass
- Build succeeds
- Code in scope
- Ready for T320 (test prompt)

## Next Step
- T320-001 (Chay Test Prompt) — verify step vua build

## Prompt Preview (see SKILL file for full prompt)
> You are acting as a senior engineer doing disciplined incremental development.
> 
> Rules:
> - One small change per step
> - No speculative features
> - Tests are mandatory
> - If tests fail, fix before proceeding
> - Ask for approval between steps
> 
> Output format:
> 1. Test cases
> 2. Implementation
> 3. How to run tes...

## Global Rules
Read `sepo_skill/CLAUDE.md` (L1) for Iron Rules, Git conventions, Tech Stack, SEPO API reference.
