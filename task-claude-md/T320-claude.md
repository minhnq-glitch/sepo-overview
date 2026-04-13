# CLAUDE.md — Task T320: Test — Verify Step

## Task Identity
- **Code:** T320-001
- **Name:** Chay Test Prompt — Verify step
- **Phase:** T3 — Build BApp (iterates per ISP step)
- **Classification:** Type A
- **Expert Team:** QA Engineer + Security Tester

## Objective
Kiem tra ky luong code vua build: happy path, edge cases, error handling, security.

## Preconditions
- T310 da complete (code + tests exist)
- Build passes

## Input Docs (Required)
- **ISP step** + test criteria
- **Code vua build**
- **Existing test suite**

## Output Contract
- Test report: pass/fail cho moi test case
- Edge cases tested
- Known limitations documented
- Security issues found (neu co)

## Instructions

You are acting as a disciplined senior engineer practicing strict incremental development.

The step description is the SINGLE source of truth.

### GLOBAL RULES:
- One step only; do not implement future steps
- No speculative features or refactors
- Do not change scope defined in the step
- Minimal code to satisfy tests
- Deterministic, headless, automatable tests only
- Stop when exit criteria are met

### PROCESS (follow in order):
1. **TEST DESIGN** — Write/review test cases covering all acceptance criteria
2. **HAPPY PATH** — Verify main flow works
3. **EDGE CASES** — Test boundary conditions
4. **ERROR HANDLING** — Test invalid inputs, network failures
5. **SECURITY** — Check for injection, XSS, auth bypass
6. **REGRESSION** — Run full test suite

## Validation Checklist
- [ ] All acceptance criteria tested
- [ ] Edge cases documented and tested
- [ ] No security vulnerabilities found
- [ ] Full test suite passes
- [ ] Known limitations documented

## Next Step
T310-001 (next ISP step) hoac T380 (neu da xong tat ca steps)

## References
- @docs/IncrementalStepPlanAudited.md — load for test criteria
