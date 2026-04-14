# CLAUDE.md — T140 — Create Project Guide (L3: Step)

> **Level:** L3 — Downloaded when user starts step T140.
> **Download:** Into project folder, overrides project CLAUDE.md.
> **Activate:** Injected into all prompts while executing step T140.

---

## Objective
Tao tai lieu huong dan du an cho developer moi join: setup, conventions, architecture overview.

## Expert Team
Technical Writer + Project Manager

## Preconditions (MUST be true before starting)
- L1, L2, L3 da hoan thanh

## Output Contract (MUST deliver these)
- ProjectGuide.md: overview, setup, conventions, architecture, team roles

## Validation Checklist
- [ ] Guide du de developer moi setup ma khong can hoi them

## Failure Cases (watch out)
- Guide thieu steps → developer mac ket khi setup

## Branching Logic
- Neu du an dung mono-repo → ghi ro cach navigate
- Neu multi-repo → list repos va muc dich

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
- ProjectGuide.md created
- Tested by following guide on clean machine

## Next Step
- T2 (AP & ISP) — xay kien truc

## Global Rules
Read `sepo_skill/CLAUDE.md` (L1) for Iron Rules, Git conventions, Tech Stack, SEPO API reference.
