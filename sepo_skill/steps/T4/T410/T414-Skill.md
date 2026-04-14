---
name: sepo-t414
description: "T414 - Deploy len Staging"
---

# Skill: T414 — Deploy len Staging

**Expert Team:** DevOps Engineer + SRE

## Objective
Deploy app len staging server de team test.

**Scope:** Deployment, Staging environment

## Trigger condition
Sau push git (T413). Keywords: "deploy staging"

## Preconditions
- Code pushed (T413)
- Staging server accessible

## Inputs
### Required
- Staging server credentials
- Deploy process docs
### Optional
- Docker config
- CI/CD pipeline

## Output Contract
- App deployed on staging
- Staging URL accessible
- Health check pass

## Internal Reasoning Checklist
- [ ] Staging config dung?
- [ ] DB staging da migrate?

## 📥 Download từ SEPO (trước khi execute)

> Step này **không cần download docs** từ SEPO.

## 📤 Upload lên SEPO (sau khi hoàn thành)

> Step này **không có output file cần upload** (chỉ là code commit / config change).

## Execution Procedure
1. Xac dinh deploy method: manual SSH, Docker, CI/CD
2. Deploy code to staging
3. Verify: curl staging-url/health
4. Smoke test on staging

## Branching Logic
- Neu deploy fail → rollback to previous version

## Validation Checklist
- [ ] Staging accessible
- [ ] Health check 200
- [ ] Basic flows work

## Failure Cases
- Deploy fail
- Staging down after deploy

## Recovery Actions
- Rollback
- Check logs

## Completion Criteria
- Staging live
- Health pass

## Next-Step Handoff
- T415 (Checkpoint STG)

## Reusable Prompt Block
```text
Hỏi người dùng đang thuộc "Usecase 1 :Nếu dự án xây mới lần đầu tiên" hay thuộc "Usecase 2: Nếu dự án này là sửa hệ thống đang có" thì sẽ sử dụng prompt tương ứng ở phía dưới đây:

Usecase 1: Nếu dự án xây mới lần đầu tiên thì sử dụng prompt
 "Đọc file vault.md để lấy credential cho git,
1. Fix CI/CD build fail
2. Tự xử lý Vault
3. Tạo domain theo tên đã chọn với format : [repo name].mikai.tech"

Usecase 2: Nếu dự án này là sửa hệ thống đang có thì không cần làm gì thêm.
```
