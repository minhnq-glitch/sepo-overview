---
name: sepo-t212
description: "T212 - Main: Build ArchitecturePack"
---

# Skill: T212 — Main: Build ArchitecturePack

**Expert Team:** Software Architect + Technical Writer + Database Architect

## Objective
Tao ArchitecturePack day du cho du an: ERD, Process, Wireframe, UserFlows, Complex Logic, Feature & Layer, Task Classification.

**Scope:** Software Architecture, Full system design document

## Trigger condition
Sau L1-L3 va T211. Keywords: "architecture pack", "build AP", "kien truc"

## Preconditions
- L1, L2, L3 docs co
- DB schema da review (T211)
- Codebase accessible

## Inputs
### Required
- L1_VoiTong.md, L2_Workflows.md, L3_WorkflowDetails.md
- DB Schema (tu T211)
- File layerevent.md (de phan loai tasks)
### Optional
- Existing AP tu version truoc
- UI mockups
- API docs

## Output Contract
- ArchitecturePack.md gom 7+ sections:
-   1. EntityRelationshipDescription (ERD + schema details)
-   2. ProcessDescription (process flow, status machines)
-   3. UIWireFrame (ASCII wireframes moi man hinh)
-   4. UserFlows (sequence diagrams actor-system-backend)
-   5. Complex Logic (algorithms, calculations, rules)
-   6. Feature & Layer Mapping (feature → code → DB)
-   7. Task Classification (A/B/C phan loai)
- Moi section phai co du detail de developer implement duoc

## Internal Reasoning Checklist
- [ ] ERD cover het entities tu L1?
- [ ] Process cover het workflows tu L2?
- [ ] Wireframes cover het UI screens tu L3?
- [ ] User flows co happy path + error cases?
- [ ] Complex logic co formulas + examples?
- [ ] Task classification dung A/B/C theo layerevent.md?

## 📥 Download từ SEPO (trước khi execute)

**Docs cần có để chạy step này:**

