# CLAUDE.md — Phase T3: Build BApp (L2: Phase)

> **Level:** L2 — Lazy loaded when user enters Build phase.
> **Download:** Into project folder.
> **Activate:** When Claude detects build context (build, implement, code, TDD, test prompt).

---

## Scope
Implement code per ISP steps using strict TDD approach. Each ISP step = T310 (build) + T320 (test).

## Steps in this phase
| Step | Name | Expert Team | Output |
|------|------|-------------|--------|
| T310-001 | Chay Build Prompt | Senior Eng + Reviewer | Code + Tests |
| T320-001 | Chay Test Prompt | QA + Security | Test results |
| T380 | Hoan thanh ISP | DevOps + Writer | ISP updated, code pushed |
| T390 | Eval BApp vs AP | Architect + Auditor + QA | PostAudit report |

## Build Rules (CRITICAL)

### TDD Flow (mandatory for every step)
```
1. Read ISP step → understand scope
2. Write tests FIRST (what should pass when done)
3. Implement code to make tests pass
4. Run ALL tests (not just new ones)
5. Fix failures → re-run
6. Verify: npm run check (TypeScript clean)
7. Output: code + tests + how to run
```

### One Step at a Time
- ONE small change per step
- No speculative features (YAGNI)
- If you discover a bug in previous step → report, don't fix (Iron Rule 1)
- Ask user for approval between steps

### Code Style
| Area | Convention |
|------|-----------|
| Backend routes | `server/routes/*.ts` |
| Business logic | `server/storage.ts` |
| DB schema | `shared/schema.ts` (Drizzle) |
| React components | `client/src/components/*.tsx` |
| Pages | `client/src/pages/*.tsx` |
| Hooks | `client/src/hooks/*.ts` |
| UI framework | Radix UI + shadcn/ui |
| Router | wouter |

### Test Conventions
| Type | Tool | Location |
|------|------|----------|
| Unit | vitest | tests/*.test.ts |
| API | supertest | tests/api/*.test.ts |
| Frontend | @testing-library/react | tests/frontend/*.test.ts |
| E2E | playwright | tests/e2e/*.spec.ts |

## Post-Build (T390)
- Compare AP vs code section by section
- List matches, gaps, discrepancies
- Each gap: severity (critical/high/medium/low)
- Output: PostAudit_{ver}.md

## Download from SEPO
| Step | Docs needed |
|------|------------|
| T310-001 | ISP Audited + AP section for current step |
| T320-001 | ISP step + code just built |
| T390 | AP + full codebase |

## After this phase → T4 (Deploy & UAT)
