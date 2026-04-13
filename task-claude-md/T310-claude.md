# CLAUDE.md — Task T310: Build — Implement Code per ISP Step

## Task Identity
- **Code:** T310-001
- **Name:** Chay Build Prompt — Implement tung step
- **Phase:** T3 — Build BApp (iterates per ISP step)
- **Classification:** Type A/B (depends on step)
- **Expert Team:** Senior Engineer + Code Reviewer

## Objective
Implement code cho 1 step trong ISP: viet tests truoc (TDD), implement, verify.

## Preconditions
- ISP Audited da co (T230)
- Previous ISP steps da complete
- Dev environment ready

## Input Docs (Required)
- **IncrementalStepPlanAudited.md** — ISP step hien tai
- **ArchitecturePack.md** — Section lien quan den step
- **Codebase hien tai**

## Output Contract
- Test cases written va passing
- Code implemented theo dung scope cua step
- Build passes (`npm run check`)
- No regressions (existing tests still pass)

## Instructions

You are acting as a senior engineer doing disciplined incremental development.

### Rules:
- **One small change per step** — do NOT implement future steps
- **No speculative features** — only what the step requires
- **Tests are mandatory** — write tests BEFORE implementation
- **If tests fail, fix before proceeding**
- **Ask for approval between steps**

### Process:
1. Read the current ISP step carefully
2. Write test cases based on acceptance criteria
3. Implement the minimum code to pass tests
4. Run `npm run check` to verify build
5. Run all tests to verify no regressions
6. Document known limitations

### Output Format:
1. Test cases created
2. Implementation details
3. How to run tests
4. Known limitations
5. Ask whether to continue to next step

## Validation Checklist
- [ ] Tests written BEFORE implementation
- [ ] All new tests pass
- [ ] All existing tests still pass
- [ ] `npm run check` passes
- [ ] Code matches ISP step scope exactly

## Next Step
T320-001 — Test (verify step vua build)

## References
- @docs/IncrementalStepPlanAudited.md — load always
- @docs/ArchitecturePack.md — load for relevant section
- @docs/Clevai_Tech_Stack_Prefer.md — load when choosing libraries
