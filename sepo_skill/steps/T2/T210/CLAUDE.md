# CLAUDE.md — T210: Dung ArchitecturePack (L2: Sub-phase)

> **Level:** L2 — Sub-phase of T2 (Architecture).
> **Steps:** T211 (Prep DB Schema) → T212 (Build AP)
> **Rule:** T211 MUST complete before T212 starts (schema informs AP).

## Flow
```
T211: Analyze entities → design schema → write migration.sql
  ↓
T212: Read L1+L2+L3+schema → write ArchitecturePack (7 sections)
```

## Key constraint: AP ERD MUST match migration.sql schema exactly.
