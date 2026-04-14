---
name: sepo-t420
description: "T420 - UAT Staging"
---

# Skill: T420 — UAT Staging

## Objective
Umbrella task. Goi cac sub-steps ben duoi de thuc hien.

## Trigger condition
User bat dau phase T420.

## Preconditions
- Previous phase completed

## Inputs
### Required
- Previous phase outputs

### Optional
- None

## Output Contract
- All sub-steps completed

## Execution Procedure
1. Xac dinh sub-steps can thuc hien
2. Goi tung sub-step theo thu tu
3. Verify moi sub-step truoc khi chuyen step tiep

## Branching Logic
- Neu sub-step fail → fix truoc khi tiep tuc

## Validation Checklist
- [ ] All sub-steps completed

## Failure Cases
- Sub-step fail

## Recovery Actions
- Fix va retry sub-step

## Completion Criteria
- All sub-steps done

## Next-Step Handoff
- Phase tiep theo

## Reusable Prompt Block
```text
Ban la SEPO Process Orchestrator. Phase T420 (UAT Staging) can thuc hien cac sub-steps sau:
[list sub-steps]
Hay bat dau voi step dau tien.
```
