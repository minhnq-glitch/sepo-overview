---
name: sepo-t390
description: "T390 - Eval BApp voi AP — Post-implementation Audit"
---

# Skill: T390 — Eval BApp voi AP — Post-implementation Audit

**Expert Team:** Software Architect + Code Auditor + QA Lead

## Objective
So sanh code da build vs ArchitecturePack: tim discrepancies, gaps, issues.

**Scope:** Code Audit, Architecture compliance

## Trigger condition
Sau T380. Keywords: "eval", "danh gia", "audit code", "post-audit"

## Preconditions
- Code da build xong (T380)
- AP co san

## Inputs
### Required
- ArchitecturePack.md
- Codebase
- ISP (de biet scope)
### Optional
- Previous audit reports

## Output Contract
- PostAudit report:
-   - AP vs Code comparison
-   - Gaps found (feature trong AP nhung code chua co)
-   - Discrepancies (code khac AP)
-   - Issues va recommendations

## Internal Reasoning Checklist
- [ ] Schema trong code match ERD trong AP?
- [ ] APIs trong code match API spec trong AP?
- [ ] UI components match wireframes?
- [ ] Business logic match complex logic section?

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

- 📄 `PostAudit_{project}_{version}.md`

**Procedure:**
```bash
# 1. Hỏi user: "Đã tạo xong [file]. Upload lên SEPO không?"
# 2. Nếu user đồng ý:
BASE=${SEPO_BASE_URL:-https://sepo.mikai.tech}

# Option A: Upload file (multipart/form-data)
curl -X POST ${BASE}/api/doc/upload-with-match \
  -b cookies.txt \
  -F "file=@PostAudit_{project}_{version}.md" \
  -F "projectId=${PROJECT_ID}"

# Option B: Create doc + version (JSON)
# DOC_ID=$(curl -X POST ${BASE}/api/docs -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"projectId":"${PROJECT_ID}","title":"PostAudit_{project}_{version}.md","category":"Project","docFormat":"md"}' | jq -r .id)
# curl -X POST ${BASE}/api/docs/${DOC_ID}/versions -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"contentText":"<content>","changeNote":"T390 output"}'

# 3. Update task-item status → Done
# TASK_ITEM_ID=$(curl -s "${BASE}/api/task-items?projectId=${PROJECT_ID}" -b cookies.txt | \
#   jq -r '.[] | select(.taskConfigCode=="T390") | .id' | head -1)
# curl -X PATCH ${BASE}/api/task-items/${TASK_ITEM_ID} -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"doneStatus":"Done","completedAt":"<ISO timestamp>"}'
```

## Execution Procedure
1. Doc AP + scan toan bo codebase
2. Section by section comparison:
3.   - ERD vs schema.ts / migration files
4.   - API spec vs route files
5.   - Wireframes vs React components
6.   - Logic vs service/handler code
7. Liet ke: matches, gaps, discrepancies
8. Output: PostAudit_{project}_{version}.md

## Branching Logic
- Neu gaps nhieu → quay lai T3 de fix
- Neu chi minor → document va continue deploy

## Validation Checklist
- [ ] Moi AP section da compared
- [ ] Gaps listed voi severity

## Failure Cases
- Major gaps found → khong nen deploy

## Recovery Actions
- Fix gaps truoc khi deploy, update AP neu code dung hon

## Completion Criteria
- Audit report created
- User reviewed findings

## Next-Step Handoff
- T4 (Deploy staging & UAT)

## Reusable Prompt Block
```text
You are a senior software architect and code auditor.

Your task is to perform a post-implementation audit.

MANDATORY:
- You MUST read and strictly follow rules defined in CLAUDE.md
- You MUST NOT ignore any constraints or conventions from CLAUDE.md

INPUT:
1. Ready deeply [Architectural Pack_version_date]
2.  Ready deeply [IncrementalStepPlan_version_date]
3. Source code (current implementation)
4. layerevent.md → defines classification rules for task types (A/B/C)

TASK:

1. COMPARISON AUDIT
- Compare:
  + Architectural Pack vs Source Code
  + IncrementalStepPlan vs Source Code
- Identify:
  + Missing components (planned but not implemented)
  + Extra components (implemented but not defined)
  + Incorrect implementations (violating design or CLAUDE.md)

2. CLASSIFICATION (MANDATORY)
- Based on layerevent.md:
  + Classify each finding into A / B / C
  + Follow EXACT definitions from layerevent.md

3. COMPLIANCE CHECK
- Validate code against CLAUDE.md:
  + Naming conventions
  + Architecture rules
  + Layer boundaries
  + Any defined constraints

4. API CONTRACT AUDIT (MANDATORY — do NOT skip)
- For EVERY endpoint defined in AP:
  a) Identify the FE payload type
  b) Identify the BE model
  c) Compare FIELD BY FIELD:
     | Field | FE Type | BE Type | Match? |
     - Check field names (exact string match, snake_case vs camelCase)
     - Check field types (string vs str, number vs float, array vs list)
     - Check required vs optional (FE optional? vs BE Optional[])
     - Check request location (POST body vs query param vs path param)
     - Check response field names between FE expected type and BE response model

  d) Flag as "Incorrect" with Impact: High if ANY of these occur:
     - FE sends a field BE does not expect
     - BE requires a field FE does not send
     - Field name mismatch
     - Field type mismatch (e.g., FE sends string[], BE expects float)
     - FE sends body but BE reads query param (or vice versa)
     - FE expects a response field that BE doesn't return

5. OUTPUT FORMAT

Return a structured audit report:

SECTION 1: SUMMARY
- Total items:
  + Missing:
  + Extra:
  + Incorrect:
- Overall status: PASS / WARNING / FAIL

SECTION 2: DETAILED AUDIT LIST

For each item:

- ID: <auto incremental>
- Type: Missing / Extra / Incorrect
- Classification: A / B / C
- Component: <module / service / table / API / class>
- Description: <what is the issue>
- Expected (from Architecture/Plan): <expected behavior/design>
- Actual (from Code): <what is implemented>
- Violation:
  + ArchitecturePack / IncrementalStepPlan / CLAUDE.md
- Impact: <low / medium / high>
- Recommendation: <what to fix>

SECTION 3: API CONTRACT AUDIT

For each endpoint in AP:

| # | Endpoint | Method | FE Interface (file:line) | BE Model (file:line) | Status | Mismatched Fields |
|---|----------|--------|--------------------------|----------------------|--------|------------------|
| 1 | /api/cascade/init | POST | CascadeInitPayload (useCascade.ts:127) | CascadeInitRequest (cascade_models.py:134) | ✅ MATCH | — |
| 2 | /api/cascade/allocate | POST | CascadeAllocatePayload (useCascade.ts:48) | CascadeAllocateRequest (cascade_models.py:178) | ❌ MISMATCH | parent_id missing, to_entity vs scope_value |
| ... | | | | | | |

For each MISMATCH, add a sub-entry:
- FE sends: <list fields with types>
- BE expects: <list fields with types>
- Delta: <list exact differences>
- Fix: <which side to change and how>

SECTION 4: COVERAGE CHECK

- List all major components from:
  + Architectural Pack
  + IncrementalStepPlan

Mark each as:
- IMPLEMENTED
- MISSING
- PARTIAL

SECTION 5: FINAL VERDICT

- Is implementation aligned with architecture? YES / NO
- Critical issues that must be fixed before release

CONSTRAINTS:
- Do NOT assume anything not present in inputs
- Be strict and precise (audit mindset)
- Do NOT explain theory
- Focus only on gaps and mismatches
```
