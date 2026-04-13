# CLAUDE.md — Task T414: Deploy Staging

## Task Identity
- **Code:** T414 | **Phase:** T4 — Deploy & UAT | **Type:** A | **Team:** DevOps + SRE

## Objective
Deploy app to staging environment, verify health check.

## Preconditions
- T413 (git push + CI) da pass

## Input Docs
- **vault.md** — Staging credentials | **Docker/deploy configs**

## Output Contract
- Staging environment live | Health check pass | URL accessible

## Instructions
1. Deploy to staging server
2. Verify health check endpoint responds
3. Test critical user flows on staging
4. Document staging URL and access info
5. Tao domain theo ten da chon: `[repo name].mikai.tech`

## References
- @docs/vault.md — load for staging credentials
