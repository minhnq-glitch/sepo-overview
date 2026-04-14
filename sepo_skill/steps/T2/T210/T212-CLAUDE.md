# CLAUDE.md — T212 — Main: Build ArchitecturePack (L3: Step)

> **Level:** L3 — Downloaded when user starts step T212.
> **Download:** Into project folder, overrides project CLAUDE.md.
> **Activate:** Injected into all prompts while executing step T212.

---

## Objective
Tao ArchitecturePack day du cho du an: ERD, Process, Wireframe, UserFlows, Complex Logic, Feature & Layer, Task Classification.

## Expert Team
Software Architect + Technical Writer + Database Architect

## Preconditions (MUST be true before starting)
- L1, L2, L3 docs co
- DB schema da review (T211)
- Codebase accessible

## Output Contract (MUST deliver these)
- ArchitecturePack.md gom 7+ sections:
-   1. EntityRelationshipDescription (ERD + schema details)
-   2. ProcessDescription (process flow, status machines)
-   3. UIWireFrame (ASCII wireframes moi man hinh)
-   4. UserFlows (sequence diagrams actor-system-backend)
-   5. Complex Logic (algorithms, calculations, rules)
-   6. Feature & Layer Mapping (feature → code → DB)
-   7. Task Classification (A/B/C phan loai)
- Moi section phai co du detail de developer implement duoc

## Validation Checklist
- [ ] Moi entity co trong ERD
- [ ] Moi workflow co user flow diagram
- [ ] Moi screen co wireframe
- [ ] Task classification cover TAT CA features
- [ ] AP du chi tiet de developer implement ma khong can hoi them

## Failure Cases (watch out)
- AP thieu section → developer khong du thong tin
- ERD khong match code → implementation se sai
- Task classification sai → sai approval flow

## Branching Logic
- Neu update AP cu → chi them "Whats new" section, giu nguyen phan cu
- Neu tao AP moi → tao day du tat ca sections
- Neu he thong co event layer → them Special Mechanisms section

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
- ArchitecturePack file created voi 7+ sections
- User reviewed va approved
- File saved vao docs/

## Next Step
- T220 (Tao Incremental Steps Plan)

## Prompt Preview (see SKILL file for full prompt)
> Hãy đề xuất ArchitecturePack  cho hệ thống này:
> 1. EntityRelationshipDescription
> 2. ProcessDescription
> 3. UIWireFrame
> 4. UserFlows
> 5. Complex Logic
> 6. Feature & Layer
> 7 Task classification
>     - Hãy đọc file [layerevent.md] để phân loại Task do user cung cấp theo tiêu chí sau:
>     - Loại A: Nếu chỉ ...

## Global Rules
Read `sepo_skill/CLAUDE.md` (L1) for Iron Rules, Git conventions, Tech Stack, SEPO API reference.
