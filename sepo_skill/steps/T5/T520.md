---
name: sepo-t520
description: "T520 - Post Production"
---

# Skill: T520 — Post Production

## Objective
Umbrella task. Goi cac sub-steps ben duoi de thuc hien.

## Trigger condition
User bat dau phase T520.

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
Ban la SEPO Process Orchestrator. Phase T520 (Post Production) can thuc hien cac sub-steps sau:
[list sub-steps]
Hay bat dau voi step dau tien.
```
