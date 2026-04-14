# CLAUDE.md — Phase T2: Architecture Pack & ISP (L2: Phase)

> **Level:** L2 — Lazy loaded when user enters Architecture phase.
> **Download:** Into project folder.
> **Activate:** When Claude detects architecture context (AP, ISP, architecture, schema, plan).

---

## Scope
Create the full technical architecture document (ArchitecturePack) and break it into implementable steps (IncrementalStepPlan).

## Steps in this phase
| Step | Name | Expert Team | Output |
|------|------|-------------|--------|
| T211 | Prep: Update DB Schema | DB Architect + Backend | migration.sql, ERD |
| T212 | Main: Build ArchitecturePack | Architect + Writer + DB | ArchitecturePack_{ver}.md |
| T220 | Tao ISP | Program Lead + Architect + QA | IncrementalStepPlan_{ver}.md |
| T230 | Audit ISP | ISP Auditor + Architect | ISPAudited_{ver}.md |

## ArchitecturePack — 7 MANDATORY Sections

Every AP MUST contain these 7 sections. Missing section = incomplete AP.

### Section 1: EntityRelationshipDescription
- ERD diagram (ASCII)
- Every entity: table name, columns, types, constraints
- Foreign keys and indexes
- Layer classification (A/B/C)

### Section 2: ProcessDescription
- Business processes listed
- Status flows (state machines)
- Phase/step mapping

### Section 3: UIWireFrame
- ASCII wireframe for EVERY screen
- Component hierarchy
- Mobile + desktop if responsive

### Section 4: UserFlows
- Sequence diagrams (ASCII) for EVERY use case
- Format: Actor → System (Browser) → Backend
- Include happy path + error cases

### Section 5: Complex Logic
- Algorithms with pseudocode
- Business rules with examples
- Calculations with formulas
- Edge cases documented

### Section 6: Feature & Layer Mapping
- Table: Feature → Controller → Service → Tables Read → Tables Write
- System → API endpoint mapping

### Section 7: Task Classification
- Read layerevent.md
- Classify EVERY feature as A/B/C
- Feature inventory table

## ISP Rules (for T220, T230)
- Every step must be implementable in 1 coding session
- TDD: write tests FIRST
- Order: schema → backend → frontend → integration
- Each step needs: scope, files, test criteria, dependencies
- No circular dependencies
- Coverage: 100% of AP features must have ISP steps

## Download from SEPO
| Step | Docs needed |
|------|------------|
| T211 | L1 docs or AP draft |
| T212 | L1, L2, L3, DB schema, layerevent.md |
| T220 | ArchitecturePack |
| T230 | ISP + AP |

## Upload to SEPO
| Step | Output |
|------|--------|
| T211 | migration.sql |
| T212 | ArchitecturePack_{ver}.md |
| T220 | IncrementalStepPlan_{ver}.md |
| T230 | ISPAudited_{ver}.md |

## After this phase → T3 (Build BApp)
