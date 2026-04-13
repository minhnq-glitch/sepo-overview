# CLAUDE.md — Task T423: Classify Feedback A/B/C

## Task Identity
- **Code:** T423 | **Phase:** T4 — UAT | **Type:** A | **Team:** QA + PM + Architect

## Objective
Phan loai feedback thanh A/B/C de dinh huong fix.

## Preconditions
- T422 (UAT feedback) da co

## Input Docs
- **TestFeedback.xlsx** | **layerevent.md** — Classification rules

## Output Contract
- Classified feedback: A-type (UI), B-type (Logic), C-type (Performance)

## Instructions
1. Doc ky TestFeedback va mo ta lai hieu biet
2. Phan loai theo layerevent.md:
   - **A:** Chi UI/API, khong sua schema
   - **B:** Co sua schema, khong touch Event/CalculateKR layers
   - **C:** Touch Event/EventDetails/CalculateKR/ExtractEvent layers
3. Luu y: B can POSUP duyet, C can POSUP + ARCH duyet

## References
- @docs/layerevent.md — load for classification rules
