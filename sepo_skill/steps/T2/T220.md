---
name: sepo-t220
description: "T220 - Tao Incremental Steps Plans cho BApp"
---

# Skill: T220 — Tao Incremental Steps Plans cho BApp

**Expert Team:** Technical Program Lead + Software Architect + QA Lead

## Objective
Chia ArchitecturePack thanh cac buoc incremental (TDD approach) de developer implement tung step.

**Scope:** Technical Planning, TDD, Task decomposition

## Trigger condition
Sau AP (T212). Keywords: "ISP", "step plan", "incremental step", "tao plan"

## Preconditions
- ArchitecturePack da hoan thanh va approved

## Inputs
### Required
- ArchitecturePack.md
### Optional
- Team capacity info
- Priority from PO

## Output Contract
- IncrementalStepPlan.md gom:
-   - Moi step: ID, scope, files affected, test criteria
-   - Dependencies giua cac steps
-   - Task classification (A/B/C) per step
-   - Estimated effort per step
-   - Build prompt + Test prompt cho moi step

## Internal Reasoning Checklist
- [ ] Moi step du nho de implement trong 1 session?
- [ ] Test criteria ro rang cho moi step?
- [ ] Dependencies dung (khong circular)?
- [ ] Steps cover 100% AP?

## 📥 Download từ SEPO (trước khi execute)

**Docs cần có để chạy step này:**

| File/Doc | Filter | Action |
|----------|--------|--------|
| `ArchitecturePack` | title chứa `ArchitecturePack` | Nếu local chưa có → download |

**Procedure:**
```bash
BASE=${SEPO_BASE_URL:-https://sepo.mikai.tech}
# 1. Login (nếu chưa có cookies)
curl -s -c cookies.txt -X POST \
  -H "Content-Type: application/json" \
  -d '{"email":"tuanpm@clevai.edu.vn"}' \
  ${BASE}/api/dev/quick-login

# 2. Lấy project ID (nếu chưa có)
PROJECT_ID=$(curl -s ${BASE}/api/projects -b cookies.txt | jq -r '.[0].id')

# 3. Lấy danh sách docs của project
curl -s "${BASE}/api/docs?projectId=${PROJECT_ID}" -b cookies.txt > docs_list.json

# 4. Với mỗi doc cần thiết:
#    - Filter: title chứa "ArchitecturePack"
#    - Check local: ls **/*ArchitecturePack* → đã có?
#    - Nếu thiếu: GET /api/docs/{docId}/versions → save contentText
```

## 📤 Upload lên SEPO (sau khi hoàn thành)

**Output files có thể upload:**

- 📄 `IncrementalStepPlan_{project}_{version}.md`

**Procedure:**
```bash
# 1. Hỏi user: "Đã tạo xong [file]. Upload lên SEPO không?"
# 2. Nếu user đồng ý:
BASE=${SEPO_BASE_URL:-https://sepo.mikai.tech}

# Option A: Upload file (multipart/form-data)
curl -X POST ${BASE}/api/doc/upload-with-match \
  -b cookies.txt \
  -F "file=@IncrementalStepPlan_{project}_{version}.md" \
  -F "projectId=${PROJECT_ID}"

# Option B: Create doc + version (JSON)
# DOC_ID=$(curl -X POST ${BASE}/api/docs -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"projectId":"${PROJECT_ID}","title":"IncrementalStepPlan_{project}_{version}.md","category":"Project","docFormat":"md"}' | jq -r .id)
# curl -X POST ${BASE}/api/docs/${DOC_ID}/versions -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"contentText":"<content>","changeNote":"T220 output"}'

# 3. Update task-item status → Done
# TASK_ITEM_ID=$(curl -s "${BASE}/api/task-items?projectId=${PROJECT_ID}" -b cookies.txt | \
#   jq -r '.[] | select(.taskConfigCode=="T220") | .id' | head -1)
# curl -X PATCH ${BASE}/api/task-items/${TASK_ITEM_ID} -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"doneStatus":"Done","completedAt":"<ISO timestamp>"}'
```

## Execution Procedure
1. Doc ArchitecturePack
2. Xac dinh order: schema first → backend → frontend → integration
3. Chia thanh steps: moi step la 1 unit co the test doc lap
4. Moi step gom: scope, files, test criteria, dependencies, classification
5. Viet Build Prompt cho moi step (instructions cho developer)
6. Viet Test Prompt cho moi step (test cases)
7. Output: IncrementalStepPlan_{project}_{version}.md
8. Verify: steps cover 100% AP, no gaps

## Branching Logic
- Neu AP lon (>20 features) → nhom features thanh milestones
- Neu co dependencies complex → ve dependency graph

