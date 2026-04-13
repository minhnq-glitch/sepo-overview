# CLAUDE.md — Task T413: Push Git & Verify CI

## Task Identity
- **Code:** T413 | **Phase:** T4 — Deploy & UAT | **Type:** A | **Team:** DevOps + Git

## Objective
Push code to remote, verify CI pipeline passes.

## Preconditions
- T412 (local test) da pass

## Input Docs
- **vault.md** — Git credentials | **GIT-DB-Structure.md** — Repo info

## Output Contract
- Code on remote | CI pipeline passed | No build failures

## Instructions
1. Hoi nguoi dung: "Usecase 1: Du an xay moi" hay "Usecase 2: Sua he thong co"
2. Commit tat ca changes voi message theo convention
3. Push to remote branch
4. Verify CI pipeline passes
5. Fix any CI failures

## References
- @docs/vault.md — load for git credentials | @docs/GIT-DB-Structure.md — load for repo info
