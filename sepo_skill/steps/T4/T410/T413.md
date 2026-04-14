---
name: sepo-t413
description: "T413 - Push len Git"
---

# Skill: T413 — Push len Git

**Expert Team:** DevOps Engineer + Git Specialist

## Objective
Commit va push code len remote repository.

**Scope:** Git, Version Control

## Trigger condition
Sau selftest pass (T412). Keywords: "push git", "commit"

## Preconditions
- SelfTest pass (T412)
- Git configured

## Inputs
### Required
- Git repo
- Commit message convention
### Optional
- PR template

## Output Contract
- Code pushed to remote
- Commit message theo convention

## Internal Reasoning Checklist
- [ ] Dung branch?
- [ ] Commit message clear?
- [ ] No sensitive data in commit?

## 📥 Download từ SEPO (trước khi execute)

> Step này **không cần download docs** từ SEPO.

## 📤 Upload lên SEPO (sau khi hoàn thành)

> Step này **không có output file cần upload** (chỉ là code commit / config change).

## Execution Procedure
1. git status — review changes
2. git add (specific files, khong add .env)
3. git commit -m "type(scope): description"
4. git push origin [branch]
5. Verify: push thanh cong

## Branching Logic
- Neu conflict → resolve conflict, test lai, push
- Neu CI fail → fix, push lai

## Validation Checklist
- [ ] Push thanh cong
- [ ] CI/CD triggered (neu co)
- [ ] No secrets in commit

## Failure Cases
- Push rejected
- CI fail

## Recovery Actions
- Pull latest, resolve conflict
- Fix CI issues

## Completion Criteria
- Code on remote
- CI pass (neu co)

## Next-Step Handoff
- T414 (Deploy Staging)

## Reusable Prompt Block
```text
Hỏi người dùng đang thuộc "Usecase 1 :Nếu dự án xây mới lần đầu tiên" hay thuộc "Usecase 2: Nếu dự án này là sửa hệ thống đang có" thì sẽ sử dụng prompt tương ứng ở phía dưới đây:
Usecase 1: Nếu dự án xây mới lần đầu tiên thì sử dụng prompt
 "Đọc file vault.md để lấy credential cho git, 
1. Dựa vào quy luật đặt mã và tên hệ thống trong file GIT-DB-Structure.md.
- Từ đầu tiên là đối tượng người dùng chính (VD: Student, Teacher, Parent, Sale, ...)
- Từ thứ 2 là chức năng chính hệ thống (VD: Tutoring, Education, Report, Compensation, ....)
- Từ thứ 3 là giao diện chính của hệ thống (VD: Web, App, Portal, ...)
Mã được ghép từ những chữ cái đầu của các tên.
Hãy gợi ý 3 mã và tên cho để tôi chọn
2. Merge code backend và frontend vào chung 1 repo với name lấy từ bước 1
3.  push code trong repo chung này lên git này https://gitlab.clevai.vn/fe/[repo name]
4. Tạo Merge Request lên staging-c
5. Thông báo trạng thái khi deploy xong

Usecase 2: Nếu dự án này là sửa hệ thống đang có thì sử dụng prompt này
"1. Trước khi push code hãy pull code từ repo về đễ giữ code luôn up to date
2. Sau khi có update code mới thì commit và đẩy code lên  ở nhánh staging-c
3. Thông báo trạng thái khi deploy xong"

```

## Examples
```text
1. Trước khi push code hãy pull code từ repo về đễ giữ code luôn up to date
2. Sửa lại gitlab repo như bên dưới
group: comaker · GitLab
repo FE: comaker-m1-fe
repo BE: comaker-m1-be
sau khi có update code mới thì commit và đẩy code lên 2 repo này ở nhánh staging-c
```