## Validation Checklist
- [ ] Moi feature trong AP co it nhat 1 step
- [ ] Moi step co test criteria
- [ ] Dependencies khong circular
- [ ] Steps co the thuc hien tuan tu

## Failure Cases
- Steps qua lon → kho implement
- Thieu step → feature khong duoc build

## Recovery Actions
- Tach step lon thanh sub-steps
- Cross-check AP vs ISP

## Completion Criteria
- ISP file created
- All AP features covered
- User reviewed

## Next-Step Handoff
- T230 (Audit ISP)

## Reusable Prompt Block
```text
You are a senior staff engineer and technical program lead.

I have already completed system design documentation in File [architecturepack_project]
- **Phần 1:** EntityRelationshipDescription — Cấu trúc dữ liệu & quan hệ entity
- **Phần 2:** ProcessDescription — Mô tả quy trình FEE / PENALTY / BONUS
- **Phần 3:** UIWireFrame — Danh sách màn hình & wireframe
- **Phần 4:** UserFlows — Luồng người dùng theo vai trò
- **Phần 5:** Complex Logic — Thuật toán, công thức, business rules
- **Phần 6:** Feature & Layer — Module inventory, layer classification, dependency graph
- **Phần 7:** Task classification
and other part 

Your task is NOT to write any production code yet.

Your task is to break this system into a sequence of SMALL, INCREMENTAL, BUILDABLE STEPS
that can be implemented one at a time using strict test-driven development.

Constraints:
- Each step must read deeply in architecturepack and use CLAUDE.MD
- Each step must be independently testable
- Each step must deliver a vertical slice (frontend → backend → logic → response), even if simplified
- No step should introduce more than ONE new concept (e.g. auth, persistence, external API, async jobs)
- Steps should start with mocked or static behavior and evolve toward real integrations
- No speculative features
- No refactoring steps unless required by failing tests
- Assume the system may already exist but is buggy; steps should still be minimal and stabilizing
- At the end there must be an integration test

For EACH step, output:
1. Step name (short and explicit)
2. Goal (one sentence, user-visible or system-observable)
3. Scope (what is included / explicitly excluded)
4. Components touched (frontend, backend, DB, integrations — be explicit)
5. Preconditions (what must already exist)
6. Test cases to validate the step
   - Backend tests
   - Frontend tests
7. Expected artifacts (endpoints, screens, entities, events — minimal)
8. Exit criteria (how we know this step is complete)

Rules:
- Do NOT write implementation code
- Do NOT merge steps together
- Do NOT skip testing considerations
- Do NOT assume future steps will fix problems introduced earlier
- If a step feels too large, split it
- Use Document "Clevai_Tech_Stack_Prefer_PR4260.md"

Output format:
- Ordered list of steps (Step 0, Step 1, Step 2, …)
- Keep steps small enough that each could be completed in under half a day
- Output markdown document name: "IncrementalStepPlan-projectname"
Wait for my confirmation before proceeding beyond step breakdown.

# EACH STEP MUST INCLUDE 2 SUBSTEPS BELOW:

## SUBSTEP 1:

You are acting as a senior engineer doing disciplined incremental development.

Rules:
- One small change per step
- No speculative features
- Tests are mandatory
- If tests fail, fix before proceeding
- Ask for approval between steps

Output format:
1. Test cases
2. Implementation
3. How to run tests
4. Known limitations
5. Ask whether to continue


## SUBSTEP 2:

You are acting as a disciplined senior engineer practicing strict incremental development.

The step description above [Step.....] is the SINGLE source of truth.

Your task is to implement this step using automated tests and a tight feedback loop.

GLOBAL RULES:
- One step only; do not implement future steps
- No speculative features or refactors
- Do not change scope defined in the step
- Minimal code to satisfy tests
- Deterministic, headless, automatable tests only
- Stop when exit criteria are met

PROCESS (follow in order):

1. TEST DESIGN
   - Derive automated test cases directly from the step’s "Test cases" and "Exit criteria"
   - Separate:
     - Backend/API tests
     - Frontend/UI tests
   - If tests are missing or ambiguous, ask before proceeding

2. WRITE FAILING TESTS
   - Write real, executable tests using the project’s existing test framework(s)
   - Tests must fail against the current codebase
   - Do NOT write implementation code yet

3. RUN TESTS
   - Execute backend tests
   - Execute frontend tests (headless)
   - Capture and display failures

4. IMPLEMENT MINIMUM CODE
   - Implement ONLY what is required to make the failing tests pass
   - Do not add extra endpoints, UI, logic, or configuration
   - Prefer the smallest possible change

5. FIX-ON-FAILURE LOOP
   - If any test fails:
     - Explain the root cause
     - Apply the smallest fix
     - Re-run the failed test
   - Repeat until all tests pass

6. FINAL VERIFICATION
   - Re-run all backend tests
   - Re-run all frontend tests
   - Confirm success

7. STOP AND REPORT
   - Summarize what was added or changed
   - List known limitations or shortcuts
   - Explicitly ask whether to proceed to the next step

STRICT PROHIBITIONS:
- No feature expansion
- No refactoring for “cleanliness”
- No skipping test execution
- No assumptions about future steps



```

