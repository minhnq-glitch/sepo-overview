# CLAUDE.md — Phase T1: Ve Voi / System Analysis (L2: Phase)

> **Level:** L2 — Lazy loaded when user enters Analysis phase.
> **Download:** Into project folder.
> **Activate:** When Claude detects analysis context (L1, L2, L3, voi, workflow, entity).

---

## Scope
Analyze the system and produce 3 levels of documentation:
- **L1: Voi Tong** — Entity overview (what exists in the system)
- **L2: Workflows** — Business processes (how things flow)
- **L3: Workflow Details** — Step-by-step details (exact data, UI, API per step)

## Steps in this phase
| Step | Name | Expert Team | Output |
|------|------|-------------|--------|
| T110 | L1: Voi Tong | BA + System Architect | L1_VoiTong.md |
| T120 | L2: Cac Workflow | BA + Process Designer | L2_Workflows.md |
| T130 | L3: Chi tiet Workflow | BA + UX + Backend | L3_WorkflowDetails.md |
| T140 | Create Project Guide | Tech Writer + PM | ProjectGuide.md |

## VoiTable Format (mandatory for L1, L2)
```markdown
| Entity | Layer | Description | Key Relationships |
|--------|-------|-------------|-------------------|
| User   | A     | System user | 1-N → Project     |
```

## Entity Layer Classification (MANDATORY)
| Layer | Type | Description | Changes how often? |
|-------|------|-------------|-------------------|
| **A** | Reference/Master | Base data (users, config) | Rarely |
| **B** | Plan/Config | Business plans, settings | Monthly |
| **C** | Transaction | Operational data | Daily/hourly |

## Naming Conventions
- Entity names: **PascalCase** (User, ProjectTask, DocumentVersion)
- Column names: **snake_case** (created_at, owner_id, display_name)
- Relationship cardinality: always specify (1-1, 1-N, N-N)

## Rules
1. Every entity MUST have Layer classification (A/B/C)
2. Every relationship MUST have cardinality
3. No orphan entities (every entity has >= 1 relationship)
4. L2 workflows must map to L1 entities (coverage table required)
5. L3 details must include: input, output, actor, UI screen, API call

## Download from SEPO
| Step | Docs to download |
|------|-----------------|
| T110 | VOI / requirement docs |
| T120 | L1_VoiTong.md (output of T110) |
| T130 | L1 + L2 docs |
| T140 | L1 + L2 + L3 docs |

## Upload to SEPO after completion
| Step | Output to upload |
|------|-----------------|
| T110 | L1_VoiTong.md |
| T120 | L2_Workflows.md |
| T130 | L3_WorkflowDetails.md |
| T140 | ProjectGuide.md |

## After this phase → T2 (Architecture Pack & ISP)
