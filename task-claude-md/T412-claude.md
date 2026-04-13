# CLAUDE.md — Task T412: Deploy Local & Smoke Test

## Task Identity
- **Code:** T412 | **Phase:** T4 — Deploy & UAT | **Type:** A | **Team:** DevOps + QA

## Objective
Deploy app locally, run smoke tests de verify basic functionality.

## Preconditions
- T411 (migration) da xong | Code da build (T380)

## Input Docs
- **Codebase** | **.env** configuration

## Output Contract
- App running locally | Smoke tests pass | No critical errors

## Instructions
1. Start dev server (`npm run dev`)
2. Verify all pages load without errors
3. Test basic CRUD operations
4. Check API endpoints respond correctly
5. Verify database connections work
6. Document any issues found

## References
- @docs/vault.md — load for credentials
