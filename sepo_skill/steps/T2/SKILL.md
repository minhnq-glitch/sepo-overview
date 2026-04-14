---
name: sepo-t2
description: "T2 AP & ISP — 4 skills: T211 DB Schema, T212 Build AP, T220 Create ISP, T230 Audit ISP"
---

# Phase T2: Architecture Pack & Incremental Step Plan

## Skills trong phase này

| Step | Skill | Expert Team | Input | Output |
|------|-------|-------------|-------|--------|
| T211 | Prep DB Schema | DB Architect + Backend | L1/AP draft | migration.sql, ERD |
| T212 | Build AP | Architect + Writer + DB | L1, L2, L3, schema | ArchitecturePack.md |
| T220 | Tạo ISP | Program Lead + Architect + QA | AP | IncrementalStepPlan.md |
| T230 | Audit ISP | ISP Auditor + Architect | ISP + AP | ISPAudited.md |

## Flow
```
T211 (DB prep) → T212 (Build AP) → T220 (Create ISP) → T230 (Audit ISP)
```

## Sub-phase: T210 Dựng ArchitecturePack
- T211: Chuẩn bị DB schema trước
- T212: Build AP dựa trên L1-L3 + schema

## Download từ SEPO
- T211: L1 docs, existing schema
- T212: L1, L2, L3, schema, layerevent.md
- T220: ArchitecturePack
- T230: ISP + AP

## Upload lên SEPO
- T211 → migration.sql
- T212 → ArchitecturePack_{version}.md
- T220 → IncrementalStepPlan_{version}.md
- T230 → ISPAudited_{version}.md
