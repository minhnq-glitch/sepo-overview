# CLAUDE.md — T422 — UAT va feature feedback (L3: Step)

> **Level:** L3 — Downloaded when user starts step T422.
> **Download:** Into project folder, overrides project CLAUDE.md.
> **Activate:** Injected into all prompts while executing step T422.

---

## Objective
Thuc hien User Acceptance Testing, ghi nhan bugs va feedback.

## Expert Team
QA Tester + UX Reviewer

## Preconditions (MUST be true before starting)
- Test cases ready (T421)
- Staging deployed

## Output Contract (MUST deliver these)
- TestFeedback file: moi bug/feedback gom mo ta, steps to reproduce, severity, evidence

## Validation Checklist
- [ ] ALL test cases executed
- [ ] Moi bug co steps to reproduce

## Failure Cases (watch out)
- Staging down during UAT

## Branching Logic
- Neu critical bug → block release, bao team ngay

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
- All test cases executed
- Feedback file complete

## Next Step
- T423 (Classify feedback)

## Prompt Preview (see SKILL file for full prompt)
> Download và sử dụng file T720_TestFeedback_Template để điền thông tin UAT...

## Global Rules
Read `sepo_skill/CLAUDE.md` (L1) for Iron Rules, Git conventions, Tech Stack, SEPO API reference.
