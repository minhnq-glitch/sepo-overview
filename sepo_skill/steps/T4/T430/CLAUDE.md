# CLAUDE.md — T430: Post-Eval Tasks (L2: Sub-phase)

> **Level:** L2 — Post-eval sub-phase of T4.
> **Steps:** T431 (A Tasks) + T432 (B Tasks) + T433 (C Tasks) — can run in parallel

## Task Types
| Type | Scope | Expert Team | May need migration? |
|------|-------|-------------|-------------------|
| T431 A | UI only | Frontend + UX | No |
| T432 B | Business logic | Backend + BA | Yes → run T411 again |
| T433 C | Performance | Perf Eng + Architect | No |

## After ALL fixes → re-deploy (loop back to T410)