## Examples
```text
# TEP230 Audit System -- Incremental Step Plan

## Context

The TEP230 Audit System has a completed system design document (`tep230.SYSTEM_DESIGN_EN.md`, 1,641 lines, 12 sections) describing 26 entities, 9 menus, 6 STD processes, and 30 KPIs. This is a **brand new project** — both BE and FE folders are currently empty. We build from scratch following strict TDD, using Clevai's preferred tech stack.

**System Design**: `document/tep230.SYSTEM_DESIGN_EN.md`
**Backend**: `tep230auditcepov14p2-be/` (empty)
**Frontend**: `tep230auditcepov14p2-fe/` (empty)
**Reference (old project)**: `D:/tep230cepov14_p2/teacher/tep230audit-be/` (patterns to follow)

### Clevai Tech Stack (PR4260)

| Layer | Technology |
|-------|-----------|
| Backend | TypeScript + Express.js |
| Frontend | ReactJS + Tailwind CSS + Material UI |
| ORM | TypeORM (MySQL) |
| Auth | jsonwebtoken (JWT) |
| Validation | Zod |
| ... | ... |

### Conventions (from reference project)

- **Service**: Class singleton, raw SQL via `AppDataSource.query(sql, params)`, errors as `{statusCode, message}`
- **Router**: `Router()` → `authMiddleware` → `roles([...])` → `validate(schema)` → controller
- **Response**: `sendSuccess(res, data, msg)` / `sendError(res, msg, code)`
- **Migration**: `up()`/`down()` functions, check INFORMATION_SCHEMA before ALTER
- **Test (BE)**: Jest + supertest, mock `AppDataSource.query()`, `makeToken(role)` for auth
- **Test (FE)**: Vitest + @testing-library/react
- **Entity**: TypeORM `@Entity('bp_tablename')` with `@PrimaryGeneratedColumn()` and `@Column(...)`
- **File naming**: `module-name.controller.ts`, `.service.ts`, `.router.ts`, `.schema.ts`
- **DB tables**: Prefixed `bp_` (main DB) or `audit_` (local DB)

### Abbreviations

- **BE** = `tep230auditcepov14p2-be/`
- **FE** = `tep230auditcepov14p2-fe/`

---

## PHASE 0: Project Initialization (Steps 0-2)

### Step 0: Backend Project Setup

**Goal**: Initialize Express + TypeScript + TypeORM + Jest backend project with all dependencies.

**Scope**:
- IN: `npm init`, install all deps, create tsconfig.json, jest.config.js, src/ directory structure. Create src/index.ts, src/app.ts, src/config/database.ts. Create common utilities: sendSuccess/sendError.
- OUT: No business logic modules yet.

**Components touched**:
- `BE/package.json` (new)
- `BE/tsconfig.json` (new)
- `BE/src/app.ts` (new)
- `BE/src/common/utils/response.ts` (new)
- ... (8 files total)

**Preconditions**: None

**Test cases** (`BE/src/__tests__/health.test.ts`):
1. `GET /api/health` returns `{success: true, message: 'OK'}`
2. Server starts without errors
3. TypeScript compiles with `tsc --noEmit`

**Expected artifacts**: Full project skeleton with health endpoint
**Exit criteria**: `npm test` passes. `npm run build` compiles. Health endpoint responds.

---

### Step 1: Backend Auth & Roles Middleware

**Goal**: Create JWT authentication and role-based authorization middleware.

**Scope**:
- IN: auth.middleware.ts (JWT verify), roles.middleware.ts (role check), validate.middleware.ts (Zod), jwt.ts config, `makeToken(role)` test helper.
- OUT: No user registration/login endpoints.

**Components touched**:
- `BE/src/middleware/auth.middleware.ts` (new)
- `BE/src/middleware/roles.middleware.ts` (new)
- `BE/src/middleware/validate.middleware.ts` (new)
- `BE/src/config/jwt.ts` (new)

**Preconditions**: Step 0

**Test cases** (`BE/src/__tests__/middleware-auth.test.ts`):
1. Request without Authorization header → 401
2. Request with valid JWT (AD role) → passes through, `req.user.role = 'AD'`
3. Request with valid JWT (QA role) hitting `roles(['AD','TO'])` route → 403
4. Zod validation: missing required field → 400
5. `makeToken('AD')` generates valid JWT

**Expected artifacts**: Middleware files, JWT config, test file
**Exit criteria**: Auth + roles + validation middleware tested.

---

### Step 2: Frontend Project Setup

**Goal**: Initialize React + Vite + Tailwind + Material UI frontend project.

**Scope**:
- IN: Vite (React + TypeScript), Tailwind, MUI, axios, react-router-dom, zustand. Vitest + @testing-library/react. Layout with Sidebar (9 menu items). RoleGuard component. api.ts (Axios + JWT interceptor).
- OUT: No page content yet.

**Components touched**:
- `FE/src/App.tsx` (new)
- `FE/src/services/api.ts` (new)
- `FE/src/components/layout/Sidebar.tsx` (new)
- `FE/src/stores/auth.store.ts` (new)
- ... (8 files total)

**Preconditions**: None (parallel with Step 0)

**Test cases** (`FE/src/__tests__/Layout.test.tsx`):
1. Sidebar renders 9 menu items
2. RoleGuard with AD role → shows children
3. RoleGuard with QA role on AD-only page → redirects
4. Axios instance sets Authorization header from store

**Expected artifacts**: Full FE project skeleton with layout
**Exit criteria**: `npm test` passes. `npm run dev` serves page with sidebar.

---

## PHASE 1: Entity Models & Migrations (Steps 3-6)

### Step 3: Layer A Reference Entities (15 entities)

**Goal**: Create TypeORM entity models for all 15 Layer A reference/master entities.

**Scope**:
- IN: USI, UST, PT, GG, DFDL, CAP, LCP, LCT, LCET, CTI, CTT, CUIE, CUI, ULC, POD. READ-ONLY (tables already exist in main database).
- OUT: No migrations. No CRUD endpoints.

**Components touched**:
- `BE/src/entities/usi-useritem.entity.ts` (new)
- ... (15 entity files total)
- `BE/src/config/database.ts` (register all 15 entities)

**Preconditions**: Step 0

**Test cases** (`BE/src/__tests__/entities-layer-a.test.ts`):
1. All 15 entity classes can be imported without errors
2. USI entity has properties: code, username, email, fullname, phone, myust
3. `tsc --noEmit` compiles successfully

**Expected artifacts**: 15 entity files, modified database.ts
**Exit criteria**: All entities compile. Table names match existing DB schema.

---

### Step 4: Layer B Template Entities (CHPT, CHST, CHLT, CHRT) + New CHPT Fields

**Goal**: Create 4 template layer entities including CHPT's 15 NEW audit config fields.

**Scope**:
- IN: CHPT with +15 new fields (passthreshold, retrainthreshold, terminatethreshold, maxscore, samplerate, feedbackdays, hassecondaudit, etc.). CHST, CHLT, CHRT.
- Migration 001: ALTER TABLE bp_chpt_checkprocesstemp ADD 15 columns.

**Components touched**:
- `BE/src/entities/chpt-checkprocesstemp.entity.ts` (new)
- ... (4 entity files total)
- `BE/src/migrations/001-add-chpt-audit-config-fields.ts` (new)

**Preconditions**: Step 0

**Test cases** (`BE/src/__tests__/entities-layer-b.test.ts`):
1. CHPT entity has all existing + 15 new properties
2. Migration `up()` adds 15 columns with correct types
3. Migration `up()` checks INFORMATION_SCHEMA (idempotent)
4. Migration `down()` drops all 15 columns

**Expected artifacts**: 4 entity files, 1 migration, test file
**Exit criteria**: Entities compile. Migration idempotent.

---

### Step 5: Layer C Instance Entities (CHPI+7, CHSI+2, CHLI+5, CHRI, BPP, BPS, BPE)

**Goal**: Create 7 instance layer entities including new fields for CHPI(+7), CHSI(+2), CHLI(+5).

**Scope**:
- IN: CHPI +7 new fields (totalscore, maxscore, thresholdresult, actionstatus, issupervisorreview, myparentchpi, feedbackdeadline). CHSI +2. CHLI +5. CHRI, BPP, BPS, BPE.
- Migration 002: ALTER TABLE for CHPI(+7), CHSI(+2), CHLI(+5).

**Components touched**:
- `BE/src/entities/chpi-checkprocessitem.entity.ts` (new)
- ... (7 entity files total)
- `BE/src/migrations/002-add-instance-fields.ts` (new)

**Preconditions**: Step 0

**Test cases** (`BE/src/__tests__/entities-layer-c.test.ts`):
1. CHPI entity has all 7 new fields with correct types
2. CHSI entity has pagescore(DECIMAL5,2) and pagemaxscore(DECIMAL5,2)
3. Migration adds correct columns to all 3 tables

**Expected artifacts**: 7 entity files, 1 migration, test file
**Exit criteria**: All 26 entities defined. Migrations idempotent.

---

### Step 6: USID Entity + Email Log Table

**Goal**: Create USID entity (auditor duty) and audit_email_log table.

**Scope**:
- IN: USID entity. New table `audit_email_log` (13 fields). Migration 003: CREATE TABLE.
- OUT: (none additional)

**Components touched**:
- `BE/src/entities/usid-usiduty.entity.ts` (new)
- `BE/src/entities/email-log.entity.ts` (new)
- `BE/src/migrations/003-create-email-log-table.ts` (new)

**Preconditions**: Step 0

**Test cases** (`BE/src/__tests__/entities-usid-emaillog.test.ts`):
1. USID entity has myusi, myust, mychrt, mypod, status fields
2. EmailLog entity has all 13 columns
3. Migration down() drops table

**Expected artifacts**: 2 entity files, 1 migration, test file
**Exit criteria**: All entities + email log table defined.

---

## PHASE 2: Score Sync Chain (Steps 7-10)

### Step 7: Scoring Module — CHSI Page Score Aggregation

**Goal**: `aggregatePageScore(chsiCode)` — calculates CHLI scores per page.

**Scope**:
- IN: New module `scoring`. `SUM(CHLI.score1)` → CHSI.pagescore.
- OUT: No CHPI aggregation.

**Preconditions**: Steps 4, 5

**Test cases** (`BE/src/__tests__/score-sync-page.test.ts`):
1. 3 CHLI with scores [1,0,1] → pagescore=2, pagemaxscore=3
2. CHLI with score1=NULL → skipped
3. Empty page → pagescore=0, pagemaxscore=0

**Exit criteria**: Page scores calculated correctly.

---

### Step 8: CHPI Total Score Aggregation

**Goal**: `aggregateProcessScore(chpiCode)` — sums CHSI pagescores into CHPI totalscore.

**Scope**:
- IN: `totalscore = (SUM(pagescore) / SUM(pagemaxscore)) * CHPT.maxscore`.
- OUT: No threshold. No status.

**Preconditions**: Step 7

**Test cases** (`BE/src/__tests__/score-sync-process.test.ts`):
1. CHPT.maxscore=4.0, 2 CHSI pagescores [5/6, 3/4] → totalscore=(8/10)*4.0=3.20
2. CHPI not found → {statusCode: 404}
3. Result rounded to 2 decimal places

**Exit criteria**: Score propagates CHSI → CHPI.

---

### Step 9: Threshold Evaluation via CHPT Fields

**Goal**: Evaluate score against thresholds → set CHPI.thresholdresult.

**Scope**:
- IN: `evaluateThreshold(chpiCode)`. score ≥ pass → PASS; score ≥ retrain → RETRAIN; else TERMINATE.
- OUT: No emails. No status change.

**Preconditions**: Steps 4, 8

**Test cases** (`BE/src/__tests__/threshold-evaluation.test.ts`):
1. pass=3.0, retrain=2.5. Score 3.2 → PASS
2. Score 2.7 → RETRAIN
3. Score 2.1 → TERMINATE
4. retrain=null → below pass goes directly to TERMINATE

**Exit criteria**: Threshold evaluation correct for all cases.

---

### Step 10: Complete Audit — Full Score Chain + Status=Audited

**Goal**: Wire full chain: save scores → aggregate pages → aggregate process → threshold → status=Audited. Set feedbackdeadline.

**Scope**:
- IN: `completeAudit(chpiCode, scores[])` → Steps 7→8→9. Sets CHPI.status='Audited', feedbackdeadline = NOW() + CHPT.feedbackdays.
- OUT: No emails (per design: NO auto email).

**Preconditions**: Steps 7-9

**Test cases** (`BE/src/__tests__/complete-audit-chain.test.ts`):
1. Full chain: scores → CHSI pagescores → CHPI totalscore → thresholdresult
2. CHPI.status set to 'Audited'
3. feedbackdeadline = NOW() + CHPT.feedbackdays
4. No email service calls (verify no sendEmail mock called)

**Exit criteria**: Full chain passes. No auto-email. Score + deadline persisted.

---

## PHASE 3: CHPI Status Machine & Audit Process (Steps 11-14)

### Step 11: CHPI Status Machine Utility

**Goal**: Status machine: Open→Assigned→Auditing→Audited transitions.

**Scope**:
- IN: `status-machine.ts` with `isValidTransition(from, to)` and `assertTransition(from, to)`.
- OUT: No endpoint integration.

**Preconditions**: None (pure utility)

**Test cases** (`BE/src/__tests__/status-machine.test.ts`):
1. Open→Assigned → valid
2. Assigned→Auditing → valid
3. Open→Audited → invalid (skip not allowed)
4. Audited→Open → invalid

**Exit criteria**: All transitions tested.

---

### Step 12: Audit Process Module — Create & Start Audit

**Goal**: `POST /api/audit-processes` creates CHPI. `PATCH /:code/start` transitions Assigned→Auditing.

**Preconditions**: Step 11, Step 5

**Test cases** (`BE/src/__tests__/audit-process.test.ts`):
1. POST creates CHPI with status=Open
2. PATCH /start on Assigned CHPI → status=Auditing
3. PATCH /start on Open CHPI → 400
4. Non-assigned auditor → 403

**Exit criteria**: CHPI creation and start working. Status transitions enforced.

---

### Step 13: Checklist Module — CHLI Scoring with Auto-Save

**Goal**: `PATCH /api/checklists/:chliCode/score` with Pass/Fail + auto-save + sync to CHSI.

**Preconditions**: Steps 5, 7, 11

**Test cases** (`BE/src/__tests__/checklist-scoring.test.ts`):
1. PATCH score1=1 → CHLI saved, CHSI.pagescore re-aggregated
2. First score on CHPI → status transitions to Auditing
3. Only assigned auditor can score → 403 for others
4. score1 must be 0 or 1 → Zod validation

**Exit criteria**: Auto-save works. CHSI syncs. Status transitions.

---

### Step 14: Audit Queue — List with Filters + Detail View

**Goal**: Read-only list and detail for Audit Queue (Menu 1).

**Scope**:
- IN: `GET /api/audit-processes` with filters, pagination, role-based scope. `GET /:code` returns detail with pages + items.
- OUT: No scoring.

**Preconditions**: Step 12, Step 1

**Test cases** (`BE/src/__tests__/audit-queue-list.test.ts`):
1. GET returns paginated CHPI list with meta
2. QA token → only sees own CHPI
3. AD token → sees all CHPI
4. GET /:code returns header + pages + items (JOIN CHST→CHSI, CHLT→CHLI)

**Exit criteria**: List paginated. Role scope enforced. Detail includes pages+items.

---

## PHASE 4: Auditor & Supervisor Assignment (Steps 15-18)

### Step 15: Auditor Assignment — Single Assign

**Goal**: `POST /api/auditors/assign` sets CHPI.mychecker + status=Assigned. AD/TO only.

**Preconditions**: Steps 5, 11

**Test cases**: Assign auditor → status=Assigned. Non-Open → 400. QA → 403. Already assigned → 400.

**Exit criteria**: Single assignment works with guards.

---

### Step 16: Batch Auditor Assignment (Even Distribution)

**Goal**: `POST /api/auditors/batch-assign` — distributes N CHPI / M auditors evenly. Max 200.

**Preconditions**: Step 15

**Test cases**: 10/2 → 5 each. 11/3 → 4,4,3. >200 → 400. Skips non-Open.

**Exit criteria**: Even distribution verified. Limits enforced.

---

### Step 17: Unassign/Reassign Auditor with Score Reset

**Goal**: Unassign reverts to Open. If Auditing: reset all scores. Cannot unassign Audited.

**Preconditions**: Steps 15, 11

**Test cases**: Unassign Assigned → Open. Unassign Auditing → Open + clear scores. Unassign Audited → 400.

**Exit criteria**: Unassign resets properly. Audited blocked.

---

### Step 18: Supervisor Assignment — CHPI Clone

**Goal**: Clone CHPI for supervisor review (issupervisorreview=1, myparentchpi=original). Only for Audited CHPI.

**Preconditions**: Step 5

**Test cases**: Creates clone with issupervisorreview=1. Original unchanged. Status≠Audited → 400. Max 1 per original.

**Exit criteria**: Clone creates new records. Original untouched.

---

## PHASE 5: Feedback System (Steps 19-20)

### Step 19: CHLI Feedback Submission

**Goal**: Teacher submits feedback within deadline. `POST /api/checklists/:chliCode/feedback`.

**Preconditions**: Steps 5, 10

**Test cases**: Submit → PENDING. Deadline past → 400. Already ACCEPTED → 400.

**Exit criteria**: Feedback PENDING within deadline.

---

### Step 20: Feedback Review — Accept/Reject with Re-aggregation

**Goal**: Accept flips score + re-aggregates CHSI→CHPI→threshold. Reject stores note.

**Preconditions**: Steps 7-9, 19

**Test cases**: Accept: score 0→1, re-aggregated, threshold may change. Reject: no score change. Not PENDING → 400.

**Exit criteria**: Accept re-aggregates. Reject preserves scores.

---

## PHASE 6: Audit Action Module — NEW (Steps 21-24)

### Step 21: Audit Action — List & Filter Endpoint

**Goal**: New module. `GET /api/audit-action` with filters, pagination. AD/TO only.

**Preconditions**: Step 5

**Test cases**: Paginated list. Filter by thresholdresult. QA/QS → 403.

**Exit criteria**: Module registered. Role restriction works.

---

### Step 22: Batch Email Send

**Goal**: `POST /api/audit-action/batch-email` — create email_log records + update actionstatus. Max 100.

**Preconditions**: Steps 6, 21

**Test cases**: 3 CHPI → 3 email_log records. actionstatus=EMAIL_SENT. >100 → 400.

**Exit criteria**: Email logs created. Status updated.

---

### Step 23: Terminate & Unreg Actions

**Goal**: Batch terminate (max 50) → actionstatus=TERMINATED. Unreg → CLOSED.

**Preconditions**: Step 21

**Test cases**: Terminate 2 → both TERMINATED. Already TERMINATED → 400. >50 → 400.

**Exit criteria**: Status updates. Limits enforced.

---

### Step 24: Email Log Read & Retry

**Goal**: `GET /api/audit-action/email-log` (paginated). `POST /:id/retry` (max 3 retries).

**Preconditions**: Step 22

**Test cases**: Paginated list. Filter by status=BOUNCED. Retry increments count. retry>=3 → 400.

**Exit criteria**: Retry respects max 3.

---

## PHASE 7: Configuration & Management (Steps 25-29)

### Step 25: Report Hotcase — Create CHPI(HOT-AUDIT)

**Goal**: `POST /api/audit-processes/hotcase` → match CHPT → create CHPI with mychpttype=HOT-AUDIT.

**Preconditions**: Step 12

**Test cases**: Valid report → CHPI created. No matching CHPT → 404. QA → 403.

**Exit criteria**: Hotcase creates CHPI correctly.

---

### Step 26: Re-audit Child CHPI

**Goal**: Create child CHPI for re-audit. Only when CHPT.hassecondaudit=1.

**Preconditions**: Steps 4, 5

**Test cases**: hassecondaudit=1 → child created. hassecondaudit=0 → 400. Duplicate → 400.

**Exit criteria**: Re-audit respects hassecondaudit flag.

---

### Step 27: PT Audit Config Endpoint (CHPT Part A)

**Goal**: Read/write threshold + audit config per PT, propagating to all CHPT under that PT.

**Preconditions**: Step 4

**Test cases**: PUT updates all CHPT where mypt matches. GET returns config. No CHPT → 404.

**Exit criteria**: Config propagates.

---

### Step 28: Email Template Mapping per PT-GG (CHPT Part C)

**Goal**: Map email templates (CTI) to CHPT by PT-GG combination.

**Preconditions**: Steps 4, 27

**Test cases**: GET returns PT-GG matrix. PUT updates CHPT rows. Partial update safe. AD/TO only.

**Exit criteria**: Mappings written.

---

### Step 29: CHPT Publish/Unpublish + Validity

**Goal**: publishstatus and validfrom/validto guard CHPI creation.

**Preconditions**: Steps 4, 12

**Test cases**: Unpublish → publishstatus='unpublished'. Create CHPI with unpublished → 400. validto<NOW → 400.

**Exit criteria**: Expired/unpublished blocks CHPI creation.

---

## PHASE 8: Auditor & History Management (Steps 30-32)

### Step 30: Manage USID — Auditor CRUD + Role Assignment

**Goal**: USID module: list, create, edit, assign CHRT roles, soft delete. AD/TO only.

**Preconditions**: Steps 3, 5, 6

**Test cases**: POST creates USID. POST /roles creates CHRI. Deactivate → status='inactive'. QA → 403.

**Exit criteria**: Auditor CRUD + roles + soft delete working.

---

### Step 31: Audit History — List + Detail + Feedback View

**Goal**: `GET /api/audit-history` (Audited CHPI list) + detail with feedback status per CHLI.

**Preconditions**: Steps 5, 10

**Test cases**: List returns only Audited. QA sees own. Detail includes feedback badges. Paginated.

**Exit criteria**: History filtered by role. Feedback visible.

---

### Step 32: Email Setting — Template CRUD

**Goal**: Email template management: list, edit HTML body, preview with sample data. AD only.

**Preconditions**: Step 3

**Test cases**: GET lists templates. PATCH updates body. Preview replaces variables. Non-AD → 403.

**Exit criteria**: CRUD + preview working.

---

## PHASE 9: Dashboard & Export (Steps 33-34)

### Step 33: Dashboard KPI Endpoint

**Goal**: `GET /api/dashboard/kpis` returning 30 KPIs. AD/TO/QS only.

**Preconditions**: Steps 5, 6

**Test cases**: Returns pass_rate, retrain_rate, terminate_rate, avg_score_per_type, queue_depth. QA → 403.

**Exit criteria**: All KPI categories present.

---

### Step 34: PDF & Excel Export

**Goal**: `GET /export/pdf/:code` → PDF. `GET /export/excel?filters` → Excel.

**Preconditions**: Step 31

**Test cases**: PDF → Content-Type application/pdf. Excel → correct content type. CHPI not found → 404.

**Exit criteria**: Correct content types. Data populated.

---

## PHASE 10: Frontend (Steps 35-39)

### Step 35: FE — Audit Queue Page

**Goal**: List + filters + audit session detail (split-screen video + CHLI Pass/Fail form).

**Preconditions**: Steps 2, 14, 13, 10

**Test cases**: Renders table. 6 filter controls. Click → detail. Detail shows video + form. Submit calls API.

**Exit criteria**: List + detail render. API calls correct.

---

### Step 36: FE — Audit Action Page + Batch Dialog

**Goal**: List + batch email dialog + terminate/unreg buttons.

**Preconditions**: Steps 2, 21-23

**Test cases**: Renders action table. Batch buttons disabled when no selection. Email dialog shows template + count.

**Exit criteria**: Batch operations wire correctly.

---

### Step 37: FE — Manage CHPT (3 Tabs) + USID Page

**Goal**: CHPT management (PT Config, Form Templates, Email Mapping tabs) + USID auditor management.

**Preconditions**: Steps 27-30

**Test cases**: 3 tabs render. PT Config shows thresholds. USID page shows auditor list with roles.

**Exit criteria**: All tabs render. CRUD operations work.

---

### Step 38: FE — Audit History + Feedback Review

**Goal**: History list + detail with CHLI-level feedback review. QS sees Accept/Reject buttons.

**Preconditions**: Steps 31, 34, 19-20

**Test cases**: Feedback badges per CHLI. QS sees buttons. QA does NOT. Export buttons trigger downloads.

**Exit criteria**: Feedback review per CHLI. Role-based buttons.

---

### Step 39: FE — Dashboard + Remaining Pages

**Goal**: Dashboard KPI cards + charts. AssignAuditor, AssignSupervisor, ReportHotcase, EmailSetting pages.

**Preconditions**: Steps 33, 16, 18, 25, 32

**Test cases**: Dashboard renders KPIs + charts. Assign shows batch UI. Hotcase has form. Email has editor.

**Exit criteria**: All 9 menus have working frontend pages.

---

## Dependency Graph

```
PHASE 0 (Setup):     [0]→[1]  [2]                         -- 2 parallel with 0/1
PHASE 1 (Entities):  [3] [4] [5] [6]                      -- all parallel after 0
PHASE 2 (Scoring):   [7]→[8]→[9]→[10]                     -- linear chain
PHASE 3 (Process):   [11] [12→11,5] [13→5,7,11] [14→12,1] -- 11 standalone
PHASE 4 (Assign):    [15→5,11] [16→15] [17→15,11] [18→5]  -- 18 independent
PHASE 5 (Feedback):  [19→5,10] [20→7-9,19]                 -- linear
PHASE 6 (Action):    [21→5] [22→6,21] [23→21] [24→22]     -- linear
PHASE 7 (Config):    [25→12] [26→4,5] [27→4] [28→4,27] [29→4,12]
PHASE 8 (Mgmt):      [30→3,5,6] [31→5,10] [32→3]
PHASE 9 (Dash):      [33→5,6] [34→31]
PHASE 10 (Frontend): [35→2,14,13,10] [36→2,21-23] [37→27-30] [38→31,19-20,34] [39→all BE]
```

## Critical Files

| File | Steps | Role |
|------|-------|------|
| `BE/src/modules/scoring/scoring.service.ts` | 7,8,9,10 | Core: score chain + threshold + complete audit |
| `BE/src/modules/audit-action/audit-action.service.ts` | 21,22,23,24 | NEW module: biggest new feature |
| `BE/src/modules/audit-process/audit-process.service.ts` | 12,14,25,26 | Create/list/hotcase/re-audit |
| `BE/src/modules/checklist/checklist.service.ts` | 13,19,20 | CHLI scoring + feedback |
| `BE/src/modules/settings/settings.service.ts` | 27,28,29 | PT Config + Email Mapping |
| `BE/src/common/utils/status-machine.ts` | 11 | Status transition guards |

## Verification

After all 40 steps (0-39):
- `npm test` in BE → all tests pass (~40 test files)
- `npm test` in FE → all frontend tests pass
- All 9 menus have backend endpoints + frontend pages
- Score sync chain: CHLI→CHSI→CHPI
- CHPI status machine: Open→Assigned→Auditing→Audited
- Supervisor CHPI clone (issupervisorreview=1)
- Re-audit child CHPI (hassecondaudit guard)
- Feedback at CHLI level (submit + accept/reject + re-aggregate)
- 30 KPIs from dashboard
- PDF + Excel export

```
