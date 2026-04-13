# CLAUDE.md — Task T220: Create Incremental Step Plans

## Task Identity
- **Code:** T220
- **Name:** Tao Incremental Steps Plans cho BApp
- **Phase:** T2 — Architecture Pack & ISP
- **Classification:** Type A
- **Expert Team:** Technical Program Lead + Software Architect + QA Lead

## Objective
Chia ArchitecturePack thanh cac buoc incremental (TDD approach) de developer implement tung step.

## Preconditions
- ArchitecturePack da hoan thanh va approved

## Input Docs (Required)
- **ArchitecturePack.md** — Full architecture document

## Output Contract
- **IncrementalStepPlan.md** gom:
  - Moi step: ID, scope, files affected, test criteria
  - Dependencies giua cac steps
  - Task classification (A/B/C) per step
  - Build prompt + Test prompt cho moi step

## Instructions

You are a senior staff engineer and technical program lead.

I have already completed system design documentation in ArchitecturePack:
- Section 1: EntityRelationshipDescription
- Section 2: ProcessDescription
- Section 3: UIWireFrame
- Section 4: UserFlows
- Section 5: Complex Logic
- Section 6: Feature & Layer
- Section 7: Task classification

Your task is NOT to write any production code yet.

Your task is to break this system into a sequence of SMALL, INCREMENTAL, BUILDABLE STEPS.

### Rules for each step:
1. **Atomic:** Each step builds ONE small piece of functionality
2. **Testable:** Each step has clear pass/fail criteria
3. **Independent:** Steps can be verified without future steps
4. **Ordered:** Steps respect dependencies (DB before API before UI)
5. **TDD:** Write tests BEFORE implementation

### Step Template:
```
### Step N: [Short Name]
- **Scope:** What exactly to build
- **Files:** Which files to create/modify
- **Dependencies:** Which previous steps required
- **Test Criteria:** How to verify this step passes
- **Classification:** A/B/C
```

## Validation Checklist
- [ ] Tat ca features trong AP da duoc cover
- [ ] Moi step co test criteria cu the
- [ ] Dependencies giua steps ro rang
- [ ] Khong co step qua lon (> 1 feature)

## Next Step
T230 — Audit ISP

## References
- @docs/ArchitecturePack.md — load always (primary input)
- @docs/Clevai_Tech_Stack_Prefer.md — load for library choices
