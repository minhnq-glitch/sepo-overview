# CLAUDE.md — Task T415: Staging Checkpoint

## Task Identity
- **Code:** T415 | **Phase:** T4 — Deploy & UAT | **Type:** A | **Team:** Tech Writer + DevOps

## Objective
Luu checkpoint staging: version commits, DB schema snapshot.

## Preconditions
- T414 (staging deploy) da thanh cong

## Input Docs
- **Git log** | **Staging DB schema**

## Output Contract
- **STG_VersionCommit.md** — Tat ca commits | **STG_DB_Schema.md** — Schema snapshot

## Instructions
1. Tong hop tat ca commits vao file STG_VersionCommit.md (voi hau to vxx_date)
2. Export DB schema ra file STG_DB_Schema_{version}_{date}.md
3. Document staging URL, version, date

## References
- @docs/vault.md — load for DB access
