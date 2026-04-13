# CLAUDE.md — Task T212: Build ArchitecturePack

## Task Identity
- **Code:** T212
- **Name:** Main: Build ArchitecturePack
- **Phase:** T2 — Architecture Pack & ISP
- **Classification:** Type A
- **Expert Team:** Software Architect + Technical Writer + Database Architect

## Objective
Tao ArchitecturePack day du cho du an: ERD, Process, Wireframe, UserFlows, Complex Logic, Feature & Layer, Task Classification.

## Preconditions
- L1, L2, L3 docs co
- DB schema da review (T211)
- Codebase accessible

## Input Docs (Required)
- **L1_VoiTong.md** — Entity overview
- **L2_Workflows.md** — Workflow descriptions
- **L3_WorkflowDetails.md** — Detailed steps
- **DB Schema** (tu T211)
- **layerevent.md** — De phan loai tasks A/B/C

## Output Contract
- **ArchitecturePack.md** gom 7 sections:
  1. EntityRelationshipDescription (ERD + schema details)
  2. ProcessDescription (process flow, status machines)
  3. UIWireFrame (ASCII wireframes moi man hinh)
  4. UserFlows (sequence diagrams actor-system-backend)
  5. Complex Logic (algorithms, calculations, rules)
  6. Feature & Layer Mapping (feature -> code -> DB)
  7. Task Classification (A/B/C phan loai)

## Instructions

Hay de xuat ArchitecturePack cho he thong nay:

### Section 1: EntityRelationshipDescription
- ERD diagram (ASCII)
- Moi entity: fields, types, constraints, relationships

### Section 2: ProcessDescription
- Process flows chinh
- Status machines (state transitions)
- Business rules

### Section 3: UIWireFrame
- ASCII wireframe cho moi man hinh
- Layout description
- Interactive elements

### Section 4: UserFlows
- Sequence diagrams (ASCII): Actor -> Frontend -> Backend -> Database
- Error flows
- Alternative flows

### Section 5: Complex Logic
- Algorithms, formulas
- Business calculations
- Validation rules phuc tap

### Section 6: Feature & Layer Mapping
- Feature -> Module -> Code files -> DB tables
- Dependency graph

### Section 7: Task Classification
Doc file layerevent.md de phan loai:
- **Loai A:** Chi chinh sua UI, API, khong sua Entity Schema
- **Loai B:** Co sua Entity Schema, nhung khong doc viet cac bang thuoc Layer Event/EventDetails/CalculateKR/ExtractEvent
- **Loai C:** Co sua/doc/viet cac bang thuoc Layer Event/EventDetails/CalculateKR/ExtractEvent

## Validation Checklist
- [ ] Tat ca 7 sections da co
- [ ] ERD khop voi DB schema
- [ ] Wireframes cover tat ca man hinh
- [ ] UserFlows cover tat ca use cases
- [ ] Task classification dung tieu chi

## Next Step
T220 — Create Incremental Step Plans

## References
- @docs/L1_VoiTong.md — load always
- @docs/L2_Workflows.md — load always
- @docs/L3_WorkflowDetails.md — load always
- @docs/layerevent.md — load for task classification
- @docs/migration.sql — load for schema reference
