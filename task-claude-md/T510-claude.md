# CLAUDE.md — Task T510: Deploy Production

## Task Identity
- **Code:** T510 | **Phase:** T5 — Production | **Type:** A | **Team:** DevOps + SRE + DB Architect

## Objective
Deploy app len production environment.

## Preconditions
- UAT pass | All critical bugs fixed | Staging verified

## Input Docs
- **vault.md** — Production credentials | **migration.sql** (neu co)

## Output Contract
- App live on production | Health check pass | No errors in monitoring

## Instructions
1. Neu database co change: tong hop vao file `changedb.sql`
2. Neu co bo sung key/token: tong hop vao file `changevault.md`
3. Tao merge request tu nhanh hien tai vao nhanh `prod-c`
4. Kem file changedb.sql va changevault.md (neu co)
5. Deploy va verify health check

## References
- @docs/vault.md — load for production credentials
- @docs/migration.sql — load if DB changes needed
