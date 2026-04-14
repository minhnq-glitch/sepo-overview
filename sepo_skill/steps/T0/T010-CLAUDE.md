# CLAUDE.md — T010 — Setup ClaudeCode: Pull Git va ket noi DB (L3: Step)

> **Level:** L3 — Downloaded when user starts step T010.
> **Download:** Into project folder, overrides project CLAUDE.md.
> **Activate:** Injected into all prompts while executing step T010.

---

## Objective
Thiet lap moi truong phat trien: clone tat ca git repos lien quan va ket noi database de developer co the bat dau code ngay.

## Expert Team
DevOps Engineer + Backend Developer

## Preconditions (MUST be true before starting)
- Co quyen truy cap GitLab (gitlab.clevai.vn)
- Co file GIT-DB-Structure.md trong project (chua thong tin repos)
- Co file vault.md trong project (chua credentials)
- Node.js >= 20 da cai
- Git da cai va config

## Output Contract (MUST deliver these)
- Tat ca repos da clone thanh cong vao may local
- DB connection verified (co the query)
- Dev server co the start (npm run dev hoac tuong duong)
- File .env da tao voi dung credentials

## Validation Checklist
- [ ] Moi repo: git status khong co loi
- [ ] DB: SELECT 1 tra ve thanh cong
- [ ] Dev server: curl localhost:PORT/health tra ve 200
- [ ] .env file ton tai va co dung format

## Failure Cases (watch out)
- Git clone fail: sai token hoac repo khong ton tai
- DB connection refused: sai host/port/password hoac firewall block
- npm install fail: Node version khong tuong thich
- Dev server crash on start: thieu env vars hoac DB chua san sang

## Branching Logic
- Neu repo da ton tai → git pull thay vi git clone
- Neu DB khong ket noi duoc → kiem tra firewall, VPN, credentials
- Neu npm install fail → xoa node_modules va package-lock.json, chay lai
- Neu co nhieu modules → hoi user setup tat ca hay chi 1 module

## SEPO Integration

### Before executing this step:
1. Login SEPO: `POST /api/dev/quick-login`
2. Check required docs exist locally (see skill file for list)
3. If missing: `GET /api/docs?projectId=X` → filter → download

### After completing this step:
1. Ask user: "Upload output to SEPO? (y/n)"
2. If yes: `POST /api/docs` → `POST /api/docs/{id}/versions`
3. Update task-item status: `PATCH /api/task-items/{id}` → doneStatus="Done"

## Completion Criteria (step DONE when)
- TAT CA repos da clone va o dung branch
- DB connection verified
- Dev server start thanh cong
- Developer co the bat dau code

## Next Step
- T110 (L1: Voi tong) — bat dau phan tich he thong

## Prompt Preview (see SKILL file for full prompt)
> Step 1:
> Đọc file GIT-DB-Structure.md và vault.md. Sau đó:
> 1. Xác định tất cả git repos phục vụ module [MODULE] từ ma trận trong GIT-DB-Structure.md
> 2. Dùng credentials trong vault.md để clone toàn bộ repos đó về thư mục [MODULE] trên local
> 3. Kết nối vào database (credentials trong vault.md), liệt k...

## Global Rules
Read `sepo_skill/CLAUDE.md` (L1) for Iron Rules, Git conventions, Tech Stack, SEPO API reference.