| File/Doc | Filter | Action |
|----------|--------|--------|
| `L1_VoiTong` | title chứa `VoiTong` | Nếu local chưa có → download |
| `L2_Workflows` | title chứa `Workflow` | Nếu local chưa có → download |
| `L3_WorkflowDetails` | title chứa `L3` | Nếu local chưa có → download |
| `DB Schema` | title chứa `Schema` | Nếu local chưa có → download |
| `layerevent.md` | title chứa `layerevent` | Nếu local chưa có → download |

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
#    - Filter: title chứa "VoiTong"
#    - Check local: ls **/*VoiTong* → đã có?
#    - Nếu thiếu: GET /api/docs/{docId}/versions → save contentText
#    - Filter: title chứa "Workflow"
#    - Check local: ls **/*Workflow* → đã có?
#    - Nếu thiếu: GET /api/docs/{docId}/versions → save contentText
#    - Filter: title chứa "L3"
#    - Check local: ls **/*L3* → đã có?
#    - Nếu thiếu: GET /api/docs/{docId}/versions → save contentText
#    - Filter: title chứa "Schema"
#    - Check local: ls **/*Schema* → đã có?
#    - Nếu thiếu: GET /api/docs/{docId}/versions → save contentText
#    - Filter: title chứa "layerevent"
#    - Check local: ls **/*layerevent* → đã có?
#    - Nếu thiếu: GET /api/docs/{docId}/versions → save contentText
```

## 📤 Upload lên SEPO (sau khi hoàn thành)

**Output files có thể upload:**

- 📄 `ArchitecturePack_{project}_{version}.md`

**Procedure:**
```bash
# 1. Hỏi user: "Đã tạo xong [file]. Upload lên SEPO không?"
# 2. Nếu user đồng ý:
BASE=${SEPO_BASE_URL:-https://sepo.mikai.tech}

# Option A: Upload file (multipart/form-data)
curl -X POST ${BASE}/api/doc/upload-with-match \
  -b cookies.txt \
  -F "file=@ArchitecturePack_{project}_{version}.md" \
  -F "projectId=${PROJECT_ID}"

# Option B: Create doc + version (JSON)
# DOC_ID=$(curl -X POST ${BASE}/api/docs -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"projectId":"${PROJECT_ID}","title":"ArchitecturePack_{project}_{version}.md","category":"Project","docFormat":"md"}' | jq -r .id)
# curl -X POST ${BASE}/api/docs/${DOC_ID}/versions -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"contentText":"<content>","changeNote":"T212 output"}'

# 3. Update task-item status → Done
# TASK_ITEM_ID=$(curl -s "${BASE}/api/task-items?projectId=${PROJECT_ID}" -b cookies.txt | \
#   jq -r '.[] | select(.taskConfigCode=="T212") | .id' | head -1)
# curl -X PATCH ${BASE}/api/task-items/${TASK_ITEM_ID} -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"doneStatus":"Done","completedAt":"<ISO timestamp>"}'
```

## Execution Procedure
1. Doc TAT CA docs: L1, L2, L3, DB schema, existing code
2. Section 1: ERD — ve entity relationships, list columns, constraints
3. Section 2: Process — describe business processes, status flows
4. Section 3: Wireframes — ASCII wireframe cho moi screen
5. Section 4: User Flows — sequence diagrams cho moi use case
6. Section 5: Complex Logic — algorithms, business rules, edge cases
7. Section 6: Feature & Layer — map feature → controller → service → tables
8. Section 7: Task Classification — doc layerevent.md, phan loai A/B/C
9. Self-review: checklist o tren
10. Output: ArchitecturePack_{project}_{version}.md

## Branching Logic
- Neu update AP cu → chi them "Whats new" section, giu nguyen phan cu
- Neu tao AP moi → tao day du tat ca sections
- Neu he thong co event layer → them Special Mechanisms section

## Validation Checklist
- [ ] Moi entity co trong ERD
- [ ] Moi workflow co user flow diagram
- [ ] Moi screen co wireframe
- [ ] Task classification cover TAT CA features
- [ ] AP du chi tiet de developer implement ma khong can hoi them

## Failure Cases
- AP thieu section → developer khong du thong tin
- ERD khong match code → implementation se sai
- Task classification sai → sai approval flow

## Recovery Actions
- Cross-check AP vs codebase sau khi viet
- Hoi user review tung section
- Dung T230 (Audit) de kiem tra lai

## Completion Criteria
- ArchitecturePack file created voi 7+ sections
- User reviewed va approved
- File saved vao docs/

## Next-Step Handoff
- T220 (Tao Incremental Steps Plan)

## Reusable Prompt Block
```text
Hãy đề xuất ArchitecturePack  cho hệ thống này:
1. EntityRelationshipDescription
2. ProcessDescription
3. UIWireFrame
4. UserFlows
5. Complex Logic
6. Feature & Layer
7 Task classification
    - Hãy đọc file [layerevent.md] để phân loại Task do user cung cấp theo tiêu chí sau:
    - Loại A: Nếu chỉ chỉnh sửa UI, API, không sửa Entity Schema
    -Loại B: Nếu có sửa Entity Schema, nhưng không đọc viết các bảng thuộc Layer Event và Event Details, và không viết các bảng thuộc Layer CalculateKR và ExtractEvent
    -Loại C: Nếu có sửa, đọc, viết các bảng thuộc Layer Event và Event Details, hoặc sửa, viết các bảng thuộc Layer CalculateKR và ExtractEvent

Lưu ý:
1. Không tạo thêm các entity mới trừ khi thật cần thiết, hãy sử dụng các entity đã có hoặc đề xuất thêm các trường trong các bảng.
2. Đưa ra các thêm các gợi ý để hoàn hiện system design
3. Đồng bộ lại các thông tin sau khi đã sửa đổi, nếu các logic mà không đồng bộ với nhau thì hỏi lại.
4. Nếu Task loại B, cần báo cho POSUP để duyệt Architecture Pack trước khi thực thi
5. Nếu Task loại C, cần báo cho POSUP và ARCH để duyệt Architecture Pack trước khi thực thi

Hãy đọc kĩ các file sau:
1. 
2. 
3. 

Xuất kết quả ra file: architecturepack_[project name]_[version]_[date].md
trong đó [project name], [version], [date] trong lịch sử chat hoặc chưa rõ thì hỏi lại user

```

## Examples
```text
<!-- EXAMPLE TEMPLATE: Phiên bản rút gọn từ ap-l1-stw.md. Dùng làm format reference cho AP L1 modules khác -->

# HỆ THỐNG STUDENT TUTOR WEB (STW) — SYSTEM DESIGN
## Phiên bản: Architecture Pack Level 1 (ap-l1 ver 1.2)
### Ngày cập nhật: 2026-03-11
### Module: STW (Micro-Frontend: stw-om + stw-om-common)

---

# MỤC LỤC

- **Phần 1:** EntityRelationshipDescription — Cấu trúc dữ liệu & quan hệ entity trong Redux Store
- **Phần 2:** ProcessDescription — Mô tả quy trình Redux Middleware & API orchestration
- **Phần 3:** UIWireFrame — Danh sách màn hình & wireframe
- **Phần 4:** UserFlows — Luồng người dùng theo vai trò
- **Phần 5:** Complex Logic — Thuật toán, công thức, business rules
- **Phần 6:** Feature & Layer — Module inventory, layer classification, dependency graph

---

> **Tech Stack:** React 18, TypeScript 4.9.5, Vite 4.4.9, Redux + Redux Persist + Redux Thunk, Ant Design 4.2.5, Axios, SockJS + StompJS (WebSocket)
> **Repos:** stw-om (container, port 3000) + stw-om-common (shared micro-FE, port 4000)
> **Deployment:** Docker + Nginx, port 80 production

---
---

# PHẦN 1: ENTITY RELATIONSHIP DESCRIPTION

Mô tả cấu trúc dữ liệu của hệ thống STW, mapping với Redux Store state và API response types.

## 1.1 Rà Soát Entity — Trạng Thái Trong Redux Store

> Đã đối chiếu với source code 2 repos stw-om và stw-om-common. Tất cả entity được quản lý qua Redux Store với redux-persist.

### Bảng tổng hợp trạng thái entity

| Entity | Redux Store Key | Module | Trạng thái | Ghi chú |
|--------|----------------|--------|------------|---------|
| **TUserAuth** | `authStore.userAuth` | auth2 | ✅ Core | Token, roles, user identity. Persist across sessions |
| **TUserInfo** | `authStore.userInfo` | auth2 | ✅ Core | Student info, grade_subject, chestnuts/stars/XP |
| **TDefaultGradeSubject** | `authStore.userInfo.default_grade_subject` | auth2 | ✅ Core | Current active pod. Chứa **product_component** flags |
| ... | ... | ... | ... | ... |

### Kết luận rà soát
- **17 entity chính** quản lý trong Redux Store qua ReducerRegistry
- **Core entities** (auth): persist qua redux-persist, available across all modules
- **Session entities** (test, live): clear sau khi kết thúc session
- **Realtime entities** (comments, DQS): cập nhật qua WebSocket
- **Dynamic entities** (rewards, banners, missions): load on-demand via API

---

## 1.2 Entity Store — User & Authentication

### Entity: TUserAuth — Token & Identity
**Purpose:** Lưu authentication state sau login. Dùng cho mọi API call (Bearer token).

| Field | Type | Stored/Computed | Mô tả | Ví dụ |
|-------|------|-----------------|-------|-------|
| `original_id` | number | **STORED (PK)** | Account ID | 12345 |
| `access_token` | string | **STORED (key)** | JWT Bearer token cho API calls | "eyJhbG..." |
| `roles` | string[] | STORED | User roles | ["STUDENT"] |
| ... | ... | ... | ... | ... |

> **Lưu ý:** `access_token` được lưu vào localStorage qua redux-persist. Dùng trong `initApi(token)` để set Axios default header.

### Entity: TUserInfo — Student Profile
**Purpose:** Thông tin học sinh: điểm, hạt dẻ, ngôi sao, pod đang active.

| Field | Type | Stored/Computed | Mô tả | Ví dụ |
|-------|------|-----------------|-------|-------|
| `student_id` | number | **STORED (PK)** | Student ID | 67890 |
| `default_grade_subject` | TDefaultGradeSubject | **STORED (FK)** | Pod đang active | (xem bảng dưới) |
| ... | ... | ... | ... | ... |

### Entity: TDefaultGradeSubject — Active Pod (Grade + Subject + Product)
**Purpose:** Pod hiện tại của học sinh. Chứa **product_component** — chuỗi feature flags quyết định UI.

| Field | Type | Stored/Computed | Mô tả | Ví dụ |
|-------|------|-----------------|-------|-------|
| `id` | number | STORED (PK) | Pod ID | 1001 |
| **`product_component`** | string | **STORED (key)** | **Feature flags** (comma-separated) | "QA,HRG,LI,PC,CAM,AIT_SOLVE" |
| `mypod` | string | **STORED (key)** | Pod ID cho API calls | "POD_1001" |
| ... | ... | ... | ... | ... |

> **product_component — Quan trọng:** Chuỗi này quyết định toàn bộ UI hiển thị trên Homepage.
> Chi tiết flags: xem §3.4 và §5.5.

---

## 1.3 Entity Store — Class & Schedule

### Entity: TClassInfo — Thông tin lớp học
**Purpose:** Lớp học của pod hiện tại. Chứa giáo viên, lịch học.

| Field | Type | Mô tả | Ví dụ |
|-------|------|-------|-------|
| `id` | number \| null | PK | 2001 |
| `class_code` | string | Mã lớp (unique) | "KMA-G4-C1-T35" |
| ... | ... | ... | ... |

### Entity: TResponseDailyScheduleClass — Lịch học hàng ngày
**Purpose:** Thông tin ca học: thời gian, GV, video backup, learning object. Nguồn chính cho Homepage schedule.

| Field | Type | Stored/Computed | Mô tả | Ví dụ |
|-------|------|-----------------|-------|-------|
| `ulc_code` | string | **STORED (PK)** | Unique Learning Component code | "KMA-G4-GET-C1-..." |
| `start_at` | number | **STORED (key)** | Thời điểm bắt đầu (Unix ms) | 1709820600000 |
| `lctType` | object | **STORED (key)** | Loại ca học | { code: "GET", name: "Get Class" } |
| ... | ... | ... | ... | ... |

> **lctType.code — Quan trọng:** Quyết định loại ca: `GET`, `LI`, `DLG`, `DLC`, `ORI`.

### Entity: TResponseScheduleDetail — Chi tiết sub-session
**Purpose:** Mỗi ca học có nhiều sub-sessions (segments). Chứa link join class.

| Field | Type | Mô tả | Ví dụ |
|-------|------|-------|-------|
| `podId` | number | Pod ID | 1001 |
| `link` | object | Link join class | { join_url, join_url_ios, join_url_adr, type } |
| ... | ... | ... | ... |

---

## 1.4 Entity Store — Test & Quiz

### Entity: TResponseStartTest — Bài kiểm tra thường
**Purpose:** Lưu 1 bài kiểm tra đang làm: danh sách câu hỏi, thời gian, trạng thái.

| Field | Type | Stored/Computed | Mô tả | Ví dụ |
|-------|------|-----------------|-------|-------|
| `test_id` | number | **STORED (PK)** | Test ID | 4001 |
| `expect_complete_in_seconds` | number | **STORED (key)** | Thời gian làm bài (giây) | 1800 |
| `student_test_details` | TResponseStartTestDetail[] | STORED (1:N) | Danh sách câu hỏi + đáp án | [{...}] |
| ... | ... | ... | ... | ... |

> **Auto-submit logic:** Khi countdown hết, hệ thống tự dispatch `SUBMIT_MISSION({ isAutoSubmit: true })`. Xem chi tiết §5.1.

---

## 1.5 Entity Store — GECE Test

### Entity: TResponseStartGECETest — Bài kiểm tra GECE (kiến trúc mới)
**Purpose:** GECE test có quiz types nâng cao: suggestion, solution, theme.

| Field | Type | Stored/Computed | Mô tả | Ví dụ |
|-------|------|-----------------|-------|-------|
| `test_id` | number | **STORED (PK)** | Test ID | 7001 |
| `questions` | TGECEQuizDetail[] | STORED (1:N) | Danh sách câu hỏi GECE | [{...}] |
| ... | ... | ... | ... | ... |

> **Khác biệt GECE vs Regular Test:**
> - GECE có `quiz_suggestion[]`, `quiz_solutions[]`, `theme_code` — regular test không có
> - GECE lưu `answer_at`, `answer_in_seconds` per quiz — tracking thời gian từng câu

---

## 1.6 Entity Store — Reward

### Entity: TStoreRewardItem — Quà tặng
**Purpose:** Danh sách quà đổi bằng hạt dẻ.

| Field | Type | Mô tả | Ví dụ |
|-------|------|-------|-------|
| `id` | number | PK | 9001 |
| `xchangeable_point_per_gift` | number | Hạt dẻ cần đổi | 100 |
| ... | ... | ... | ... |

---

## 1.7 Entity Store — AI Tutor

### Entity: AiTutorThread — Cuộc trò chuyện AI
**Purpose:** Thread AI Tutor. Mỗi thread gắn với 1 pod + 1 ULC.

| Field | Type | Stored/Computed | Mô tả | Ví dụ |
|-------|------|-----------------|-------|-------|
| `threadCode` | string | **STORED (PK)** | Thread code (unique) | "AIT_001" |
| `aiThreadStatus` | string | STORED | Trạng thái thread | "OPEN", "CLOSED" |
| ... | ... | ... | ... | ... |

---

## 1.8 Entity Store — Live Session & Comments

### Entity: ClassInfoType — Thông tin lớp LIVE
**Purpose:** Chi tiết buổi live: GV, thời gian, video, stage machine settings.

| Field | Type | Mô tả | Ví dụ |
|-------|------|-------|-------|
| `ulc_code` | string | FK → ULC | "KMA-G4-GET-..." |
| `video_url` | string \| null | URL stream video | "https://stream..." |
| ... | ... | ... | ... |

### Entity: DqsData — Dynamic Quiz Session (quiz trong live)
**Purpose:** Quiz popup trong buổi live. GV mở quiz → HS trả lời → tracking status.

| Field | Type | Stored/Computed | Mô tả | Ví dụ |
|-------|------|-----------------|-------|-------|
| `no` | number | **STORED (PK)** | Số thứ tự DQS | 1, 2, 3 |
| `stStatus` | string | **STORED (key)** | Trạng thái HS | "NONE", "RECEIVED", "ANSWERED" |
| ... | ... | ... | ... | ... |

> **DQS State Machine:** `NONE` → (GV mở quiz) → `RECEIVED` → (HS trả lời) → `ANSWERED`

### Entity: TResponseGetComment — Comment trong live session

| Field | Type | Mô tả | Ví dụ |
|-------|------|-------|-------|
| `id` | string | PK | "CMT_001" |
| `content` | string | Nội dung | "Em hiểu rồi!" |
| ... | ... | ... | ... |

---

## 1.9 Entity Store — Mission & Homework

### Entity: TMissionType — Bài tập về nhà
**Purpose:** Mission = homework assignment.

| Field | Type | Mô tả | Ví dụ |
|-------|------|-------|-------|
| `id` | number | PK | 10001 |
| `status` | string | Trạng thái | "NEW", "IN_PROGRESS", "COMPLETED", "EXPIRED" |
| ... | ... | ... | ... |

---

## 1.10 Entity Store — Homepage & Campaign

### Entity: TStoreBanner — Banner campaign
**Purpose:** Banner hiển thị trên homepage. Campaign-driven từ CMS.

| Field | Type | Mô tả | Ví dụ |
|-------|------|-------|-------|
| `cta` | string (URL) | Call-to-action link | "https://clevai.vn/promo" |
| `template` | object | Template config | { id, code: "CTT_IMG", type, value } |
| ... | ... | ... | ... |

> **Template types:** `CTT_IMG` (image), `CTT_HTML` (popup), `CTT_GECO` (GECE Online), `CTT_GECE` (GECE redirect).

---

## 1.11 Entity Store — 247 Q&A

### Entity: ThreadItemType — Thread hỏi đáp 24/7
**Purpose:** Câu hỏi của HS gửi cho GV.

| Field | Type | Mô tả | Ví dụ |
|-------|------|-------|-------|
| `ulc_code` | string | FK → ULC | "KMA-G4-GET-..." |
| `question` | ThreadContentType \| null | Câu hỏi | { id, text, file_url, created_at } |
| ... | ... | ... | ... |

---

## 1.12 Mối Quan Hệ Giữa Các Entity

### Store → Store Relationships

| Quan hệ | Cardinality | Mô tả |
|---------|-------------|-------|
| TUserAuth → TUserInfo | 1:1 | `student_id` link account → student |
| TUserInfo → TDefaultGradeSubject | 1:N | `list_grade_subject[]` — HS enroll nhiều pods |
| TDefaultGradeSubject → TClassInfo | N:1 | `class_info` — pod thuộc 1 lớp |
| ... | ... | ... |

### Cross-Module Store Dependencies

| From Module | To Module | Data Used | Mechanism |
|------------|----------|-----------|-----------|
| ALL modules | auth2 | `userAuth`, `userInfo`, `mypod` | Direct store access |
| stw-om-common | stw-om | Container store state | `window["container-store"]` |
| ... | ... | ... | ... |

---

## 1.13 Entity Relationship Diagram (Text)

```
┌──────────────────────────────────────────────────────────────────────┐
│                        USER & AUTH LAYER                             │
├──────────────────────────────────────────────────────────────────────┤
│  TUserAuth ──1:1──> TUserInfo ──1:N──> TDefaultGradeSubject         │
│                                          │ product_component (flags) │
│                                          │ class_info (FK)           │
├──────────────────────────────────────────────────────────────────────┤
│                   SCHEDULE & CLASS LAYER                              │
│  TClassInfo ◄─N:1── TResponseDailyScheduleClass ──1:N──> Detail     │
├──────────────────────────────────────────────────────────────────────┤
│                      ASSESSMENT LAYER                                │
│  TResponseStartTest ──1:N──> TestDetail                              │
│  TResponseStartGECETest ──1:N──> GECEQuizDetail                     │
├──────────────────────────────────────────────────────────────────────┤
│                    ENGAGEMENT & AI LAYER                              │
│  TStoreRewardItem | AiTutorThread | TMissionType | ClassInfoType    │
│  DqsData[] | ThreadItemType | TStoreBanner | TStoreTestInfo         │
└──────────────────────────────────────────────────────────────────────┘
```

---

## 1.14 Enums

```typescript
// === Auth & User ===
enum PodStatusEnum { ACTIVE = "ACTIVE", INACTIVE = "INACTIVE", DEFERRED = "DEFERRED", REFUNED = "REFUNED" }
enum ProductTypeEnum { LIVE = "LIVE", LMS = "LMS" }

// === Session Types (lctType.code) ===
type LctCode = "GE" | "LI" | "DLG" | "DLC" | "ORI" | "PC" | "RC" | "HW";

// === Live Stream Stages ===
enum LIVE_STAGE {
    WAITING_VIDEO, PLAY_VIDEO, COACHING, COACHING_END, COACHING_SUMMARY,
    RATING, BATTLE, BATTLE_START, BATTLE_SUMMARY, OUT_OF_TIME,
    GET_CLASS, LIVE_ZOOM_START, LIVE_ZOOM_RUNNING, LIVE_ZOOM_END
}

// === WebSocket Events ===
enum SOCKET_EVENT {
    ERROR = 1000, ERROR_TOKEN = 1001, PACKING_MESSAGE = 1002, GZIP_PACKING_MESSAGE = 1003,
    JOIN_ROOM = 3000, SEND_CHAT = 3002, LIKE_CHAT = 3003, GZIP_LOAD_COMMENT = 3005,
    DQS_OPEN_ANSWER = 3007, DQS_UPDATE_ST_STATUS = 3008, FINISH_ULC = 3011
    // ... thêm các event khác
}

// === Product Component Flags ===
type ProductComponentFlag =
    | "LI" | "GE" | "PC" | "HRG" | "HAV" | "DHW"
    | "HORG" | "RL" | "QA" | "AITUTOR" | "AIT_SOLVE" | "AIT_SPEAK" | "CAM";
```

### Kết luận rà soát

- **17 entity chính** trong Redux Store, quản lý qua ReducerRegistry singleton
- **2 kiến trúc module**: Modern (`application/`) dùng Symbol-based, Legacy (`redux/`) dùng string-based
- **Cross-micro-FE**: stw-om-common đọc container store qua `window["container-store"]`
- **Key enums**: 8 enum groups covering auth, session, component states, DQS, live stages, WebSocket, product flags
- **product_component** (comma-separated string) là flag system quan trọng nhất — quyết định toàn bộ UI Homepage

---

## 1.15 ERD Cross-Reference — Backend Database Entities

> Mapping giữa STW Redux Store entities (frontend) và Backend Database entities (ERD files).

### Bảng tổng hợp ERD References

| # | STW Entity (Frontend) | ERD File | DB Entity (Backend) | DB Table | Mô tả liên kết |
|---|----------------------|----------|--------------------|---------|----|
| 1 | **TUserAuth** / **TUserInfo** | PR2411_Commercial_POD | **USI** | `bp_usi_useritem` | Username, email, phone → GET /v1/sts/my-info |
| 2 | **TDefaultGradeSubject** (mypod) | PR2411_Commercial_POD | **POD** | `bp_pod_productofdeal` | Sản phẩm đã mua: status ACTIVE/INACTIVE |
| 3 | **TResponseDailyScheduleClass** | PR2451_Schedule | **ULC** / **CAP** | `bp_ulc_*` / `bp_cap_*` | Buổi học + Calendar hierarchy |
| ... | ... | ... | ... | ... | ... |

### Chi tiết ERD Files và STW Entities liên quan

| # | ERD File | Mô tả ERD | STW Entities liên quan | Mức độ |
|---|----------|----------|----------------------|--------|
| 1 | **PR2411_Commercial_POD** | User, Deal, POD, Product | TUserAuth, TUserInfo, TDefaultGradeSubject | 🔴 Cao |
| 2 | **PR2421_Register_LO** | Learning Objects, Quiz | TResponseStartTest, TMissionType | 🔴 Cao |
| 3 | **PR2433_Delivery_ULC** | ULC, CLAG, CUI | TResponseDailyScheduleClass, ClassInfoType | 🔴 Cao |
| ... | ... | ... | ... | ... |

---
---

# PHẦN 2: PROCESS DESCRIPTION

Mô tả kiến trúc Redux Middleware, quy trình xử lý từng module, state machine, và bảng tổng hợp API endpoints.

## 2.0 Kiến Trúc: Redux Middleware Architecture

> STW sử dụng Redux Middleware pattern thay cho Redux Thunk thuần túy. Mỗi module đăng ký middleware riêng thông qua **MiddlewareRegistry** (singleton).

```
┌──────────────────────────────────────────────────────────────┐
│                       COMPONENT (View)                        │
│   dispatch(ACTION_TYPE)                                       │
└───────────────────────────┬──────────────────────────────────┘
                            │
                            ▼
┌──────────────────────────────────────────────────────────────┐
│                    MIDDLEWARE CHAIN                            │
│   MiddlewareRegistry.applyMiddleware()                        │
│   auth2MW → homepageMW → takeTestMW → learningMW → ...      │
│   Each MW: if (action.type === MY_ACTION) { handle() }        │
│            else { next(action) }                              │
└───────────────────────────┬──────────────────────────────────┘
                            │
                            ▼
┌──────────────────────────────────────────────────────────────┐
│                     REDUCER (Domain)                          │
│   ReducerRegistry.combineReducers(persistConfig)              │
│   + redux-persist → localStorage (offline caching)            │
└───────────────────────────┬──────────────────────────────────┘
                            │
                            ▼
┌──────────────────────────────────────────────────────────────┐
│              window["container-store"]                         │
│   Cross-micro-FE: stw-om-common reads container store         │
└──────────────────────────────────────────────────────────────┘
```

## 2.1 Bảng Tổng Hợp Process Theo Module

| # | Module | Middleware Actions | Trigger | API Endpoint chính | Ghi chú |
|---|--------|-------------------|---------|-------------------|---------|
| 1 | **auth2** | SUBMIT_LOGIN, GET_USER_INFO | User login, page load | POST /v1/user/login/lms-web, GET /v1/sts/my-info | Core — mọi module phụ thuộc |
| 2 | **homepage (app)** | GET_SCHEDULE_INFO, GET_BANNER, GET_TIME_SERVER | Homepage load | GET /v1/sts/my-shift, GET /v1/cmp/show-by-usi-ulc | Parallel API calls |
| 3 | **take-test** | START_TEST, SUBMIT_QUIZ, SUBMIT_MISSION | User click "Làm bài" | POST .../start-test, .../answer/quiz, .../submit-test | Session-scoped |
| ... | ... | ... | ... | ... | ... |

## 2.2 Chi Tiết Process Steps

### P01 — AUTH (Đăng nhập & Xác thực)

| Step | Action | API | Input | Output | Ghi chú |
|------|--------|-----|-------|--------|---------|
| P01.1 | SUBMIT_LOGIN | POST /v1/user/login/lms-web | {username, password} | {access_token, refresh_token, roles} | initApi(token) sau khi nhận |
| P01.2 | SET_USER_AUTH | — (dispatch) | Login response | authStore.userAuth | Persist across sessions |
| P01.3 | UPDATE_ACCOUNT_INFO | — (dispatch) | Login response | authStore.accountInfo | username, full_name |
| P01.4 | GET_USER_INFO | GET /v1/sts/my-info | access_token (header) | {student_id, default_grade_subject} | Validate pod status |
| P01.5 | SET_USER_INFO | — (dispatch) | my-info response | authStore.userInfo | Check DEFERRED/REFUNED → expired popup |
| P01.6 | SET_META_DATA | — (dispatch) | Derived data | authStore.metaData | Redirect to /stw-common/homepage |

> **Luồng SSO**: Nếu user từ LMS → token trong URL params → skip P01.1 → trực tiếp P01.4

```
User ──[username/pwd]──→ POST /v1/user/login/lms-web
                              │
                              ▼
                     {access_token, refresh_token, roles}
                              │
                     initApi(access_token) → Axios default header
                              │
                     GET /v1/sts/my-info → Check pod → Homepage
```

### P02 — HOMEPAGE (Tải trang chủ)

> **Parallel dispatch**: P02.1–P02.5 dispatch đồng thời khi page mount. Không có dependency.

### P03 — TAKE TEST (Làm bài kiểm tra)

> Luồng: START_TEST → Quiz Loop (SUBMIT_QUIZ) → SUBMIT_MISSION (manual/auto)

### P04 — GECE TEST (Kiểm tra định kỳ GECE)

> New architecture: request/success/failure. Trigger từ popup banner. localStorage tracking.

### P05 — LIVE STREAM (Phiên học trực tuyến)

> Check lct_code → Join request → Parallel API calls → WebSocket Connect → Live Loop

### P06 — REWARD (Đổi quà)

> GET gifts → Like/Favorite → Exchange (check chestnuts >= cost)

### P07 — AI TUTOR (Trợ lý AI)

> Thread list → Create thread → WebSocket chat loop → Rate thread

### P08 — 247 Q&A (Hỏi đáp 24/7)

> Thread list → Filter → Submit question (FormData + image compress)

### P09 — COACHING (Luyện tập sau live)

> Live end → Coaching list → Quiz per LO → BPP_FINISH_AQR → XP rewards

## 2.3 Process State Machine

```
                    ┌───────────┐
                    │   IDLE    │ ←── Initial state
                    └─────┬─────┘
                          │ dispatch(ACTION_REQUEST)
                    ┌─────▼─────┐
                    │  LOADING  │
                    └─────┬─────┘
                     ┌────┴────┐
                ┌────▼───┐ ┌──▼─────┐
                │SUCCESS │ │ ERROR  │
                └────────┘ └────────┘
```

> **Modern modules**: `_REQUEST / _SUCCESS / _FAILURE` pattern.
> **Legacy modules**: `ACTION → SET_DATA` (implicit success).

## 2.4 Bảng Tổng Hợp API Endpoints

| # | Method | Endpoint | Module | Middleware Action | Mô tả |
|---|--------|----------|--------|------------------|-------|
| 1 | POST | /v1/user/login/lms-web | auth2 | SUBMIT_LOGIN | Đăng nhập |
| 2 | GET | /v1/sts/my-info | auth2 | GET_USER_INFO | Thông tin student |
| 3 | GET | /v1/sts/my-shift | homepage | GET_SCHEDULE_INFO | Lịch học |
| 4 | POST | /v1/scheduling/student-test/start-test | take-test | START_TEST | Bắt đầu làm bài |
| 5 | GET | /v1/community/ws/auth/principal | learning | WS_AUTH | Token WebSocket |
| ... | ... | ... | ... | ... | ... |

### Kết luận

- **38 API endpoints** chính, phân bố qua 13 modules có middleware
- **2 pattern**: Modern (request/success/failure) và Legacy (action/set)
- **3 external services**: Gateway REST API, SEDA (background processing), WebSocket (real-time)
- **Parallel dispatch** trên Homepage: 5+ API calls đồng thời khi page load
- **WebSocket**: SockJS + StompJS, JWT auth, GZIP compressed, 18 event types (cmd 1000–3015)

---
---

# PHẦN 3: UI WIREFRAME

Danh sách màn hình, ASCII wireframe cho các screens chính, conditional rendering logic, và mobile/desktop differences.

## 3.1 Danh Sách Screens

### A. Core Pages (8)

| # | Screen | URL | Mô tả | Module | Repo |
|---|--------|-----|-------|--------|------|
| 1 | **Homepage** | /stw-common/homepage | Trang chủ: schedule, banners, AI study, gifts, tests | homepage | stw-om-common |
| 2 | **Take Test** | /stw-common/test | Làm bài kiểm tra: countdown, quiz navigation | take-test | stw-om |
| 3 | **Live Stream** | /livestream/room | Phòng học live: video, DQS, comments, camera | learning | stw-om |
| ... | ... | ... | ... | ... | ... |

### B. Feature Pages (8)

| # | Screen | URL | Mô tả | Module | Repo |
|---|--------|-----|-------|--------|------|
| 9 | **Reward Store** | /stw-common/rewards | Cửa hàng quà | reward / base2 | stw-om / stw-om-common |
| ... | ... | ... | ... | ... | ... |

### C. Coaching Pages (5)

| # | Screen | URL | Mô tả | Module | Repo |
|---|--------|-----|-------|--------|------|
| 17 | **Coaching List** | /coachings | Danh sách coaching LOs | coaching | stw-om |
| ... | ... | ... | ... | ... | ... |

> **Tổng: 21 screens** — 8 core, 8 feature, 5 coaching.

## 3.2 ASCII Wireframes

### Screen #1: Homepage

```
+================================================================+
|  [Logo]    [SubjectSelect ▼]                   [Avatar] [Bell]  |
+================================================================+
|                                                                  |
|  +--[Banner Carousel]----------------------------------------+  |
|  |  [Campaign Image / HTML / GECE Banner]                    |  |
|  +-----------------------------------------------------------+  |
|                                                                  |
|  +--LEFT BLOCK------------------+ +--RIGHT BLOCK--------------+ |
|  |  [Banner100Acorn]            | |  [ScheduleArea]            | |
|  |  [GroupAIButton] (desktop)   | |  | Prev | Current | Next  || |
|  |  [GiftArea]                  | |  | [Countdown Timer]      || |
|  +------------------------------+ |  | [Vào lớp LIVE] btn     || |
|                                    |  [AIStudyArea] (HRG/HAV) | |
|                                    |  [ChallengeArea] (PC)    | |
|                                    +----------------------------+ |
+==================================================================+
```

> **Conditional rendering**: Mỗi block hiển thị dựa trên **product_component** flags. Xem §3.4.
> (Xem thêm 7 wireframes khác: Take Test, Live Stream Desktop/Mobile, Reward, Profile, 247 QA, Coaching trong bản đầy đủ)

## 3.3 Tổng Hợp Screens Per Module

| Module | Screens | Repo | Ghi chú |
|--------|---------|------|---------|
| **homepage** | Homepage, Curriculum | stw-om-common | product_component flags quyết định layout |
| **take-test** | Take Test, Test Result, Test Summary | stw-om | Countdown + auto-submit |
| **learning** | Live Stream, Live Ref | stw-om | WebSocket + video + DQS |
| ... | ... | ... | ... |

## 3.4 Conditional Rendering — Product Component Flags

> **product_component** là comma-separated string. Quyết định UI nào hiển thị trên Homepage.

| # | Function | Flag Code | UI Component | Mô tả |
|---|----------|-----------|-------------|-------|
| 1 | `isShowLivestream()` | **LI**, **GE** | ScheduleArea → "Vào lớp LIVE" btn | Live Interactive hoặc GET class |
| 2 | `isShowMission()` | **PC** | ChallengeArea → Test Cards | Challenge/test trên homepage |
| 3 | `isShowAIStudy()` | **HRG**, **HAV** | AIStudyArea → LO grids | AI Study với learning objects |
| ... | ... | ... | ... | ... |

## 3.5 Mobile vs Desktop Layout

| Aspect | Desktop | Mobile | Ghi chú |
|--------|---------|--------|---------|
| **Homepage** | 2 columns (LEFT + RIGHT) | Single column, stacked | GroupAIButton hidden on mobile |
| **Live Stream** | Video + Panel side-by-side | Video top, redirect external (DLG) | isDLG → ClassIn URL |
| ... | ... | ... | ... |

### Kết luận

- **21 screens** phân bố qua 11 modules, 2 repos
- **8 ASCII wireframes** cho các screens chính
- **12 product_component flags** quyết định conditional rendering trên Homepage
- **Mobile/Desktop**: Layout chuyển đổi qua `isMobileDevice()`

---
---

# PHẦN 4: USER FLOWS

Luồng người dùng theo từng vai trò. STW có 2 vai trò chính: **Student** (end user) và **System** (automated processes).

## 4.1 Các Vai Trò

| Vai trò | Mô tả | Quyền hạn | Ghi chú |
|---------|-------|-----------|---------|
| **Student** | Học sinh sử dụng STW platform | Login, xem schedule, làm bài, join live, đổi quà, hỏi AI, 247 QA | End user chính |
| **System** | Automated backend processes | WebSocket events, server time sync, auto-submit, session detection | Không có UI riêng |
| **Teacher (Live)** | Giáo viên trong live session | Open DQS quiz, send comments, control session stages | Tương tác gián tiếp qua WebSocket |

## 4.2 Flow 1: Student — Login & Homepage Load

> **Actor**: Student
> **Precondition**: Có tài khoản STW hoặc SSO từ LMS
> **Postcondition**: Homepage rendered với schedule, banners, tests, LOs

| Step | Actor | Action | Screen/URL | API Call | Kết quả |
|------|-------|--------|------------|----------|---------|
| F1.1 | Student | Nhập username + password | /login | — | — |
| F1.2 | Student | Click "Đăng nhập" | /login | POST /v1/user/login/lms-web | {access_token, refresh_token} |
| F1.3 | System | initApi(access_token) | — | — | Axios header set |
| F1.4 | System | Fetch user info | — | GET /v1/sts/my-info | {student_id, default_grade_subject} |
| F1.5 | System | Validate pod status | — | — | DEFERRED/REFUNED → expired popup |
| F1.6 | System | Redirect to homepage | /stw-common/homepage | — | SET_USER_INFO, SET_META_DATA |
| F1.7 | System | Parallel data fetch | /stw-common/homepage | GET my-shift, banners, tests, homework, time-server | Homepage data loaded |
| F1.8 | Student | Xem homepage | /stw-common/homepage | — | Schedule + banners + tests + LOs |

```
Student ──[Login]──→ POST login → GET my-info
   ├──[pod valid]──→ SET_USER_INFO → Homepage (parallel: schedule, banners, tests, homework, time)
   └──[pod expired]──→ Show "Hết hạn" popup
```

## 4.3 Flow 2: Student — Take Test (Regular)

> START_TEST → Quiz Loop (answer each) → SUBMIT (manual or auto-countdown=0) → Results

## 4.4 Flow 3: Student — GECE Test

> GECE banner popup → START_TEST_REQUEST → Answer per quiz → Submit → localStorage tracking

## 4.5 Flow 4: Student — Join Live Stream

> Schedule countdown → Click "Vào lớp LIVE" → Check lct_code → Join → WebSocket → DQS + Comments

> **Mobile (DLG)**: redirect external URL (ClassIn). Không vào /livestream/room.

## 4.6 Flow 5: Student — Reward Exchange

> Gift list → Like/Favorite → "Nhận quà" → Confirm → Exchange (check chestnuts)

## 4.7 Flow 6: Student — AI Tutor Chat

> AITUTOR flag → Thread list → Create thread → WebSocket chat → Rate

## 4.8 Flow 7: Student — 247 Q&A Submit Question

> QA flag → Thread list → Filter → Create question (FormData + image) → Prepend to list

## 4.9 Flow 8: Student — Coaching

> Live session end → Coaching list → Quiz per LO → XP rewards → BPP_FINISH_AQR

## 4.10 Flow 9: System — WebSocket Events (DQS, Comments)

| Step | Actor | Event | WebSocket cmd | Data | Kết quả |
|------|-------|-------|--------------|------|---------|
| F9.1 | System | Join room | 3000 | {ulcCode, clagGroupCode} | Initial DQS + comments |
| F9.2 | Teacher | Open DQS quiz | 3007 | {dqsNo, teStatus: "OPEN"} | Quiz popup shown |
| F9.3 | Student | Answer DQS | 3008 | {dqsNo, stStatus: "ANSWERED"} | Answer recorded |
| ... | ... | ... | ... | ... | ... |

## 4.11 Flow 10: System — Server Time Sync & Session Detection

> Fetch server time → Calculate delta → Store in localStorage → getServerDateTime() everywhere

### Kết luận

- **10 user flows**: 8 Student flows + 2 System flows
- **Key patterns**: Parallel API calls (homepage), WebSocket real-time (live), localStorage persistence
- **Mobile differences**: Live stream DLG → external redirect

---
---

# PHẦN 5: COMPLEX LOGIC

Thuật toán, business rules, pseudocode cho các logic phức tạp trong hệ thống STW.

## 5.1 Test Countdown & Auto-Submit Algorithm

> **Source**: `stw-om/src/modules/take-test/views/components/test-count-down.tsx`

```python
# === Test Countdown & Auto-Submit Algorithm ===
def calculate_countdown(student_test_time, expect_complete_in_seconds):
    """Tính thời gian còn lại dựa trên server time."""
    tested_at = student_test_time.tested_at if student_test_time else 0
    server_now = get_server_date_time()
    countdown_seconds = (expect_complete_in_seconds * 1000 + tested_at - server_now) / 1000
    if countdown_seconds <= 0:
        trigger_auto_submit()
        return 0
    return countdown_seconds

def on_countdown_complete(finished_at, flag_auto_submit, test_id, mypod):
    """Callback khi timer về 0."""
    if finished_at is None and flag_auto_submit is False:
        flag_auto_submit = True  # Prevent double submit
        dispatch(SUBMIT_MISSION, {"isAutoSubmit": True, "test_id": test_id, "mypod": mypod})
    # ...
```

> **Key insight**: `getServerDateTime()` thay vì `Date.now()` — client clock có thể sai/bị chỉnh.

## 5.2 WebSocket Connection Algorithm (JWT, Reconnect, GZIP)

> **Source**: `stw-om/src/modules/common2/lib/websocket.ts`

```python
# === WebSocket Connection Algorithm ===
def connect_websocket():
    token = api_get("/v1/community/ws/auth/principal").access_token
    socket = SockJS(f"{BASE_API_URL}websocket/sockjs?access_token={token}")
    stomp = Stomp.over(socket)
    stomp.connect({}, on_connected, on_error)

def on_message_received(raw_message):
    cmd = parse_header(raw_message).cmd
    if cmd == 1003:  # GZIP_PACKING_MESSAGE
        data = pako.inflate(raw_message.body, {"to": "string"})
    # Route by cmd: 3000=JOIN_ROOM, 3007=DQS_OPEN, 3008=DQS_STATUS, 3011=FINISH_ULC...
    # ...
```

## 5.3 Session Detection Algorithm

> **Source**: `stw-om/src/modules/homepage/adapter/functions.ts`

```python
def is_in_the_session(start_at, duration):
    current_time = get_server_date_time()
    return current_time >= start_at and current_time <= start_at + duration * 1000

def is_get_session(lct_code): return lct_code.startswith("GE")
def is_live_session(lct_code): return lct_code.startswith("LI")
# ...
```

## 5.4 Server Time Synchronization (deltaTime)

> **Source**: `stw-om/src/modules/common2/utils/date.ts`

```python
def sync_server_time():
    server_time = api_get("/v1/dictionary/current-time-server")
    delta_time = server_time - time.now()
    localStorage.set("delta_time", delta_time)

def get_server_date_time():
    delta_time = localStorage.get("delta_time") or 0
    return time.now() + delta_time
# Sử dụng: test countdown, session detection, JWT check, schedule countdown, auto-submit
```

## 5.5 Feature Flags — Product Component Logic

> **Source**: `stw-om/src/modules/homepage/adapter/functions.ts`

```python
def is_show_livestream(pc): return "LI" in pc or "GE" in pc
def is_show_mission(pc): return "PC" in pc
def is_show_ai_study(pc): return "HRG" in pc or "HAV" in pc
# ... 12 flag functions total
# JSX: {is_show_ai_study(user_info.default_grade_subject.product_component) && <AIStudyArea />}
```

## 5.6 Live Stream Stage Machine

> **Source**: `stw-om/src/modules/learning/utils/constants.ts`

```
WAITING_VIDEO → PLAY_VIDEO → [COACHING | BATTLE | RATING]
  COACHING → COACHING_END → COACHING_SUMMARY
  BATTLE → BATTLE_START → BATTLE_SUMMARY
  (terminal: RATING, OUT_OF_TIME)
```

## 5.7 DQS (Dynamic Quiz Session) Algorithm

> Teacher opens quiz (cmd 3007) → Student receives → Student answers (cmd 3008) → Status tracked

```python
# DQS State: NONE → RECEIVED → ANSWERED
def handle_dqs_open(data):  # cmd 3007
    dqs.opening = True; dqs.teStatus = "OPEN"; show_dqs_popup(dqs)
def student_answer_dqs(dqs_no, selected_options):  # cmd 3008
    dqs.stStatus = "ANSWERED"; stomp.send(message)
# ...
```

## 5.8 Redux Middleware Chain Pattern

> **Source**: `stw-om/src/modules/common2/redux/MiddlewareRegistry.ts`

```python
class MiddlewareRegistry:  # Singleton
    def register(self, middlewares): self._middlewares.extend(middlewares)
    def apply_middleware(self): return redux.applyMiddleware(*self._middlewares)

class ReducerRegistry:  # Singleton
    def register(self, name, reducer): self._reducers[name] = reducer
    def combine_reducers(self, persist_config): # combineReducers + persistReducer
# ...
```

## 5.9 Micro-Frontend Communication

> `window["container-store"]` — stw-om exposes store, stw-om-common reads/dispatches.

## 5.10 Performance KPIs

| # | KPI | Target | Module liên quan |
|---|-----|--------|-----------------|
| 1 | **Homepage Load Time** | < 3s | homepage, auth2 |
| 2 | **WebSocket Connect Time** | < 1s | learning |
| 3 | **DQS Response Time** | < 500ms | learning (WebSocket) |
| ... | ... | ... | ... |

## 5.11 Consistency Validation Checklist

| # | Kiểm tra | Kết quả |
|---|----------|---------|
| 1 | Mọi entity trong §1 đều có process sử dụng trong §2 | ✅ 17/17 |
| 2 | Mọi API endpoint trong §2.4 đều có entity response type trong §1 | ✅ 38 endpoints |
| 3 | Mọi screen trong §3.1 đều có user flow trong §4 | ✅ 21 screens → 10 flows |
| 4 | product_component flags trong §3.4 match §5.5 pseudocode | ✅ 12 flags |
| 5 | WebSocket events trong §4.10 match §5.2 event handling | ✅ 10 events |
| ... | ... | ... |

## 5.12 Gợi Ý Hoàn Thiện

| # | Gợi ý | Ưu tiên | Mô tả |
|---|-------|---------|-------|
| 1 | **Error Boundary per Module** | Cao | Mỗi module nên có error boundary riêng |
| 2 | **Retry with Exponential Backoff** | Cao | WebSocket reconnect dùng exponential backoff |
| 3 | **Redux Toolkit Migration** | Thấp | Migrate từ raw Redux sang RTK |
| ... | ... | ... | ... |

### Kết luận

- **12 algorithms** với pseudocode: test countdown, WebSocket, session detection, deltaTime, feature flags, stage machine, DQS, middleware chain, micro-FE communication
- **10 performance KPIs** với targets cụ thể
- **25-item consistency checklist** — tất cả ✅ PASS
- **10 gợi ý hoàn thiện** ưu tiên từ Cao → Thấp

---
---

# PHẦN 6: FEATURE & LAYER

Module inventory, layer classification, dependency graph, action types per module, MiddlewareRegistry/ReducerRegistry pattern, và Clean Architecture per module.

## 6.1 Module Inventory

### A. STW-OM Modules (22 modules — Container App)

| # | Module | Purpose | Has Reducer | Has Middleware | Architecture Style | Ghi chú |
|---|--------|---------|-------------|---------------|-------------------|---------|
| 1 | **ask-ai-tutor** | AI tutoring assistant | ✅ | ✅ | Modern (application/) | WebSocket chat |
| 2 | **auth2** | Authentication & user info | ✅ | ✅ | Modern (application/) | Core — mọi module phụ thuộc |
| 3 | **homepage** | Homepage with schedule/banners | ✅ | ✅ ×2 | **Hybrid** (app + redux) | 2 middleware files |
| ... | ... | ... | ... | ... | ... | ... |

### B. STW-OM-Common Modules (8 modules — Child Micro-FE)

| # | Module | Purpose | Has Reducer | Has Middleware | Architecture Style |
|---|--------|---------|-------------|---------------|-------------------|
| 1 | **247** | 24/7 Q&A threads | ✅ | ✅ | Modern (application/) |
| 2 | **homepage** | Homepage (common version) | ✅ | ✅ | Modern (application/) |
| ... | ... | ... | ... | ... | ... |

> **Tổng: 30 modules** (22 + 8). **23 modules có reducer**, **17 modules có middleware**.

## 6.2 Layer Classification (7 Layers)

```
AUTH LAYER      → auth2 (core dependency)
UI LAYER        → homepage, live, common, tour, profile
LEARNING LAYER  → learning, coaching, 247, classin
ASSESSMENT LAYER → take-test, take-test-gece, sse, missions
AI LAYER        → ask-ai-tutor
ENGAGEMENT LAYER → reward, gamification, rating, base2
UTILITY LAYER   → common2, caches, redirect, subject (base layer, no dependencies)
```

| Layer | Modules | Vai trò | Dependencies |
|-------|---------|---------|-------------|
| **Auth** | auth2 ×2 | Xác thực, token, user info | Utility |
| **UI** | homepage, live, common, tour, wellcome, profile | Rendering, navigation | Auth, Utility |
| ... | ... | ... | ... |

## 6.3 Module Dependency Graph

```
common2 (utility core — no dependencies)
  ↑
  ├── auth2 → common2
  │     ↑
  │     ├── homepage → auth2, common2
  │     │     ├── take-test → homepage, auth2, common2
  │     │     └── live → homepage, common2
  │     ├── learning → homepage, auth2, common2, live
  │     │     └── ask-ai-tutor → learning, auth2, common2
  │     ├── base2 → auth2, common2
  │     │     └── coaching → base2, auth2, common2
  │     ├── reward, rating, tour, profile → auth2, common2

Cross-micro-FE (stw-om-common reads stw-om):
  247, sse, classin, base, homepage(common), profile(common)
    → ALL read window["container-store"]
```

## 6.4 Action Types Per Module

| # | Module | Action Count | Key Actions | Pattern |
|---|--------|-------------|-------------|---------|
| 1 | ask-ai-tutor | **22** | GET/SET threads, CREATE/RATE thread | Modern |
| 2 | auth2 | **14** | LOGIN, GET/SET user info, SSO | Modern |
| 3 | base2 | **28** | BPP_JOIN_REQUEST (AQR/CQS/AQS), REWARDS | Modern |
| ... | ... | ... | ... | ... |

> **Tổng: ~223 action types** qua 21 modules.

## 6.5 Middleware Registry Pattern

```
Module index.ts (import time)
  ├── import './domain/reducer'         → ReducerRegistry.register()
  └── import './application/middleware'  → MiddlewareRegistry.register()
  ▼
Store creation → ReducerRegistry.combineReducers() + MiddlewareRegistry.applyMiddleware()
  ▼
window["container-store"] = store → stw-om-common reads
```

## 6.6 Clean Architecture Pattern Per Module

```
views/          → pages/, components/, styles/ (UI rendering)
application/    → action-types.ts, actions.ts, middleware.ts, functions.ts (orchestration)
domain/         → request-type.ts, response-type.ts, store-type.ts, reducer.ts (types & state)
adapter/        → services.ts, functions.ts (external communication)
utils/          → constants.ts, types.ts, functions.ts (utilities)
index.ts        → Module entry point, registration trigger
```

### Architecture Style Comparison

| Aspect | Modern (application/) | Legacy (redux/) |
|--------|----------------------|-----------------|
| **Action types** | Symbol-based → type safety | String-based → simpler |
| **Modules using** | auth2, take-test, learning, coaching, reward, ask-ai-tutor... (17 modules) | homepage (partial), live, common, caches... (6 modules) |

> **homepage** là module duy nhất dùng **Hybrid**: application/ + redux/ — 2 middleware files cùng tồn tại.

### Kết luận

- **30 modules** (22 stw-om + 8 stw-om-common), **7 layers**, **~223 action types**
- **MiddlewareRegistry + ReducerRegistry**: Singleton pattern cho dynamic module registration
- **Clean Architecture**: 5 directories per module (views, application, domain, adapter, utils)
- **2 architecture styles**: Modern (17 modules) và Legacy (6 modules), 1 Hybrid (homepage)
- **Cross-micro-FE**: `window["container-store"]`
- **Docker + Nginx**: Multi-stage build, port 80 production

---

> **Tổng kết tài liệu ap-l1 ver 1.2**:
> - **PHẦN 1**: 17 entities, 13 TypeScript enums, layered ERD, relationship mapping
> - **PHẦN 2**: 13 middleware modules, 9 detail processes (P01–P09), 38 API endpoints, state machine
> - **PHẦN 3**: 21 screens, 8 ASCII wireframes, 12 product_component flags, mobile/desktop differences
> - **PHẦN 4**: 10 user flows (8 Student + 2 System), role-based step tables
> - **PHẦN 5**: 12 algorithms (pseudocode), 10 KPIs, 25-item consistency checklist, 10 improvement suggestions
> - **PHẦN 6**: 30 modules, 7 layers, dependency graph, 223 action types, Clean Architecture pattern

```
