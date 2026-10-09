# AI Appendix (Level 1)

Under the course policy, AI use at Level 1 is allowed **with prompts and corrections attached**. Undisclosed use is treated as plagiarism. This appendix records what was generated with AI and what was checked and changed by the author.

> **Author — complete the bracketed fields.** The prompts below are the real messages sent in the session. The "What I changed and why" column can only be filled in by the person who reviewed the files; it is left blank on purpose, because inventing edits would make this appendix a false statement.

**Tool:** Claude (Anthropic), used in a chat session on 9 October 2026.
**Material given to the model:** the Week 4 lecture slides (`Week_4_Masters_ST.pptx`) only. The week 3 transfer brief and PICT output were **not** provided to the model.

## 1. Prompts used

| # | Prompt (verbatim) | Used for |
|---|---|---|
| 1 | "what must i do here in homework" (with the Week 4 slides attached) | Reading the assignment from slides 38–39 |
| 2 | "create me a test plan" (followed by two choices: use the slide requirements; mobile banking app) | `1_test_plan.md` |
| 3 | "okay then generate me everything" | `2_test_cases.md`, `3_traceability_matrix.md`, `4_sms_release_checklist.md`, `5_defect_reports.md`, this appendix |

## 2. Raw output and what I changed

| File | Raw AI output (summary) | What I changed and why |
|---|---|---|
| `1_test_plan.md` | Scope in/out, approach, entry and exit criteria, three product risks, assumptions | _[fill in]_ |
| `2_test_cases.md` | Twelve cases: 4 × AMT, 2 × DAY, 4 × OTP, 2 × BAL, with common steps and an uncovered list | _[fill in]_ |
| `3_traceability_matrix.md` | Requirement → test table, reverse table, uncovered items, coverage summary | _[fill in]_ |
| `4_sms_release_checklist.md` | Twelve-item checklist, each traced to a requirement or test | _[fill in]_ |
| `5_defect_reports.md` | Three defect reports designed from requirement gaps | _[fill in]_ |

## 3. Things the AI assumed that I must check against my real material

These are facts the model could not know. Each one is a factual claim in the documents.

- Requirement texts REQ-3.1 – REQ-3.4 are paraphrased from the lecture slides, not from my week 3 brief.
- Test customers TC-014 (from the slides) and TC-021 (invented), recipient account ending 4417 (from the slides), and the "Profile → Limits → Used today" screen for the daily total (invented).
- SMS code valid for 120 seconds, three wrong attempts cancel the transfer, SMS arrives within 30 seconds, code screen within 3 seconds (taken from the slide examples; not confirmed as requirements — see DEF-001).
- Amounts up to 100,000 KZT complete without a confirmation screen, or with a single Confirm tap.
- A balance of 0 after transfer is allowed (no minimum balance).
- Android 8 and 14 and iOS 18.2 as target devices (Android 8 and iOS 18.2 are from the slides).
- The three defect reports describe gaps in the requirements; I must confirm each gap exists in my actual brief.

## 4. My checks before submitting

- [ ] I read every file and changed what did not match my own week 3 work.
- [ ] I replaced the `_[your name]_` and `_[your name, group]_` placeholders.
- [ ] I re-followed at least one test case and one defect report step by step, as the instructor will.
- [ ] I filled in the "What I changed and why" column above with edits I actually made.
- [ ] Severity and priority in the defect reports are marked as proposals, and I agree with them.
