---
name: sepo-t300-001
description: "T300-001 - Build BApp tung step"
---

# Skill: T300-001 — Build BApp tung step

**Expert Team:** Senior Engineer + Code Reviewer

## Objective
Umbrella step: iterate qua cac ISP steps, goi T310 (build) + T320 (test) cho tung step.

**Scope:** Coding, TDD, Implementation per ISP step

## Trigger condition
Khi bat dau build 1 step cu the. Keywords: "build prompt", "chay build", "implement step"

## Preconditions
- ISP Audited da co
- Previous steps da complete
- Dev environment ready

## Inputs
### Required
- ISP step hien tai (scope, files, test criteria)
- ArchitecturePack section lien quan
- Codebase hien tai
### Optional
- Previous step code (de hieu context)

## Output Contract
- Test cases written va passing
- Code implemented theo dung scope cua step
- Build passes (npm run check)
- No regressions (existing tests still pass)

## Internal Reasoning Checklist
- [ ] Step nay can tao files moi nao?
- [ ] Step nay sua files nao existing?
- [ ] Test cases cover het acceptance criteria?
- [ ] Code co follow project conventions?

## Execution Procedure
1. Doc ISP step hien tai — hieu scope va test criteria
2. Doc AP section lien quan — hieu requirements chi tiet
3. Viet test cases TRUOC (TDD): unit tests + integration tests
4. Implement code de tests pass
5. Chay ALL tests (khong chi tests moi)
6. Neu test fail → fix → chay lai
7. Verify: npm run check (no TypeScript errors)
8. Output: code changes + test files + how to run tests

## Branching Logic
- Neu test fail sau 3 lan fix → bao cao va hoi user
- Neu step qua lon → de xuat tach thanh sub-steps
- Neu phat hien bug o step truoc → tao bug report, khong fix (stay in scope)

## Validation Checklist
- [ ] ALL new tests pass
- [ ] ALL existing tests still pass
- [ ] npm run check thanh cong
- [ ] Code changes chi trong scope cua step

## Failure Cases
- Tests fail va khong fix duoc → co the ISP step scope sai
- TypeScript errors → import/type issues
- Regression → code moi break code cu

## Recovery Actions
- Test fail → doc error, fix root cause, khong skip test
- TS error → check imports, types
- Regression → git diff de tim code moi gay ra, revert neu can

## Completion Criteria
- ALL tests pass
- Build succeeds
- Code in scope
- Ready for T320 (test prompt)

## Next-Step Handoff
- T310-001 (Build Prompt cho step dau tien)

## Reusable Prompt Block
```text
Ban la team chuyen gia gom Senior Engineer + Code Reviewer.
Nhiem vu: Umbrella step: iterate qua cac ISP steps, goi T310 (build) + T320 (test) cho tung step.
Hay thuc hien theo Execution Procedure o tren.
```
