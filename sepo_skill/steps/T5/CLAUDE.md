# CLAUDE.md — Phase T5: Production (L2: Phase)

> **Level:** L2 — Lazy loaded when user enters Production phase.
> **Download:** Into project folder.
> **Activate:** When Claude detects production context (production, deploy prod, go live).

---

## Scope
Deploy to production and create final checkpoints.

## Steps
| Step | Name | Expert Team |
|------|------|-------------|
| T510 | Deploy Production | DevOps + SRE + DB Architect |
| T520 | Post Production | Monitoring + Support |
| T530 | Checkpoint PRD | Tech Writer + DevOps |

## Production Rules (CRITICAL — no mistakes allowed)
1. **BACKUP production DB FIRST** — no exceptions
2. Run migration dry-run before actual execution
3. **Zero-downtime deployment** required
4. Rollback plan MUST exist before deploying
5. Monitor for 30 minutes after deploy
6. No direct DB edits — always via migration scripts
7. No `--force` push to production branch

## Checkpoint Format
- `PRD_VersionCommit_{date}.md` — all git commits
- `PRD_DB_Schema_{date}.md` — exported schema snapshot
- Store in `docs/` folder in repo

## After T530 → PROJECT COMPLETE! 🎉
Or loop back to T1 for next version.
