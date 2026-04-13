# CLAUDE.md — Task T422: Execute UAT & Collect Feedback

## Task Identity
- **Code:** T422 | **Phase:** T4 — UAT | **Type:** A | **Team:** QA Tester + UX

## Objective
Thuc hien UAT, thu thap feedback tu nguoi dung.

## Preconditions
- T421 (test prep) da xong

## Input Docs
- **Test cases** | **T720_TestFeedback_Template** — Feedback template

## Output Contract
- **TestFeedback_{date}.xlsx** — Bugs, screenshots, severity ratings

## Instructions
1. Download va su dung file T720_TestFeedback_Template
2. Thuc hien tung test case theo plan
3. Ghi lai ket qua: pass/fail, screenshots, notes
4. Danh gia severity: Critical / High / Medium / Low
5. Tao file TestFeedback_{date}.xlsx

## References
- @docs/T720_TestFeedback_Template.xlsx — load for feedback template
