# CLAUDE.md — Phase T4: Deploy Staging & UAT (L2: Phase)

> **Level:** L2 — Lazy loaded when user enters Deploy phase.
> **Download:** Into project folder.
> **Activate:** When Claude detects deploy/test context (deploy, migration, UAT, staging, test feedback).

---

## Scope
Deploy to staging, run UAT, classify feedback, execute fixes. Three sub-phases:
- **T410**: Deploy Staging (5 steps: migrate → local test → push → deploy → checkpoint)
- **T420**: UAT Staging (3 steps: prepare → execute → classify)
- **T430**: Post-Eval Tasks (3 types: A=UI, B=Business, C=Performance)

## Steps Summary
| Sub | Steps | Flow |
|-----|-------|------|
| T410 | T411→T412→T413→T414→T415 | Sequential (each depends on previous) |
| T420 | T421→T422→T423 | Sequential |
| T430 | T431 + T432 + T433 | Parallel by type (A/B/C) |

## Deploy Rules (CRITICAL)
1. **ALWAYS backup DB before migration** (`pg_dump`)
2. Migration MUST be reversible (include rollback script)
3. Test on localhost BEFORE pushing to staging
4. Health check after EVERY deploy: `curl /api/external/health`
5. No secrets in git commits
6. Checkpoint docs after successful deploy

## UAT Rules
- Use T720_TestFeedback_Template for reporting
- Every bug: description + steps to reproduce + expected vs actual + screenshot
- Severity: Critical > High > Medium > Low
- **Critical bugs BLOCK deployment**

## Feedback Classification (T423)
Read `layerevent.md` for rules:
| Type | Scope | When |
|------|-------|------|
| A | UI only — no backend changes | Frontend CSS/layout/styling |
| B | Business logic — may need migration | API, DB, validation rules |
| C | Performance — needs profiling | Query optimization, caching |

## After fixes → re-deploy (loop back to T410)
## After all clear → T5 (Production)
