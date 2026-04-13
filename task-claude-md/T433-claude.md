# CLAUDE.md — Task T433: Fix C-Type Issues (Performance/Core)

## Task Identity
- **Code:** T433 | **Phase:** T4 — Post-Eval | **Type:** C (needs POSUP + ARCH) | **Team:** Perf Engineer + Architect

## Objective
Fix C-type feedback: performance optimization, core data layer changes. Can POSUP + ARCH duyet.

## Preconditions
- T423 (classification) da xong | C-type items exist | **POSUP + ARCH da duyet**

## Input Docs
- **Classified C-type feedback** | **ArchitecturePack.md** | **layerevent.md**

## Output Contract
- C-type items fixed | Performance improved | Core layers stable

## Instructions
1. **Bao POSUP va ARCH de duyet truoc khi thuc thi**
2. Phan tich performance bottlenecks
3. Fix tung item voi can than (touch Event/CalculateKR layers)
4. Benchmark before/after
5. Run full test suite + performance tests

## References
- @docs/ArchitecturePack.md — load always
- @docs/layerevent.md — load for layer classification
