# CLAUDE.md — Task T530: Production Checkpoint

## Task Identity
- **Code:** T530 | **Phase:** T5 — Production | **Type:** A | **Team:** Tech Writer + DevOps

## Objective
Luu checkpoint production: version commits, DB schema, final documentation.

## Preconditions
- Production deployed va stable (T510)

## Input Docs
- **Git log** | **Production DB schema**

## Output Contract
- **PRD_VersionCommit.md** — All commits | **PRD_DB_Schema.md** — Production schema snapshot

## Instructions
1. Tong hop tat ca commits vao file PRD_VersionCommit.md (hau to vxx_date)
2. Export DB schema ra file PRD_DB_Schema_{version}_{date}.md
3. Document production URL, version, date, deployment notes

## References
- @docs/vault.md — load for DB access
