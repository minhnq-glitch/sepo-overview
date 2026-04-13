# CLAUDE.md — Task T380: Finish ISP — Commit & Push

## Task Identity
- **Code:** T380
- **Name:** Hoan thanh Incremental Step Plan
- **Phase:** T3 — Build BApp
- **Classification:** Type A
- **Expert Team:** DevOps Engineer + Technical Writer

## Objective
Dong goi ket qua build: update ISP status, commit code, push to git, chuan bi cho deployment.

## Preconditions
- Tat ca ISP steps da build (T310) va test (T320)

## Input Docs (Required)
- **ISP file** — De update status
- **vault.md** — Git credentials
- **GIT-DB-Structure.md** — Repo naming conventions

## Output Contract
- ISP updated: all steps marked complete
- Code committed va pushed to remote
- Merge Request created
- Ready for deploy

## Instructions

1. Doc file vault.md de lay credential cho git
2. Dua vao quy luat dat ma va ten he thong trong GIT-DB-Structure.md:
   - Tu dau tien la doi tuong nguoi dung chinh (VD: Student, Teacher, Parent)
   - Tu thu 2 la chuc nang chinh he thong (VD: Tutoring, Education, Report)
   - Tu thu 3 la giao dien chinh (VD: Web, App, Portal)
   - Ma duoc ghep tu nhung chu cai dau cua cac ten
3. Merge code backend va frontend vao chung 1 repo
4. Push code len git: `https://gitlab.clevai.vn/fe/[repo name]`
5. Tao Merge Request len `staging-c`
6. Fix CI/CD build fail
7. Check code local va push
8. Chuyen sang nhanh `staging-c` de lam viec

## Validation Checklist
- [ ] Code pushed to remote
- [ ] Merge Request created
- [ ] CI/CD build passes
- [ ] All tests pass on CI

## Next Step
T390 — Eval BApp vs AP (Post-implementation Audit)

## References
- @docs/vault.md — load for git credentials
- @docs/GIT-DB-Structure.md — load for naming conventions
