# CLAUDE.md — T390 — Eval BApp voi AP — Post-implementation Audit (L3: Step)

> **Level:** L3 — Downloaded when user starts step T390.
> **Download:** Into project folder, overrides project CLAUDE.md.
> **Activate:** Injected into all prompts while executing step T390.

---

## Objective
So sanh code da build vs ArchitecturePack: tim discrepancies, gaps, issues.

## Expert Team
Software Architect + Code Auditor + QA Lead

## Preconditions (MUST be true before starting)
- Code da build xong (T380)
- AP co san

## Output Contract (MUST deliver these)
- PostAudit report:
-   - AP vs Code comparison
-   - Gaps found (feature trong AP nhung code chua co)
-   - Discrepancies (code khac AP)
-   - Issues va recommendations

## Validation Checklist
- [ ] Moi AP section da compared
- [ ] Gaps listed voi severity

## Failure Cases (watch out)
- Major gaps found → khong nen deploy

## Branching Logic
- Neu gaps nhieu → quay lai T3 de fix
- Neu chi minor → document va continue deploy

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
- Audit report created
- User reviewed findings

## Next Step
- T4 (Deploy staging & UAT)

## Prompt Preview (see SKILL file for full prompt)
> You are a senior software architect and code auditor.
> 
> Your task is to perform a post-implementation audit.
> 
> MANDATORY:
> - You MUST read and strictly follow rules defined in CLAUDE.md
> - You MUST NOT ignore any constraints or conventions from CLAUDE.md
> 
> INPUT:
> 1. Ready deeply [Architectural Pack_versi...

## Global Rules
Read `sepo_skill/CLAUDE.md` (L1) for Iron Rules, Git conventions, Tech Stack, SEPO API reference.
