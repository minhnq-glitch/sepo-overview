# CLAUDE.md — Task T390: Post-Implementation Audit

## Task Identity
- **Code:** T390
- **Name:** Eval BApp voi AP — Post Audit
- **Phase:** T3 — Build BApp
- **Classification:** Type A
- **Expert Team:** Software Architect + Code Auditor + QA Lead

## Objective
So sanh code da build vs ArchitecturePack: tim discrepancies, gaps, issues. Day la **Quality Gate #2**.

## Preconditions
- Code da build xong (T380)
- AP co san

## Input Docs (Required)
- **ArchitecturePack.md** — Source of truth
- **IncrementalStepPlan.md** — Scope reference
- **Codebase** — Current implementation
- **layerevent.md** — Task classification rules

## Output Contract
- **PostAudit.md**:
  - AP vs Code comparison matrix
  - Gaps found (feature trong AP nhung code chua co)
  - Discrepancies (code khac AP)
  - Issues va recommendations

## Instructions

You are a senior software architect and code auditor.

MANDATORY: You MUST read and strictly follow rules defined in CLAUDE.md.

### TASK:
1. **COMPARISON AUDIT** — Compare each AP section with actual code
2. **SCHEMA AUDIT** — Does DB schema match ERD?
3. **API AUDIT** — Do endpoints match specifications?
4. **UI AUDIT** — Do components match wireframes?
5. **LOGIC AUDIT** — Does business logic match algorithms?
6. **CLASSIFICATION** — Re-classify each feature (A/B/C) based on actual implementation

### Output Format:
| AP Section | Expected | Actual | Status | Notes |
|-----------|----------|--------|--------|-------|
| ERD | ... | ... | Match/Gap/Diff | ... |

## Validation Checklist
- [ ] All AP sections audited
- [ ] Gaps clearly documented
- [ ] Recommendations actionable
- [ ] No critical discrepancies unresolved

## Next Step
T411 — Run Migration (Phase T4)

## References
- @docs/ArchitecturePack.md — load always (source of truth)
- @docs/IncrementalStepPlanAudited.md — load for scope reference
- @docs/layerevent.md — load for classification rules
